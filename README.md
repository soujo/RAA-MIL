### RAA-MIL: A NOVEL FRAMEWORK FOR CLASSIFICATION OF ORAL CYTOLOGY
========================================================

📌 Accepted at [IEEE ISBI 2026](https://biomedicalimaging.org/2026/).

### 👥 Authors

- [Rupam Mukherjee†](https://rmkjee1510.github.io/index.html)
- [Rajkumar Daniel†](https://scholar.google.com/citations?user=DF3jmgkAAAAJ&hl=en)
- [Soujanya Hazra†](https://scholar.google.com/citations?user=6XN-V18AAAAJ&hl=en)
- [Dr. Shirin Dasgupta](https://scholar.google.com/citations?user=hWYKU7YAAAAJ&hl=en)
- [Dr. Subhamoy Mandal](https://sites.google.com/site/smandalbiomed/home)

† Equal contribution

### Affiliations:

- IIT Kharagpur (SMST, EE, BCRIMSR)

### 📄 Overview
This repository contains the official implementation of RAA-MIL, a weakly supervised, patient-level deep learning framework for oral cytology whole slide image (WSI) classification.
RAA-MIL is the first patient-level MIL benchmark for oral cytology, designed to reflect real clinical workflows where only a single diagnosis per patient is available, while fine-grained patch-level labels are absent.
The framework introduces Region-Affinity Attention (RAA) to explicitly model local spatial relationships between cytological regions, improving diagnostic reliability under weak supervision.

### 🏥 Clinical Motivation
* Oral Squamous Cell Carcinoma (OSCC) is often diagnosed late despite being highly preventable.
* Manual cytology screening is:
  - Time-consuming
  - Subjective
  - Dependent on expert availability
* Real-world cytology diagnosis is patient-centric, not patch-centric.

RAA-MIL bridges this gap by enabling robust patient-level predictions using only weak labels.

### 🧠 Method Summary
Each patient is treated as a bag of cytology patches, following a Multiple Instance Learning (MIL) formulation.

![architecture](/figures/architecture.png)

### 🧩 Key Components

1. Frozen Self-Supervised ViT Tokenization
   - DINO-pretrained ViT-S/16 (pathology SSL benchmark)
   - Encoder remains fully frozen
   - Tokens cached for deterministic training

2. Region-Affinity Attention (RAA) ⭐
   - Operates on the ViT token grid (14×14)
   - Learns affinity between spatially neighboring regions
   - Uses:
        - Distance-based affinity
        - Lightweight MLP
        - Residual gated refinement

    This enables context-aware cytological representation learning.

3. Gated Attention MIL Pooling
    - Based on [Ilse et al. (ICML 2018)](https://proceedings.mlr.press/v80/ilse18a.html)
    - Learns instance importance under weak supervision
    - Aggregates all refined tokens into a single patient embedding

4. Prediction-Averaging Ensemble
    - 5-fold stratified cross-validation
    - Final predictions obtained via fold-wise probability averaging
    - Reduces MIL-induced variance

### 📊 Results
1. Hold-out test (ensemble prediction averaging):

    | Model        | Accuracy | F1 (Weighted) | PR-AUC (Weighted) |
    |--------------|----------|---------------|-------------------|
    | Vanilla MIL  | 0.6970   | 0.6749        | 0.7389            |
    | **RAA-MIL**  | **0.7273** | **0.6970**   | **0.7969**        |

2. Per-class PR-AUC (test set):

    | Class   | Vanilla MIL | RAA-MIL |
    |---------|-------------|---------|
    | Benign  | 0.1879      | 0.1772  |
    | Healthy | 0.9193      | 0.9441  |
    | OPMD    | 0.6937      | 0.7594  |
    | OSCC    | 0.5437      | 0.7667  |

### 🖼️ Qualitative Visualization

RAA-MIL attention maps demonstrate:
- Suppression of background artifacts
- Focused activation on diagnostically relevant cell clusters
- Distinct patterns across Healthy, OPMD, and OSCC cases

![attention](/figures/attn_maps.png)


### 📁 Dataset
This work builds upon the Oral Cytology Dataset by [Jain et al. (2025)](https://arxiv.org/abs/2506.09661), consisting of:
- 368 PAP & MGG stained WSIs
- 10 medical centers across India
- 40× magnification
- Expert-curated representative regions

### 🛠️ Code (Coming Soon)
The following will be released:
- Full training & evaluation pipeline
- RAA module implementation
- MIL pooling code
- Preprocessing and token caching scripts

### 📚 Citation (pre-print)
```bibtex
@article{mukherjee2025raa,
  title={RAA-MIL: A Novel Framework for Classification of Oral Cytology},
  author={Mukherjee, Rupam and Daniel, Rajkumar and Hazra, Soujanya and Dasgupta, Shirin and Mandal, Subhamoy},
  journal={arXiv preprint arXiv:2511.12269},
  year={2025},
  url= {https://arxiv.org/abs/2511.12269}
}

```
