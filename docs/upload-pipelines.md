# Upload an Existing Pipeline

[← Back to the main README](../README.md)

Uploading a pipeline lets you explore or test a predefined workflow without building it from scratch.

## Before you begin

Make sure that:

- INSAFEDARE is running at <http://localhost:8080>;
- you have downloaded the pipeline file supplied by the INSAFEDARE team or from the repository's `examples/` folder; and
- any datasets required by the example are stored in the locations specified by its accompanying instructions.

## Upload the pipeline

1. Open INSAFEDARE in your browser.
2. From the projects page, choose the option for uploading or importing an existing project or pipeline.
3. Select the supplied pipeline file from your computer.
4. Confirm the upload and wait for the import to finish.
5. Open the imported project, then open its pipeline.
6. Check that the pipeline diagram and its components are visible.

> The wording and position of the upload/import control may vary between testing releases.

## Review the configuration

Before generating or executing the pipeline:

1. Select each component that requires configuration.
2. Check its input and output paths.
3. Replace paths that refer to another user's computer.
4. Check other required parameters, such as column names, identifiers, or model settings.
5. Save the updated pipeline.

Absolute paths must refer to locations that exist on the computer running INSAFEDARE. If the pipeline uses Docker-based tasks, use the shared source and target directories defined for that pipeline.

## Generate and execute

After reviewing the configuration:

1. Run the application's code-generation action.
2. Review any validation or generation messages.
3. Execute the generated pipeline according to the example's instructions.
4. Monitor the execution logs and inspect the generated outputs.

## Example pipelines

Each example should ideally include:

- the pipeline/project import file;
- a small test dataset or a link to the dataset;
- required directory settings;
- expected outputs; and
- any component-specific notes.

Do not upload confidential or identifiable healthcare data unless the approved secure environment and data-governance procedures are in place.

For import or execution problems, see [Troubleshooting](troubleshooting.md).

Next: **[Create a New Pipeline →](create-pipelines.md)**
