# **Final Report — Modeling Summary**

## **i. Insights Gained from EDA**

Our EDA revealed several important structural patterns in the Facebook Metrics dataset:

- **Page Total Likes does not predict engagement. ** It showed little linear association with engagement. It had near zero correlation with likes, comments, or shares.
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
  - *Page Total Likes* (little linear association with the engagement outcomes/ no predictive signal)
- **Dropped highly correlated duplicates:**  
  - Kept *Lifetime Post Total Reach*, dropped *Impressions*  
  - Kept *Lifetime Post Consumers*, dropped *Consumptions*
- **Dropped Total Interactions:**  
  - It duplicated the sum of like + comment + share.

We kept features that provided **unique, meaningful information** and removed those that repeated the same signal.

---

## **iii. Baseline Modeling Strategy**

We used **Random Forest Regressor** as our primary supervised model because the dataset contained mixed feature types, skewed engagement metrics, outliers, and nonlinear relationships. The processed data was divided into an **80% training set and 20% test set** using `random_state=42`. We used the model’s default parameters to establish a baseline and did not perform grid-search or random-search tuning. 

This allowed us to evaluate the dataset’s natural predictive structure before introducing complexity.

---

## **iv. Baseline Model Performance**

Since no hyperparameter tuning was applied, the baseline Random Forest results represent the model’s natural performance. The baseline Random Forest produced the following test results:

- **MAE:** 0.193 — On average, the model’s predictions differed from the actual transformed target values by approximately 0.19 units. 
- **RMSE:** 0.464 — The higher RMSE indicates that some posts produced larger prediction errors, potentially because of unusual or viral engagement patterns. 
- **R²:** 0.511 — The model explained approximately 51% of the test-set variance in the transformed `like` target.


Because the target variable, `like` was scaled before modeling,  MAE and RMSE are expressed in transformed units rather than raw numbers of likes.
These metrics indicate that the model explains ~51% of the test-set variance in the transformed 'like' target and demonstrates moderate predictive performance in nonlinear relationships.

---

## **v. Final F1 Score, Precision, and Recall**

Our task is **regression**, not classification, so **F1 Score, precision, and recall do not apply**.  
Instead, we evaluate performance using MAE, RMSE, and R².

---

# **Supplementary Model: Multiple Linear Regression (Interpretability Only)**

Although our primary supervised model predicts **like** using a nonlinear algorithm (Random Forest), we included a **Multiple Linear Regression (MLR)** model as a supplementary interpretability tool. Earlier EDA showed that Facebook engagement metrics are **nonlinear, skewed, and influenced by viral outliers**, making MLR less suitable as a predictive model. However, MLR still provides valuable coefficient‑level insight into how individual features directionally relate to engagement.

To make the regression more stable, we used **Lifetime Engaged Users** as the target instead of `like`, because it is more continuous and less zero‑inflated. This allows the model to produce interpretable coefficients even though assumptions are violated.

### **Assumption Diagnostics**
- **Multicollinearity:** VIF < 3.2 → acceptable  
- **Homoscedasticity:** Breusch‑Pagan p < 0.001 → violated  
- **Normality:** Shapiro‑Wilk p < 0.001 → violated  

The diagnostics showed acceptable multicollinearity levels, but the assumptions of constant residual variance and normally distributed residuals were not met. These violations make MLR less suitable as the project’s primary predictive model and limit the reliability of standard OLS inference.

Accordingly, the coefficients should be interpreted as exploratory associations rather than causal effects. 

### **MLR Key Findings**
- **R²:** 0.48
- **Reach** had a statistically significant positive association with engagement (coef = 0.58).
- **Status and photo posts** had statistically significant positive associations relative to the reference post type.
- **Video posts, paid promotion, and content categories 2 and 3** were not statistically significant.

These results provide an exploratory, interpretable view of how selected post characteristics relate to Lifetime Engaged Users. These findings align with earlier EDA and support the Random Forest model’s behavior.

---

# **Unsupervised Technique: K‑Means Clustering**

Using a three-cluster solution, K-Means segmented the posts into three performance groups:

| **Cluster** | **Count** | **Interpretation** |
|-------------|-----------|---------------------|
| 0 | 417 | Normal posts (low reach/engagement) |
| 1 | 78 | Above‑average posts |
| 2 | 5 | Viral posts (extreme outliers) |

These clusters provide structural insight into engagement behavior and can be used as an additional feature in a future supervised pipeline.

---

# **Model Comparison Summary**

| Model | Type | Target | MAE | RMSE | R² | Key Insight |
|---|---|---|---:|---:|---:|---|
| Random Forest | Nonlinear regression | Scaled likes | 0.193 | 0.463 | 0.511 | Primary predictive model; captured nonlinear relationships |
| Multiple Linear Regression | Linear regression | Lifetime Engaged Users | — | — | 0.480 | Supplementary interpretability model; assumption violations require cautious interpretation |
| K-Means | Unsupervised clustering | — | — | — | — | Segmented posts into normal, above-average, and viral performance groups |
---

# **Future Work**

To expand this project, we would explore:

- **Gradient Boosting or XGBoost** for improved nonlinear regression performance.  
- **Hyperparameter tuning** (Grid Search / Random Search) for Random Forest.  
- **Log‑transformed regression models** to stabilize variance and reduce skew.  
- **Using cluster labels as features** to improve supervised model accuracy.  
- **Time‑based modeling** (post hour, weekday trends) for temporal engagement patterns.  
- **Feature engineering** such as interaction terms (e.g., Type × Paid).


