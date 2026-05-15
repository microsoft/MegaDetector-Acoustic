---
description: "MegaDetector-Acoustic — Microsoft AI for Good Lab's open-source bioacoustic AI for audio-based wildlife detection, classification, and biodiversity monitoring."
tags:
  - MegaDetector-Acoustic
  - bioacoustics
  - wildlife monitoring
  - audio classification
  - passive acoustic monitoring
  - PyTorch-Wildlife
  - conservation AI
---

# Microsoft MegaDetector-Acoustic

**Open-source bioacoustic AI for audio-based wildlife detection and classification.**

MegaDetector-Acoustic is a toolkit from the [Microsoft AI for Good Lab](https://www.microsoft.com/en-us/research/group/ai-for-good-research-lab/) for training, evaluating, and deploying deep learning models on passive acoustic monitoring recordings. It is powered by the [PyTorch-Wildlife](https://github.com/microsoft/PytorchWildlife) framework and is part of the [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) ecosystem.

---

## What It Does

MegaDetector-Acoustic turns raw audio recordings into structured species predictions:

1. **Dataset preparation** — generates sliding windows and GPU-accelerated mel spectrograms from raw `.wav` / `.flac` files
2. **Training** — binary or multiclass classification using ResNet backbones with spectrogram augmentation (SpecAugment, MixUp)
3. **Inference** — sliding-window predictions on long recordings, exported as CSV with per-window probabilities

---

## Get Started

See the [Installation](installation.md) page, then run the demo notebook:

```bash
git clone https://github.com/microsoft/MegaDetector-Acoustic
cd MegaDetector-Acoustic
pip install -r requirements.txt
jupyter notebook demo/bioacoustics_demo.ipynb
```

The demo walks through the full pipeline using real bird recordings from the [PteroSet](https://zenodo.org/records/19137071) dataset.

---

## Part of the Biodiversity Ecosystem

MegaDetector-Acoustic is one model in a larger open-source ecosystem from the Microsoft AI for Good Lab.

| Repository | Description |
|---|---|
| [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) | Umbrella hub — PyTorch-Wildlife, MegaDetector, ecosystem overview |
| [microsoft/MegaDetector](https://github.com/microsoft/MegaDetector) | Animal, human, and vehicle detection for camera-trap images |
| [microsoft/PytorchWildlife](https://github.com/microsoft/PytorchWildlife) | The collaborative deep learning framework for wildlife monitoring |
| [microsoft/MegaDetector-Acoustic](https://github.com/microsoft/MegaDetector-Acoustic) | **This repo** — bioacoustic AI for audio-based wildlife monitoring |
| [microsoft/MegaDetector-Overhead](https://github.com/microsoft/MegaDetector-Overhead) | Wildlife detection in aerial and drone imagery |
| [microsoft/MegaDetector-Sonar](https://github.com/microsoft/MegaDetector-Sonar) | Sonar-based wildlife detection for aquatic monitoring |
| [microsoft/MegaDetector-Classifier](https://github.com/microsoft/MegaDetector-Classifier) | Camera-trap species classification fine-tuning — adapt classifiers to your own datasets and geographic regions |
| [microsoft/SPARROW](https://github.com/microsoft/SPARROW) | Solar-Powered Acoustic and Remote Recording Observation Watch — AI-enabled edge device |
| SPARROW Studio | Desktop application for all AI for Good Lab models |
