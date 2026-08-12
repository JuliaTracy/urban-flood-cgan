# Model package usage

## 1. Verify the package

Keep `SHA256SUMS.txt` in the same directory as the downloaded artifacts.

PowerShell:

```powershell
Get-FileHash -Algorithm SHA256 .\*.pth, .\*.ts
```

Linux/macOS:

```bash
sha256sum -c SHA256SUMS.txt
```

Do not use an artifact if its digest differs from the supplied checksum.

## 2. Runtime requirement

The source-free artifacts were exported with PyTorch and are intended to be loaded with a compatible PyTorch 2.x runtime. Install a suitable CPU or CUDA build for your platform from the official PyTorch distribution channel.

## 3. Load the TorchScript generator

```python
import torch

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = torch.jit.load("urban_flood_generator_finetuned.ts", map_location=device)
model.eval()

# `conditions` must be prepared by an authorized, compatible pipeline.
# Required shape: [batch, 11, height, width], float32.
conditions = conditions.to(device=device, dtype=torch.float32)

if conditions.ndim != 4 or conditions.shape[1] != 11:
    raise ValueError("Expected conditions with shape [N, 11, H, W]")
if conditions.shape[-2] % 16 or conditions.shape[-1] % 16:
    raise ValueError("H and W must be divisible by 16")

with torch.inference_mode():
    next_state = model(conditions)

surface_depth = next_state[:, 0:1]
virtual_state = next_state[:, 1:2]
```

The snippet intentionally does not define preprocessing. A numerically valid call is not evidence that the input semantics match the study.

## 4. Original `.pth` checkpoints

The `.pth` files are archival dictionaries with top-level keys `G` and `D`. They are not standalone models. Loading them requires a matching generator/discriminator implementation and should use PyTorch’s restricted weights mode where supported:

```python
checkpoint = torch.load("finetune_early_stop_20250902_1620_1.pth",
                        map_location="cpu", weights_only=True)
generator.load_state_dict(checkpoint["G"], strict=True)
```

Never load an untrusted pickle-based checkpoint. Verify the SHA-256 digest first.

## 5. Autoregressive use

The generator predicts one next state. Multi-step rollout requires the predicted surface-depth channel to be transformed and inserted into the appropriate next-step condition channel while rainfall and other dynamic inputs advance consistently. The published model-release package does not include the private preprocessing/rollout implementation. Do not improvise channel order or scaling; request an approved collaboration or source-code access if exact reproduction is required.
