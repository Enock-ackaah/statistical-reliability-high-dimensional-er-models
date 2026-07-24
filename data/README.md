# Data

The raw and processed expression matrices are not stored in
this repository.

The analyses use publicly available Gene Expression Omnibus
datasets:

- GSE81538: development cohort
- GSE96058: independent external-validation cohort

Use the data-preparation notebook and the accession identifiers
above to obtain and prepare the source data.

Do not commit the following local project folders:

- GSE81538/
- GSE96058/
- Processed/
- .venv_repro/

The saved model requires the exact shared-gene order contained in:

`models/GSE81538_GSE96058_shared_genes_405.csv`
