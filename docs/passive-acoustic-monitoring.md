---
title: "Terrestrial Passive Acoustic Monitoring with AI"
description: "Use AI for terrestrial passive acoustic monitoring: build audio species classifiers for birds and other vocal taxa, then scan long field recordings at scale."
tags:
  - terrestrial bioacoustic monitoring
  - passive acoustic monitoring
  - bioacoustic AI
  - audio species classification
  - MegaDetector-Acoustic
---

# Terrestrial Passive Acoustic Monitoring with AI

Passive acoustic monitoring (PAM) places recorders in the field and lets them listen, often for weeks at a time. The result is a large archive of continuous audio that is far too big to review by ear. MegaDetector-Acoustic is the modeling layer that makes that archive searchable: you train a classifier for the species in your study, then run it across every recording to find where they vocalize.

This page covers how to apply the toolkit to a terrestrial monitoring study. For the step-by-step pipeline, see the [Bioacoustic workflow](workflow.md). To install, see [Installation](installation.md).

## What "terrestrial" means here

MegaDetector-Acoustic is domain-configurable through YAML, so the same code can be fit to many acoustic domains. Its terrestrial use case centers on vocal land animals, with birds being the most common target in passive acoustic monitoring. The bundled demo uses [PteroSet](https://github.com/microsoft/PteroSet), a benchmark of tropical bird vocalizations recorded through passive acoustic monitoring, as a worked terrestrial example.

Because the domain is defined entirely in config, adapting to a new terrestrial soundscape means declaring your species and pointing at your recordings, not rewriting code.

## A monitoring study, end to end

A typical terrestrial PAM study with MegaDetector-Acoustic follows the four-stage [workflow](workflow.md):

1. **Deploy recorders** and gather `.wav` / `.flac` audio. The recording hardware itself is out of scope here; for a field-ready solar recorder see [SPARROW](https://microsoft.github.io/SPARROW/).
2. **Prepare the dataset** by segmenting recordings into overlapping windows and generating mel spectrograms.
3. **Train a classifier.** Binary to confirm presence or absence of a single species, multiclass to separate several at once.
4. **Run inference** across the full archive and review the per-window CSV output against the source audio.

## Choosing binary vs. multiclass

The task type follows your monitoring question:

- A **single-species** question ("is this bird present at this site?") maps cleanly to a binary classifier, trained as the target species versus background noise.
- A **community** question ("which of these species are calling, and when?") maps to a multiclass classifier covering the species of interest.

Both share the same dataset-preparation and inference steps; only the training command and the `class_names` in your config change.

## Scaling to long deployments

PAM deployments produce hours of continuous audio per recorder per day. Sliding-window inference is built for exactly this: it steps a fixed-length window across each recording and emits a probability per window, so a multi-week deployment becomes a single reviewable CSV per recording. Window size and overlap are tuned in the `audio` block of your config to match how long your target vocalizations last.

## Related reading

- [Bioacoustic workflow](workflow.md) walks through the collect, segment, classify, review pipeline in detail.
- [Evaluation and limitations](evaluation.md) covers how to interpret predictions and what the toolkit does not do.
- [microsoft/Biodiversity](https://github.com/microsoft/Biodiversity) is the umbrella hub for the AI for Good Lab's biodiversity monitoring projects.
