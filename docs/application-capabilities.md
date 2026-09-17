# Application Capabilities

[← Back to the main README](../README.md)

The INSAFEDARE Web Application supports the design, documentation, execution, and assurance of data-processing pipelines. Its capabilities are intended to help users construct reproducible workflows while improving the traceability, semantic description, and assessment of healthcare data processing.

## 1. Visual Pipeline Design and Execution

INSAFEDARE provides a graphical environment in which users can construct a pipeline by selecting, configuring, and connecting reusable processing components.

The application supports:

- creation and management of pipeline projects;
- graphical composition of processing workflows;
- configuration of pipeline-level and component-level parameters;
- validation of component connections and required information;
- generation of executable workflow code;
- execution of containerized pipeline tasks; and
- monitoring of task execution and generated results.

Pipeline components cover data ingestion, integration, preprocessing, privacy-preserving transformations, synthetic data generation, and multidimensional evaluation.

Related documentation:

- **[Create a Pipeline](create-pipelines.md)**
- **[Upload an Existing Pipeline](upload-pipelines.md)**
- **[Pipeline Components](pipeline-components.md)**

## 2. FAIR-Aware Metadata Capture

INSAFEDARE captures metadata throughout pipeline design and execution to improve the **findability, accessibility, interoperability, and reusability** of datasets and processing results.

The captured information can include:

- dataset identity and descriptive metadata;
- data source and format;
- pipeline structure and component sequence;
- component parameters and execution settings;
- input, intermediate, and output artefacts;
- data lineage and provenance relationships;
- data-quality information; and
- details of the transformations applied to a dataset.

This metadata supports reproducibility and helps users understand how a dataset or result was produced. The term **FAIR-aware** indicates that the application supports FAIR-oriented practices without implying that every generated dataset is automatically fully FAIR-compliant.

## 3. Intelligent Pipeline Recommendations

The intelligent recommendation capability assists users while they construct and configure pipelines. Recommendations are derived from available dataset information, pipeline context, component compatibility, and domain knowledge.

Depending on the available context, recommendations can help users:

- identify suitable processing components;
- select an appropriate next pipeline step;
- detect potentially incompatible component connections;
- identify required preprocessing operations;
- select relevant privacy-preserving transformations;
- choose suitable evaluation approaches; and
- review parameters or configuration choices that may require attention.

Recommendations are intended to support decision-making. Users remain responsible for confirming that each recommendation is appropriate for the dataset, research objective, and applicable data-governance requirements.

## 4. Ontology-Driven Assurance Portal

The Ontology-Driven Assurance Portal uses structured domain knowledge to support semantic interpretation, verification, and assurance of datasets and pipeline processes.

The portal can support:

- semantic annotation of datasets, variables, and pipeline outputs;
- mapping of data elements to relevant ontology concepts;
- reuse of standardized clinical or biomedical terminology;
- consistency checks based on defined concepts and relationships;
- navigation between pipeline evidence, metadata, and assurance information; and
- improved traceability of decisions and processing results.

Ontology services can be used to retrieve and cache relevant concepts, reducing repeated requests and supporting consistent annotations across pipeline stages.

The assurance portal complements, rather than replaces, technical privacy and utility evaluation. Ontology-based evidence, execution metadata, and quantitative evaluation results collectively provide a more complete view of a pipeline and its outputs.

## How the Capabilities Work Together

| Capability | Primary contribution |
|---|---|
| **Visual Pipeline Design and Execution** | Enables users to construct and run reproducible processing workflows. |
| **FAIR-Aware Metadata Capture** | Records the information needed to understand, trace, and reuse data-processing results. |
| **Intelligent Pipeline Recommendations** | Assists users in selecting and configuring suitable pipeline operations. |
| **Ontology-Driven Assurance Portal** | Adds semantic context and structured evidence for interpreting and assuring pipeline results. |

Together, these capabilities support the creation of traceable, reusable, and assessable workflows for privacy-preserving healthcare data processing.
