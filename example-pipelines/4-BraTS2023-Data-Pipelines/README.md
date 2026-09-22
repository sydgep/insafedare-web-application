# Image Pipelines: BraTS 2023 Glioma MRI

These two INSAFEDARE example projects use the **BraTS 2023 Adult Glioma (GLI) training dataset** to illustrate image preprocessing and synthetic MRI image generation. 
The projects are supplied as **Images Pipeline - DCGAN.zip** and **Images Pipeline - WPGAN.zip** (the latter names its generation component WGAN).

## Source dataset

| Item | Description |
|---|---|
| Source | [ASNR-MICCAI-BraTS2023-GLI-Challenge-TrainingData.zip on Synapse](https://www.synapse.org/Synapse:syn51514132) |
| Subject matter | Brain MRI from the BraTS 2023 adult glioma challenge |
| Image format | Compressed NIfTI MRI volumes (`.nii.gz`) |
| MRI sequences | Native T1 (`t1n`), contrast-enhanced T1 (`t1c`), T2-weighted (`t2w`), and T2-FLAIR (`t2f`) |
| Access | A Synapse account and acceptance of the applicable BraTS data access terms may be required; follow the instructions on the Synapse data page. |

The dataset is **not included** in either pipeline project export. The preprocessing component in both projects refers to a source folder named `BraTS2023`. 
Arrange the downloaded data in the location expected by your local pipeline configuration and update the source directory if necessary. 
Respect the dataset's terms when storing or sharing original images and generated outputs.

## Pipeline components

| Stage | Saved settings | Purpose |
|---|---|---|
| **Image Preprocessing** | Input: `BraTS2023`; minimum foreground: `1000`; total slices: `6000` | Select and prepare MRI image slices from the source dataset. |
| **Synthetic Image Generation** | A trained model checkpoint; number of images: `2000` | Generate images using the chosen model. |

Both exports contain these two components, with the following model-specific generation settings:

| Project export | Generation component | Model checkpoint input | Named output | Configured target directory |
|---|---|---|---|---|
| `Images Pipeline - DCGAN.zip` | `Images_generation_DCGAN` | `DCGAN_trained_model/checkpoint_latest.pt` | `DCGAN_synthetic_images` | `Datasets/outputs/DCGAN_pipeline` |
| `Images Pipeline - WPGAN.zip` | `Images_generation_WGAN` | `WGAN_trained_model/checkpoint_latest.pt` | `WGAN_synthetic_images` | `Datasets/WGAN_pipeline_outputs` |

The paths in the exports are configured relative to an author's local `Datasets` directory; set the source directory, target directory, and checkpoint locations for your computer.
**The checkpoint files are not included in the exports.** Synthetic image generation therefore requires an appropriate pretrained checkpoint for each model.

For general guidance on configuring pipeline components, see the [pipeline creation guide](../docs/create-pipelines.md) and [component reference](../docs/pipeline-components.md).
