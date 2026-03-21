# 🌌 GalaxyMorph  
### Deep Learning for Galaxy Morphology Classification

GalaxyMorph is an astronomy-focused machine learning project exploring galaxy morphology classification using deep convolutional neural networks and Galaxy Zoo data.

This repository follows a structured, multi-layer development plan:

- **Layer 1:** Clean, reproducible 3-class baseline (completed)  
- **Layer 2:** Soft-label modeling and uncertainty analysis (completed)  
- Layer 3: Calibration and uncertainty reliability (next)  
- Layer 4: Robustness, domain shift, and scientific interpretability  

---

# 🔭 Project Goal

Build scientifically meaningful ML models that classify galaxy morphology while maintaining:

- Reproducibility  
- Proper evaluation (macro-F1, KL divergence)  
- Error analysis  
- Uncertainty awareness  

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

- Source: Galaxy Zoo – The Galaxy Challenge (Kaggle)
- Images: `images_training_rev1`
- Labels: `training_solutions_rev1.csv`
- ~61k galaxies
- Confidence filtering: `p_max ≥ 0.7`

### Label Mapping

- `Elliptical = Class1.1`
- `Spiral = Class1.2 × Class2.1`
- `Irregular = Class1.2 × Class6.1`

---

## 🏗 Model

- ResNet-18 (ImageNet pretrained)
- Stage 1: frozen backbone
- Stage 2: fine-tuned layer4 + FC
- Loss: CrossEntropy (class-weighted)

---

## 📊 Results

- Validation macro-F1 ≈ **0.92**
- Strong confusion matrix
- Stable training behavior

---

## 🔬 Artifacts

- Train/val/test splits  
- Confusion matrix  
- Classification report  
- Confident mistakes gallery  
- Config + reproducibility setup  

---

# 🌌 Layer 2 — Soft Label Modeling (Uncertainty-Aware)

## Conceptual Shift

Layer 1 treated galaxy morphology as a **hard classification problem**.

Layer 2 models it as a **probabilistic problem**, using the full vote fraction distribution from Galaxy Zoo.

Instead of predicting:
``elliptical``

we predict:
``[0.55, 0.40, 0.05]``


This preserves human disagreement and captures morphological ambiguity.

---

## 🎯 Objective

Learn the **distribution of morphology probabilities**:

- p_elliptical  
- p_spiral  
- p_irregular  

Rather than collapsing them into a single class.

---

## 🏗 Model Changes

- Same architecture: **ResNet-18**
- Output: 3 logits → softmax probabilities
- Loss: **KL Divergence**

Training strategy:

- Stage 1: train classifier head  
- Stage 2: fine-tune layer4 + FC  

---

## 📊 Evaluation Metrics

Layer 2 introduces distribution-aware evaluation:

- **KL Divergence (primary)** → how close predicted distribution is to human votes  
- **MSE (secondary)** → numerical distance between probabilities  
- **Macro-F1 (argmax)** → comparison with Layer 1  

---

## 🔬 Uncertainty Analysis

### Entropy

We compute entropy of:

- Human vote distributions  
- Model predictions  

This measures **how ambiguous a galaxy is**.

Key insight:

- Low entropy → clear morphology  
- High entropy → ambiguous / transitional galaxy  

The model’s entropy is compared to human entropy to assess whether it learns **where uncertainty exists**.

---

## 🖼️ Disagreement Galleries

We visualize:

### 1. High human disagreement
Galaxies where volunteers strongly disagreed  
→ often mergers, faint structures, edge cases  

### 2. Model overconfidence cases
Galaxies where:
- humans are uncertain  
- model is confident  

These reveal:
- potential model shortcuts  
- difficult morphological structures  
- scientifically interesting edge cases  

---

## 📊 Results Summary

- Stable KL training with clear improvement after fine-tuning  
- Model learns smooth probability distributions  
- Entropy patterns align with human uncertainty (dataset-dependent)  
- Argmax macro-F1 remains strong but slightly lower than Layer 1 (expected)

---

## 🧠 What Layer 2 Adds

Layer 1 answered:
> What class is this galaxy?

Layer 2 answers:
> How uncertain is its morphology?

This transforms the project from **classification** to **uncertainty-aware modeling**, closer to real astrophysical data interpretation.

---

# 🚀 Next Step — Layer 3

Layer 3 will focus on **calibration and reliability**:

- Is the model’s confidence trustworthy?  
- When it predicts 80%, is it correct ~80% of the time?  
- Can we improve calibration?  

---

# 🌠 Why This Project?

Galaxy morphology is not binary truth.

This project focuses on:

- Modeling human disagreement  
- Understanding uncertainty  
- Bridging ML engineering with astrophysical reasoning  

---

# 👩‍🚀 Author

Reihaneh Shahri  
MSc Artificial Intelligence — Astro-AI Focus  
