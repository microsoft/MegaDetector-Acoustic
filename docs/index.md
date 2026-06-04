---
title: "MegaDetector-Acoustic: Terrestrial Bioacoustic Monitoring AI"
description: "Open-source bioacoustic AI for terrestrial passive acoustic monitoring. Train and run audio species classification on your field recordings."
tags:
  - MegaDetector-Acoustic
  - bioacoustic AI
  - terrestrial bioacoustic monitoring
  - passive acoustic monitoring
  - audio species classification
  - PyTorch-Wildlife
  - conservation AI
---

# MegaDetector-Acoustic: Terrestrial Bioacoustic Monitoring AI

**Open-source bioacoustic AI for terrestrial passive acoustic monitoring. Train, evaluate, and run audio species classification on field recordings.**

Passive acoustic monitoring leaves researchers with terabytes of unlabeled audio. MegaDetector-Acoustic is the modeling toolkit that turns those recordings into structured species predictions. You point it at a folder of `.wav` or `.flac` files, and it returns per-window probabilities for the species you trained it on, exported as CSV.

It is a project of the [Microsoft AI for Good Lab](https://www.microsoft.com/en-us/research/group/ai-for-good-research-lab/), built on the [PyTorch-Wildlife](https://github.com/microsoft/PytorchWildlife) framework, and part of the [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) ecosystem. It is free, open-source, and MIT-licensed.

---

## Why MegaDetector-Acoustic

Most bioacoustic tooling ships a fixed model for a fixed taxon. MegaDetector-Acoustic takes a different stance: it gives you the training and inference machinery so you can fit a classifier to *your* terrestrial soundscape, your target species, and your recorder's sample rate. A single YAML config defines the domain; the same pipeline then handles dataset preparation, training, and sliding-window inference end to end.

- **Domain-configurable.** One YAML file declares your species, datasets, audio parameters, and model backbone.
- **GPU-accelerated spectrograms.** Mel spectrograms are generated from raw audio on the GPU.
- **Binary or multiclass.** Detect one target species against background noise, or classify several at once.
- **Built for long recordings.** Sliding-window inference scans hours of continuous audio and writes per-window probabilities to CSV.
- **Backed by a maintained framework.** Models, datasets, and spectrogram utilities come from [PyTorch-Wildlife](https://github.com/microsoft/PytorchWildlife).

---

## How it works

MegaDetector-Acoustic turns raw audio into species predictions in three stages. The full bioacoustic workflow is described on the [Bioacoustic workflow](workflow.md) page.

1. **Dataset preparation** slices recordings into overlapping windows and generates GPU-accelerated mel spectrograms from raw `.wav` / `.flac` files.
2. **Training** fits a ResNet-backbone classifier for binary or multiclass tasks, with spectrogram augmentation (SpecAugment, MixUp).
3. **Inference** runs sliding-window predictions across long recordings and exports per-window probabilities as CSV.

For the longer view on deploying this in a study, see [Passive acoustic monitoring (terrestrial)](passive-acoustic-monitoring.md), and read [Evaluation and limitations](evaluation.md) before acting on predictions.

---

## Get started

Install, then run the demo notebook:

```bash
git clone https://github.com/microsoft/MegaDetector-Acoustic
cd MegaDetector-Acoustic
pip install -r requirements.txt
jupyter notebook demo/bioacoustics_demo.ipynb
```

The demo walks through the full pipeline using real bird recordings from the [PteroSet](https://zenodo.org/records/19137071) dataset, a benchmark of tropical bird vocalizations collected through passive acoustic monitoring. Full setup details are on the [Installation](installation.md) page.

New to acoustic versus image-based monitoring? The [Audio vs. image-based monitoring](positioning.md) note explains where MegaDetector-Acoustic fits alongside the rest of the ecosystem.

---

## Part of the Biodiversity Ecosystem

MegaDetector-Acoustic is one model in a larger open-source ecosystem from the Microsoft AI for Good Lab. Each project handles a different sensing modality; the [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) umbrella hub ties them together.

| Repository | Description |
|---|---|
| [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) | Umbrella hub for PyTorch-Wildlife, MegaDetector, and the ecosystem overview |
| [microsoft/MegaDetector](https://github.com/microsoft/MegaDetector) | Animal, human, and vehicle detection for camera-trap images |
| [microsoft/PytorchWildlife](https://github.com/microsoft/PytorchWildlife) | The collaborative deep learning framework for wildlife monitoring |
| [microsoft/MegaDetector-Acoustic](https://github.com/microsoft/MegaDetector-Acoustic) | **This repo.** Bioacoustic AI for terrestrial audio-based monitoring |
| [microsoft/MegaDetector-Overhead](https://github.com/microsoft/MegaDetector-Overhead) | Wildlife detection in aerial and drone imagery |
| [microsoft/MegaDetector-Sonar](https://github.com/microsoft/MegaDetector-Sonar) | Sonar-based wildlife detection for aquatic monitoring |
| [microsoft/MegaDetector-Classifier](https://github.com/microsoft/MegaDetector-Classifier) | Camera-trap species classification fine-tuning to adapt classifiers to your own datasets and geographic regions |
| [microsoft/SPARROW](https://github.com/microsoft/SPARROW) | Solar-Powered Acoustic and Remote Recording Observation Watch, an AI-enabled edge device for field recording |
| SPARROW Studio | Desktop application for all AI for Good Lab models |
