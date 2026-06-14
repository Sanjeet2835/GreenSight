# GreenSight 🌿
### AI-Powered Real-Time Plant Health Monitoring and Disease Assistance System

GreenSight is an integrated plant health monitoring system that combines a lightweight CNN classifier, live camera surveillance, weather-aware risk context, and an LM-based guidance assistant — designed as a low-cost, practical tool for precision agriculture.

---

## Overview

Early detection of plant diseases is critical to minimizing crop loss. Existing tools often provide only static classification without real-world context, guidance, or continuous monitoring. GreenSight addresses this by unifying four components into a single Streamlit dashboard:

- **LeafNet** — ResNet-9-based CNN for leaf disease classification
- **Live Monitoring** — Webcam or ESP32-CAM stream with snapshot inference
- **Weather Intelligence** — Location-based forecast for disease risk context
- **Guidance Assistant** — Language model for concise, safety-aware recommendations

---

## Results

| Metric | Value |
|--------|-------|
| Validation Accuracy | **99.2%** |
| Classes | 38 (26 diseases across 11 crops) |
| Dataset Size | ~87K RGB images |
| Training Epochs | 2 (one-cycle LR schedule) |

---

## Model Architecture — LeafNet

LeafNet is a compact residual CNN built from scratch:

- 4 convolutional stages with BatchNorm and ReLU
- 2 residual blocks with identity skip connections: **y = F(x; W) + x**
- Spatial pooling → fully connected head → 38-class softmax output
- Input: 256×256 RGB images

Trained using **Adam optimizer** with a **one-cycle learning rate schedule**, achieving 99.2% validation accuracy in 2 epochs — consistent with super-convergence behavior.

---

## Dataset

Built from the [PlantVillage dataset](https://arxiv.org/abs/1511.08060) via offline augmentation:

- ~87K RGB images, 256×256 resolution
- 38 classes: 26 diseases + 12 healthy conditions across 11 major crops
- 80/20 train/validation split preserving directory structure
- Augmentations: random rotation, horizontal flip, mild zoom/crop
- Extended with real-world leaf and non-leaf samples for robustness and unknown-class rejection

---

## System Pipeline

```
Image Capture (upload / webcam / ESP32-CAM)
        ↓
Preprocessing (resize to 256×256, normalize)
        ↓
LeafNet Inference (ResNet-9)
        ↓
Top-k Predictions + Confidence Thresholding
        ↓
Weather Context Fusion
        ↓
Guidance Generation (Language Model)
        ↓
Streamlit Dashboard (Visualization + Alerts)
```

---

## Features

- **Upload-based inference** — single image prediction with top-k ranked results
- **Live snapshot inference** — webcam or ESP32-CAM with periodic capture
- **Unknown-class rejection** — confidence thresholding filters non-leaf inputs
- **Weather intelligence** — location-based forecast augments risk interpretation
- **Guidance assistant** — text-query interface for disease progression, watering, and stress symptoms
- **Low digital literacy UI** — designed for field users; future versions will support Hindi and multilingual prompts

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Model | PyTorch (custom ResNet-9) |
| UI & Pipeline | Streamlit |
| Edge Camera | ESP32-CAM |
| Experiment Tracking | — |
| Dataset | PlantVillage (augmented) |

---

## Limitations

- Leaf-level assessment only; struggles with heavy occlusion or early-stage symptoms
- Performance degrades when leaves occupy a small fraction of the frame
- Guidance assistant avoids chemical recommendations for safety
- Current deployment requires a camera with reasonable lighting

---

## Future Work

- Knowledge distillation for fully offline, on-device inference
- Multi-camera dashboards for farm-scale deployment
- Larger in-field datasets with partial occlusions and early infections
- Region-specific pest management knowledge base under expert supervision
- Multilingual support (Hindi priority)

---
