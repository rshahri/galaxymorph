# 🌌 GalaxyMorph  
### Deep Learning for Galaxy Morphology Classification

GalaxyMorph is an astronomy-focused machine learning project exploring galaxy morphology classification using deep convolutional neural networks and Galaxy Zoo data.

This repository follows a structured, multi-layer development plan:

- **Layer 1:** Clean, reproducible 3-class baseline (completed)
- Layer 2: Soft-label modeling (vote fraction prediction)
- Layer 3: Uncertainty & calibration analysis
- Layer 4: Robustness, domain shift, and scientific interpretability

---

# 🔭 Project Goal

Build scientifically meaningful ML models that classify galaxy morphology while maintaining:

- Reproducibility  
- Proper evaluation (macro-F1)  
- Error analysis  
- Interpretability  

This is not a leaderboard project.  
It is a structured Astro-AI engineering pipeline.

---

# 🧠 Layer 1 — Baseline Morphology Classifier

## Objective

Train a clean 3-class classifier:

- **Elliptical**
- **Spiral**
- **Irregular**

Using high-confidence Galaxy Zoo labels.

---

## 📦 Dataset

**Source:** Galaxy Zoo – The Galaxy Challenge (Kaggle competition dataset)

- Images: `images_training_rev1`
- Labels: `training_solutions_rev1.csv`
- Total samples: 61,578
- Confidence filtering: `p_max ≥ 0.7`

### 3-Class Mapping

- `Elliptical = Class1.1`
- `Spiral = Class1.2 × Class2.1`
- `Irregular = Class1.2 × Class6.1`

---

## 🏗 Model Architecture

- Backbone: **ResNet-18 (ImageNet pretrained)**
- Stage 1: Frozen backbone, trained classifier head
- Stage 2: Fine-tuned last residual block (`layer4`) + fully connected layer

**Image size:** 224×224  
**Loss:** CrossEntropy (class-weighted)  
**Optimizer:** AdamW  

---

## 📊 Performance (Layer 1)

After fine-tuning:

- **Validation macro-F1 ≈ 0.92**
- Strong diagonal confusion matrix
- Class imbalance handled via weighted loss

Test performance is reported in the notebook.

---

## 🔬 Evaluation Artifacts

Layer 1 includes:

- Stratified train/val/test splits  
- Validation and test confusion matrices  
- Classification report (precision / recall / F1)  
- “Confident mistakes” gallery for qualitative error analysis  
- Saved configuration and dataset splits for reproducibility  

---

## 🔁 Reproducibility

Layer 1 includes:

- Fixed random seed  
- Saved dataset splits  
- Training configuration in `config.json`  
- Explicit label construction logic  

This allows exact re-training of the baseline.

---

# 🚀 Next Steps (Layer 2)

Layer 2 will move beyond hard labels and predict:

- Full vote fraction distributions  
- Soft targets  
- Uncertainty calibration  

The goal is to transition from clean classification to more realistic astrophysical modeling.

---

# 🌠 Why This Project?

Galaxy morphology classification is a foundational problem in astronomy.

This project focuses on:

- Applying modern ML rigor to astronomical data  
- Understanding model behavior, not just accuracy  
- Bridging machine learning engineering with astrophysical reasoning  

---

# 👩‍🚀 Author

Reihaneh Shahri  
