# RGBT-UAVDet: A Dual-Modal Visible–Infrared Dataset for Anti-UAV Detection

**RGBT-UAVDet** is a dual-modal visible–infrared (RGB-T) dataset for **anti-UAV object detection**. Each sample is a spatially registered visible/infrared image pair, additionally provided with two degraded visible variants — **fog** and **low-light** — so that detection robustness can be evaluated under adverse imaging conditions and through cross-modal collaboration.

> 📦 **Release note.** To keep the review process fair, this repository currently publishes only the statistical description and the distribution analysis of the dataset. **The full dataset will be released here upon paper acceptance.**

---

## 📊 Overview

| Item | Description |
|---|---|
| Task | Anti-UAV object detection (dual-modal) |
| Modalities | Visible (RGB) + Infrared (IR), spatially registered |
| Image pairs | **4,869** (train 4,469 / test 400) |
| Versions per pair | `vis` visible, `inf` infrared, `vis_fog` fog-degraded, `vis_ll` low-light-degraded — **19,476 images** in total |
| Resolution | 640 × 640, stored as RGB |
| Classes | **1 class: UAV** |
| Annotations | **6,344** bounding boxes (train 5,944 / test 400) |
| Format | YOLO — `class x_center y_center width height`, all normalized |
| Source | Frames sampled from UAV-captured video sequences: 11 training sequences / 9 test sequences |
| Split | Per-sequence temporal split; train and test share sequences but never share frames |

## 🗂 Folder Structure

```
uav_data_ori/
├── vis/          # original visible images
│   ├── train/    #   4,469 images
│   └── test/     #     400 images
├── inf/          # infrared images (registered, same filenames as vis)
├── vis_fog/      # fog-degraded visible images
├── vis_ll/       # low-light-degraded visible images
└── labels/       # YOLO annotations (same filenames as vis)
    ├── train/    #   4,469 .txt files
    └── test/     #     400 .txt files
```

All four modality directories and the label directory share **exactly the same filenames**, so any visible–infrared pair refers to the same set of bounding boxes.

## 📈 Distribution Analysis

### Annotation distribution overview

![dataset_distribution](figures/dataset_distribution.png)

### Spatial and scale distribution

![spatial_and_scale](figures/spatial_and_scale.png)

### Dual-modal image characteristics

![modality_statistics](figures/modality_statistics.png)

## 🔍 Key Statistics

### Bounding-box statistics (normalized coordinates)

| Metric | Train | Test |
|---|---|---|
| Objects | 5,944 | 400 |
| Median width | 0.0425 | 0.0591 |
| Median height | 0.0315 | 0.0380 |
| Median aspect ratio | 1.32 | 1.45 |
| Median area | 0.00135 | 0.00229 |

### Object scale composition (by normalized area)

| Scale | Range | Train | Test |
|---|---|---|---|
| Small | < 0.0009 | 34.3% (2,036) | 23.8% (95) |
| Medium | 0.0009 – 0.0081 | 62.3% (3,702) | 67.8% (271) |
| Large | ≥ 0.0081 | 3.5% (206) | 8.5% (34) |

The dataset is **dominated by small objects**: most targets occupy only a few dozen pixels in a 640 × 640 frame, which matches the long-range imaging characteristics of real anti-UAV scenarios.

### Objects per image

| Objects / image | Train | Test |
|---|---|---|
| 1 | 3,436 (76.9%) | 400 (100%) |
| 2 | 591 (13.2%) | 0 |
| 3 | 442 (9.9%) | 0 |

### Modality imaging characteristics (600 images sampled per modality)

| Modality | Mean brightness | Brightness std | Contrast (grayscale std) |
|---|---|---|---|
| Visible `vis` | 0.587 ± 0.051 | 0.051 | **0.132** |
| Infrared `inf` | 0.126 ± 0.030 | 0.030 | 0.117 |
| Fog `vis_fog` | 0.601 ± 0.023 | 0.023 | 0.070 |
| Low-light `vis_ll` | 0.294 ± 0.025 | 0.025 | 0.066 |

Observations:

- **Infrared images are much darker** (about one fifth of the visible brightness) while retaining comparable local contrast, so UAV thermal signatures remain salient.
- **The fog variant keeps visible-like brightness but loses about 47% of the contrast**, i.e. a typical low-contrast degradation.
- **The low-light variant is roughly half as bright** as the visible original, with a similar ~50% contrast drop.
- The paired cross-modality brightness scatter shows a strong linear correlation between the degraded variants and the original visible images, which confirms that the degraded versions are derived from the same source frames and that all four versions can safely share one annotation.

### Samples per sequence

| Sequence | 001 | 002 | 004 | 005 | 006 | 007 | 008 | 009 | 010 | 011 | 014 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Train images | 344 | 282 | 110 | 320 | 89 | 323 | 310 | 175 | 470 | 719 | 1,327 |
| Train objects | 344 | 282 | 110 | 320 | 89 | 323 | 310 | 175 | 470 | 719 | 2,802 |
| Test images | 63 | 44 | 22 | 59 | 15 | 11 | 54 | 42 | 90 | — | — |

Multi-object frames concentrate in sequence **014** (1,327 frames / 2,802 objects, about 2.1 objects per frame); all other sequences consist mainly of single-object frames. Sequences **011** and **014** are not included in the test split.

## 📋 Annotation Format

Each image has a same-named `.txt` file with one line per object:

```
<class_id> <x_center> <y_center> <width> <height>
0 0.460740 0.133084 0.265209 0.144321
```

- `class_id` — class index; this dataset has only `0` (UAV).
- All coordinates are normalized to the image width and height (0–1).
- **There are no empty annotations**: every image in both splits contains at least one object.

## 📦 Obtaining the Full Dataset

This repository currently provides only the statistics and the distribution analysis.

**The full dataset (all 19,476 images and their annotations) will be published here upon paper acceptance.**

If you need early access for academic collaboration, please open a GitHub Issue.

## 📚 Citation

Please cite (BibTeX will be updated upon acceptance):

```bibtex
@misc{RGBT-UAVDet,
  title  = {RGBT-UAVDet: A Dual-Modal Visible-Infrared Dataset for Anti-UAV Detection},
  author = {Zhou},
  year   = {2026},
  url    = {https://github.com/Surprise-Zhou/RGBT-UAVDet-dataset}
}
```

## 📄 License

The dataset will be released after paper acceptance under the license announced at that time (CC BY-NC-SA 4.0 is planned).
