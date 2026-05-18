# MegaDetector-Acoustic

Microsoft AI for Good Lab's open-source AI for bioacoustic biodiversity monitoring — audio-based species detection and classification from passive acoustic recordings.

[![License](https://img.shields.io/github/license/microsoft/MegaDetector-Acoustic)](https://github.com/microsoft/MegaDetector-Acoustic/blob/main/LICENSE)
[![Python 3.9+](https://img.shields.io/badge/python-3.9%2B-blue.svg)](https://www.python.org/downloads/)
[![PyTorch-Wildlife](https://img.shields.io/badge/PyTorch--Wildlife-ecosystem-green.svg)](https://github.com/microsoft/Biodiversity)

MegaDetector-Acoustic is part of the [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) ecosystem and is powered by the [PyTorch-Wildlife](https://github.com/microsoft/PytorchWildlife) framework. It is free, open-source, and available under the MIT license.

## Part of the Biodiversity Ecosystem

MegaDetector-Acoustic is one model in a larger open-source ecosystem from the Microsoft AI for Good Lab. Each project lives in its own repository, with the [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) umbrella tying them together.

| Repository | Description |
|---|---|
| [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) | The umbrella repository — documentation hub for the AI for Good Lab's biodiversity work |
| [microsoft/MegaDetector](https://github.com/microsoft/MegaDetector) | Animal, human, and vehicle detection for camera-trap images |
| [microsoft/PytorchWildlife](https://github.com/microsoft/PytorchWildlife) | The collaborative deep learning framework that hosts MegaDetector, species classifiers, and demo notebooks |
| [microsoft/MegaDetector-Acoustic](https://github.com/microsoft/MegaDetector-Acoustic) | **This repo** — bioacoustic AI for audio-based wildlife detection and classification |
| [microsoft/MegaDetector-Overhead](https://github.com/microsoft/MegaDetector-Overhead) | Wildlife detection in aerial and drone imagery |
| [microsoft/MegaDetector-Sonar](https://github.com/microsoft/MegaDetector-Sonar) | Sonar-based wildlife detection for aquatic monitoring |
| [microsoft/MegaDetector-Classifier](https://github.com/microsoft/MegaDetector-Classifier) | Camera-trap species classification fine-tuning — adapt classifiers to your own datasets and geographic regions |
| [microsoft/SPARROW](https://github.com/microsoft/SPARROW) | Solar-Powered Acoustic and Remote Recording Observation Watch — AI-enabled edge device for field recording |
| SPARROW Studio | Desktop application for all AI for Good Lab models |

## Overview

MegaDetector-Acoustic provides CLI scripts and training tools for audio-based wildlife detection and classification. The core deep learning infrastructure (models, datasets, spectrogram utilities) is provided by [PyTorch-Wildlife](https://github.com/microsoft/PytorchWildlife).

**Key capabilities:**
- Mel spectrogram generation from raw audio (GPU-accelerated)
- Binary and multiclass species classification using ResNet backbones
- Flexible YAML-based domain configuration
- Sliding-window inference on long audio recordings
- End-to-end pipeline from raw audio → model predictions → CSV output

**Tested on:**
- [PteroSet](https://github.com/microsoft/PteroSet) — tropical bird vocalizations from passive acoustic monitoring
- [CookInlet_Belugas](https://github.com/microsoft/CookInlet_Belugas) — endangered Cook Inlet beluga whale detection

## Installation

```bash
git clone https://github.com/microsoft/MegaDetector-Acoustic
cd MegaDetector-Acoustic
pip install -r requirements.txt
```

**Requirements:** Python 3.9+, PyTorch 2.0+

## Quick Start

### 1. Configure your domain

Create a YAML config file for your domain (see `template.yaml` as reference):

```yaml
name: "my_domain"
datasets:
  - "dataset_name_1"

class_names:
  0: "noise"
  1: "target_species"

paths:
  data_root: "${DATA_ROOT}"
  output_root: "${OUTPUT_ROOT}"

audio:
  sample_rate: 48000
  window_size_sec: 5.0
  overlap_sec: 4.0

spectrogram:
  n_fft: 2048
  hop_length: 512
  n_mels: 224

training:
  batch_size: 32
  lr: 0.0001
  epochs: 50
  backbone: "resnet18"
```

### 2. Prepare dataset

```bash
python prepare_dataset.py --config config/my_domain.yaml
```

### 3. Train

```bash
# Binary classification
python train.py --config config/my_domain.yaml \
    --train_csv train_split.csv \
    --val_csv val_split.csv \
    --test_csv test_split.csv

# Multiclass classification (e.g. 4 species)
python train.py --config config/my_domain.yaml \
    --train_csv train_split.csv \
    --test_csv test_split.csv \
    --num_classes 4
```

### 4. Run inference

```bash
python inference.py --config config/my_domain.yaml \
    --checkpoint model.ckpt \
    --audios_source /path/to/audio/folder \
    --dataset my_inference
```

## Demo

The recommended way to get started is the **end-to-end demo notebook** at [`demo/bioacoustics_demo.ipynb`](demo/bioacoustics_demo.ipynb). It uses real bird recordings from the [PteroSet](https://zenodo.org/records/19137071) dataset and walks through:

1. Data exploration — annotation counts, species distribution
2. ONNX inference — download `MD_AudioBirds_V1.onnx` from Zenodo, run inference, visualise predictions vs. ground-truth
3. Training — binary classification (AVEVOC vs. noise) and multiclass (top-4 species) using `ResNetClassifier`

See [`demo/README.md`](demo/README.md) for setup instructions and expected runtimes.

## Repository Structure

```
MegaDetector-Acoustic/
├── train.py              # Training CLI script
├── inference.py          # Inference CLI script
├── prepare_dataset.py    # Dataset preparation pipeline
├── template.yaml         # Template configuration file
├── requirements.txt      # Python dependencies
└── demo/
    ├── bioacoustics_demo.ipynb   # End-to-end demo notebook
    ├── README.md
    ├── data/                     # Sample audio + annotations
    └── config/                   # Demo YAML configs
```

## Citation

If you use MegaDetector-Acoustic in your research, please cite the PyTorch-Wildlife paper:

```bibtex
@misc{hernandez2024pytorchwildlife,
      title={Pytorch-Wildlife: A Collaborative Deep Learning Framework for Conservation},
      author={Andres Hernandez and Zhongqi Miao and Luisa Vargas and Sara Beery and Rahul Dodhia and Juan Lavista},
      year={2024},
      eprint={2405.12930},
      archivePrefix={arXiv},
}
```

You can also cite this software directly using the [`citation.cff`](citation.cff) file in this repository.

## Contributing

Issues, feature requests, and pull requests are welcome at [microsoft/MegaDetector-Acoustic/issues](https://github.com/microsoft/MegaDetector-Acoustic/issues).

For framework-level changes (PyTorch-Wildlife API, models, datasets), see [microsoft/PytorchWildlife](https://github.com/microsoft/PytorchWildlife). For ecosystem-wide questions, see the [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) umbrella.
