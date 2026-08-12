# Model package usage

## 1. Download and extract the package

Download the encrypted compressed archive from the repository release. Complete the [model-access questionnaire](https://v.wjx.cn/vm/wMNCnEA.aspx#) to obtain the extraction password.

The questionnaire collects city, organization or institution, professional identity or role, and industry or sector. See [ARCHIVE_ACCESS.md](ARCHIVE_ACCESS.md) and [PRIVACY_NOTICE.md](PRIVACY_NOTICE.md) for details. Questions may be sent to the author listed in [README.md](README.md).

The archive also contains the image water-level recognition weights. See [WATER_LEVEL_USAGE.md](WATER_LEVEL_USAGE.md) for those artifacts and for the language-model-assisted social-media text-processing interface.

## 2. Verify the package

Keep `SHA256SUMS.txt` beside the downloaded artifacts.

PowerShell:

```powershell
Get-FileHash -Algorithm SHA256 .\*.pth, .\*.ts
```

Linux/macOS:

```bash
sha256sum -c SHA256SUMS.txt
```

## 3. Environmental tensor

The model requires `float32[N,11,H,W]` in this exact order:

```text
0  cumulative precipitation
1  mean precipitation intensity
2  maximum precipitation intensity
3  runoff coefficient
4  storage capacity
5  elevation (DEM)
6  slope
7  topographic wetness index (TWI)
8  stream power index (SPI)
9  pipe capacity density
10 road density
```

## 4. Load the TorchScript generator

This example applies only when the package manifest confirms that the TorchScript file matches the tensor schema above.

```python
import torch

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = torch.jit.load("urban_flood_generator_finetuned.ts", map_location=device)
model.eval()

conditions = conditions.to(device=device, dtype=torch.float32)
if conditions.ndim != 4 or conditions.shape[1] != 11:
    raise ValueError("Expected conditions with shape [N, 11, H, W]")
if conditions.shape[-2] % 16 or conditions.shape[-1] % 16:
    raise ValueError("H and W must be divisible by 16")

with torch.inference_mode():
    next_state = model(conditions)

surface_depth = next_state[:, 0:1]
```

## 5. Original `.pth` checkpoints

The `.pth` files are dictionaries with top-level `G` and `D` keys, not standalone models. Loading requires an exactly matching private implementation. Verify the checksum and use restricted weights mode where supported:

```python
checkpoint = torch.load(
    "finetune_wr.pth",
    map_location="cpu",
    weights_only=True,
)
generator.load_state_dict(checkpoint["G"], strict=True)
```

## 6. Sequential inference

At each step, update cumulative, mean, and maximum precipitation and the time-varying storage-capacity layer while retaining compatible static layers.
