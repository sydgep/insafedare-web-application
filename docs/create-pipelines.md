# Create a Pipeline

[← Back to the main README](../README.md)

INSAFEDARE provides a graphical interface for assembling data-processing components into an executable workflow.

## Typical workflow

1. Create a project.
2. Create a pipeline inside the project.
3. Add the required components.
4. Connect the components in processing order.
5. Configure the pipeline and component parameters.
6. Validate the model.
7. Generate the executable workflow.
8. Execute and monitor the pipeline.

## 1. Create a project and pipeline

1. Launch INSAFEDARE and open <http://localhost:8080>.
2. Create a new project from the projects page and give it a meaningful name.
3. Open the project and create a new pipeline.
4. Give the pipeline a short, descriptive name.

## 2. Add components

Choose components according to the workflow you want to build. Available categories can include:

- data ingestion;
- data integration;
- preprocessing;
- direct-identifier removal and anonymization;
- synthetic data generation;
- privacy evaluation;
- statistical evaluation; and
- utility evaluation.

Add only the components needed for the intended use case.

## 3. Connect the workflow

Connect each component's output to the next component's input. A typical pipeline might follow this order:

**Ingestion → Integration → Preprocessing → Privacy-preserving operation → Evaluation**

Ensure that the output format and columns produced by one component are compatible with the next component.

## 4. Configure the pipeline

Configure the pipeline-level directories:

- **Source directory:** location of the original input data.
- **Target directory:** location for intermediate and final outputs.

Then configure each component. Depending on the component, parameters may include:

- input and output filenames;
- selected or excluded columns;
- identifiers and join columns;
- preprocessing strategies;
- anonymization attributes;
- synthesis settings such as epochs and batch size; and
- evaluation targets and metrics.

Paths must exist and be accessible to the application and its Docker containers. Avoid overwriting original datasets; write processed outputs to the target directory.

## 5. Validate and generate

1. Confirm that all required components are connected.
2. Check that mandatory parameters have values.
3. Save the pipeline.
4. Run the validation action, if available.
5. Run code generation.
6. Review the generation messages and resolve reported errors before execution.

## 6. Execute and monitor

1. Start the required execution services for the generated workflow.
2. Execute the generated pipeline.
3. Follow the task logs to monitor progress.
4. Inspect intermediate outputs when diagnosing a failed step.
5. Review the final datasets, reports, plots, and evaluation results in the configured target directory.

## Good practice

- Begin with a small, non-confidential test dataset.
- Use descriptive component and output names.
- Keep source data separate from generated outputs.
- Change one configuration at a time when troubleshooting.
- Record the pipeline version, component parameters, and Docker image versions used for important experiments.
- Follow the project's data-governance rules when working with healthcare data.

For common problems, see the [troubleshooting guide](troubleshooting.md).
