# Model release: rapid urban pluvial-flood prediction

This repository is the model-release companion to:

> Ruyi Li, Zhenyu Huang, Yang Dong, Qing Hu, Xin Dong, and Lijin Zhong (2026). “A mechanism-data hybrid pretraining-finetuning framework for rapid and reliable urban pluvial flooding prediction.” *Water Research*, 305, 126491. https://doi.org/10.1016/j.watres.2026.126491

## What is—and is not—released

This repository intentionally contains documentation only. The associated release package contains trained model artifacts and checksums. Training code, inference source code, the water-depth extraction source code, raw observations, mechanistic simulations, social-media records, GIS layers, coordinates, API credentials, and study-area-specific configuration are not distributed here.

Two original PyTorch checkpoints are preserved by the authors:

- `pretrain_early_stop_20250903_0301.pth` — checkpoint after mechanism-informed pretraining;
- `finetune_early_stop_20250902_1620_1.pth` — checkpoint after observation-guided finetuning.

Because a PyTorch `state_dict` depends on the private architecture implementation, source-free use should prefer the exported generator artifacts (`*.ts`, TorchScript) supplied in the approved download package. See [MODEL_USAGE.md](MODEL_USAGE.md) for the public runtime interface.

## Requesting model access

Model files are distributed through a registration-based access process so the authors can understand the communities and sectors using the research. GitHub itself cannot require a questionnaire before a public clone or ZIP download, so model binaries are not stored in the Git repository.

Please email a completed copy of [ACCESS_REQUEST_TEMPLATE.md](ACCESS_REQUEST_TEMPLATE.md) from an institutional or professional email address to one of the corresponding authors:

- Prof. Xin Dong — `dongxin@tsinghua.edu.cn`
- Prof. Lijin Zhong — `zhonglijin@huanding.org`

The request asks for your role/identity, sector, country or region, organization, intended use, and agreement to the access terms. Do not include government ID numbers, home addresses, or other sensitive personal data. Read [PRIVACY_NOTICE.md](PRIVACY_NOTICE.md) before sending a request.

Access is not automatic. If approved, the corresponding author will provide the current download location and the terms that apply to the model package.

## Requesting source code

The source code is not publicly distributed. Researchers who have a justified need for source access may describe that need in an email to the corresponding authors. Requests are considered case by case and may require additional terms or institutional approval.

## Documentation

- [MODEL_CARD.md](MODEL_CARD.md): scope, limitations, and responsible use;
- [MODEL_USAGE.md](MODEL_USAGE.md): artifact verification and TorchScript interface;
- [ACCESS_REQUEST_TEMPLATE.md](ACCESS_REQUEST_TEMPLATE.md): registration email template;
- [PRIVACY_NOTICE.md](PRIVACY_NOTICE.md): purpose and handling of registration information;
- [TERMS_OF_USE.md](TERMS_OF_USE.md): repository and model-access terms;
- [CITATION.cff](CITATION.cff): machine-readable citation metadata.

## Citation

If this model or its documentation contributes to your work, cite the paper above. A BibTeX entry is provided in [CITATION.bib](CITATION.bib).

## Disclaimer

The artifacts are research outputs, not an operational warning system. They must not be used as the sole basis for emergency response, public-safety, engineering-design, insurance, or regulatory decisions.
