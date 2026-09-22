# Kaggle Data Pipeline

This example uses the INSAFEDARE Web Application to ingest a lung-cancer dataset, generate synthetic tabular data, and evaluate the generated dataset. The example contains:

- `dataset.json` — the source data;
- `Kaggle Data Pipeline.zip` — the pipeline project to import into the application.

## Dataset and Source

The supplied `dataset.json` corresponds to the [Lung Cancer Dataset on Kaggle](https://www.kaggle.com/datasets/akashnath29/lung-cancer-dataset). It contains demographic, behavioural, and symptom-related fields, with `LUNG_CANCER` as the classification target. The README describes the **supplied JSON file**; values and row counts may differ from other files or versions on the Kaggle page.

| Property | Value |
|---|---|
| Input file | `dataset.json` |
| Source | [Kaggle: Lung Cancer Dataset](https://www.kaggle.com/datasets/akashnath29/lung-cancer-dataset) |
| Format | JSON array of records |
| Records | 3,000 |
| Columns | 16 (15 predictors and one target) |
| Age range in supplied file | 30–80 years |
| Prediction target | `LUNG_CANCER` (`YES`: 1,518; `NO`: 1,482) |
| Missing values in supplied file | None |

The columns are:

| Group | Columns |
|---|---|
| Demographics | `GENDER`, `AGE` |
| Behavioural and health indicators | `SMOKING`, `YELLOW_FINGERS`, `ANXIETY`, `PEER_PRESSURE`, `CHRONIC_DISEASE`, `FATIGUE`, `ALLERGY`, `WHEEZING`, `ALCOHOL_CONSUMING`, `COUGHING`, `SHORTNESS_OF_BREATH`, `SWALLOWING_DIFFICULTY`, `CHEST_PAIN` |
| Classification target | `LUNG_CANCER` |

The supplied file has two duplicate complete records. The pipeline export does not include a duplicate-removal component.

## Pipeline Overview

**File Extraction → Label Encoder → Copula Synthesizer → Statistical, Utility, and Privacy Evaluations**

After encoding, the pipeline branches: the encoded original dataset feeds the synthesizer and serves as the real-data input for each evaluation. The synthesizer's output serves as the second input for the evaluations.

| Step | Component | Configuration and role |
|---|---|---|
| 1 | **File Extraction** (`realData`) | Reads `dataset.json` and converts it to the pipeline's Parquet format. No column filter is specified, so all columns are ingested. |
| 2 | **Label Encoder** (`encodedData`) | Encodes `GENDER` and `LUNG_CANCER` as numeric labels. |
| 3 | **Copula Synthesizer** (`syntheticData`) | Generates a synthetic tabular dataset from the encoded data. The exported model does not specify component-specific training parameters. |
| 4 | **Statistical Evaluation** (`statisticalEvaluation`) | Compares statistical properties of the encoded original and synthetic datasets. |
| 5 | **Utility Evaluation** (`utilityEvaluation`) | Uses both datasets to assess predictive utility for `LUNG_CANCER`. The export specifies the target only; check the model, feature selection, training split, and metrics in the application before execution. |
| 6 | **Privacy Evaluation** (`privacyEvaluation`) | Uses the original and synthetic datasets to evaluate privacy. The export does not identify a particular privacy test or its parameters; inspect this component in the imported pipeline before execution. |

This dataset and example pipeline are intended for demonstration and pipeline testing. Their outputs should not be treated as a validated clinical prediction tool.
