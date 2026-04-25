## 🚀 Getting Started on UCloud

This project is stored in `/work/Member Files/echocardiography_segmentation`. 
To ensure all dependencies for the DANARC segmentation models are met, follow these steps:

### 1. Environment Setup
If the `venv` does not exist, create it:
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

<!-- # cardiacUDC Dataset Organization
Format: All imaging data is stored in NIfTI (.nii.gz) format, the standard for medical imaging research.
Scope: The dataset includes data from at least six different medical imaging sites (Sites 20, 29, 52, 73, 100, 126), providing the "multi-source" diversity required for robust AI training.

## Directory Structure
- `Site_XXX/`: Original NIfTI volumes organized by scanning site.
- `extracted_frames/`: It contains individual 2D slices (frames) extracted from the larger site volumes.

         Naming Convention: Site_{ID}_patient{NUM}_{TYPE}_{FRAME_INDEX}.nii.gz.
         Types: Each image frame has a corresponding label frame. For example, Site_100_patient82-4_image_064.nii.gz is the ultrasound image, and its mask is Site_100_patient82-4_label_064.nii.gz.
- `label_all_frame/`: High-density annotations where all video frames are labelled.
- `dataset.csv`: Master metadata file mapping image frames to segmentation masks.
video_id: Identifies the source patient volume.

frame_num: The specific index of the frame within the original video.

frame_path: The direct path to the 2D image in extracted_frames.

mask_path: The direct path to the 2D segmentation mask in extracted_frames.

Key Insight: Notice that for a single video_id, only specific frame_num values (e.g., 12, 13, 21, 22) are listed. This aligns with the DANARC Labelling Protocol, which focuses on five key time points: End-Diastole, Systolic, End-Systole, Mid-Diastole, and the second End-Diastole.

## Data Format
Images and labels are provided in compressed NIfTI format (`.nii.gz`). 
The `dataset.csv` follows this format:
`video_id, frame_num, frame_path, mask_path`

## Annotation Protocol
The dataset is primarily annotated at key cardiac phases (ED, Systole, ES) to support the DANARC labelling protocol. The `label_all_frame` subset supports temporal-aware segmentation research (e.g., MemSAM). -->

# 🫀 cardiacUDC Dataset Organization

This dataset is designed to support the development of autonomous echocardiography systems within the **DANARC project**. It provides a multi-source collection of transthoracic echocardiography (TTE) data optimized for 2D segmentation tasks.

---

## 📊 Dataset Overview

- **Format:** All imaging data and ground-truth masks are stored in NIfTI (`.nii.gz`) format, ensuring compatibility with standard medical imaging pipelines.
- **Scope:** To ensure high model generalizability and robustness against "speckle noise," data is collected across six distinct clinical sites: **Sites 20, 29, 52, 73, 100, and 126**.

---

## 📂 Directory Structure

| Path | Description |
|------|-------------|
| `Site_XXX/` | Contains the raw, original 3D/4D NIfTI volumes categorized by clinical site. |
| `extracted_frames/` | The core training directory containing individual 2D slices extracted from the site volumes. |
| `label_all_frame/` | A high-density subset containing fully annotated videos (every frame labelled) for temporal research. |
| `dataset.csv` | The master metadata file used to programmatically map image frames to their corresponding masks. |

### Naming Convention in `extracted_frames/`

Frames are named using a strictly structured format:

```
Site_{ID}_patient{NUM}_{TYPE}_{FRAME_INDEX}.nii.gz
```

| Example | Path |
|---------|------|
| Image | `Site_100_patient82-4_image_064.nii.gz` |
| Corresponding Mask | `Site_100_patient82-4_label_064.nii.gz` |

---

## 📝 Metadata (`dataset.csv`)

The `dataset.csv` is the primary entry point for data loaders. It contains the following columns:

| Column | Description |
|--------|-------------|
| `video_id` | The unique identifier for the source patient volume. |
| `frame_num` | The specific index of the frame within the original temporal sequence. |
| `frame_path` | The absolute or relative path to the 2D image file. |
| `mask_path` | The absolute or relative path to the 2D segmentation mask. |

> [NOTE]
> **Key Insight:** Most patient entries only list specific `frame_num` values (e.g., `12`, `13`, `21`, `22`). This is intentional and follows the **DANARC Labelling Protocol**.

---

## 🛠 Annotation Protocol

The dataset follows a **dual-purpose annotation strategy** to support both static and temporal AI models:

### 1. Phase-Specific Annotations

Most data is annotated at **five key time points** within the cardiac cycle to capture critical anatomical changes:

1. **End-Diastole (ED)** — Largest LV volume
2. **Systolic frame**
3. **End-Systole (ES)** — Smallest LV volume
4. **Mid-Diastolic frame**
5. **Second ED frame** — To complete the cycle
