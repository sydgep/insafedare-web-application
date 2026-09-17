# Create a Pipeline

[← Back to the main README](../README.md)

## 1. Create and configure a pipeline

1. From the **Front** page, create a new project and give it a meaningful name.
2. Open the project and create a new pipeline.
3. At the pipeline level, configure the directories used to locate source data and store generated results.

| Parameter | Description | Default value |
|---|---|---|
| `Name` | Name of the pipeline. | `SynDataPipelines` |
| `Comments` | Optional description or notes about the pipeline. | — |
| `Source Directory` | Directory containing source datasets and other required inputs. | `./Datasets/` |
| `Target Directory` | Directory used to store intermediate and final pipeline outputs. | `local_volume_mount` |
| `Capture Metadata` | Enables FAIR-aware metadata capture. | Disabled |
| `Use DVC` | Enables dataset and output versioning with DVC. | Disabled |

## 2. Add and Connect Components

1. In the **Explorer** view on the left-hand side, locate `SynDataPipelines` or the name assigned to your pipeline.
2. Click the **⋮** menu next to the pipeline name.
3. Select **New object** to open the **Create a new object** window.
4. Select a component, such as `File Extraction`. You can expand the categories to view all available components.
5. Click **CREATE**. A new component block will appear in the **Pipeline Diagram** at the centre of the interface.
6. Select the component block and use the **Details** view on the right-hand side to configure its parameters.
7. Repeat these steps to add the remaining components.
8. Connect the components in their required execution order.
9. The output of each component is automatically passed to the next connected component.

Detailed information about the available components, their parameters, and outputs is provided in the **[Pipeline Components](pipeline-components.md)** guide.

## 3. Generate and Execute the Pipeline

1. Click the **⋮** menu next to the root element in the **Explorer** view.
2. Select **Generate Code**.
3. Click **START SERVER** to start the Prefect server.
4. Click **EXECUTE CODE** to run the generated pipeline.

## 4. Monitor Pipeline Execution

1. Open the Prefect interface from within the application or visit <http://127.0.0.1:4200> in your web browser.
2. Use the Prefect interface to monitor flow runs, task execution, logs, and errors.
3. Open the configured `Target Directory` to access the intermediate and final output files.

For solutions to common problems, see the **[Troubleshooting Guide](troubleshooting.md)**.
