# Results Summary

## Study overview

This study evaluated the statistical reliability of
high-dimensional RNA-sequencing models for predicting
estrogen-receptor status in breast cancer.

The analyses examined:

- repeated predictive performance;
- development-sample-size sensitivity;
- feature-selection stability;
- gene-selection frequency;
- coefficient-direction reproducibility;
- independent external validation;
- sensitivity to alternative ER-status coding.

## Datasets

### Development dataset

GSE81538 was used for model development.

Primary cohort:

- 397 tumors
- 82 ER-negative
- 315 ER-positive

Sensitivity cohort:

- 405 tumors
- 82 ER-negative
- 323 ER-positive

### External-validation dataset

GSE96058 was used for independent external validation.

- 3,073 tumors
- 241 ER-negative
- 2,832 ER-positive

The development and validation datasets contained
18,213 shared genes.

## Prediction pipeline

The prediction pipeline contained:

1. Removal of zero-variance genes.
2. Selection of the top 1,000 genes using the ANOVA F statistic.
3. Standardization of selected genes.
4. Class-weighted L2 logistic regression.

The classification threshold was 0.50.

## Full 405-tumor external validation

The model trained on all 405 sensitivity-cohort tumors
was evaluated on the unchanged 3,073-tumor GSE96058 cohort.

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

## External development-sample-size stress test

The stress test fitted 201 models.

| Development sample size | Models fitted |
|---:|---:|
| 50 | 50 |
| 100 | 50 |
| 200 | 50 |
| 300 | 50 |
| 405 | 1 |

Every fitted model was evaluated on the same
3,073-tumor external cohort.

### Results at development sample size 50

| Metric | Mean | Standard deviation | Minimum | Median | Maximum |
|---|---:|---:|---:|---:|---:|
| AUROC | 0.8562 | 0.1577 | 0.5384 | 0.9493 | 0.9684 |
| AUPRC | 0.9805 | 0.0233 | 0.9221 | 0.9948 | 0.9969 |
| Balanced accuracy | 0.8231 | 0.1077 | 0.6046 | 0.8695 | 0.9279 |
| Sensitivity | 0.7638 | 0.2123 | 0.2701 | 0.8678 | 0.9559 |
| Specificity | 0.8824 | 0.1497 | 0.3402 | 0.9274 | 0.9917 |
| Brier score | 0.2039 | 0.1914 | 0.0476 | 0.1161 | 0.6456 |

Although median AUROC was high, some models developed
from 50 tumors generalized poorly.

The minimum AUROC of 0.5384 was close to random
discrimination, and the maximum Brier score of 0.6456
indicated severely unreliable predicted probabilities.

### Results at development sample size 100

For AUROC:

- Mean: 0.9433
- Standard deviation: 0.0790
- Minimum: 0.5295
- Median: 0.9621
- Maximum: 0.9692

Average performance improved, but occasional external
failure remained possible.

### Full 405-tumor reference model

| Metric | Value |
|---|---:|
| AUROC | 0.9672 |
| AUPRC | 0.9966 |
| Balanced accuracy | 0.9341 |
| Sensitivity | 0.9428 |
| Specificity | 0.9253 |
| Brier score | 0.0510 |

## Feature-selection stability

Mean pairwise Jaccard similarity increased with
development sample size.

| Development sample size | Mean Jaccard similarity |
|---:|---:|
| 50 | 0.2510 |
| 100 | 0.3967 |
| 200 | 0.5450 |
| 300 | 0.6659 |
| 405 | 0.7693 |

This indicates that larger development cohorts produced
more reproducible selected-gene sets.

## Gene-selection frequency

| Development sample size | Selected in at least 50% | Selected in at least 80% |
|---:|---:|---:|
| 50 | 518 | 145 |
| 100 | 765 | 361 |
| 200 | 881 | 561 |
| 300 | 937 | 696 |
| 405 | 965 | 808 |

The number of repeatedly selected genes increased with
development sample size.

## Coefficient-direction stability

Direction stability was evaluated among genes selected
in at least 50% of fitted models.

A gene was classified as directionally stable when at
least 90% of its fitted coefficients had the same sign.

| Development sample size | Frequently selected genes | Directionally stable genes | Percentage stable |
|---:|---:|---:|---:|
| 50 | 518 | 196 | 37.84% |
| 100 | 765 | 200 | 26.14% |
| 200 | 881 | 257 | 29.17% |
| 300 | 937 | 386 | 41.20% |
| 405 | 965 | 542 | 56.17% |

Even with the full 405-tumor cohort, a substantial
proportion of frequently selected genes did not achieve
90% coefficient-direction consistency.

## Sensitivity-analysis conclusion

Including the eight borderline ER-coded tumors as
ER-positive caused only modest changes in external
performance.

The principal scientific conclusion remained unchanged:

strong discrimination can conceal instability in
feature selection, coefficient direction, minority-class
performance, and probability estimates.

## Reproducibility result

The saved 405-tumor model was reloaded in a separately
created Python environment.

The verification produced:

- prediction disagreements: 0;
- maximum probability difference: approximately 1.11e-16;
- identical external metrics;
- identical confusion matrix;
- successful SHA-256 verification.

Reproducibility status:

```text
PASSED
```

## Overall conclusion

Larger development samples improved:

- external predictive stability;
- feature-selection agreement;
- coefficient-direction reproducibility;
- sensitivity and specificity consistency;
- probability-estimation accuracy.

High-dimensional cancer-prediction models should therefore
be evaluated using multiple reliability measures rather
than judged solely by a single accuracy estimate or AUROC.
