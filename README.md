# Metabolic Syndrome 

### We've been given a dataset of people from various backgrounds and health factors, some of which have metabolic syndrome. 
### The task is to develop a model that can predict metabolic syndrome for someone, given certain characteristics and life habits.

# Exploration

### Diagnosing metabolic syndrome is a binary classification problem. A value count was done, revealing roughly two thirds of cases in the data weren't metabolic. After exploring data for anomalies and entry errors, we have two visuals that may point to correlations

![Metabolic Syndrome Races with Triglyceride Counts](https://github.com/Rovidicus/Metabolic-Syndrome/assets/141533406/8421e1b4-3817-4ad3-bb90-2952d4bc24ba)

### Triglyceride counts are nearly double for those with Metabolic Syndrome. There doesn't appear a key distinction across races yet.

![Case Count by Age](https://github.com/Rovidicus/Metabolic-Syndrome/assets/141533406/9b87cf10-152d-4828-bfd8-1e301a8851a9)

### We observe a generally rising degree of cases as people age.

![Income_Histplot](https://github.com/Rovidicus/Metabolic-Syndrome/assets/141533406/b9cf2104-26e1-414f-b794-13f42463b515)

### Not finding income to be particularly relevant, I ran a histplot to observe if there appeared anything bizarre. All the distributions appear standard 2:1 no/yes cases just like our dataset distribution.

# Preprocessing

### The more irrelevant features we can cut, the better and easier it will be for modeling later. The following columns were cut: ['BMI','Income','seqn']. We already have a feature for waist circumference that fills a similar, if superior, role to BMI that makes it too redundant. Income doesn't have much bearing on our task and 'seqn' is an unnecessary index feature.

### For preprocessing, median was imputed for numeric data due to many integer features like Age and Triglyceride Count. With categoricals I imputed nulls with most frequent before one hot encoding.

![Basic Random Forest](https://github.com/Rovidicus/Metabolic-Syndrome/assets/141533406/904b7003-3542-485a-bab3-4a485898bda8)

### Our first model is overfit with 89% accuracy. From here we delve into permutation importances to uncover more.

![Permutation Importance](https://github.com/Rovidicus/Metabolic-Syndrome/assets/141533406/f0a4be71-18dc-4335-8d20-b6e82416e1df)

### Blood glucose levels have the most relevance with waist circumference and triglyceride counts following. These aren't too surprising and HDL cholesterol count is a distant fourth. What's really not emphasized is age, to my surprise. Race and marital status have very modest to detrimental influence, so it would appear.

![Bloodglucose_Histplot](https://github.com/Rovidicus/Metabolic-Syndrome/assets/141533406/0948f08c-ce36-425b-b22a-183a81c4785f)

### Here we see the great majority of negative cases have blood glucose levels at or below 100 while positives are generally over that threshold.

![metsyn_age_lineplot](https://github.com/Rovidicus/Metabolic-Syndrome/assets/141533406/860707b2-dabb-4777-b8e8-8b7b85ef97d0)

### An age lineplot shows a consistently higher waist circumference level for positive cases.

### K-Means Clustering helped us cluster the data into three clusters. From there, we analyzed permutation importances and noticed several were having a negative impact on modeling like marital status and uric acid levels. We narrowed our data down to the top 12 features by descending importance going forward.

### A positive of our new random forest model was a slight drop in false negatives while maintaining the same accuracy at 89%.

### From here we ran two neural networks to see if modeling could be improved. While the second model was an upgrade, its metrics still weren't as strong as the tuned random forest model at only 85% and worse precision.

### A Keras tuner was also launched with several parameters to test which was optional from dropout rate to layer values and optimizer choice. Nonetheless, the best model reached about an 86.7% accuracy. Given the nature of how serious diagnoses are and accuracy needed, the tuned random forest model remains the optimal choice going forward.











