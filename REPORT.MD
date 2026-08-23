# **Final Report — Modeling Summary**

## **i. Insights Gained from EDA**

Our EDA revealed several important structural patterns in the Facebook Metrics dataset:

- **Page Total Likes does not predict engagement.** It showed near‑zero correlation with likes, comments, or shares.
- **Several reach and consumption metrics were redundant.**  
  - *Lifetime Post Total Reach* ≈ *Lifetime Post Total Impressions*  
  - *Lifetime Post Consumers* ≈ *Lifetime Post Consumptions*
- **Engagement metrics are naturally correlated.** Likes, comments, and shares move together, and *Total Interactions* is simply their sum.
- **Viral posts behave as outliers**, creating skew and nonlinear relationships across engagement features.

These insights guided our feature selection and modeling strategy.

---

## **ii. How We Determined Which Columns to Drop or Keep**

We used the correlation matrix to identify irrelevant or redundant features:

- **Dropped low‑value features:**  
  - *Page Total Likes* (no predictive signal)
- **Dropped highly correlated duplicates:**  
  - Kept *Lifetime Post Total Reach*, dropped *Impressions*  
  - Kept *Lifetime Post Consumers*, dropped *Consumptions*
- **Dropped Total Interactions:**  
  - It duplicated the sum of like + comment + share.

We kept features that provided **unique, meaningful information** and removed those that repeated the same signal.

---

## **iii. Hyperparameter Tuning Strategy**

We used **Random Forest Regressor** as our primary supervised model.  
To keep the modeling pipeline simple and aligned with TKH requirements, we used **default parameters** for the baseline model and did **not perform grid‑search or random‑search tuning**.

This allowed us to evaluate the dataset’s natural predictive structure before introducing complexity.

---

## **iv. Model Performance After Tuning**

Since no hyperparameter tuning was applied, the baseline Random Forest results represent the model’s natural performance:

- **MAE:** 0.193  
- **RMSE:** 0.463  
- **R²:** 0.511  

These metrics indicate that the model explains ~51% of the variance in likes and handles nonlinear relationships effectively.

---

## **v. Final F1 Score, Precision, and Recall**

Our task is **regression**, not classification, so **F1 Score, precision, and recall do not apply**.  
Instead, we evaluate performance using MAE, RMSE, and R².

---

# **Supplementary Model: Multiple Linear Regression (Interpretability Only)**

Although our primary supervised model predicts **likes** using a nonlinear algorithm (Random Forest), we included a **Multiple Linear Regression (MLR)** model as a supplementary interpretability tool. Earlier EDA showed that Facebook engagement metrics are **nonlinear, skewed, and influenced by viral outliers**, making OLS regression unsuitable as a predictive model. However, MLR still provides valuable **coefficient‑level insight** into how individual features directionally relate to engagement.

To make the regression more stable, we used **Lifetime Engaged Users** as the target instead of `like`, because it is more continuous and less zero‑inflated. This allows the model to produce interpretable coefficients even though assumptions are violated.

### **Assumption Diagnostics**
- **Multicollinearity:** VIF < 3.2 → acceptable  
- **Homoscedasticity:** Breusch‑Pagan p < 0.001 → violated  
- **Normality:** Shapiro‑Wilk p < 0.001 → violated  

These violations confirm that linear regression is **not appropriate as a predictive model**, but still useful for interpretation.

### **MLR Key Findings**
- **R²:** 0.48  
- **Reach** is the strongest driver of engagement (coef = 0.58).  
- **Status and photo posts** outperform video posts.  
- **Paid promotion** does not significantly increase engagement.  
- **Content category** has mild influence.

These findings align with earlier EDA and support the Random Forest model’s behavior.

---

# **Unsupervised Technique: K‑Means Clustering**

K‑Means revealed three natural post groups:

| **Cluster** | **Count** | **Interpretation** |
|-------------|-----------|---------------------|
| 0 | 417 | Normal posts (low reach/engagement) |
| 1 | 78 | Above‑average posts |
| 2 | 5 | Viral posts (extreme outliers) |

These clusters provide structural insight into engagement behavior and can be used as an additional feature in future supervised models.

---

# **Model Comparison Summary**

| Model | Type | Target | MAE | RMSE | R² | Key Insights |
| --- | --- | --- | --- | --- | --- | --- |
| Random Forest | Nonlinear Regression | Likes | 0.193 | 0.463 | 0.511 | Best predictive performance; handles nonlinear relationships and skew |
| Multiple Linear Regression | Linear Regression | Engaged Users | — | — | 0.48 | Useful for interpretability; assumptions violated due to viral outliers |
| K‑Means Clustering | Unsupervised | — | — | — | — | Reveals 3 natural post groups (Normal, Above Average, Viral); cluster labels can improve supervised models |

---

# **Future Work**

To expand this project, we would explore:

- **Gradient Boosting or XGBoost** for improved nonlinear regression performance.  
- **Hyperparameter tuning** (Grid Search / Random Search) for Random Forest.  
- **Log‑transformed regression models** to stabilize variance and reduce skew.  
- **Using cluster labels as features** to improve supervised model accuracy.  
- **Time‑based modeling** (post hour, weekday trends) for temporal engagement patterns.  
- **Feature engineering** such as interaction terms (e.g., Type × Paid).


