# Football predictor 
### Introduction
This project aims to predict the outcome of La liga matches using only pre-game information. 
### Data introduction and initial preparation
Our dataset encompasses match outcomes for every La Liga game from the 1995-1996 season to the 2023-2024 season, up to Matchday 8, and it also includes half-time scores.
Columns:
- Season [str]
- HomeTeam [str]
- AwayTeam [str]
- FTHG (Full-time home goals) [int]
- FTAG (Full-time away goals) [int]
- FTR (Full-time result): Home (H) Away (A) Draw (D) [str]
- HTHG (Half-time home goals) [float]
- HTAG (Half-time away goals) [float]
- HTR (Half-time result) [str]

Our objective is to predict the outcome of the matches using only pre-game information. This is a multi-class classification problem with three classes: Home team wins, draw or home team loses

### EDA and feature selection
We compute the cumulative goal difference of the teams on that season prior to each game as well as the position in the league of each team. The features we will add to our model are:
- Home team name
- Away team name
- Position difference of the teams prior to the game
- Home cumulative goal difference
- Away cumulative goal difference
- Home points: points of the home team when it plays home
- Away points: points of the away team when it plays away.
Our target variable is the full time result. If we plot the distributions of the full time result across all seasons we can see that there is clearly a class imbalance problem


![output](https://github.com/user-attachments/assets/ae65a67b-38ea-4478-9a89-45f8cea6cffd)

### Model selection and training
We will need to make a stratified train-test split to fix class imbalance but also the split should repsect the time ordering of the seasons. 

We will try three models:
- Logisitic regression (Baseline).
- XGBoost with SMOTE: To handle the class imbalance with XGBOOST we will use SMOTE (Synthetic Minority Over Sampling Technique) which essentially creates synthetic samples of the minority classes by interpolation. 
- Random forests with class weighting: For random forests we use class weighting which effectively increases the weights of the minority classes so that misclassification of the minority classes is penalized.

Last season will be saved for testing purposes.

Comparison:
- Logisitc regression: 70% accuracy
- XGBoost with SMOTE: 61% accuracy
- Random forests: 70% accuracy

| Model | Class | Precision | Recall | F1-score |
|----------|----------|----------|----------|----------|
LR | Draw | 0.67 |0.18 | 0.29 |
LR | Away | 0.74 | 0.88 | 0.80 |
LR |Home | 0.68 | 0.88 | 0.77 |
XG | Draw | 0.31 |0.45 | 0.37 |
XG | Away | 0.80 | 0.75 | 0.77 |
XG |Home | 0.77 | 0.59 | 0.67 |
RF | Draw | 0.50 |0.36 | 0.42 |
RF | Away | 0.74 | 0.88 | 0.80 |
RF |Home | 0.76 | 0.76 | 0.77 |### XGBoost

### Conclusions

- Logistic Regression and Random Forests both perform similarly in terms of accuracy (70%), but Random Forests generally have a better balance across the "Home" and "Away" classes.
- XGBoost, despite having a good performance for "Away" and "Home," performs poorly for "Draw," which results in a significantly lower F1-score and overall performance.
- Overall Random Forests seems to be the most well-rounded model across all classes (with decent precision, recall, and F1-score for all three categories).
