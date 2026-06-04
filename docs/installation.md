---
title: "Install MegaDetector-Acoustic for Bioacoustic AI"
description: "Install and set up MegaDetector-Acoustic for terrestrial bioacoustic monitoring and audio species classification, including GPU setup and dependency details."
tags:
  - MegaDetector-Acoustic installation
  - bioacoustic AI
  - passive acoustic monitoring
  - PyTorch-Wildlife
  - audio species classification
---

# Installation

This page covers installing MegaDetector-Acoustic so you can run the [bioacoustic workflow](workflow.md) on your own recordings. New to the toolkit? Start with the [Overview](index.md).

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
- Read the [bioacoustic workflow](workflow.md) to understand the collect, segment, classify, review pipeline
- See how the toolkit fits a [terrestrial passive acoustic monitoring](passive-acoustic-monitoring.md) study
- See the [README](https://github.com/microsoft/MegaDetector-Acoustic#quick-start) for full CLI usage
