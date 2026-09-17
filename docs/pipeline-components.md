# Pipeline Components

[← Back to the main README](../README.md)

INSAFEDARE provides reusable components for constructing tabular-data and medical-image pipelines. Each component performs a defined operation and can be connected to compatible components through the graphical pipeline editor.

## Common Pipeline Configuration

At pipeline level, users configure the directories used to locate source data and store generated results.

| Parameter | Description | Default value |
|---|---|---|
| `name` | Name of the pipeline. | `SynDataPipelines` |
| `comments` | Optional description or notes about the pipeline. | — |
| `sourceDirectory` | Directory containing source datasets and other required inputs. | `./Datasets/` |
| `targetDirectory` | Directory for intermediate and final pipeline outputs. | `local_volume_mount` |
| `captureMetadata` | Enables FAIR-aware metadata capture. | Disabled |
| `useDVC` | Enables dataset and output versioning with DVC. | Disabled |

## Data Ingestion

| Component | Purpose | Input Parameters | Output |
|---|---|---|---|
| **File Extraction** | Ingests data from supported source files. |`Input File`: filename (.csv, .json, .parquet, .txt, .xls, .xlsx, .gzip or .zip). <br>`Columns`: columns to extract (all columns will be extracted if empty). | Name.parquet (Name is specified by the user) |
| **Database Extraction** | Connects to a PostgreSQL database and extracts selected columns from a table. | `Host`, `Port`, `Username`, `Password`, `Database Name`, `Schema`, `Table Name`, `Columns` | Name.parquet (Name is specified by the user)|
| **API Extraction** | Retrieves records from an API response and extracts the requested fields. |`url`: API endpoint.<br>`Record Path`: path to the records.<br>`Columns`: fields to retain. | Name.parquet (Name is specified by the user) |

Regardless of the source format, the component writes the extracted dataset to a **Parquet file** for use by downstream pipeline components.

## Data Integration and Preprocessing

| Component | Purpose | Input | Parameters | Output |
|---|---|---|---|---|
| **Merge Data** | Combines two datasets using a common column and a selected relational join method. | Two Parquet datasets containing the merge column. | `mergeOn`: common column.<br>`mergeHow`: `left`, `right`, `inner`, or `outer`. | Merged Parquet dataset. |
| **Merge on Date** | Aligns two datasets using identifier and date columns. | Two Parquet datasets containing the merge and date columns. | `mergeOn`: common identifier.<br>`dateColBase`: date column in the base dataset.<br>`dateColMerge`: date column in the second dataset.<br>`mergeDirection`: `nearest`, `forward`, or `backward`. | Date-aligned Parquet dataset. |
| **Count Measurement** | Counts records within groups and creates a measurement-count feature. | Parquet dataset containing the grouping column. | `groupby`: column used to group records.<br>`outputColname`: name of the generated count column. | Parquet dataset containing the measurement count. |
| **Extract Measurement** | Selects records representing a specified measurement. | Long-format Parquet dataset containing measurement names and values. | `nameCol`: measurement-name column.<br>`valueCol`: measurement-value column.<br>`measurementName`: measurement to extract.<br>`matchType`: `startswith`, `exact`, or `contains`. | Parquet dataset containing the selected measurement. |
| **Expand Feature** | Expands compound or repeated feature values into separate columns. | Parquet dataset containing the feature to expand. | `feature`: feature column.<br>`expandOn`: separator or expansion criterion. | Expanded Parquet dataset. |
| **Select Cohort** | Selects records belonging to a defined population or clinical cohort. | Parquet dataset containing identifiers and target values. | `targetCol`: column searched for the cohort criterion.<br>`idCol`: record or patient identifier.<br>`matchOn`: value or code to match.<br>`matchOnLen`: number of leading characters used for matching. | Cohort-specific Parquet dataset. |
| **Feature Selection** | Retains the columns required for subsequent processing or analysis. | Parquet dataset. | `columns`: columns to retain. | Parquet dataset containing the selected features. |
| **Label Encoder** | Converts categorical values into numerical labels. | Parquet dataset containing categorical columns. | `columns`: categorical columns to encode. | Label-encoded Parquet dataset. |
| **One-Hot Encoder** | Converts categorical variables into binary indicator columns. | Parquet dataset containing categorical columns. | `columns`: categorical columns to encode. | One-hot-encoded Parquet dataset. |
| **Convert to Numeric** | Converts compatible columns to numerical data types. | Parquet dataset. | `columns`: columns to convert. | Numerically converted Parquet dataset. |
| **Drop NaN** | Removes records containing missing values. | Parquet dataset containing missing values. | No component-specific parameters. | Parquet dataset without records containing missing values. |
| **Missing-Value Imputation** | Automatically fills missing numerical values with the median and categorical values with the mode. | Parquet dataset containing missing values. | `excludedColumns`: columns that must not be imputed. | Imputed Parquet dataset. |
| **Missing-Value Removal** | Removes remaining records with unresolved missing values. | Parquet dataset containing unresolved missing values. | No component-specific parameters. | Cleaned Parquet dataset. |
| **Rename Columns** | Renames selected columns to standardize the dataset schema. | Parquet dataset. | `oldColumns`: existing column names.<br>`newColumns`: corresponding replacement names. | Parquet dataset with renamed columns. |
| **OMOP CDM Transformer** | Maps source healthcare data to OMOP Common Data Model concepts and produces transformation metadata. | Source Parquet dataset, OMOP concept file, and optional column-description file. | `baseInput`, `conceptFile`, `userColumnDescriptions`, `nrows`, `omopHubApiKey`, `similarityThreshold`, `enableNoteNlp`, `useEmbeddings`, `nlpEntityTypes`, `nlpMaxWords`, `exportFormat`, `embeddingModel`, `standardOnly`, `omopOutputDir`, `outputMetadata`, `outputOmopMapping` | OMOP-formatted tables, metadata, and mapping results. |
| **OMOP Table Merger** | Combines generated OMOP tables and records information about the resulting CDM package. | Directory containing generated OMOP tables. | `omopTablesOutputDir`, `sharedFolder`, `format`, `cdmHolder`, `vocabularyVersion`, `cdmEtlReference`, `sourceDocumentationReference` | Consolidated OMOP CDM output. |

## Privacy-Preserving Transformations

| Component | Purpose | Input | Parameters | Output |
|---|---|---|---|---|
| **De-identification** | Removes columns containing direct identifiers. | Parquet dataset containing direct identifiers. | `identifierColumns`: direct-identifier columns to remove. | De-identified Parquet dataset. |
| **Anonymization** | Transforms identifying or quasi-identifying values through the selected anonymization strategy. | De-identified Parquet dataset. | `identifierColumn`: column to transform.<br>`strategy`: anonymization method.<br>`precision`: precision used by the selected strategy.<br>`interval`: generalization interval. | Anonymized Parquet dataset. |
| **Gaussian Copula Synthesizer** | Learns statistical distributions and dependencies and generates new tabular records. | Preprocessed Parquet dataset. | No component-specific parameters. | Synthetic Parquet dataset. |
| **VAE Synthesizer** | Generates synthetic tabular data using a variational autoencoder. | Preprocessed numerical or encoded Parquet dataset. | `epochs` (default: `400`), `batchSize` (default: `100`), `lossFactor` (default: `2`). | Synthetic Parquet dataset. |
| **CTGAN Synthesizer** | Generates synthetic tabular data using a conditional generative adversarial network. | Preprocessed Parquet dataset. | `epochs` (default: `400`), `batchSize` (default: `100`). | Synthetic Parquet dataset. |
| **CopulaGAN Synthesizer** | Generates synthetic tabular data by combining copula-based modelling with a generative adversarial network. | Preprocessed Parquet dataset. | `epochs` (default: `400`), `batchSize` (default: `100`). | Synthetic Parquet dataset. |

## Evaluations

| Component | Purpose | Input | Parameters | Output |
|---|---|---|---|---|
| **Statistical Evaluation** | Compares the statistical distributions and relationships of original and transformed data. | Original and transformed Parquet datasets. | No component-specific parameters. | Statistical-fidelity metrics and visualizations. |
| **Data Quality Evaluation** | Evaluates the completeness, validity, consistency, and general quality of a processed dataset. | Dataset to evaluate. | No component-specific parameters. | Data-quality measurements and report. |
| **Overfitting Evaluation** | Assesses whether generated data reproduce training records or patterns too closely. | Original and synthetic datasets. | No component-specific parameters. | Overfitting-risk results. |
| **Classical Privacy Evaluation** | Evaluates privacy using quasi-identifiers and sensitive attributes. | Anonymized or transformed dataset. | `quasiIdentifiers` (default: `Age, Gender`).<br>`sensitiveAttributes` (default: `Patient_Status`). | Classical privacy-evaluation results. |
| **Attribute Inference Attack** | Evaluates whether sensitive attributes can be inferred from known attributes. | Transformed or synthetic dataset. | `quasiIdentifiers`, `sensitiveAttributes`, `attackModel` (default: `random_forest`). | Attribute-inference risk metrics. |
| **Record Linkage Attack** | Estimates whether released records can be linked using quasi-identifying attributes. | Original and transformed datasets. | `quasiIdentifiers` (default: `Age, Gender`). | Record-linkage risk metrics. |
| **Distance to Closest Real Record** | Measures the similarity between generated records and their closest original records. | Original and synthetic datasets. | No component-specific parameters. | Distance-based privacy metrics. |
| **K-Anonymity** | Verifies whether each quasi-identifier combination is shared by at least the expected number of records. | Anonymized dataset. | `quasiIdentifiers`.<br>`expectedK` (default: `5`). | K-anonymity results. |
| **L-Diversity** | Measures sensitive-value diversity within quasi-identifier equivalence classes. | Anonymized dataset. | `quasiIdentifiers`, `sensitiveAttributes`, `expectedL` (default: `2`). | L-diversity results. |
| **T-Closeness** | Measures the distance between sensitive-attribute distributions within equivalence classes and the complete dataset. | Anonymized dataset. | `quasiIdentifiers`, `sensitiveAttributes`, `expectedT` (default: `0.2`). | T-closeness results. |
| **Utility Evaluation** | Compares downstream machine-learning performance obtained from original and transformed data. | Original and transformed datasets. | `featureColumns`, `targetCol`, `trainSize` (default: `0.7`), `modelName` (default: `SVC`), `metrics` (default: `accuracy,f1,roc_auc`). | Predictive-utility metrics and comparisons. |

## Image Pipeline Components

| Component | Purpose | Input | Parameters | Output |
|---|---|---|---|---|
| **Image Preprocessing** | Prepares medical-image slices for model training through modality selection, resizing, foreground filtering, and sampling. | BraTS medical images, including NIfTI (`.nii` or `.nii.gz`) files. | `input` (default: `BraTS`), `modality` (default: `t2f`), `targetSize` (default: `64`), `minForeground` (default: `500`), `totalSlices` (default: `10000`), `selection` (default: `topk_foreground`), `seed` (default: `42`), `output` (default: `preprocessed`). | Preprocessed image dataset. |
| **DCGAN Model Training** | Trains a DCGAN model using the preprocessed medical images. | Preprocessed image dataset. | `input`, `imageSize` (default: `64`), `batchSize` (default: `128`), `emaBeta` (default: `0.9999`), `epochs` (default: `100`), `lrG` (default: `0.0004`), `lrD` (default: `0.0002`), `output`. | Trained DCGAN model and checkpoint. |
| **WGAN-GP Model Training** | Trains a Wasserstein GAN with gradient penalty using the preprocessed medical images. | Preprocessed image dataset. | `input`, `imageSize` (default: `64`), `batchSize` (default: `128`), `emaBeta` (default: `0.9999`), `epochs` (default: `100`), `gpEvery` (default: `16`), `lambdaGp` (default: `10`), `nCritic` (default: `5`), `output`. | Trained WGAN-GP model and checkpoint. |
| **Synthetic Image Generation** | Uses a trained image model to generate synthetic medical images. | Trained model checkpoint. | `input` (default: `trained_model/checkpoint_latest.pt`), `numImages` (default: `1000`), `batchSize` (default: `64`), `output` (default: `synthetic_images`). | Synthetic medical-image collection. |
| **Image Statistical Evaluation** | Compares real and generated medical images using statistical image-generation metrics. | Preprocessed real images and a trained model checkpoint. | `input1`, `input2`, `numReal` (default: `1000`), `numFake` (default: `1000`), `batchSize` (default: `32`), `kidSubsetSize` (default: `500`). | Statistical image-evaluation results. |
| **Image Privacy Evaluation** | Evaluates similarity and privacy risks between training, reference, and generated images. | Preprocessed images and a trained model checkpoint. | `input1`, `input2`, `referenceSplit` (default: `test`), `numTrainReal` (default: `2000`), `numReferenceReal` (default: `2000`), `numFake` (default: `2000`), `batchSize` (default: `32`), `numWorkers` (default: `0`). | Image privacy-evaluation results. |

For step-by-step instructions, see **[Create a Pipeline](create-pipelines.md)**.
