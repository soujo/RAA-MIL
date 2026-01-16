### RAA-MIL: A NOVEL FRAMEWORK FOR CLASSIFICATION OF ORAL CYTOLOGY
<hr>

<!-- 📌 Accepted at [IEEE ISBI 2026](https://biomedicalimaging.org/2026/). -->

<a href="https://arxiv.org/abs/2511.12269">
<img src="https://img.shields.io/badge/arXiv-2511.12269-B31B1B?style=for-the-badge&logoColor=white">
</a>
<a href="https://biomedicalimaging.org/2026/">
<img src="https://img.shields.io/badge/IEEE-ISBI'26-4285F4?style=for-the-badge&logoColor=white">
</a>
<a href="https://www.iitkgp.ac.in/">
<img src="https://img.shields.io/badge/IIT%20KGP-db5903?style=for-the-badge&logoColor=white">
</a>

The paper can be accessed over at: https://arxiv.org/abs/2511.12269

### 📝 Table of Contents
- [Authors](#authors)
- [Paper Abstract](#abstract)
- [Overview](#overview)
- [Clinical Motivation](#motivation)
- [Method](#method)
- [Components](#components)
- [Results](#results)
- [Qualitative Visualization](#qualitative)
- [Dataset](#dataset)

### 👥 Authors <a name = "authors"></a>

- [Rupam Mukherjee †](https://rmkjee1510.github.io/index.html), School of Medical Science & Technology, IIT Kharagpur.
- [Rajkumar Daniel †](https://scholar.google.com/citations?user=DF3jmgkAAAAJ&hl=en), School of Medical Science & Technology, IIT Kharagpur.
- [Soujanya Hazra †](https://scholar.google.com/citations?user=6XN-V18AAAAJ&hl=en), Department of Electrical Engineering, IIT Kharagpur.
- [Dr. Shirin Dasgupta](https://scholar.google.com/citations?user=hWYKU7YAAAAJ&hl=en), Dr. B.C. Roy Multispeciality Medical Research Centre, IIT Kharagpur.
- [Dr. Subhamoy Mandal](https://sites.google.com/site/smandalbiomed/home), School of Medical Science & Technology, IIT Kharagpur.

† Equal contribution

### 📚 Paper Abstract <a name = "abstract"></a>
Cytology is a valuable tool for early detection of oral squamous cell carcinoma (OSCC). However, manual examination
of cytology whole slide images (WSIs) is slow, subjective, and depends heavily on expert pathologists. To address this, we introduce the first weakly supervised deep learning framework for patient-level diagnosis of oral cytology whole slide images, leveraging the newly released Oral Cytology Dataset, which provides annotated cytology WSIs from ten medical centres across India. Each patient case is represented as a bag of cytology patches and assigned a diagnosis label (Healthy, Benign, Oral Potentially Malignant Disorders (OPMD), OSCC) by an in-house expert pathologist. These patient-level weak
labels form a new extension to the dataset. We evaluate a
baseline multiple-instance learning (MIL) model and a proposed Region-Affinity Attention MIL (RAA-MIL) that models spatial relationships between regions within each slide. The RAA-MIL achieves an average accuracy of 72.7%, weighted F1-score of 0.69 on an unseen test set, outperforming the baseline. This study establishes the first patient-level weakly supervised benchmark for oral cytology and moves toward reliable AI-assisted digital pathology.

### 📄 Overview <a name = "overview"></a>
This repository contains the official implementation of RAA-MIL, a weakly supervised, patient-level deep learning framework for oral cytology whole slide image (WSI) classification.
RAA-MIL is the first patient-level MIL benchmark for oral cytology, designed to reflect real clinical workflows where only a single diagnosis per patient is available, while fine-grained patch-level labels are absent.
The framework introduces Region-Affinity Attention (RAA) to explicitly model local spatial relationships between cytological regions, improving diagnostic reliability under weak supervision.

### 🏥 Clinical Motivation <a name = "motivation"></a>
* Oral Squamous Cell Carcinoma (OSCC) is often diagnosed late despite being highly preventable.
* Manual cytology screening is:
  - Time-consuming
  - Subjective
  - Dependent on expert availability
* Real-world cytology diagnosis is patient-centric, not patch-centric.

RAA-MIL bridges this gap by enabling robust patient-level predictions using only weak labels.

### 🧠 Method <a name = "method"></a>
Each patient is treated as a bag of cytology patches, following a Multiple Instance Learning (MIL) formulation.

![architecture](/figures/architecture.png)

### 🧩 Components <a name = "components"></a>

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

### 📊 Results <a name = "results"></a>
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

### 🖼️ Qualitative Visualization <a name = "qualitative"></a>

RAA-MIL attention maps demonstrate:
- Suppression of background artifacts
- Focused activation on diagnostically relevant cell clusters
- Distinct patterns across Healthy, OPMD, and OSCC cases

![attention](/figures/attn_maps.png)


### 📁 Dataset <a name = "dataset"></a>
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

### 📚 Citation
```bibtex
@article{mukherjee2025raa,
  title={RAA-MIL: A Novel Framework for Classification of Oral Cytology},
  author={Mukherjee, Rupam and Daniel, Rajkumar and Hazra, Soujanya and Dasgupta, Shirin and Mandal, Subhamoy},
  journal={arXiv preprint arXiv:2511.12269},
  year={2025},
  url= {https://arxiv.org/abs/2511.12269}
}

```
