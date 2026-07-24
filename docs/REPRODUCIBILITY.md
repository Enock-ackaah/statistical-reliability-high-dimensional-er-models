# Reproducibility Guide

## Purpose

This document describes the reproducibility evidence retained
for the high-dimensional ER-status prediction study.

The completed verification demonstrates that the saved fitted
model reproduces the external predictions and metrics in a
separately created Python environment.

This is a clean-environment model-artifact verification.
It is not described as a complete raw-data-to-final-results rerun.

## Repository materials

The repository preserves:

- analysis notebooks;
- compact result tables;
- fitted-model artifacts;
- the required shared-gene order;
- model metadata;
- pinned package versions;
- SHA-256 artifact checksums;
- clean-environment verification reports.

## Verified software environment

The clean environment used:

- Python 3.14.5
- NumPy 2.4.5
- pandas 3.0.3
- SciPy 1.17.1
- scikit-learn 1.8.0
- matplotlib 3.10.9
- joblib 1.5.3
- openpyxl 3.1.5
- notebook 7.5.6
- ipykernel 7.2.0

The pinned dependency file is:

`reproducibility/requirements_reproducibility.txt`

A copy is also available as:

`requirements.txt`

## Creating the environment

From the repository root, create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

Upgrade the installation tools:

```bash
python -m pip install --upgrade pip setuptools wheel
```

Install the pinned packages:

```bash
python -m pip install -r requirements.txt
```

## Model verification notebook

Run:

`notebooks/05_Clean_Environment_Verification.ipynb`

The notebook performs the following checks:

1. Confirms that all required files exist.
2. Recalculates SHA-256 checksums.
3. Loads the saved joblib model bundle.
4. Verifies the stored model metadata.
5. Verifies the exact 18,213-gene order.
6. Loads the unchanged GSE96058 external cohort.
7. Regenerates probabilities and classifications.
8. Compares regenerated predictions with saved predictions.
9. Recalculates all external-validation metrics.
10. Saves machine-readable verification evidence.

## Reproducibility outcome

The clean-environment verification produced:

- all required files found;
- all saved artifacts passed SHA-256 verification;
- model bundle loaded successfully;
- exact shared-gene order verified;
- 3,073 external predictions regenerated;
- prediction disagreements: 0;
- maximum probability difference: approximately 1.11e-16;
- mean probability difference: approximately 4.37e-17;
- identical external-validation metrics;
- identical confusion matrix.

The reproduced external metrics were:

| Metric | Reproduced value |
|---|---:|
| AUROC | 0.967171 |
| AUPRC | 0.996570 |
| Balanced accuracy | 0.934054 |
| Sensitivity | 0.942797 |
| Specificity | 0.925311 |
| Brier score | 0.050984 |

The reproduced confusion matrix was:

```text
[[223, 18],
 [162, 2670]]
```

Final verification status:

```text
PASSED
```

## Verification evidence

The following files contain the machine-readable evidence:

- `reproducibility/clean_environment_model_verification.json`
- `reproducibility/clean_environment_external_metrics.csv`
- `reproducibility/clean_environment_checksum_verification.csv`
- `reproducibility/clean_environment_metric_comparison.csv`
- `reproducibility/model_artifact_sha256_checksums.csv`
- `reproducibility/environment_information.json`
- `reproducibility/REPRODUCIBILITY_RECORD.txt`

## Model artifacts

The fitted model bundle is:

`models/GSE81538_405_ER_prediction_model_bundle.joblib`

The required shared-gene order is:

`models/GSE81538_GSE96058_shared_genes_405.csv`

The model metadata file is:

`models/GSE81538_405_ER_prediction_model_metadata.json`

## Full end-to-end reproducibility

A complete end-to-end rerun would additionally require:

1. Downloading the original GEO datasets.
2. Rerunning all data-preparation procedures.
3. Reconstructing the development and validation cohorts.
4. Rerunning all internal reliability experiments.
5. Rerunning feature and coefficient stability analyses.
6. Rerunning the 201-model external stress test.
7. Comparing every regenerated result with the archived outputs.

The notebooks and environment specifications preserved in this
repository provide the foundation for that additional audit.
