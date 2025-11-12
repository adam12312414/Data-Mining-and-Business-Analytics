# 🚦 Predicting Accident Severity Using SAS Enterprise Miner  
**Temasek Polytechnic – Data Mining and Business Analytics (CDA2C02)**  
**Assignment 1 | AY2024/2025 October Semester**

---

## 📘 Project Overview
This project investigates the development of predictive models to estimate **road accident severity** using datasets from the **National Highway Traffic Safety Administration (NHTSA)** covering **2019–2021**.  
The analysis integrates data preprocessing, feature engineering, and predictive modeling in **SAS Enterprise Miner (SAS EM)** to identify factors that most influence accident outcomes.

The target variable, **`MAXSEV_IM`**, represents the maximum severity of injury in each crash.  
The project explores how environmental, human, and mechanical variables affect severity outcomes.

---

## 🎯 Objectives
- Merge and clean multi-year accident data for comprehensive trend analysis.  
- Engineer new features to enhance predictive accuracy.  
- Identify significant predictors through statistical tests.  
- Build, evaluate, and compare **Logistic Regression**, **Decision Tree**, and **Neural Network** models.  
- Recommend strategies to improve road safety and reduce accident severity.

---

## 🧩 Data Preparation

### 1️⃣ Merging and Cleaning
- Combined **2019–2021 datasets** to ensure complete coverage of accident trends.  
- Removed redundant and inconsistent columns (`CASENUM`, `PSU_VAR`, etc.).  
- Replaced missing work-zone values in `WRK_ZONENAME` with `"None"`.  
- Dropped high-missing fields like `WEATHER1`, `CF1`, `CF2`, and `CF3` to maintain uniformity.

### 2️⃣ Feature Engineering
Created meaningful derived variables:
- **`RUSH_HOUR`** → 1 if the crash occurred during **6–9 AM** or **4–7 PM**, else 0.  
- **`INJURY_RATIO`** → `(NO_INJ_IM) / (PERMVIT + PERNOTMVIT)` to represent injury severity across all participants.

### 3️⃣ Statistical Feature Selection
| Test | Purpose | Key Findings |
|------|----------|---------------|
| **Chi-Square & ANOVA** | Identify significant relationships | Variables like `URBANICITY`, `EVENT1_IM`, and `TYP_INT` strongly associated with severity (p < 0.05). |
| **Cramér’s V** | Measure categorical correlation | `ALCHL_IM = 0.1788`, `EVENT1_IM = 0.1618` (strong predictors). |
| **VIF** | Detect multicollinearity | Kept `INJURY_RATIO` and dropped redundant numeric variables. |

✅ **Final Predictor Set:**  
`INJURY_RATIO`, `EVENT1_IM`, `ALCHL_IM`, `RUSH_HOUR`, and `TYP_INT`.

---

## 🧠 Predictive Modelling

Three machine learning algorithms were implemented using **SAS Enterprise Miner**:

| Model | Type | Description |
|--------|------|-------------|
| **Baseline Logistic Regression** | Linear | Initial model; revealed strong class imbalance. |
| **Decision Tree** | Non-linear | Tuned (leaf size 10, depth 4) to prevent overfitting. |
| **Neural Network** | Non-linear | Used standardization and hidden units for better generalization. |

### ⚖️ Addressing Class Imbalance
- **Undersampling** Class 0 (majority) by 90% using stratified sampling.  
- **Class Weighting** using a custom `ClassWeight` formula:
(MAXSEV_IM = 0)*4.96 +
(MAXSEV_IM = 1)*1.00 +
(MAXSEV_IM = 2)*1.36 +
(MAXSEV_IM = 3)*2.15 +
(MAXSEV_IM = 4)*10.62 +
(MAXSEV_IM = 5)*70.42 +
(MAXSEV_IM = 6)*7308 +
(MAXSEV_IM = 8)*769.26

This ensured minority classes were not ignored during model training.

---

## 📊 Model Evaluation

| Model | Accuracy | Precision | Recall | F1-Score | Observation |
|--------|-----------|------------|---------|-----------|--------------|
| **Baseline Logistic Regression** | 99.97% | 0% | 0% | 0% | High accuracy but only due to majority class bias. |
| **Weighted Logistic Regression** | 89.47% | 99.99% | 89.26% | 94.32% | Significant improvement after balancing. |
| **Decision Tree** | 97.04% | 100.00% | 76.32% | 86.57% | Simplified model; generalizes well. |
| **Neural Network** | 90.05% | 99.91% | 89.86% | 94.61% | Excellent fit; mild overfitting observed. |

📈 **Model Comparison (Misclassification Rate):**
- Decision Tree achieved the **lowest test misclassification rate (28.71%)**, confirming best generalization.  
- Logistic Regression and Neural Network performed well but showed possible overfitting.

---

## 🔍 Key Insights
- **Most Important Predictor:** `INJURY_RATIO` (importance score = 1.000).  
- **Moderate Predictors:** `EVENT1_IM`, `ALCHL_IM`, `TYP_INT`.  
- **Least Impactful:** `RUSH_HOUR`.  
- **Class Imbalance** remains the key modeling limitation.  

---

## 💡 Recommendations

### 🚗 Drivers
- Avoid **intoxicated or distracted driving**, key predictors of severe injury.  
- Practice safe speeds and situational awareness during intersections.

### 🏭 Manufacturers
- Enhance **airbag systems** and **anti-lock braking** to reduce impact severity.  
- Provide transparent vehicle safety ratings.

### 🚓 Law Enforcement
- Enforce stricter **DUI penalties** and monitor **high-risk intersections**.  
- Conduct regular awareness campaigns.

### 🏙️ City Planners
- Improve **road lighting**, **signage**, and **intersection design**.  
- Optimize **traffic management systems**.

### 🏥 Emergency Services
- Enhance **ambulance dispatch** in high-accident areas.  
- Equip nearby hospitals with adequate trauma facilities.

---

## 🧭 Limitations & Future Work
### Limitations
- Persistent **class imbalance** may reduce fairness in predictions.  
- Risk of **overfitting** in complex models like Neural Networks.  
- Limited scope of models tested (only 3 algorithms).

### Future Improvements
- Use **ensemble models** (Random Forest, Gradient Boosting) for robustness.  
- Apply **cross-validation** and **hyperparameter tuning**.  
- Integrate **real-time traffic and environmental data**.  
- Add **temporal trend analysis** and **dimensionality reduction (PCA)**.

---

## 🧰 Tools & Techniques
- **Software:** SAS Enterprise Miner, JupyterLab  
- **Techniques:** Encoding, feature engineering, class weighting, undersampling  
- **Statistical Methods:** Chi-Square, ANOVA, Cramér’s V, VIF  
- **Metrics Used:** Accuracy, Precision, Recall, F1-Score, Misclassification Rate  

---

## 👨‍💻 Author
**Adam Haizad Bin Mohamad Faizal**  
📍 Temasek Polytechnic – School of Informatics & IT  
🎓 Diploma in Big Data & Analytics (T60)  

---

## ⚠️ Disclaimer
> All data used are **synthetic or publicly available** for educational purposes.  
> This project was completed as part of **Temasek Polytechnic’s CDA2C02 coursework** and does **not include confidential or personal data**.


