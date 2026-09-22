# Benchmark Data Pipelines

This example contains the artificial benchmark dataset, `raw.csv`, and two INSAFEDARE pipelines:

1. **Benchmark Data Pipeline - Anonymization**
2. **Benchmark Data Pipeline - Synthetic Data Generation**

Both pipelines begin with the same source dataset. The first transforms the original records through de-identification and anonymization. The second generates new synthetic records from the benchmark data.

## Dataset and Source

`raw.csv` is a **purpose-built artificial healthcare benchmark**, created to test and demonstrate the INSAFEDARE data-processing and privacy-evaluation components. Its names, contact details, diagnoses, treatments, and outcomes are synthetic examples; the file does not originate from a hospital or patient-record system. The records resemble a simple admissions dataset so that transformations and evaluations can be inspected easily.

The dataset contains **2,000 records and 13 columns**. The same source file is used by both example pipelines. File Extraction ingests the CSV and produces a Parquet file for downstream processing.

### Dataset Metadata

| Property | Value |
|---|---|
| Dataset | Artificial healthcare benchmark (`raw.csv`) |
| Source | Generated specifically for the INSAFEDARE/SYDGEP benchmark; no external clinical data source |
| Format | CSV input; Parquet is used between pipeline components |
| Records | 2,000 artificial patient records |
| Columns | 13 |
| Unit of observation | One artificial patient record |
| Prediction target | `Outcome` |
| Direct identifiers | `Patient_ID`, `Full_Name`, `Email`, `Phone` |
| Quasi-identifiers in the source dataset | `Age`, `Gender`, `ZIP_Code`, `Marital_Status`, `Occupation`, `Admission_Date` |
| Sensitive attributes | `Diagnosis`, `Treatment`, `Outcome` |

The direct identifiers are **invented values** included to exercise the de-identification component. In the anonymization pipeline, the privacy tests use the selected quasi-identifiers `Age`, `Gender`, `Marital_Status`, and `Admission_Date`, with `Diagnosis` as the sensitive attribute for diversity and closeness tests.

## Benchmark Data Pipeline - Anonymization

This pipeline transforms the original records and then evaluates the result against the ingested source data. Its stages are:

**File Extraction → De-identification → Age anonymization → Admission date anonymization → Statistical fidelity and ML utility evaluation → Privacy evaluation**

| Step | Component | Configuration and purpose |
|---|---|---|
| 1 | **File Extraction** | Ingest `raw.csv` and write a Parquet dataset for subsequent components. |
| 2 | **De-identification** | Remove the direct-identifier columns `Patient_ID`, `Full_Name`, `Email`, and `Phone`. |
| 3 | **Anonymization: Age** | Generalize the quasi-identifier `Age` into **10-year intervals** (`interval = 10`). |
| 4 | **Anonymization: Admission_Date** | Generalize the quasi-identifier `Admission_Date` to **year precision** (`strategy = date`, `precision = year`). |
| 5 | **Statistical Fidelity Evaluation** | Compare the original and anonymized datasets to assess how well the transformation preserves their statistical properties. |
| 6 | **ML Utility Evaluation** | Assess whether the anonymized data retains predictive value for `Outcome`, using the settings below. |
| 7 | **K-Anonymity** | Evaluate the anonymized data using the four selected quasi-identifiers with `expected K = 5`. |
| 8 | **L-Diversity** | Evaluate the diversity of `Diagnosis` within groups defined by the same four quasi-identifiers, with `expected L = 2`. |
| 9 | **T-Closeness** | Compare each group's `Diagnosis` distribution with the overall distribution, using the same four quasi-identifiers and `expected T = 0.2`. |

### ML Utility Settings

| Parameter | Value |
|---|---|
| Features | `Age`, `Gender`, `Marital_Status`, `Diagnosis`, `Treatment` |
| Target | `Outcome` |
| Model | `SVC` (support vector classifier) |
| Training proportion | `0.7` |
| Metrics | `accuracy`, `f1`, `roc_auc` |

The utility component trains a classifier using original data as a baseline and compares it with a classifier trained using the anonymized data; both are evaluated against held-out original data. The selected features and target must be present in both input datasets.

### Privacy Evaluation Settings

All three privacy evaluations use these quasi-identifiers:

`Age`, `Gender`, `Marital_Status`, `Admission_Date`

| Evaluation | Additional setting | Expected threshold |
|---|---|---|
| K-Anonymity | — | `K = 5` |
| L-Diversity | Sensitive attribute: `Diagnosis` | `L = 2` |
| T-Closeness | Sensitive attribute: `Diagnosis` | `T = 0.2` |

The thresholds are **configured evaluation criteria**, not claims that the pipeline automatically satisfies them. Inspect the generated evaluation results after running the pipeline.

## Benchmark Data Pipeline - Synthetic Data Generation

This section will describe the second example pipeline and its generation and evaluation settings.
