---
title: "Bioacoustic Workflow: Collect, Segment, Classify, Review"
description: "A bioacoustic AI workflow for audio species classification: segment recordings, generate spectrograms, train a classifier, and review predictions."
tags:
  - bioacoustic AI
  - audio species classification
  - bioacoustic workflow
  - passive acoustic monitoring
  - MegaDetector-Acoustic
---

# Bioacoustic Workflow: Collect, Segment, Classify, Review

This page describes the end-to-end bioacoustic AI workflow that MegaDetector-Acoustic supports, from raw field recordings to reviewed species predictions. Each stage maps to a CLI script in the repository, and the same path is demonstrated in the [demo notebook](https://github.com/microsoft/MegaDetector-Acoustic/blob/main/demo/bioacoustics_demo.ipynb).

If you are setting up for the first time, start with [Installation](installation.md). For where this approach fits among recording hardware and image-based methods, see [Audio vs. image-based monitoring](positioning.md).

## 1. Collect

The workflow begins with audio you have already recorded. MegaDetector-Acoustic reads standard `.wav` and `.flac` files, so any passive acoustic monitoring recorder that writes those formats can feed the pipeline. The recorder's sample rate is declared in your YAML config (the demo configs use 48 kHz), which keeps spectrogram parameters consistent between training and inference.

Recording in the field is a separate concern handled by dedicated hardware. For a solar-powered edge recorder built for remote deployments, see [SPARROW](https://microsoft.github.io/SPARROW/).

## 2. Segment

Continuous recordings are long, but vocalizations are short. The `prepare_dataset.py` script slices each recording into overlapping windows and generates a mel spectrogram for every window. Window length and overlap are set in the `audio` block of your config:

```yaml
audio:
  sample_rate: 48000
  window_size_sec: 5.0
  overlap_sec: 4.0

spectrogram:
  n_fft: 2048
  hop_length: 512
  n_mels: 224
```

Spectrogram generation is GPU-accelerated and falls back to CPU when no GPU is available. The output is a set of spectrograms plus train, validation, and test splits ready for the classifier.

```bash
python prepare_dataset.py --config config/my_domain.yaml
```

## 3. Classify

With spectrograms prepared, you train a classifier. MegaDetector-Acoustic uses ResNet backbones and supports two task types:

- **Binary.** Distinguish one target signal from background noise (for example, the demo's AVEVOC-vs-noise task).
- **Multiclass.** Assign each window to one of several species (the demo trains a top-4 species classifier).

```bash
# Binary classification
python train.py --config config/my_domain.yaml \
    --train_csv train_split.csv \
    --val_csv val_split.csv \
    --test_csv test_split.csv

# Multiclass classification (for example, 4 species)
python train.py --config config/my_domain.yaml \
    --train_csv train_split.csv \
    --test_csv test_split.csv \
    --num_classes 4
```

Training applies spectrogram augmentation such as SpecAugment and MixUp to improve generalization. Backbone, batch size, learning rate, and epochs are all set in the `training` block of the config.

## 4. Review

Inference scans long recordings with a sliding window and writes per-window probabilities to CSV, so you can review predictions against the source audio and ground-truth annotations.

```bash
python inference.py --config config/my_domain.yaml \
    --checkpoint model.ckpt \
    --audios_source /path/to/audio/folder \
    --dataset my_inference
```

For teams that prefer not to train from scratch, the demo also shows how to run a pre-trained ONNX model (`MD_AudioBirds_V1.onnx`, downloaded from Zenodo) and visualize its predictions against ground truth. How to read and threshold these outputs, and the limits of doing so, are covered on the [Evaluation and limitations](evaluation.md) page.

## Putting it together

The four stages form one continuous pipeline: collect audio, segment it into spectrogram windows, classify each window, and review the results. Because the whole workflow is driven by a single YAML config, adapting it to a new terrestrial soundscape is mostly a matter of declaring new species and pointing at new data. See [Passive acoustic monitoring (terrestrial)](passive-acoustic-monitoring.md) for how this fits into a monitoring study.
