# Menstrual Blood Color Classification

Deep learning classification of menstrual blood color with class imbalance handling. Accepted at **IICAIET 2026**.

> 📌 **Status:** Paper accepted, pending presentation.

---

## Overview

This repository contains the implementation for our paper, **"Classification of Menstrual Blood Color Using Deep Learning with Class Imbalance Handling,"** accepted at the International Conference on Artificial Intelligence for Engineering and Technology (IICAIET) 2026.

The project addresses the classification of menstrual blood color categories using deep learning, with a focus on handling class imbalance in the dataset — a common challenge in medical/health-related image classification tasks.

### Motivation

- Muslim women face unique challenges: menstrual status determines eligibility for religious practices like prayer and fasting
- Determining the end of menstruation relies on subjective indicators: blood color, consistency, and visibility
- Current tracking methods (e.g., Bom Calendar, Flo, Maya) rely on calendar-based prediction and self-reporting, not actual blood visualization
- Calendar-based methods are unreliable for irregular cycles, creating tension between religious obligation and biological uncertainty
- No reliable, accessible tool currently exists for visualizing and judging menstrual blood color to support religious decision-making

## Paper

- **Title:** Classification of Menstrual Blood Color Using Deep Learning with Class Imbalance Handling
- **Conference:** IICAIET 2026
- **Authors:** Nur Insyirah Iman Mohd Azman, Nor Hidayati Abdul Aziz
- **Status:** Accepted (pending presentation)
- **DOI / Link:** <!-- Add once available -->

## Citation

If you use this work, please cite:

```bibtex
@inproceedings{yourname2026menstrual,
  title     = {Classification of Menstrual Blood Color Using Deep Learning with Class Imbalance Handling},
  author    = {Nur Insyirah Iman Mohd Azman and Nor Hidayati Abdul Aziz},
  booktitle = {Proceedings of the International Conference on Artificial Intelligence for Engineering and Technology (IICAIET)},
  year      = {2026}
}
```

<!-- Update with the official citation once conference proceedings are published -->

## Dataset

> ⚠️ Due to the sensitive nature of the data, the dataset is not publicly released in this repository. See [Data Access](#data-access) below for details.

- **Classes:** 6 categories — Red, Brown, Black, Yellow, Murky, No Visible Blood
- **Number of samples:** 997 total images
  - Red: 500
  - Brown: 223
  - Black: 109
  - No Visible Blood: 73
  - Yellow: 58
  - Murky: 34
- **Collection method:** Female volunteers captured images 3–5 times/day using personal mobile devices under natural, everyday lighting conditions (no controlled lab environment). Images were manually screened to remove blurry/unsuitable samples and classified based on participants' visual perception.
- **Data split (stratified):**
  - Training: 597 images
  - Validation: 200 images
  - Test: 200 images
- **Class imbalance handling:** SMOTE (Synthetic Minority Oversampling Technique) applied only to the training set, increasing minority classes to 300 samples each (597 → 1,800 training images). Validation and test sets retained their original imbalanced distribution to avoid data leakage.
- **Ethics:** No personally identifiable information was collected or linked to the dataset; all images were kept anonymous.

## Method

- **Model architectures compared:**
  - Custom CNN — 4 convolutional blocks (32/64/128/256 filters), 2 fully connected layers (256, 128 neurons), ReLU activation, dropout (0.5, 0.3), softmax output
  - InceptionV3 — transfer learning with ImageNet pre-trained weights, custom head (global average pooling, batch normalization, dense layers 512/256, dropout, softmax)
  - ResNet50 — transfer learning with the same custom head as InceptionV3, using focal loss to better handle class imbalance
- **Class imbalance strategy:** SMOTE-based oversampling on flattened training image vectors, compared against original imbalanced baseline
- **Training details:**
  - Input size: 256 × 256 × 3
  - Optimizer: Adam (learning rate = 0.0001)
  - Batch size: 16
  - Max epochs: 30, with early stopping (patience = 5)
  - Best model saved via checkpointing based on validation accuracy
  - Loss function: categorical cross-entropy (CNN, InceptionV3); focal loss (ResNet50)

## Results

### Imbalanced Baseline — Overall Test Performance

| Model | Accuracy | Precision | Recall | F1-score (macro) |
|---|---|---|---|---|
| CNN | 63.50% | 0.20 | 0.29 | 0.24 |
| InceptionV3 | 74.50% | 0.62 | 0.57 | 0.58 |
| ResNet50 | 50.00% | 0.08 | 0.17 | 0.11 |

### SMOTE-Balanced — Overall Test Performance

| Model | Accuracy | Precision | Recall | F1-score (macro) |
|---|---|---|---|---|
| CNN | 62.00% | 0.43 | 0.48 | 0.45 |
| InceptionV3 | 74.50% | 0.55 | 0.56 | 0.55 |
| **ResNet50** | **75.50%** | **0.62** | **0.60** | **0.61** |

**Key findings:**
- ResNet50 achieved the best overall test accuracy (75.50%) after SMOTE balancing and was the only model to successfully detect the minority **Murky** class (recall = 0.14)
- Baseline (imbalanced) models were heavily biased toward the majority **Red** class, with ResNet50 collapsing to predict only Red
- SMOTE improved minority-class recognition across models, though gains varied by architecture — CNN slightly declined, InceptionV3 stayed flat, ResNet50 improved most
- A large train-test gap for ResNet50 (99.89% → 75.50%) indicates a generalization gap requiring further investigation

## Data Access

Data is not shareable.

## License

This project's code is licensed under the [MIT License](LICENSE).

## Acknowledgements

This work was supported by the Fisabilillah Research Development Grant Scheme (Grant No. MMUE/240128) awarded to Multimedia University.

## Contact

For questions about this work, please contact <!-- insyirazman@gmail.com --> or open an issue in this repository.
