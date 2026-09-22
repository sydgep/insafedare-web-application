# Create a Pipeline

[← Back to the main README](../README.md)

## 1. Create a new pipeline

1. On the **Front** page, click on `New Project` and then click on `Create Project`.
2. Click on  `⋯` at the top right-part, the click on `Open Sirius`
3. Expand the `>` under the **Explorer** view and Click on `SynDataPipelines`
4. Click the `⋮` next to `SynDataPipelines` then click on `New Representation` to open the pipeline diagram.

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
3. Click **CREATE**. The component will appear in the **Pipeline Diagram** at the centre of the interface.
4. Select the component and configure its parameters in the **Details** view on the right.
5. Repeat these steps to add the remaining components.
6. Connect the components in the order you want them to run. A connected component receives the output of the preceding component.

Detailed information about the available components, their parameters, and outputs is provided in the **[Pipeline Components](pipeline-components.md)** guide.

### Generate and Execute the Pipeline

1. Click the **⋮** menu next to the root element in the **Explorer** view.
2. Select **Generate Code**.
3. Click **START SERVER** to start the Prefect server.
4. Click **EXECUTE CODE** to run the generated pipeline.

### Monitor Execution

1. Open Prefect from the application or visit <http://127.0.0.1:4200>.
2. Check flow runs, task status, and logs in the Prefect interface.
3. Open the configured `Target Directory` to find the pipeline outputs.

## 2. Download a Pipeline

1. In the **Explorer** view, click the **⋮** menu next to the project name at the top.
2. Select **Download**.
3. Your browser will download the project as a ZIP file. Keep this file if you want to import the pipeline later or share it with a partner.

## 3. Upload an Existing Pipeline

Uploading a project lets you explore an existing pipeline without creating one from scratch. 

You can find [example pipelines in the repository](https://github.com/sydgep/insafedare-web-application/tree/main/example-pipelines).

1. On the **Front** page, click **Import Project**.
2. In the **Upload a project** window, select a project ZIP file from your computer.
3. Click **UPLOAD**.
4. Open the imported project and click **Pipeline Diagram** to view its pipeline.

For help with common problems, see the **[Troubleshooting Guide](troubleshooting.md)**.
