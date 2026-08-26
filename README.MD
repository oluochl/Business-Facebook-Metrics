## Facebook Engagement Prediction
*A machine learning project analyzing what drives user engagement on Facebook posts.*

## Problem / Business Question
Which post characteristics and exposure metrics most strongly predict user engagement (likes, comments, shares), and how can a brand optimize its posting strategy to increase interactions?

## Intended Audience
Digital marketing teams, social media strategists, and content managers who need data-driven guidance on improving engagement.

## Dataset & Source
- **Data Set**: Facebook Metrics Dataset (19 post level features)
- **Size**: 500+ posts
- **Scope**: Post metadata, reach and impression metrics, and engagement outcomes
- **Full variable description**: See README → Appendix Section

## High-Level Approach
- Exploratory Data Analysis
- Data Cleaning & Preprocessing 
- Unsupervised Modeling (K-Means Clustering)
- Supervised Modeling to Predict Engagement
- Tableau Dashboard to Summarize Insights

## Key Findings 
- Page Total Likes had almost no correlation with Engagement
- Several Reach/ Impression metrics were highly redundant and removed
- Engagement is most strongly driven by post type, posting time, and reach/impression metrics
- The final model (Random Forest) explained `51% of the variance in likes; MLR provided coefficient level interpretability (details in Final Report)

## Tableau Dashboard
The Tableau dashboard provides a visual summary of key engagement patterns identified throughout our analysis. See the Final Report for detailed findings and connections to our modeling results.

<iframe src="https://public.tableau.com/app/profile/oluoch.lavenda/viz/BusinessFacebookMetricsDashboard08_24_2026/FacebookDashboard?publish=yes" width="800" height="600"></iframe>

## Recommendations
- Use impressions based metrics to monitor content performance (much stronger predictor than Page Total Likes or Lifetime Post Total Impressions)
- Prioritize content formats that historically drive higher engagement (status + photo posts)
- Optimize posting time based on weekday/hour patterns 
- Paid promotion does not reliably increase enegagement - focus on organic drivers instead 

## Limitations and Next Steps
- Viral posts behave as outliers and introduce skew/nonlinearity
- Engagement definitions are platform-specific (not the uniform across platforms)
- *Future work:*
    - Gradient Boosting/ XG Boost for improved nonlinear modeling
    - Use cluster labels as features in supervised models 
    - Hyperparameter tuning for Random Forest
    - Log- transformed regression models to stabalize variance (in instances similar to MLR)
    See  **[Full Project Report](REPORT.MD)** for more details 

## Team Members & Contributions
Team Member 	Contributions

| Team Member | Contributions |
|---|---|
| **Lavenda** | Led exploratory data analysis, correlation analysis, and feature reduction; contributed to documentation and repository organization, MLR Modeling, initial README design and co-authored the final report; and produced Tableau Dashboard design. |
| **Sal** | Led preprocessing decisions and contributed to correlation analysis and feature reduction; supported repository organization and formatting, authored the revised README and co-authored the final report; and contributed to the Tableau Dashboard Design. |
| **Theo** | Led model development, including supervised modeling, tuning, and evaluation; contributed to final report development; and co-authored the README. |
| **Catherine** | Contributed to repository organization and Tableau dashboard design, analysis, and insights. |

## Final Report
Review the complete methodology, model evaluation, assumption testing, findings, and future opportunities in the **[Full Project Report](REPORT.MD)**.

## Appendix 
The 19 columns in the data are divided into three functional categories: 
Post Metadata 
Reach & Impression Metrics
Engagement & Interaction Metrics

### 1. Post Metadata (Input & Context Features)

Variables describing the post characteristics, published time, and promotion status:
Page total likes (Integer): Total likes of the Facebook page at the time the post was published.

**Type (Categorical)**: Format of the post content (Photo, Status, Link, or Video).

**Category(Categorical/Integer)**: Content classification assigned to the post (1: Action/Manual, 2: Product/Brand, 3: Inspiration/Quote).

**Post Month (Integer)**: Month of publication (1 = January to 12 = December).

**Post Weekday (Integer)**: Day of the week published (1 = Sunday to 7 = Saturday).

**Post Hour (Integer)**: Time of publication in 24-hour format (1 to 23).

**Paid(Binary / Float)**: Promotion indicator ($0.0 = Organic, $1.0 = Paid boost).

---

### 2. Reach & Impression Metrics (Exposure)

Metrics capturing how widely the post was distributed and seen across feeds:

**Lifetime Post Total Reach (Integer)**: Number of unique users who saw the post at least once.

**Lifetime Post Total Impressions (Integer)**: Total number of times the post entered a user's screen (includes multiple views by the same person).

**Lifetime Engaged Users (Integer)**: Number of unique users who clicked anywhere on the post (liked, commented, shared, clicked photos, or watched videos).

**Lifetime Post Consumers (Integer)**: Unique users who clicked on the post content 

**Lifetime Post Consumptions (Integer)**: Total count of all clicks performed on the post content.

**Lifetime Post Impressions by people who have liked your Page (Integer)**: Total impressions generated specifically from existing page followers.

**Lifetime Post reach by people who like your Page (Integer)**: Unique existing page followers reached by the post.

**Lifetime People who have liked your Page and engaged with your post (Integer)**: Unique existing followers who interacted with or clicked the post.

---

### 3. Engagement & Interaction Metrics (User Actions)

Specific user actions generated by the post:

Comment (Integer): Number of comments left on the post.
Like (Float): Number of likes given to the post.
Share (Float): Number of times users shared the post to their timelines.
Total Interactions (Integer): Aggregate interaction score calculated as the direct sum.
