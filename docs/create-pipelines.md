# Create a Pipeline

[← Back to the main README](../README.md)

## 1. Create a new pipeline

1. From the **Front** page, click on `New Project` and then click on `Create Project`.
2. Click on  `...` at the top right-part, the click on `Open Sirius`
3. Expand the `>` under the **Explorer** view and Click on `SynDataPipelines`
4. Click the `⋮` next to `SynDataPipelines` then click on `New Representation` 

### Configure the pipeline

| Parameter | Description | Default value |
|---|---|---|
| `Name` | Name of the pipeline. | `SynDataPipelines` |
| `Comments` | Optional description or notes about the pipeline. | — |
| `Source Directory` | Directory containing source datasets and other required inputs. | `./Datasets/` |
| `Target Directory` | Directory used to store intermediate and final pipeline outputs. | `local_volume_mount` |
| `Capture Metadata` | Enables FAIR-aware metadata capture (optional). | Disabled |
| `Use DVC` | Enables dataset and output versioning with DVC (optional). | Disabled |

### Add and Connect Components

1. Click the `⋮` next to the `SynDataPipelines`, then select **New object** to open the **Create a new object** window.
2. Select a component, such as `File Extraction`. You can expand the categories to view all available components.
3. Click **CREATE**. A new component block will appear in the **Pipeline Diagram** at the centre of the interface.
4. Select the component block and use the **Details** view on the right-hand side to configure its parameters.
5. Repeat these steps to add the remaining components.
6. Connect the components in their required execution order.
7. The output of each component is automatically passed to the next connected component.

Detailed information about the available components, their parameters, and outputs is provided in the **[Pipeline Components](pipeline-components.md)** guide.

### Generate and Execute the Pipeline

1. Click the **⋮** menu next to the root element in the **Explorer** view.
2. Select **Generate Code**.
3. Click **START SERVER** to start the Prefect server.
4. Click **EXECUTE CODE** to run the generated pipeline.

### Monitor Pipeline Execution

1. Open the Prefect interface from within the application or visit <http://127.0.0.1:4200> in your web browser.
2. Use the Prefect interface to monitor flow runs, task execution, logs, and errors.
3. Open the configured `Target Directory` to access the intermediate and final output files.

## 2. Download the Pipeline

1. In the **Explorer** view, click the **⋮** menu next to the project name at the top.
2. Select **Download**.
3. Your browser will download the project as a ZIP file. Keep this file if you want to import the pipeline later or share it with a partner.

## 3. Upload an Existing Pipeline
Uploading a pipeline lets you explore or test a predefined workflow without building it from scratch.

2. From the projects page, choose the option for uploading or importing an existing project or pipeline.
3. Select the supplied pipeline file from your computer.
4. Confirm the upload and wait for the import to finish.
5. Open the imported project, then open its pipeline.
6. Check that the pipeline diagram and its components are visible.

For solutions to common problems, see the **[Troubleshooting Guide](troubleshooting.md)**.
