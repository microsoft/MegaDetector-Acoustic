---
title: "Audio vs. Image-Based Wildlife Monitoring"
description: "When to use bioacoustic AI versus camera-trap imagery for wildlife monitoring, and how MegaDetector-Acoustic fits the AI for Good biodiversity tools."
tags:
  - bioacoustic AI
  - passive acoustic monitoring
  - terrestrial bioacoustic monitoring
  - wildlife monitoring
  - MegaDetector-Acoustic
---

# Audio vs. Image-Based Wildlife Monitoring

Sound and images answer different questions about the same landscape. This short note explains where audio-based monitoring is the right tool and how MegaDetector-Acoustic works alongside the image-based and hardware projects in the same ecosystem. These methods are complementary, not competing: many studies use more than one.

## When audio is the right sensor

Audio monitoring suits species and questions that are easier to hear than to see:

- **Vocal species.** Birds, frogs, and other animals that call far more often than they walk past a camera.
- **Dense or dark habitats.** Forest canopy and nighttime activity, where a microphone has range that a camera does not.
- **Continuous coverage.** A recorder listens around the clock, capturing activity between the moments a camera would trigger.

MegaDetector-Acoustic is the modeling layer for this case: train a classifier on your target sounds, then scan long recordings for them. See the [Bioacoustic workflow](workflow.md), the [Terrestrial passive acoustic monitoring](passive-acoustic-monitoring.md) guide, and [Evaluation and limitations](evaluation.md).

## When imagery is the right sensor

Cameras answer questions that sound cannot. Visual identification of quiet mammals, counts of individuals in a frame, and detection of humans or vehicles are image problems. For those, use [MegaDetector](https://microsoft.github.io/MegaDetector/), the AI for Good Lab's detector for camera-trap images. It locates animals, people, and vehicles in photos so reviewers can skip the empty frames. MegaDetector-Acoustic does not process imagery, and MegaDetector does not process audio; each is purpose-built for its modality.

## Where the recording happens

Both methods depend on getting data off the landscape first. Field recording hardware is a separate layer from the models that interpret the data. For a solar-powered edge recorder designed for remote acoustic deployments, see [SPARROW](https://microsoft.github.io/SPARROW/). MegaDetector-Acoustic begins once you have audio files in hand.

## The shared foundation

The classifiers in MegaDetector-Acoustic are built on [PyTorch-Wildlife](https://github.com/microsoft/PytorchWildlife), the framework that also hosts MegaDetector and other models. Using a common framework means the audio and image tools share conventions and can be combined in a single conservation workflow.

## Related Microsoft biodiversity AI projects

- **[microsoft/Biodiversity](https://github.com/microsoft/Biodiversity)** is the umbrella hub that ties the ecosystem together and links to every project.
- **[MegaDetector](https://microsoft.github.io/MegaDetector/)** handles animal, human, and vehicle detection for camera-trap imagery, the image-based counterpart to this audio toolkit.
- **[SPARROW](https://microsoft.github.io/SPARROW/)** is a solar-powered edge device for recording in the field, upstream of the modeling MegaDetector-Acoustic does.
- **[PyTorch-Wildlife](https://github.com/microsoft/PytorchWildlife)** is the shared deep learning framework that hosts the models behind these projects.
