# Model card

## Overview

This release supports research on rapid, spatially distributed urban pluvial-flood prediction using the mechanism-data hybrid pretraining-finetuning framework described in the associated *Water Research* article and Supplementary Information.

## Artifact set

| Artifact | Stage | Intended role |
|---|---|---|
| `pretrain_wr.pth` | Pretraining | PyTorch checkpoint containing generator (`G`) and discriminator (`D`) state dictionaries |
| `finetune_wr.pth` | Finetuning | PyTorch checkpoint containing generator (`G`) and discriminator (`D`) state dictionaries |
| `urban_flood_generator_pretrained.ts` | Pretraining | Generator without finetune |
| `urban_flood_generator_finetuned.ts` | Finetuning | Generator for inference |
| `water_level_cnn.ckpt` | Observation extraction | Four-class image water-level classifier |
| `human_reference_detector.pt` | Observation extraction | Human reference-object detector |
| `wheel_water_level_cnn.ckpt` | Observation extraction | Six-class wheel-inundation water-level classifier |

The exact SHA-256 values are distributed in `SHA256SUMS.txt` alongside the model package.

## Model architecture

The 11-channel environmental condition tensor is supplied to a U-Net generator. The encoder progresses through 64, 128, 256, 512, and 1024 channels. A symmetric transposed-convolution decoder reconstructs the spatial output using skip connections from the corresponding encoder levels. A PatchGAN discriminator evaluates the concatenation of the environmental condition tensor and predicted or reference next-step surface-water-depth field.

Pretraining uses randomized environmental scenarios, coupled SWMM-CADDIES teacher signals, physical constraints, and knowledge distillation. Finetuning adapts the pretrained model using sparse observations.

## Image-based water-level recognition

The study uses YOLOv9 to detect reference objects such as vehicle wheels and human body parts in social-media images. Detected reference positions are combined with wheel specifications and the body-part depth mapping documented in the Supplementary Information to infer water depth. A two-layer CNN supports direct classification into four water-depth intervals—`(0, 10]`, `(10, 20]`, `(20, 30]`, and `(30, 40]` cm—when a usable reference object is unavailable. Image normalization and augmentation are used during training; evaluation includes top-1 accuracy and mean class-distance error.

Associated locations must be supported by explicit text, POI, road-sign, building, station, or other visible geographic evidence.

Social-media text is processed through a compatible language-model API to screen flood-related reports and extract time, location evidence, quantitative or qualitative water-depth expressions, and supporting source text. This workflow does not require a separate local language-model checkpoint. The model package does not contain raw posts, images, API credentials, or geocoding credentials.
## Inputs and outputs

- Input: `float32` tensor shaped `[N, 11, H, W]`.
- Input order: cumulative precipitation; mean precipitation intensity; maximum precipitation intensity; runoff coefficient; storage capacity; elevation/DEM; slope; TWI; SPI; pipe capacity density; road density.
- Output: `float32` tensor shaped `[N, 2, H, W]`.
- Output channel 0: predicted next-step surface-water depth.
- Output channel 1: predicted auxiliary virtual/drainage-network water state.
- Outputs are non-negative by construction.
- Spatial dimensions should be divisible by 16.

## Environmental-variable definitions

- Precipitation features summarize event history available at the current step: cumulative amount, mean intensity, and maximum intensity. Forecast precipitation is required for prospective inference; the study workflow uses ARIMA-based forecasting.
- Runoff coefficient is derived from land-surface or land-use characteristics.
- Storage capacity combines static depression storage and time-varying infiltration/storage effects derived from catchment and Horton-infiltration parameters.
- DEM, slope, TWI, and SPI describe elevation and terrain-controlled accumulation or flow propensity.
- Pipe capacity density is pipe volume per land-parcel area.
- Road density is road length per land-parcel area.

All variables must share the grid, spatial reference, resolution, temporal alignment, units, transformations, and normalization statistics specified for the model package.

## Intended uses

- Research evaluation of rapid urban pluvial-flood prediction;
- Crowdsourced water‑depth label generation

## Out-of-scope uses

- Direct life-safety or emergency decisions;
- Unsupervised deployment in a new city, climate, drainage system, or grid;
- Use with unverified channel definitions, scales, or time steps;
- Claims of universal transferability or physical validity;
- Re-identification of locations or people from research data;
- Redistribution, reverse engineering, or commercial use unless expressly authorized in writing.

## Limitations

Reported performance is conditional on the study datasets, preprocessing, calibration, observation coverage, and evaluation design. Distribution shift, sensor errors, drainage changes, topographic uncertainty, rainfall forecast error, and different spatial or temporal resolutions can materially degrade predictions. Non-negative outputs do not guarantee conservation, hydraulic feasibility, calibrated uncertainty, or operational safety.

Compatibility requires the documented architecture, channel order, normalization statistics, time-step definition, output semantics, and model parameters.

## Citation and contact

See [README.md](README.md) and [CITATION.bib](CITATION.bib). Questions should be directed to the author listed there.
