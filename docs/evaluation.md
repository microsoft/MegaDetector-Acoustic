---
title: "Evaluating Audio Species Classification Models"
description: "How to evaluate MegaDetector-Acoustic predictions: per-window probabilities, validation splits, and the practical limits of bioacoustic AI classifiers."
tags:
  - bioacoustic AI
  - audio species classification
  - model evaluation
  - passive acoustic monitoring
  - MegaDetector-Acoustic
---

# Evaluating Audio Species Classification Models

A bioacoustic classifier is only useful if you know how far to trust it. This page explains how MegaDetector-Acoustic reports results, how to interpret them, and where the practical limits lie. It pairs with the [Bioacoustic workflow](workflow.md) and the [Terrestrial passive acoustic monitoring](passive-acoustic-monitoring.md) guide.

## How results are reported

Training uses train, validation, and test splits produced during dataset preparation, with metrics computed through `torchmetrics`. Inference does not return a single label per recording; it returns a probability for every sliding window, written to CSV. That means evaluation happens at the window level: each window has a predicted probability that you compare against ground-truth annotations.

The demo notebook shows this directly. It runs a pre-trained ONNX model on PteroSet recordings and visualizes predictions against ground truth, so you can see where the model agrees with the annotations and where it does not.

## Reading per-window probabilities

Because output is per-window, you choose how to act on it:

- **Thresholding.** Pick a probability cutoff that suits your tolerance for false positives versus missed detections. A lower threshold surfaces more candidate detections for review; a higher one keeps only confident windows.
- **Aggregation.** Combine adjacent positive windows into events, since a single vocalization usually spans several overlapping windows.
- **Review.** For monitoring work, treat high-probability windows as candidates to confirm against the source audio rather than as final labels.

## Practical limitations

A few limits are worth stating plainly so results are not over-interpreted:

- **It classifies what it was trained on.** A model only recognizes the species (or the target-versus-noise split) declared in its config and present in its training data. New species require new training data.
- **Domain shift matters.** A classifier trained on one site, recorder, or season may perform differently elsewhere. Performance reported on one dataset does not transfer automatically to another soundscape.
- **Audio quality affects detection.** Overlapping calls, wind, rain, and distant or faint vocalizations are harder, as they are for any acoustic method.
- **Predictions are probabilities, not certainties.** The output is a per-window score; converting that into presence or absence requires a threshold you set and, for high-stakes conclusions, human review.

<!-- CLAIM: verify with zhmiao. No published accuracy, precision/recall, or per-species performance numbers are documented in the repo. None are stated on this page deliberately. -->

## What this toolkit does not do

MegaDetector-Acoustic is the modeling layer. It does not record audio in the field (see [SPARROW](https://microsoft.github.io/SPARROW/) for hardware) and it does not process imagery (see [MegaDetector](https://microsoft.github.io/MegaDetector/) for camera-trap detection). Its job is to turn audio you already have into reviewable species predictions. For how these pieces relate, see [Audio vs. image-based monitoring](positioning.md).
