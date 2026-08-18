To Conclude this project we will discuss:

i. Which insights we gained from our EDA? 

ii. How did we determine which columns to drop or keep? If the EDA informed this process, explain which insights you used to determine which columns were not needed. 

iii. Which hyperparameter tuning strategy did we use? Grid-search or random-search? Why? 

iv. How did your model's performance change after discovering optimal hyperparameters? 

v. What was our final F1 Score? And what is your interpretation of the precision metric vs the recall metric?


From our EDA, we learned a few key things about how the Facebook metrics relate to each other:

- 'Page Total Likes' doesn’t really matter for engagement: It barely correlated with likes, comments, or shares, which told us it isn’t a useful predictor of how a specific post performs.
- Some metrics were basically duplicates: For example, Lifetime Post Total Reach and Lifetime Post Total Impressions had almost a perfect correlation. Same thing with Lifetime Post Consumers and Lifetime Post Consumptions. These pairs were giving us the same information twice.
- Engagement metrics naturally correlate with each other: Likes, comments, shares, and Total Interactions all moved together. This makes sense because Total Interactions is literally the sum of the other three.
Overall, the EDA showed us which columns were meaningful and which ones were just repeating the same signal.

To determine which columns to drop or keep, we used the correlation matrix from our EDA. We kept columns that provided unique, meaningful information, and we dropped columns that were irrelevant or repeated the same pattern. All of these decisions came directly from what we saw in the correlation matrix.

 - We dropped the column that didn’t help predict engagement.
 - We removed redundant columns: When two columns had extremely high correlation (0.95–0.99), we kept the one that added depth and easier to interpret, and dropped the duplicate. 
        For example we:
            Dropped Lifetime Post Total Impressions, but kept Lifetime Post Total Reach
            Dropped Lifetime Post Total Consumptions, but kept Lifetime Post Consumers
- We removed 'Total Interactions' becuase it duplicated the engagement metrics. (total interactions is the same as the sum of 'like' + 'comment ' + 'share')


