# 🏗️ Construction Site Safety Monitoring — YOLOv8 Object Detection

> Real-time detection of workers, PPE compliance, machinery, and safety barriers on construction sites using YOLOv8.

---

## 📌 AECO Problem Statement

Construction sites are among the most hazardous work environments. Falls, struck-by incidents, and PPE non-compliance account for a significant portion of workplace injuries and fatalities in the Architecture, Engineering, Construction, and Operations (AECO) sector.

**Goal:** Develop an automated visual monitoring system that detects key safety-related objects on construction sites — enabling real-time alerts for PPE violations and unsafe conditions.

**Success Criteria:**
- mAP@50 ≥ 0.60 across all target classes
- Precision ≥ 0.70 for critical safety classes (e.g., no-helmet, no-vest)
- Real-time inference capability (≥ 15 FPS on standard GPU)
- Reproducible training pipeline runnable in Google Colab

---

## 🏷️ Class List & Label Rules

| # | Class Name | Description | Label Rule |
|---|-----------|-------------|------------|
| 0 | `worker` | Any person visible on site | Bounding box around full body |
| 1 | `helmet` | Safety helmet/hard hat worn | Box around helmet on head |
| 2 | `no-helmet` | Worker without helmet | Box around head area |
| 3 | `vest` | High-visibility safety vest worn | Box around torso with vest |
| 4 | `no-vest` | Worker without safety vest | Box around torso without vest |
| 5 | `machinery` | Heavy equipment (cranes, excavators, etc.) | Box around full machine |
| 6 | `vehicle` | Site vehicles (trucks, forklifts) | Box around full vehicle |
| 7 | `safety-barrier` | Barricades, cones, fencing | Box around barrier element |

> **Note:** Adjust class names and IDs to match your specific Roboflow dataset export. The above is a representative schema.

**Labeling Guidelines:**
- Minimum bounding box size: 20×20 pixels
- Occluded objects (>50% hidden): do not label
- Overlapping PPE: label both the PPE item and the compliance status separately

---

## 📊 Dataset

- **Source:** Stored directly in this repository under `images dataset/`
- **Format:** YOLOv8 (YOLO-formatted `.txt` annotation files)
- **Split:** 80% Train / 20% Validation
- **Dataset Version:** `v1.0`
- **Total Images:** ~X,XXX _(update with actual count)_
- **Access:** The notebooks clone the repo automatically — no Roboflow account or API key required.

---

## 🔁 How to Reproduce (Colab Steps)

### Quick Start
1. Open the training notebook:  
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPO/blob/main/notebooks/01_training_eval.ipynb)

2. In Colab: **Runtime → Change runtime type → GPU (T4)**

3. **Runtime → Restart runtime → Run all**

4. The notebook will:
   - Clone the repository and access the dataset from `images dataset/`
   - Install dependencies (`ultralytics`)
   - Train YOLOv8 for ≥30 epochs (or load pre-trained weights)
   - Display metrics (P/R/mAP) and training curves
   - Run inference on validation + new images

### Manual Steps
```bash
# 1. Clone the repo (dataset included)
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO

# 2. Install dependencies
pip install ultralytics==8.2.0

# 3. Train
yolo detect train model=yolov8n.pt data="images dataset/data.yaml" epochs=50 imgsz=640 batch=16

# 4. Evaluate
yolo detect val model=runs/detect/train/weights/best.pt data="images dataset/data.yaml"

# 5. Inference
yolo detect predict model=runs/detect/train/weights/best.pt source=path/to/images
```

---

## 📈 Results Summary

| Metric | Value |
|--------|-------|
| **Precision** | 0.XX |
| **Recall** | 0.XX |
| **mAP@50** | 0.XX |
| **mAP@50-95** | 0.XX |
| **Epochs** | 50 |
| **Best Epoch** | XX |

> _(Update with your actual training results)_

### Key Takeaways
1. **Helmet/No-Helmet detection** achieved the highest mAP, likely due to clear visual features and consistent labeling.
2. **Safety barriers** showed lower recall — many barriers are partially occluded or blend with the background.
3. **Small object detection** (distant workers) remains challenging; higher `imgsz` or tiling strategies could improve this.

### Training Curves
See [`/results/`](./results/) for:
- `confusion_matrix.png`
- `results.png` (P/R/mAP curves over epochs)
- `F1_curve.png`
- `PR_curve.png`

---

## ✅ Reproducibility Checklist

- [ ] **Dataset:** Stored in `images dataset/` within the repository
- [ ] **Model variant:** `yolov8n` (YOLOv8 Nano)
- [ ] **Epochs:** 50
- [ ] **Batch size:** 16
- [ ] **Image size:** 640×640
- [ ] **Ultralytics version:** `8.2.0`
- [ ] **Python version:** 3.10+
- [ ] **Pip freeze snippet:**
  ```
  ultralytics==8.2.0
  torch==2.1.0+cu121
  ```

---

## 🔬 Reproducibility Proof

| Item | Detail |
|------|--------|
| **Last successful run** | YYYY-MM-DD HH:MM UTC |
| **Runtime** | Google Colab (free tier) |
| **GPU** | NVIDIA T4 (15GB VRAM) |
| **Expected runtime** | ~25–40 min for 50 epochs |
| **Verification strategy** | Full training run; if GPU unavailable, 5-epoch verification run + load saved `best.pt` weights |

> If Colab GPU is unavailable, the notebook automatically falls back to loading pre-trained weights from the GitHub Release for inference demonstrations.

---

## 📂 Repository Structure

```
├── README.md                          # This file
├── LICENSE                            # MIT License
├── images dataset/
│   ├── data.yaml                      # Dataset config (paths + class names)
│   ├── train/
│   │   ├── images/                    # Training images
│   │   └── labels/                    # Training labels (.txt YOLO format)
│   └── valid/
│       ├── images/                    # Validation images
│       └── labels/                    # Validation labels
├── notebooks/
│   ├── 01_training_eval.ipynb         # Full training + evaluation pipeline
│   └── 02_baseline_inference.ipynb    # Baseline inference + SAM exploration
├── docs/
│   ├── problem_statement.md           # AECO problem framing
│   ├── class_definitions.md           # Detailed class list & labeling rules
│   ├── error_analysis.md              # False positives/negatives analysis
│   └── governance_checklist.md        # Privacy, consent, licensing, risk
├── results/
│   ├── confusion_matrix.png           # Confusion matrix
│   ├── results.png                    # Training curves
│   ├── F1_curve.png                   # F1 score curve
│   ├── PR_curve.png                   # Precision-Recall curve
│   └── evidence/
│       ├── annotation_examples/       # 3–5 annotation screenshots
│       ├── val_predictions/           # 10 validation prediction screenshots
│       └── new_predictions/           # 5 new-image prediction screenshots
└── reports/
    ├── slides.pdf                     # 6–8 slide presentation
    └── mini_report.pdf                # 2-page executive summary
```

---

## 🔗 Weights

Pre-trained weights are available via GitHub Releases:  
📥 **[Download best.pt](https://github.com/YOUR_USERNAME/YOUR_REPO/releases/tag/v1.0)**

---

## 📄 Documentation

- [Problem Statement](./docs/problem_statement.md)
- [Class Definitions](./docs/class_definitions.md)
- [Error Analysis](./docs/error_analysis.md)
- [Governance Checklist](./docs/governance_checklist.md)

---

## 📑 Reports

- [Presentation Slides (PDF)](./reports/slides.pdf) — 6–8 slides
- [Mini Report (PDF)](./reports/mini_report.pdf) — 2-page executive summary

---

## 📜 License

This project is licensed under the **MIT License** — see [LICENSE](./LICENSE) for details.

**Dataset Rights:** The dataset is stored directly in this repository under `images dataset/`. Refer to the original source for specific usage terms.

---

## 🙏 Acknowledgments

- [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics)
- [Roboflow](https://roboflow.com/) for dataset hosting and annotation tools
- [Segment Anything Model (SAM)](https://segment-anything.com/) for exploration experiments
- Google Colab for free GPU access
