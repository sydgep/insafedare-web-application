# MIMIC-IV Pipeline: Integration and Synthetic Data Generation

This example defines a pipeline that integrates selected MIMIC-IV hospital tables, constructs a heart-failure cohort, prepares an analytical dataset, removes record linkage identifiers, generates synthetic data with a Copula synthesizer, and evaluates the resulting dataset.

## Data Source

The source is [MIMIC-IV version 2.0](https://physionet.org/content/mimiciv/2.0/), hosted by PhysioNet and developed from clinical records at Beth Israel Deaconess Medical Center. 

| Source file in `hosp/` | Columns selected by the pipeline | Use in the pipeline |
|---|---|---|
| `diagnoses_icd.csv.gz` | `hadm_id`, `icd_code`, `seq_num` | Select heart-failure admissions and count diagnoses per admission. |
| `prescriptions.csv.gz` | `hadm_id`, `subject_id` | Count prescription records per admission (`n_medications`). |
| `admissions.csv.gz` | `hadm_id`, `subject_id`, `admittime`, `dischtime`, `admission_type`, `admission_location`, `insurance`, `language`, `marital_status`, `race`, `hospital_expire_flag` | Add admission details and the mortality prediction target. |
| `patients.csv.gz` | `subject_id`, `anchor_age`, `gender` | Add patient demographics. |
| `omr.csv.gz` | `subject_id`, `chartdate`, `result_name`, `result_value` | Extract BMI and blood-pressure measurements. |

The source files are read as compressed CSV; the File Extraction components convert the selected data to Parquet for downstream processing. 
The pipeline is designed around the **v2.0 `hosp` layout**. Its exported source directory is a path from the original author's computer.

### Access Conditions

MIMIC-IV files are **restricted-access data**, even though the project description is publicly visible. To obtain the files, a user must:

1. Create a [PhysioNet account](https://physionet.org/).
2. Complete PhysioNet credentialing and the required **CITI Data or Specimens Only Research** training.
3. Sign the **MIMIC-IV Data Use Agreement** on the [MIMIC-IV v2.0 page](https://physionet.org/content/mimiciv/2.0/).

Each partner who accesses the source files must have their **own authorization**. PhysioNet does not permit sharing credentialed MIMIC files within a team or class. Do not add the source CSV files or row-level derived data to a public repository. Access and downstream use remain subject to the signed agreement and PhysioNet's current policies. See the [PhysioNet access FAQ](https://physionet.org/about/faqs/) for the current process.

## Pipeline Overview

The designed workflow is:

**Extract source tables → Select heart-failure admissions and derive counts/measurements → Merge tables → Select features and handle missing values → Remove linkage identifiers → Generate synthetic data → Evaluate fidelity, utility, and privacy**

### 1. Cohort Selection and Feature Engineering

| Component | Configuration | Result |
|---|---|---|
| **Select Cohort** | Match `icd_code` against `I50,428` using the first `3` characters (`matchOnLen = 3`), grouped by `hadm_id`. | Heart-failure admission cohort. |
| **Count Measurement** | Group diagnoses by `hadm_id`; output column `n_diagnoses`. | Diagnosis count per admission. |
| **Count Measurement** | Group prescriptions by `hadm_id`; output column `n_medications`. | Prescription-record count per admission. |
| **Extract Measurement** | Select `BMI` and `Blood Pressure` from `omr.result_name`, using `result_value`. | BMI and blood-pressure records. |
| **Expand Feature / Rename Columns** | Split `Blood Pressure` on `/` and rename the resulting columns to `systolic_bp` and `diastolic_bp`. | Separate systolic and diastolic measurements. |

`n_medications` counts prescription **records** in the selected table; it is not necessarily a count of distinct medicines.

### 2. Integration and Preprocessing

The pipeline merges admission-level datasets on `hadm_id`, adds patient information on `subject_id`, and uses two **Merge on Date** components to align BMI and blood-pressure records by `subject_id`, comparing admission time (`admittime`) with measurement date (`chartdate`). 
The export does not set a merge direction explicitly, so the application default applies. Feature Selection retains:

`hadm_id`, `subject_id`, `admittime`, `dischtime`, `admission_type`, `admission_location`, `insurance`, `language`, `marital_status`, `race`, `gender`, `anchor_age`, `n_diagnoses`, `n_medications`, `systolic_bp`, `diastolic_bp`, `BMI`, `hospital_expire_flag`.

Next, **Missing-Value Imputation** fills eligible missing values while excluding `hadm_id` and `subject_id`. **Missing-Value Removal** removes remaining records with unresolved missing values. 
**De-identification** then removes `hadm_id` and `subject_id` from the prepared dataset. The retained date and demographic fields still warrant privacy assessment; removing these two identifiers does not, by itself, establish anonymity.

### 3. Synthetic Data Generation

The **Copula Synthesizer** consumes the de-identified, cleaned dataset and produces a synthetic tabular dataset. No component-specific generation parameters are set in the exported model. The prepared original dataset and synthetic dataset both feed the evaluation components.

### 4. Evaluations

| Component | Inputs and configuration | Purpose |
|---|---|---|
| **Statistical Evaluation** | Prepared original dataset and synthetic dataset. | Compare their statistical properties. |
| **Utility Evaluation** | Both datasets; target `hospital_expire_flag`; features `insurance`, `language`, `marital_status`, `race`, `gender`, `anchor_age`, `n_diagnoses`, `n_medications`, `systolic_bp`, `diastolic_bp`, `BMI`. | Assess utility for in-hospital mortality prediction. The export does not explicitly set the model, train size, or metrics. |
| **Attribute Inference Attack** | Both datasets; quasi-identifiers `insurance`, `language`, `marital_status`, `race`, `gender`, `anchor_age`; sensitive attribute `hospital_expire_flag`. | Evaluate inference risk for the mortality label. |
| **Record Linkage Attack** | Both datasets; the same six quasi-identifiers. | Assess whether synthetic records can be linked to prepared original records. |
| **Distance to Closest Real Record** | Prepared original dataset and synthetic dataset. | Measure proximity of synthetic records to real records. |

The evaluation settings describe what is configured in the **pipeline model**. They do not assert that an evaluation has run or that any privacy, fidelity, or utility threshold has been met.
