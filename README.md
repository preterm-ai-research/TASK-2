# Preterm Birth Prediction: SMOTE and ADASYN

This project studies binary preterm-birth prediction from maternal and pregnancy
data. It compares Logistic Regression and Decision Tree models trained with:

- The original imbalanced training data
- SMOTE (Synthetic Minority Over-sampling Technique)
- ADASYN (Adaptive Synthetic Sampling)

The positive class is `preterm_birth = 1` (preterm birth), and the negative
class is `preterm_birth = 0` (term birth).

## Project objective

The supplementary experiment evaluates whether resampling improves detection
of preterm births. SMOTE and ADASYN are applied **only to the training set**.
Every model is evaluated on the same original, untouched test set so that the
results remain comparable.

## Dataset and experiment design

| Item | Value |
| --- | ---: |
| Total observations | 10,000 |
| Training observations | 8,000 |
| Test observations | 2,000 |
| Input features | 18 |
| Test Term / Preterm | 1,788 / 212 |
| Test Preterm prevalence | 10.6% |
| Classification threshold | 0.50 |
| Training strategies | Original, ADASYN, SMOTE |

The resampled training sets contain approximately balanced classes:

- SMOTE: 7,150 Term and 7,150 Preterm observations
- ADASYN: 7,150 Term and 7,356 Preterm observations

## Models

The experiments evaluate three model configurations:

1. Logistic Regression
2. Baseline Decision Tree
3. Regularized Decision Tree

The reported metrics are accuracy, precision, recall, F1-score, ROC-AUC, and
confusion-matrix counts. Precision, recall, and F1-score refer to the Preterm
class.

## Main results

| Model | Training data | Accuracy | Precision | Recall | F1-score | ROC-AUC |
| --- | --- | ---: | ---: | ---: | ---: | ---: |
| Logistic Regression | Original | 0.9075 | 0.7143 | 0.2123 | 0.3273 | 0.7427 |
| Logistic Regression | ADASYN | 0.7665 | 0.2465 | **0.5849** | 0.3469 | 0.7356 |
| Logistic Regression | SMOTE | 0.8295 | 0.3162 | 0.5236 | **0.3943** | 0.7369 |
| Baseline Decision Tree | Original | 0.8480 | 0.2982 | 0.3208 | 0.3091 | 0.6153 |
| Baseline Decision Tree | ADASYN | 0.8035 | 0.2181 | 0.3302 | 0.2627 | 0.5943 |
| Baseline Decision Tree | SMOTE | 0.7985 | 0.2080 | 0.3208 | 0.2523 | 0.5861 |
| Regularized Decision Tree | Original | 0.7930 | 0.2694 | 0.5566 | 0.3631 | 0.7130 |
| Regularized Decision Tree | ADASYN | 0.8345 | 0.3111 | 0.4623 | 0.3719 | 0.6936 |
| Regularized Decision Tree | SMOTE | 0.8275 | 0.2979 | 0.4623 | 0.3623 | 0.6970 |

### Interpretation

- Logistic Regression with ADASYN gives the highest Preterm recall (0.5849),
  detecting 124 of 212 Preterm cases, but produces 379 false positives.
- Logistic Regression with SMOTE gives the highest F1-score (0.3943), which is
  the best balance between precision and recall in these experiments.
- Logistic Regression trained on the original data gives the highest accuracy
  (0.9075) and precision (0.7143), but detects only 45 Preterm cases.
- No single model is best for every metric. The appropriate operating point
  depends on the relative cost of missed cases and false alarms.

## Repository structure

```text
ADASYN/
├── class_distribution/
│   └── adasyn_class_distribution.png
├── notebook/
│   ├── adasyn_implementation.ipynb
│   └── train_preprocessed.csv
└── resampled_dataset/
    └── adasyn_training_data.csv

SMOTE/
├── smote_class_distribution.png
├── smote_implementation.ipynb
├── smote_training_data.csv
├── test_preprocessed.csv
└── train_preprocessed.csv

Retraining_and_Evaluation_ADASYN/
├── adasyn_model_evaluation.ipynb
├── adasyn_evaluation_results.csv
├── adasyn_methodology_checklist.csv
├── adasyn_confusion_matrices.png
├── train_preprocessed.csv
├── test_preprocessed.csv
└── trained model files

Retraining_and_Evaluation_SMOTE/
├── smote_model_evaluation.ipynb
├── smote_evaluation_results.csv
└── confusion_matrices/
```

The two PDF files contain the assignment brief and the comparative evaluation
report.

## Running the notebooks

Install the main Python dependencies:

```bash
pip install jupyter pandas numpy scikit-learn imbalanced-learn matplotlib joblib
```

Run each notebook from its corresponding directory so that its relative data
paths resolve correctly:

```bash
jupyter notebook
```

The implementation notebooks generate resampled training data and class
distribution plots. The evaluation notebooks retrain the models, evaluate
them on the untouched test set, save metric tables, and generate confusion
matrices.

## Important limitations

This is an experimental academic comparison, not a clinically validated
prediction tool. The test set is imbalanced, and resampling changes the
precision-recall trade-off rather than guaranteeing better performance.
External validation, calibration, threshold tuning, fairness analysis, and
clinical assessment are required before any real-world use.
