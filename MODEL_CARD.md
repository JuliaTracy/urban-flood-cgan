# Model card

## Overview

The released artifacts support research on rapid, spatially distributed urban pluvial-flood prediction using a mechanism–data hybrid pretraining and finetuning framework. The archived checkpoints were verified to use the legacy 11-channel generator interface and produce two non-negative output channels for the next prediction step. They are not compatible with the later 12-channel/RainEncoder source revision.

## Artifact set

| Artifact | Stage | Intended role |
|---|---|---|
| `pretrain_early_stop_20250903_0301.pth` | Pretraining | Archival PyTorch checkpoint containing generator (`G`) and discriminator (`D`) state dictionaries |
| `finetune_early_stop_20250902_1620_1.pth` | Finetuning | Archival PyTorch checkpoint containing generator (`G`) and discriminator (`D`) state dictionaries |
| `urban_flood_generator_pretrained.ts` | Pretraining | Source-free TorchScript generator for research inference |
| `urban_flood_generator_finetuned.ts` | Finetuning | Source-free TorchScript generator; preferred artifact for research inference |

The exact SHA-256 values are distributed in `SHA256SUMS.txt` alongside the approved model package.

## Inputs and outputs

- Input: `float32` tensor shaped `[N, 11, H, W]`.
- Output: `float32` tensor shaped `[N, 2, H, W]`.
- Output channel 0: predicted surface-water depth for the next step.
- Output channel 1: predicted virtual/drainage-network water state for the next step.
- Outputs are non-negative by construction.
- Spatial dimensions should be divisible by 16. Padding/cropping and all feature preparation must be performed by the user.

The model expects the same channel ordering, transformations, units, temporal alignment, spatial reference, raster resolution, and scaling used in the study. Those preprocessing assets may contain study-specific information and are not included in the public documentation. Consequently, the model package is best suited to verification by approved collaborators who possess an authorized, compatible preprocessing pipeline.

## Intended uses

- Research evaluation of rapid urban pluvial-flood prediction;
- Method comparison and sensitivity analysis using compatible, authorized inputs;
- Non-operational demonstrations with expert review;
- Reproduction or extension under a separately approved collaboration.

## Out-of-scope uses

- Direct life-safety or emergency decisions;
- Unsupervised deployment in a new city, climate, drainage system, or grid;
- Use with unverified channel definitions, scales, or time steps;
- Claims of universal transferability or physical validity;
- Re-identification of locations or people from research data;
- Redistribution, reverse engineering, or commercial use unless expressly authorized in writing.

## Limitations

Performance reported in the paper is conditional on the study datasets, preprocessing, calibration, observation coverage, and evaluation design. Distribution shift, sensor errors, drainage changes, topographic uncertainty, rainfall uncertainty, and different spatial or temporal resolutions can materially degrade predictions. Non-negative outputs do not guarantee conservation, hydraulic feasibility, calibrated uncertainty, or safe operational behavior.

The TorchScript export protects ordinary source distribution but is not a cryptographic or legal guarantee against model extraction or reverse engineering. Access control and written terms remain necessary where stronger protection is required.

## Human oversight

Outputs should be reviewed by qualified hydrology/flood-risk practitioners and compared against observations or a validated mechanistic baseline. Users should document preprocessing, model version, checksums, local validation, failure cases, and any post-processing.

## Citation and contact

See [README.md](README.md) and [CITATION.bib](CITATION.bib). Questions and source-code requests should be directed to the corresponding authors listed there.
