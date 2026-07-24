# Model Card

## Model name

GSE81538 405-Tumor Estrogen-Receptor Status Prediction Model

## Model purpose

This model predicts binary estrogen-receptor status from
high-dimensional breast-cancer RNA-sequencing data.

The model was developed for research on:

- statistical reliability;
- development-sample-size sensitivity;
- feature-selection stability;
- coefficient-direction reproducibility;
- independent external validation.

The model is not intended for clinical diagnosis, treatment
selection, or patient-management decisions.

## Development dataset

- GEO accession: GSE81538
- Development tumors: 405
- ER-negative tumors: 82
- ER-positive tumors: 323
- Shared input genes: 18,213

## External-validation dataset

- GEO accession: GSE96058
- External tumors: 3,073
- ER-negative tumors: 241
- ER-positive tumors: 2,832

## Prediction pipeline

1. Remove zero-variance genes using VarianceThreshold.
2. Select the top 1,000 genes using the ANOVA F statistic.
3. Standardize the selected features.
4. Fit class-weighted L2 logistic regression.

Classification threshold: 0.50.

## External-validation performance

| Metric | Value |
|---|---:|
| AUROC | 0.967171 |
| AUPRC | 0.996570 |
| Balanced accuracy | 0.934054 |
| Sensitivity | 0.942797 |
| Specificity | 0.925311 |
| Brier score | 0.050984 |

External confusion matrix:

```text
[[223, 18],
 [162, 2670]]
```

## Sample-size reliability findings

The external stress test used development sample sizes of
50, 100, 200, 300, and 405 tumors.

The reduced sample-size conditions were repeated 50 times.
The complete 405-tumor model was fitted once.

At development sample size 50:

- Mean external AUROC: 0.8562
- Median external AUROC: 0.9493
- Minimum external AUROC: 0.5384
- Mean balanced accuracy: 0.8231
- Minimum sensitivity: 0.2701
- Maximum Brier score: 0.6456

These results show that strong median performance can coexist
with occasional severe external failure.

## Feature and coefficient stability

For the complete 405-tumor development condition:

- Mean pairwise Jaccard similarity: 0.7693
- Genes selected in at least 50% of fits: 965
- Genes selected in at least 80% of fits: 808
- Directionally stable genes: 542
- Percentage directionally stable: 56.17%

## Saved model files

Model bundle:

`models/GSE81538_405_ER_prediction_model_bundle.joblib`

Shared-gene order:

`models/GSE81538_GSE96058_shared_genes_405.csv`

Model metadata:

`models/GSE81538_405_ER_prediction_model_metadata.json`

## Reproducibility

The saved model was loaded in a separately created Python
environment and applied to the unchanged external cohort.

The verification produced:

- zero classification disagreements;
- maximum probability difference of approximately 1.11e-16;
- identical external metrics;
- successful SHA-256 checksum verification.

## Limitations

- Retrospective computational analysis.
- No prospective clinical validation.
- No evaluation of clinical utility.
- Potential dataset shift across populations and laboratories.
- Substantial class imbalance.
- Selected genes are predictive features, not necessarily causal biomarkers.
- Compatibility with differently processed RNA-sequencing data is uncertain.

## Clinical-use warning

This model is supplied only for research, education, and
reproducibility. It must not be used as a medical device or
as a substitute for qualified clinical judgment.
