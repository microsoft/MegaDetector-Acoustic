---
title: "Getting Started with MegaDetector-Acoustic: Audio Species Classification"
description: "Getting started with MegaDetector-Acoustic: install the toolkit, prepare audio into spectrograms, train a classifier, and run sliding-window inference."
slug: getting-started
tags:
  - getting started
  - MegaDetector-Acoustic
  - bioacoustics
  - audio species classification
  - passive acoustic monitoring
---

# Getting Started with MegaDetector-Acoustic

New to MegaDetector-Acoustic? This page is the short path from raw recordings to species predictions. It runs four steps: install the toolkit, turn audio into spectrograms, train a classifier, and run inference on long recordings. Each step points to the deeper reference when you want more detail.

If you would rather see the whole pipeline end to end first, the demo notebook walks through it on real bird recordings.

## Is MegaDetector-Acoustic right for my project?

MegaDetector-Acoustic is the Microsoft AI for Good Lab toolkit for training, evaluating, and deploying deep learning models on passive acoustic monitoring recordings. It is built on the [PyTorch-Wildlife](https://github.com/microsoft/Pytorch-Wildlife) framework and turns raw audio into structured, per-window species predictions.

Reach for it when you:

- Work with passive acoustic recordings rather than camera-trap images.
- Want to train a classifier on your own taxa and habitats.
- Need to scan long field recordings and get probabilities you can review.

If your data is imagery instead of audio, [MegaDetector](https://github.com/microsoft/MegaDetector) is the camera-trap counterpart, and the [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) hub maps the rest of the ecosystem.

## Step 1: Install

Clone the repository and install its dependencies:

```bash
git clone https://github.com/microsoft/MegaDetector-Acoustic
cd MegaDetector-Acoustic
pip install -r requirements.txt
```

You need Python 3.9+ and PyTorch 2.0+. A CUDA GPU is optional but recommended, since spectrogram generation and training run much faster on one. The full requirements list, a verification snippet, and GPU notes are on the [Installation](installation.md) page.

## Step 2: Prepare your audio

The toolkit converts raw `.wav` and `.flac` recordings into model-ready input. First it slices each recording into sliding windows. Then it builds GPU-accelerated mel spectrograms from those windows. This preparation pass feeds both training and inference, so run it on a small sample first and confirm the windows and spectrograms look right before you commit to a full dataset.

## Step 3: Train a classifier

With spectrograms ready, train a classifier. MegaDetector-Acoustic uses ResNet backbones. It supports both binary classification (is this one target species present or not) and multiclass classification (which of several species is calling), and spectrogram augmentation such as SpecAugment and MixUp helps the model generalize from the limited labeled audio that bioacoustic projects usually have. Copy `template.yaml` as the starting point for your domain configuration.

## Step 4: Run inference and review

Point a trained model at long recordings to get predictions. Inference runs as a sliding window across each file and exports a CSV with per-window probabilities. The output is probabilities, not hard labels. That gives you room to choose: set a confidence cutoff, aggregate adjacent windows, or send the uncertain ones to a human for review, depending on how much precision your study needs.

## Next steps and help

- Run the [demo notebook](https://github.com/microsoft/MegaDetector-Acoustic/blob/main/demo/bioacoustics_demo.ipynb) for an end-to-end walkthrough on real bird recordings.
- See the [README](https://github.com/microsoft/MegaDetector-Acoustic#quick-start) for full command-line usage.
- File questions and issues at [microsoft/MegaDetector-Acoustic](https://github.com/microsoft/MegaDetector-Acoustic).

MegaDetector-Acoustic is part of the [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) umbrella, which links every tool in the AI for Good Lab wildlife ecosystem.
