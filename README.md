# HealthGenie

HealthGenie is an educational machine learning project focused on predicting potential diseases from user-reported symptoms. The project emphasizes model design, ensemble learning, probability calibration, and evaluation metrics rather than application deployment.

---

## Project Objective

The primary goal of HealthGenie is to explore:
- Multi-label symptom representation
- Classical machine learning models for medical-style tabular data
- Ensemble learning strategies
- Probability calibration for reliable confidence estimation

---

## Dataset Description

### `dataset.csv`
- Each row corresponds to a patient case
- Features:
  - `Symptom_1, Symptom_2, ..., Symptom_n`
- Target:
  - `Disease`

### `Symptom-severity.csv`
- Provides numeric severity weights for symptoms
- Columns:
  - `Symptom`
  - `weight`

### `symptom_Description.csv`
- Maps diseases to short textual descriptions

### `symptom_precaution.csv`
- Maps diseases to precautionary recommendations

---

## Feature Engineering

- Symptoms are encoded using **multi-hot (binary) vectors**
- A fixed symptom vocabulary is constructed from the training data
- The same vocabulary is enforced during inference to ensure consistency
- Severity scores are used as auxiliary information for interpretation (not direct supervision)

---

## Models Implemented

### Bernoulli Naive Bayes
- Well-suited for sparse binary feature spaces
- Provides fast inference and interpretable probabilistic outputs
- Serves as a strong baseline model

### Random Forest Classifier
- Handles non-linear feature interactions
- More robust to noisy symptom inputs
- Provides improved predictive performance over linear baselines

---

## Ensemble Learning

A **weighted soft-voting ensemble** is used to combine model predictions.

**Weighting Strategy:**
- Random Forest: **0.7**
- Bernoulli Naive Bayes: **0.3**

The ensemble aggregates class probabilities rather than hard labels, leading to improved robustness and stability.

---

## Probability Calibration

- Implemented using `CalibratedClassifierCV`
- Applied to the ensemble model
- Improves the reliability of predicted confidence scores
- Does **not** aim to increase classification accuracy

---

## Evaluation Methodology

- Train/Test split: **80 / 20**
- Cross-validation: **4-fold**
- Primary metric:
  - Macro F1-score (robust to class imbalance)
- Secondary metrics:
  - Accuracy
  - Balanced Accuracy
  - Weighted F1-score

---

## Performance Results 


| Model                     | Test Accuracy | Balanced Accuracy | Macro F1 | Weighted F1 | 4-Fold CV Accuracy |
|--------------------------|---------------|-------------------|----------|-------------|--------------------|
| Bernoulli Naive Bayes    | 0.82          | 0.78              | 0.72     | 0.82        | 0.84 ± 0.04        |
| Random Forest            | 0.90          | 0.86              | 0.84     | 0.90        | 0.91 ± 0.02        |
| Weighted Ensemble        | 0.91          | 0.87              | 0.85     | 0.91        | 0.92 ± 0.02        |
| Calibrated Ensemble      | 0.91          | 0.87              | 0.85     | 0.91        | 0.92 ± 0.02        |

---

## Key Observations

- Ensemble learning provides modest but consistent gains over individual models
- Probability calibration improves confidence estimation without affecting accuracy
- Macro F1-score is a more informative metric than accuracy for imbalanced disease classes

---


