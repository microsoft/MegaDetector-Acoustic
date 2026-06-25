---
description: "How to install and set up MegaDetector-Acoustic for bioacoustic species identification from audio recordings."
tags:
  - MegaDetector-Acoustic installation
  - bioacoustics setup
  - PyTorch-Wildlife
  - audio AI setup
  - wildlife monitoring
---

# Installation

New to MegaDetector-Acoustic? The [Getting Started](getting-started.md) guide puts this install step in the full path from audio to species predictions.

## Requirements

- Python 3.9+
- PyTorch 2.0+
- CUDA (optional, but recommended for spectrogram generation and training)

## Install

```bash
git clone https://github.com/microsoft/MegaDetector-Acoustic
cd MegaDetector-Acoustic
pip install -r requirements.txt
```

This installs the following dependencies:

| Package | Purpose |
|---|---|
| `PytorchWildlife` | Core models, datasets, and spectrogram utilities |
| `librosa` | Audio loading and feature extraction |
| `soundfile` | Audio file I/O |
| `torchaudio` | GPU-accelerated audio processing |
| `pyyaml` | YAML configuration loading |
| `torchmetrics` | Training metrics |
| `pytorch-lightning` | Training loop and checkpointing |
| `pandas` / `numpy` | Data manipulation |

## Verify

```python
from PytorchWildlife.models.bioacoustics import ResNetClassifier
print("MegaDetector-Acoustic is ready.")
```

## GPU Setup

MegaDetector-Acoustic uses GPU acceleration for mel spectrogram generation. Ensure CUDA is available:

```python
import torch
print(torch.cuda.is_available())  # should print True on a CUDA-enabled machine
```

If running on CPU, spectrogram generation will fall back to CPU automatically (slower for large datasets).

## Next Steps

- Run the [demo notebook](https://github.com/microsoft/MegaDetector-Acoustic/blob/main/demo/bioacoustics_demo.ipynb) for an end-to-end walkthrough
- Copy `template.yaml` as a starting point for your domain configuration
- See the [README](https://github.com/microsoft/MegaDetector-Acoustic#quick-start) for full CLI usage
