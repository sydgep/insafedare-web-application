# Benchmark Data Pipelines

This example contains the artificial benchmark dataset, `raw_data.csv`, and two pipelines (`Benchmark Data Pipeline - Anonymization` and  `Benchmark Data Pipeline - Synthetic Data Generation`).

## Dataset and Source

`raw_data.csv` is a **purpose-built artificial healthcare benchmark**, created to test and demonstrate the components developed within the INSAFEDARE project. Its names, contact details, diagnoses, treatments, and outcomes are synthetic examples; the file does not originate from a hospital or patient-record system. The records resemble a simple admissions dataset so that transformations and evaluations can be inspected easily.

### Dataset Metadata

| Property | Value |
|---|---|
| Dataset | Artificial healthcare benchmark (`raw_data.csv`) |
| Source | The dataset is generated with ChatGPT; not derived from real patient records. |
| Format | CSV input |
| Records | 2,000 artificial patient records |
| Columns | 13 |
| Direct identifiers | `Patient_ID`, `Full_Name`, `Email`, `Phone` |
| Quasi-identifiers in the source dataset | `Age`, `Gender`, `ZIP_Code`, `Marital_Status`, `Occupation`, `Admission_Date` |
| Sensitive attributes | `Diagnosis`, `Treatment`, `Outcome` |

## Benchmark Data Pipeline - Anonymization

This pipeline transforms the original records and then evaluates the result against the ingested source data. Its stages are:

**File Extraction → De-identification → Anonymization (Age & Admission date) → Evaluations (Statistical , Utility & Privacy)**

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

This pipeline uses CTGAN to generate new artificial records from the de-identified benchmark data. It then compares the synthetic dataset with the original data for statistical fidelity, predictive utility, and privacy risk.

**File Extraction → De-identification → CTGAN synthesis → Statistical fidelity, ML utility & Privacy evaluation**

| Step | Component | Configuration and purpose |
|---|---|---|
| 1 | **File Extraction** | Ingest `raw.csv` and write a Parquet dataset for subsequent components. |
| 2 | **De-identification** | Remove `Patient_ID`, `Full_Name`, `Email`, and `Phone` before training the synthesizer. |
| 3 | **CTGAN Synthesizer** | Train on the de-identified dataset and generate a synthetic dataset. The privacy-preserving output is available in CSV and Parquet formats. |
| 4 | **Statistical Fidelity Evaluation** | Compare the original and synthetic datasets to assess how well their statistical properties agree. |
| 5 | **ML Utility Evaluation** | Compare predictive performance using the original-data baseline and the synthetic dataset, with the same features, target, model, split, and metrics as in the anonymization pipeline. |
| 6 | **Attribute Inference Attack** | Assess how well `Diagnosis` can be inferred using the specified quasi-identifiers and a `random_forest` attack model. |
| 7 | **Record Linkage Attack** | Evaluate whether synthetic records can be linked to original records using the same quasi-identifiers. |
| 8 | **Distance to Closest Real Record** | Measure how close synthetic records are to records in the original dataset. |

### ML Utility Settings

| Parameter | Value |
|---|---|
| Features | `Age`, `Gender`, `Marital_Status`, `Diagnosis`, `Treatment` |
| Target | `Outcome` |
| Model | `SVC` (support vector classifier) |
| Training proportion | `0.7` |
| Metrics | `accuracy`, `f1`, `roc_auc` |

The utility evaluation compares **Train on Real, Test on Real (TRTR)** with **Train on Synthetic, Test on Real (TSTR)**. Both modes use held-out original records for testing.

### Privacy Evaluation Settings

The privacy evaluations compare the original and synthetic datasets. Attribute inference and record linkage use the quasi-identifiers `Age`, `Gender`, `Marital_Status`, and `Admission_Date`.

| Evaluation | Configuration |
|---|---|
| **Attribute Inference Attack** | Quasi-identifiers: `Age`, `Gender`, `Marital_Status`, `Admission_Date`; sensitive attribute: `Diagnosis`; attack model: `random_forest`. |
| **Record Linkage Attack** | Quasi-identifiers: `Age`, `Gender`, `Marital_Status`, `Admission_Date`. |
| **Distance to Closest Real Record** | Original dataset and synthetic dataset; no additional component-specific parameters. |

Review all three results together. Synthetic generation alone does not establish that the resulting records are sufficiently private for release.
