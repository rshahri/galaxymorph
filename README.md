# 🌌 GalaxyMorph  
### Deep Learning for Galaxy Morphology Classification

GalaxyMorph is an astronomy-focused machine learning project exploring galaxy morphology classification using deep convolutional neural networks and Galaxy Zoo data.

This repository follows a structured, multi-layer development plan:

- **Layer 1:** Clean, reproducible 3-class baseline (completed)  
- **Layer 2:** Soft-label modeling and uncertainty analysis (completed)  
- **Layer 3:** Grad-CAM interpretability and model inspection (completed)  
- **Layer 4:** Robustness analysis and mitigation (completed)  

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

## 🎯 Objective

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

## 🧠 Conceptual Shift

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

- **KL Divergence (primary)** → distribution similarity  
- **MSE (secondary)** → probability distance  
- **Macro-F1 (argmax)** → comparison with Layer 1  

---

## 🔬 Uncertainty Analysis

### Entropy

We compute entropy of:

- Human vote distributions  
- Model predictions  

This measures **how ambiguous a galaxy is**.

- Low entropy → clear morphology  
- High entropy → ambiguous / transitional galaxy  

---

## 🖼️ Disagreement Galleries

We visualize:

### 1. High human disagreement  
Galaxies where volunteers strongly disagreed  
→ mergers, faint structures, edge cases  

### 2. Model overconfidence cases  
Galaxies where:

- humans are uncertain  
- model is confident  

These reveal:
- potential shortcuts  
- difficult morphologies  
- scientifically interesting cases  

---

## 📊 Results Summary

- Stable KL training with clear improvement after fine-tuning  
- Smooth probability predictions  
- Entropy aligns with human uncertainty  
- Slight drop in macro-F1 (expected)

---

## 🧠 What Layer 2 Adds

Layer 1 answered:

> What class is this galaxy?

Layer 2 answers:

> How uncertain is its morphology?

---

# 🔥 Layer 3 — Grad-CAM Interpretability

## 🎯 Objective

Understand **why** the model predicts a galaxy as:

- Elliptical  
- Spiral  
- Irregular  

using **Grad-CAM visual explanations**.

---

## 🧠 Concept

Verify whether the model focuses on **meaningful astrophysical structures**:

- spiral arms  
- central bulges  
- asymmetric regions  

instead of artifacts.

---

## 🏗 Method

- Model: Layer 1 fine-tuned ResNet-18  
- Data: test split  
- Layer: `layer4`  
- Technique: Grad-CAM heatmaps  

---

## 🔬 Analysis

### ✅ Correct Predictions

- Spirals → attention on arms  
- Ellipticals → central brightness  
- Irregulars → diffuse structure  

### ❌ Failure Cases

- Missed faint spiral arms  
- Confusion with compact galaxies  
- Over-focus on bulge  

---

## 🧠 What Layer 3 Adds

Layer 3 answers:

> What visual evidence is the model using?

This introduces **interpretability and trust**.

---

# 🛡️ Layer 4 — Robustness & Reliability

## 🎯 Objective

Evaluate how stable the model is under **realistic image degradations**:

- noise  
- blur  
- brightness shifts  

Then improve robustness using **data augmentation**.

---

## 🧪 Perturbations Simulated

- **Gaussian noise** → sensor noise  
- **Gaussian blur** → low resolution / seeing  
- **Brightness shifts** → exposure variation  

---

## 📊 Baseline Model Behavior

Performance drop from clean:

- Noise → **large drop (~ -0.19)**  
- Blur → **moderate drop (~ -0.12)**  
- Brightness → **minimal effect (~ 0.00)**  

👉 The model is highly sensitive to noise and somewhat sensitive to blur.

---

## 🛠️ Mitigation Strategy

Train a robustness-aware model with:

- blur augmentation  
- brightness variation  
- geometric augmentation  

---

## 📈 Robust Model Results

Performance drop from clean:

- Noise → still high (~ -0.20)  
- Blur → **significantly improved (~ -0.05)**  
- Brightness → stable  

---

## 🔬 Key Findings

- Model relies on **fine pixel-level features** → sensitive to noise  
- Learns **larger-scale structure** with augmentation → improved blur robustness  
- Already robust to brightness due to normalization + pretrained features  

---

## 🧠 What Layer 4 Adds

Layer 4 answers:

> How reliable is the model under real-world conditions?

This introduces:

- robustness evaluation  
- stress testing  
- mitigation strategies  

---

# 🌠 Why This Project?

Galaxy morphology is not binary truth.

This project focuses on:

- modeling human disagreement  
- understanding uncertainty  
- explaining model decisions  
- ensuring robustness  

---

# 👩‍🚀 Author

Reihaneh Shahri
