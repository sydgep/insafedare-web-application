# Pipeline Components

[← Back to the main README](../README.md)

INSAFEDARE provides reusable components for constructing data-processing and privacy-preserving pipelines. Each component performs a specific operation and can be connected to other compatible components through the graphical pipeline editor.

This reference summarizes the purpose, expected inputs, principal parameters, outputs, and execution image associated with each component in the current testing version.

> Component availability and image versions may change between releases. Consult the release notes when reproducing an earlier experiment.

## Data Ingestion

| Component | Purpose | Input | Main parameters | Output | Docker image |
|---|---|---|---|---|---|
| **File Extraction** | Loads a tabular file and extracts the required columns. | CSV or compressed CSV file | Input file; selected columns | Extracted tabular dataset | `ghcr.io/sydgep/docker-images/file_extraction:v2` |
| **Database Extraction** | Connects to a PostgreSQL database and extracts selected data from a table or query. | PostgreSQL database | Connection settings; table or query; selected columns | Extracted tabular dataset | Release-specific image |

## Data Integration

| Component | Purpose | Input | Main parameters | Output | Docker image |
|---|---|---|---|---|---|
| **Select Cohort** | Selects records belonging to a defined population or clinical cohort. | Tabular dataset | Identifier column; target column; matching value or code; matching length | Cohort dataset | `ghcr.io/sydgep/docker-images/select_cohort:v1` |
| **Merge Data** | Combines two datasets using one or more common keys. | Two tabular datasets | Join columns; join type | Merged dataset | `ghcr.io/sydgep/docker-images/merge_data:v1` |
| **Merge on Date** | Performs a nearest-date or time-aware merge between datasets. | Two datasets containing date or time columns | Left and right date columns; grouping key; direction or tolerance | Time-aligned dataset | `ghcr.io/sydgep/docker-images/merge_on_date:v1` |
| **Extract Measurement** | Extracts a requested measurement from a long-format clinical table. | Long-format measurement dataset | Measurement-name column; measurement value; identifier columns | Selected measurements | `ghcr.io/sydgep/docker-images/extract_measurement:v1` |
| **Expand Feature** | Converts repeated or named measurements into separate feature columns. | Long-format or grouped dataset | Identifier columns; feature-name column; value column | Expanded feature dataset | `ghcr.io/sydgep/docker-images/expand_feature:v1` |
| **Rename Columns** | Renames selected columns to standardize schemas between pipeline stages. | Tabular dataset | Existing and replacement column names | Dataset with renamed columns | `ghcr.io/sydgep/docker-images/rename_columns:v1` |

## Data Preprocessing

| Component | Purpose | Input | Main parameters | Output | Docker image |
|---|---|---|---|---|---|
| **Missing-Value Imputation** | Automatically imputes missing numerical values using the median and categorical values using the mode. | Tabular dataset | Excluded columns | Imputed dataset | `ghcr.io/sydgep/docker-images/missing_value_imputation:v1` |
| **Missing-Value Removal** | Removes records containing unresolved missing values. | Tabular dataset | Columns or removal scope | Cleaned dataset | `ghcr.io/sydgep/docker-images/missing_value_removal:v1` |
| **Outlier Removal** | Detects and treats numerical outliers. | Tabular dataset | Selected columns; method; threshold or limits | Dataset with outliers removed or adjusted | `ghcr.io/sydgep/docker-images/outliers_removal:v2` |
| **Label Encoder** | Converts categorical values into numerical labels. | Dataset containing categorical columns | Columns to encode | Label-encoded dataset | `ghcr.io/sydgep/docker-images/label_encoder:v1` |
| **One-Hot Encoder** | Converts categorical variables into binary indicator columns. | Dataset containing categorical columns | Columns to encode | One-hot-encoded dataset | `ghcr.io/sydgep/docker-images/one_hot:v1` |
| **Convert to Numeric** | Converts compatible columns to numerical data types. | Tabular dataset | Selected or excluded columns | Numerically converted dataset | `ghcr.io/sydgep/docker-images/convert_to_numeric:v1` |

## Privacy-Preserving Transformations

| Component | Purpose | Input | Main parameters | Output | Docker image |
|---|---|---|---|---|---|
| **Direct-Identifier Removal** | Removes columns that directly identify individuals. | Original or integrated dataset | Direct-identifier columns | De-identified dataset | `ghcr.io/sydgep/docker-images/direct_identifier_removal:v1` |
| **Anonymization** | Applies transformations such as generalization or suppression to reduce disclosure risk. | De-identified tabular dataset | Quasi-identifiers; sensitive attributes; anonymization settings | Anonymized dataset | `ghcr.io/sydgep/docker-images/anonymization:v1` |

Removing direct identifiers alone does not guarantee anonymity. Quasi-identifiers and sensitive attributes must be selected according to the dataset, intended use, and applicable governance requirements.

## Synthetic Data Generation

| Component | Purpose | Input | Main parameters | Output | Docker image |
|---|---|---|---|---|---|
| **CTGAN Synthesizer** | Generates synthetic tabular data using a conditional generative adversarial network. | Preprocessed tabular dataset | Epochs; batch size | Synthetic dataset | `ghcr.io/sydgep/docker-images/ctgan_synthesizer:v2` |
| **CTGAN Synthesizer GPU** | Runs CTGAN synthesis with optional GPU acceleration. | Preprocessed tabular dataset | Use GPU; epochs; batch size | Synthetic dataset | `ghcr.io/sydgep/docker-images/ctgan_synthesizer_gpu:v1` |
| **VAE Synthesizer** | Generates synthetic data using a variational autoencoder. | Preprocessed numerical or encoded dataset | Training and generation settings | Synthetic dataset | `ghcr.io/sydgep/docker-images/vae_synthesizer:v1` |
| **Gaussian Copula Synthesizer** | Generates synthetic data using learned marginal distributions and dependencies. | Preprocessed tabular dataset | Generation settings; number of rows | Synthetic dataset | `ghcr.io/sydgep/docker-images/gaussian_copula_synthesizer:v1` |

## Privacy Evaluation

| Component | Purpose | Input | Main parameters | Output | Docker image |
|---|---|---|---|---|---|
| **Attribute Inference Attack (AIA)** | Evaluates whether sensitive attributes can be inferred from other synthetic attributes. | Synthetic dataset | Known attributes; sensitive attributes | Attack-risk metrics | `ghcr.io/sydgep/docker-images/aia_evaluation:v1` |
| **Record Linkage Risk (RLR)** | Estimates whether synthetic or transformed records can be linked to original records. | Original and released datasets | Matching attributes; thresholds | Linkage-risk metrics | `ghcr.io/sydgep/docker-images/rlr_evaluation:v1` |
| **Distance to Closest Record (DCR)** | Measures the distance between generated records and their closest original records. | Original and synthetic datasets | Comparison columns; distance settings | Distance-based privacy metrics | `ghcr.io/sydgep/docker-images/dcr_evaluation:v1` |
| **K-Anonymity** | Measures whether each quasi-identifier combination is shared by at least `k` records. | Anonymized dataset | Quasi-identifiers | K-anonymity results | `ghcr.io/sydgep/docker-images/k_anonymity:v1` |
| **L-Diversity** | Measures sensitive-value diversity within equivalence classes. | Anonymized dataset | Quasi-identifiers; sensitive attribute | L-diversity results | `ghcr.io/sydgep/docker-images/l_diversity:v1` |
| **T-Closeness** | Measures the distance between sensitive-attribute distributions in equivalence classes and the complete dataset. | Anonymized dataset | Quasi-identifiers; sensitive attribute; threshold | T-closeness results | `ghcr.io/sydgep/docker-images/t_closeness:v1` |

## Statistical and Utility Evaluation

| Component | Purpose | Input | Main parameters | Output | Docker image |
|---|---|---|---|---|---|
| **Statistical Evaluation** | Compares real and synthetic data distributions and dependencies. | Original and synthetic datasets | Compared columns; statistical metrics | Fidelity metrics and plots | `ghcr.io/sydgep/docker-images/statistical_evaluation:v1` |
| **Utility Evaluation** | Compares downstream machine-learning performance using real and synthetic data. | Original and synthetic datasets | Target column; model; evaluation metrics | Accuracy, F1, ROC-AUC, and related results | `ghcr.io/sydgep/docker-images/utility_evaluation:v1` |

Statistical fidelity, analytical utility, and privacy risk should be interpreted together. A high score in one dimension does not establish that a released dataset is suitable in every other dimension.

## Selecting and Connecting Components

When constructing a pipeline:

1. Start with an ingestion component appropriate for the data source.
2. Integrate the required tables or datasets.
3. Preprocess the data before applying privacy-preserving transformations.
4. Select either anonymization, synthetic data generation, or both according to the use case.
5. Add the privacy, statistical, and utility evaluations needed to assess the result.
6. Confirm that each component's output columns and format satisfy the next component's input requirements.

For step-by-step instructions, see **[Create a Pipeline](create-pipelines.md)**.
