# Statistical Reliability of High-Dimensional ER-Status Prediction Models

This repository contains analysis code, compact results, fitted-model
artifacts, and reproducibility evidence for an RNA-sequencing study
of estrogen-receptor status prediction in breast cancer.

## Study objective

The project examines whether strong predictive accuracy remains reliable
when the development sample size changes.

The analyses evaluate:

- predictive performance;
- probability accuracy;
- feature-selection stability;
- coefficient-direction reproducibility;
- independent external validation;
- sensitivity to alternative ER-status coding.

## Datasets

- GSE81538: development cohort
- GSE96058: independent external-validation cohort

### Primary development cohort

- 397 tumors
- 82 ER-negative
- 315 ER-positive

### Sensitivity development cohort

- 405 tumors
- 82 ER-negative
- 323 ER-positive

### External-validation cohort

- 3,073 tumors
- 241 ER-negative
- 2,832 ER-positive

The datasets contain 18,213 shared genes.

Raw and processed expression matrices are not included in this repository.
See `data/README.md` for data-access information.

## Prediction pipeline

Each model uses the following scikit-learn pipeline:

1. VarianceThreshold(threshold=0.0)
2. SelectKBest(score_func=f_classif, k=1000)
3. StandardScaler()
4. Class-weighted L2 logistic regression

The classification threshold is 0.50.

## External sensitivity-analysis results

| Metric | Estimate | Bootstrap 95% CI |
|---|---:|---:|
| AUROC | 0.9672 | 0.9545-0.9779 |
| AUPRC | 0.9966 | 0.9948-0.9980 |
| Balanced accuracy | 0.9341 | 0.9166-0.9496 |
| Sensitivity | 0.9428 | 0.9340-0.9516 |
| Specificity | 0.9253 | 0.8921-0.9544 |
| Brier score | 0.0510 | 0.0443-0.0577 |

External confusion matrix:

```text
[[223, 18],
 [162, 2670]]
```

## Development-sample-size stress test

The reduced development sample sizes of 50, 100, 200, and 300
were each repeated 50 times. The complete 405-tumor model was fitted
once, producing 201 fitted models.

At development sample size 50:

- Mean external AUROC: 0.8562
- Median external AUROC: 0.9493
- Minimum external AUROC: 0.5384
- Mean balanced accuracy: 0.8231
- Minimum sensitivity: 0.2701
- Maximum Brier score: 0.6456

These results show that high median performance can coexist with
occasional severe external failure.

## Feature and coefficient stability

For the complete 405-tumor condition:

- Mean feature-selection Jaccard similarity: 0.7693
- Genes selected in at least 50% of model fits: 965
- Genes selected in at least 80% of model fits: 808
- Directionally stable genes: 542
- Percentage directionally stable: 56.17%

## Reproducibility

The saved model was loaded in a separately created Python environment.

The verification produced:

- zero prediction disagreements;
- maximum probability difference of approximately 1.11e-16;
- exact reproduction of all external metrics;
- successful SHA-256 verification of model artifacts.

See `docs/REPRODUCIBILITY.md` and the files in `reproducibility/`.

## Saved model

The reusable fitted model is stored at:

`models/GSE81538_405_ER_prediction_model_bundle.joblib`

The required shared-gene order is stored at:

`models/GSE81538_GSE96058_shared_genes_405.csv`

## Limitations

- This is a retrospective computational study.
- The model has not been prospectively validated.
- The model is not approved for clinical deployment.
- Selected genes must not automatically be interpreted as causal biomarkers.
- The model must not be used for diagnosis or treatment decisions.

## Author

**Enock Kumi Ackaah**

- GitHub: https://github.com/Enock-ackaah
- LinkedIn: https://www.linkedin.com/in/ackaah-enock-kumi

## License

The analysis code and repository documentation are released under
the MIT License. Source datasets remain subject to the terms of
their original providers.
