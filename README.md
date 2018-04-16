# Kaggle Challenge - Don't Get Kicked!
https://www.kaggle.com/c/DontGetKicked

### Results
| Method        | Public Score (Gini index)          | Private Score (Gini index) | Prediction File |
| ------------- |:-------------:| -----:| ----:|
| Random Forest | 0.195 | 0.189 | results/test_y.csv |
| XGB      		| 0.222      | 0.217 | results/test_y_xgb.csv |


### Report

In this challenge, we are tasked with predicting kicks (bad purchases) for auto dealerships. My best model has a Gini score of 0.22, while the 1st place in Kaggle leaderboard is 0.27.

My solution (First pass.ipynb) focuses primarily on understanding the data, data cleaning and priliminary modeling. The goal is to remove any irrelevant features and add relevant features. Irrelevant features may include identifier number, repetitive information and unrelated variables like color of the car. In contrast, we can enrich our features by transforming some of the seemingly unhelpful features. For example, there are 8 MMR prices, but these prices are only useful when compared to prices of the car itself. Therefore, I decided to transform MMR acquisition prices at the time of purchase to a percentage increase compared to the cost of the purchase. Similarly, there are four prices about the current MMR prices. Since these cars were bought at different times, current MMR prices are only comparable when holding the car and the time lapsed since purchase constant. Lastly, I converted all categorical variables to dummy variables for the modeling that comes next.

In the modeling part of the solution, I first ran logistic regression to get a baseline result. One mistake I made early on was to evalute the models using only accuracies, while the competition is scored based on Gini score where ranking is more important. As soon as I realized, I evaluated all models using AUC, since it also emphasizes on ranking. I continued modeling with Naive Bayes, Random Forest and XGBoost, and got better and better AUC score.

Lastly, I analyzed the best model I got from XGBoost and found that the most important features are:
[('MMRCurrentAuctionAveragePrice_DepPerYear', 443),
 ('VehBCost', 441),
 ('MMRCurrentAuctionCleanPrice_DepPerYear', 403),
 ('VehOdo', 378),
 ('MMRCurrentRetailAveragePrice_DepPerYear', 345),
 ('MMRCurrentRetailCleanPrice_DepPerYear', 304),
 ('MMRAcquisitionAuctionAveragePrice_diff', 291),
 ('MMRAcquisitionRetailAveragePrice_diff', 272),
 ('MMRAcquisitionRetailCleanPrice_diff', 270),
 ('MMRAcquisitionAuctionCleanPrice_diff', 234)]

 If I had more time, I would work on the following items:

- Overlay income and consumer price data using zipcode to get a better idea of pricing
- Perform grid Search for all models
- XGBoost proves to be powerful here (even without much parameter tuning). The current best model overfits. I'd like to tune parameters in that direction.
- Stratify training data, given only around 10% was a bad buy
- dig deeper into the relationship between Current Prices and Acquisition Prices and how that affects the probability of being a kick. 
- there are over 2000 columns after we convert categories into dummies. it's too sparse, meaning we would want to drop some of the less frequent columns to prevent overfitting in our model.
- write a customized Gini score evaluator to evaluate all models