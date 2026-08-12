# Water-level recognition and social-media text processing

## Included model artifacts

| File | Function | Interface |
|---|---|---|
| `water_level_cnn.ckpt` | Four-class water-level classification | RGB image to `(0, 10]`, `(10, 20]`, `(20, 30]`, or `(30, 40]` cm |
| `human_reference_detector.pt` | Human reference-object detection | Detector classes `head`, `all`, and `visible` |
| `wheel_water_level_cnn.ckpt` | Wheel-inundation water-level classification | RGB wheel crop to `0`, `10`, `20`, `30`, `40`, or `50` cm |

Verify all files against `SHA256SUMS.txt` before use.

## Four-class CNN

The classifier expects an RGB image resized and centre-cropped to `64 x 64`, converted to a tensor, and normalized with:

```text
mean = [0.35167608, 0.34581554, 0.35012535]
std  = [0.21065188, 0.20044526, 0.20746833]
```

The checkpoint is a PyTorch `state_dict` for a two-layer CNN. Its four logits correspond to the four increasing water-depth intervals. Loading requires the matching private architecture implementation.

## Human reference detector

`human_reference_detector.pt` contains stripped inference weights for the human-reference detector. It was trained with `512 x 512` images and the classes `head`, `all`, and `visible`. Detection results provide reference positions used with the body-part-to-water-depth mapping in the Supplementary Information.

## Wheel water-level classifier

`wheel_water_level_cnn.ckpt` is a PyTorch `state_dict` for a two-layer CNN trained on wheel images grouped into six water-level labels: `0`, `10`, `20`, `30`, `40`, and `50` cm. It expects RGB images resized and centre-cropped to `64 x 64` and uses ImageNet normalization:

```text
mean = [0.485, 0.456, 0.406]
std  = [0.229, 0.224, 0.225]
```

The six logits follow the increasing label order above. Loading requires the matching private architecture implementation.

## Social-media text processing

The text workflow reads posts from Weibo, Douyin, and Xiaohongshu and calls a compatible language-model API to extract:

- urban-pluvial-flood relevance;
- reported event time and supporting text;
- location expression, source, and supporting text;
- quantitative water depth and units;
- qualitative body-part water-depth descriptions;
- confidence and exclusion reason.

Explicit numerical depths are converted to metres. Qualitative expressions are mapped using the body-part correspondence table in the Supplementary Information. Missing information remains unknown; the workflow must not invent water depths, locations, times, or coordinates.

Location candidates are assigned a quality level based on explicit geographic evidence. User IP addresses are not used, and geocoding candidates require manual checking.

The language-model service, credentials, processing source code, raw posts, images, OCR outputs, and geocoding credentials are not included. No separate local language-model checkpoint is required.
