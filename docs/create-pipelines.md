# Create a Pipeline

[← Back to the main README](../README.md)

INSAFEDARE provides a graphical interface for assembling data-processing components into an executable workflow.

## 1. Create a project and pipeline

1. Launch INSAFEDARE and open <http://localhost:8080>.
2. Create a new project from the projects page and give it a meaningful name.
3. Open the project and create a new pipeline.
4. Give the pipeline a short, descriptive name.

## 2. Configure the pipeline

At pipeline level, users configure the directories used to locate source data and store generated results.

| Parameter | Description | Default value |
|---|---|---|
| `Name` | Name of the pipeline. | `SynDataPipelines` |
| `Comments` | Optional description or notes about the pipeline. | — |
| `Source Directory` | Directory containing source datasets and other required inputs. | `./Datasets/` |
| `Target Directory` | Directory for intermediate and final pipeline outputs. | `local_volume_mount` |
| `Capture Metadata` | Enables FAIR-aware metadata capture. | Disabled |
| `Use DVC` | Enables dataset and output versioning with DVC. | Disabled |

## 3. Add components and Connect the workflow

1. From the `Explorer` view (on the left-hand side), click on the three-dots of the `SynDataPipelines` or the name use used.
2. Click on `New object`, you will see "Create a new object"
3. Select the component e.g `File Extraction`, (you can expand the inverted triangle too see all the components)
4. Click on `CREATE`. It will instantiate a block on the Pipeline Diagram View ( At the center).
5. Go to the Details view (on the right hand-side) to fill in the input parameters of the component.
6. Add another components and connect them. The output of the first block is directly passed as an input to the subsequent component.

Detailed information about the components can be find [Here](pipeline-components.md).

## 4. Generate code and execute

1. Click on the `three-dots` at the root of the Explorer view
2. Click on `Generate Code`
3. Click on `START SERVER` to start the prefect server.
4. Click on `EXECUTE CODE`

## 5. Monitor the pipeline execution on prefect UI

1. Access the prefect server from the application or by pasting `http://127.0.0.1:4200` on your web browser.
2. Go to the configured "Target Directory" to see the outputs files.

For common problems, see the [troubleshooting guide](troubleshooting.md).
