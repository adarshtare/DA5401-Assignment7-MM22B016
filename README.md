# DA5401-Assignment7-MM22B016
# 🛰️ Satellite Land‑Cover Classification (Multiclass ROC–PRC Analysis)

## 🧩 Overview
This project benchmarks multiple classifiers on a multi‑class **Landsat** dataset and compares them using **Weighted F1**, **ROC–AUC**, and **Precision–Recall Average Precision (PRC–AP)**. The goal is to identify the most reliable model for land‑cover classification and to visualize performance with OvR ROC and PR curves.

---

## 📂 Folder Structure
```
├── DA5401_Assignment7_MM22B016.ipynb    # Main Jupyter Notebook
├── sat.trn                     # Training dataset
├── sat.tst                     # Test dataset
├── README.md                   # This file
```
---

## 🎯 Goals
- Load and preprocess the satellite dataset ✅  
- Train and evaluate multiple models (baseline + advanced) 🧠  
- Compute **ROC–AUC** and **PRC–AP** for the multiclass setting 📊  
- Visualize **OvR ROC** and **OvR PRC** for all models 🎨  
- Compare metrics and select the final model 🏆  

---

## 🧮 Dataset
- **Files:** `sat.trn` (train), `sat.tst` (test)  
- **Features:** Numerical reflectance bands from Landsat imagery  
- **Labels:** 6 land‑cover classes (multiclass)  
- **Size:** ~4.4K samples (train + test)  

---

## ⚙️ Preprocessing
- Read `.trn` / `.tst` via **pandas**  
- Split into features and labels  
- Scale features with **StandardScaler** (for distance‑based methods)  
- Encode targets with **LabelEncoder**  
- Use **One‑vs‑Rest (OvR)** setup for ROC/PRC computation (macro averaging)

---

## 🧠 Models
**Baselines**
- 🔍 K‑Nearest Neighbors (KNN)  
- ⚙️ Support Vector Classifier (SVC)  
- 📈 Logistic Regression  
- 🌳 Decision Tree  
- 🧠 Gaussian Naive Bayes  
- 🎲 Dummy (Prior) — frequency‑based baseline

**Advanced**
- 🌲 Random Forest  
- 🚀 XGBoost (multi:softprob)

---

## 📊 Evaluation Metrics
- **Accuracy** — overall correctness  
- **Weighted F1** — class‑frequency‑weighted precision/recall balance  
- **ROC–AUC (macro, OvR)** — global separability averaged across classes  
- **PRC–AP (macro)** — average precision focused on the positive class

> **Note:** ROC–AUC evaluates both positive and negative classes; PRC–AP is more informative for minority class detection.

---

## 📈 Results (Screenshots)

**Advanced Models**
![Advanced models metrics](/mnt/data/3114e0d6-49dd-4b0a-90a7-bd861abb35de.png)

**All Baselines (AUC & AP)**
![Baselines metrics AUC/AP](/mnt/data/bd6519da-183e-483a-b7b2-d3acd3757d87.png)

**Accuracy & Weighted‑F1 (Baselines)**
![Baselines accuracy & F1](/mnt/data/c716ec32-71f6-42e1-8a54-3f070777ab49.png)

---

## 🧭 ROC–AUC Analysis (Macro, OvR)
- **SVC** attains the highest AUC ≈ **0.985**, indicating excellent global class separability.  
- **KNN** is close (AUC ≈ **0.979**), implying strong overall discrimination as well.  
- **Logistic Regression** is solid (AUC ≈ **0.976**); **GaussianNB** and **Decision Tree** are moderate.  
- **Dummy (Prior)** sits near **0.500**, i.e., random‑chance behavior.

---

## 📉 Precision–Recall Analysis (Macro AP)
- **KNN** records the highest **PRC–AP = 0.922**, closely followed by **SVC = 0.918**.  
- **Logistic Regression (0.871)** and **GaussianNB (0.811)** perform reasonably.  
- **Decision Tree (0.737)** drops in precision at higher recall.  
- **Dummy (0.167)** behaves near random.

---

## 🧪 “Bad Model” Stress Test (Flipped Labels)
Training **Logistic Regression** on **shuffled labels** produces **AUC < 0.5** — **worse than random guessing**.  
The model learns **inverse relationships** between features and classes, so its **ROC curve lies below the diagonal**: pushing for more true positives actually increases **false positives** faster — clear evidence of **misdirected learning**.

---

## 🧷 Synthesis
Across the three core metrics — **Weighted F1**, **ROC–AUC**, and **PRC–AP** — **KNN**, **SVC**, and **Logistic Regression** consistently lead, while **Decision Tree** and **GaussianNB** are moderate. The **Dummy (Prior)** serves as a minimal baseline.

Although rankings differ by metric, **KNN** achieves the **highest Weighted F1 (0.904)** and **PRC–AP (0.922)**, demonstrating strong precision–recall balance and well‑distributed class performance. **SVC**, while slightly behind on those two, secures the **best ROC–AUC (0.985)**, reflecting excellent global separability. The small AUC gap (0.985 vs 0.979) suggests both models discriminate well overall, but KNN’s superior PRC–AP indicates better identification of positive/minority classes.

**Logistic Regression** performs reasonably (AUC = **0.976**) but shows lower F1 (**0.830**) and PRC–AP (**0.871**), consistent with underfitting from linear decision boundaries. **GaussianNB** and **Decision Tree** trail due to independence assumptions and high variance, respectively.

**Summary:** **ROC–AUC** captures global separability; **PRC–AP** and **F1** emphasize precision–recall effectiveness — particularly important under imbalance. **KNN** offers the most balanced trade‑off of precision, recall, and separability.

---

## ✅ Recommendation
The **K‑Nearest Neighbors (KNN)** model is the **most balanced and practically effective** choice. It achieves the **best F1 and PRC–AP**, delivering reliable precision and recall across all land‑cover categories, including minority classes. Although **SVC** has a slight ROC–AUC edge, the difference is marginal and is outweighed by KNN’s precision–recall stability.

**Reasons to prefer KNN**
- **Superior precision–recall balance:** Top **F1** and **PRC–AP** even in overlapping/complex regions.  
- **Comparable global separability:** ROC–AUC nearly matches SVC’s, with minimal loss in discrimination.  
- **Non‑parametric adaptability:** Captures local, non‑linear structure common in remote‑sensing data without kernel tuning.

**Conclusion:** While both **SVC** and **KNN** perform exceptionally well, **KNN** provides a slightly better equilibrium between precision, recall, and interpretability — making it the preferred model for this **multi‑class land‑cover classification** task.

---
