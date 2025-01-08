# 🏥 Prediction of Transplant Survival Rates for Patients Undergoing Allogeneic Hematopoietic Cell Transplantation (HCT)

## 📌 Overview
This Kaggle competition aims to develop **survival predictive models** to improve the prediction of transplant survival for patients undergoing **allogeneic Hematopoietic Cell Transplantation (HCT).**

## 🛠 Tech Stack
- **Programming Language:** Python  
- **Libraries & Frameworks:**
  - Pandas
  - NumPy
  - Scikit-Learn
  - Lifelines *(Cox-Proportional Hazard Model)*
  - XGBoost
  - LightGBM
  - StatsModel

## 📊 Dataset Description
The dataset is provided by the **Center for International Blood and Marrow Transplant Research (CIBMTR)** and includes:

- **Demographic Information:** Age, Gender, Race, Ethnicity  
- **Clinical Variables:** Disease Type, Stage, Prior Treatments  
- **Transplant Information:** Donor Type, Graft Source, Conditioning Regimen  
- **Outcome Data:** Event-Free Survival, Time to Event  

---

## 🔎 Project Workflow

### 📌 1. Installing Libraries and Loading Dataset

---

### 📌 2. Analyzing and Preprocessing Data

#### 🏷 Handling Categorical Variables
- **One-Hot Encoding:** Applied when no natural hierarchy relationship existed.  
- **Label Encoding:** Applied when a natural hierarchy relationship existed.  

#### ❓ Handling Missing Values
- **Used LightGBM to impute missing values.**  
- **Categorical Data:** Replaced with its **mode**.  
- **Numerical Data:** Replaced with its **mean**.  

#### 🔄 Addressing Multicollinearity
- **Calculated** **Variable Inflation Factor (VIF)** for all independent variables.  
- **Iteratively removed variables** with the highest VIF until all remaining variables had **VIF < 4**.  

#### ⚖ Scaling
- Used **MinMaxScaler** to scale values **between 0 and 1**.  

---

### 📌 3. Predictive Analysis

#### 🎯 XGBoost Predictive Model
- Used **KaplanMeierFitter** to fit dependent variables `efs` and `efs_time` into a single **y variable**.  
- Trained **XGBRegressor** with:
  - `n_estimators=100`
  - `learning_rate=0.1`
  - `max_depth=6`
  - `objective="reg:squarederror"`
- **Train/Test Split:** 90% Train / 10% Test  
- **Evaluation Metric:** **Concordance Index (C-Index)**  
- **Outcome:** The model predicts **survival probabilities** of patients.  

#### 🏥 Survival Analysis using Cox Proportional Hazard Model
- Used **Lifelines** library to build the **Cox-Proportional Hazard Model**.
- **Key Model Components:**
  - `efs_time` ➝ **Represents duration of hazard**
  - `efs` ➝ **Indicates hazard event occurrence**
- **Optimization:**
  - Used **K-fold cross-validation** to find the **optimal penalizer**.  
  - Removed independent variables **with hazard ratio ≈ 1** to improve model performance.  

#### 🔄 Combination of XGBoost & Cox-Proportional Hazard Model
- Evaluated performance using the **Concordance Index**.

---

## 📌 Future Enhancements
- Implement **Hyperparameter Tuning** to optimize model performance.  
- Explore **Deep Learning techniques** for survival analysis.  
- Perform **Feature Engineering** to extract more informative variables.  

---
## 🚀 How to Run the Project

### 1️⃣ Clone the Repository

### 2️⃣ Install Dependencies

### 3️⃣ Download and Place the Dataset

### 4️⃣ Run the .ipynb file 

---

## 🔍 **Modeling Approach & Evaluation**

### **1️⃣ XGBoost Predictive Model**
- **Kaplan-Meier Estimator** transformed survival time (`efs_time`) and event status (`efs`) into a **single survival probability target (`y`)**.
- Model trained using **`XGBRegressor`**, evaluated with **Concordance Index (C-Index)**.

### **2️⃣ Cox Proportional Hazards Model**
- Used **Lifelines** library for survival analysis.
- Applied **K-Fold Cross-Validation** to optimize the penalizer.
- Identified **most influential features** affecting survival.

### **3️⃣ Model Ensembling**
- Combined **XGBoost and Cox models** using weighted averaging.

### **📈 Performance Metrics (C-Index)**
- **XGBoost Model:** `0.3625`
- **Cox Model:** `0.3620`
- **Final Ensemble Model:** `0.3592`

## 🔬 Feature Importance Analysis for Cox Model

### **Top 5 Features Increasing Risk (Hazard Ratio > 1)**
1. **comorbidity_score** → **2.21**
2. **prod_type** → **1.42**
3. **cyto_score_Poor** → **1.22**
4. **prim_disease_hct_AML** → **1.05**
5. **prim_disease_hct_IEA** → **1.08**

### **Top 5 Features Decreasing Risk (Hazard Ratio < 1)**
1. **prim_disease_hct_HD** → **0.15**
2. **hepatic_mild** → **0.95**
3. **dri_score_Intermediate** → **0.82**
4. **gvhd_proph_Cyclophosphamide treatments** → **0.81**
5. **race_group_More than one race** → **0.90**

## 🏁 **Closing Statement**
The results indicate **similar performance across XGBoost, Cox, and ensemble models**, with **no significant gain from ensembling**.  
Key insights from **feature importance analysis** suggest that **comorbidities, disease type, and cytogenetic risk scores** play a crucial role in survival prediction.  

