# LOL-2025-Analysis

Project for DSC80 at UCSD on League of Legends

by Ryan Conrad Atienza (ratienza@ucsd.edu)

---

# Introduction

## Dataset Introduction

League of Legends is a video game created by Riot Games. It is one of the most popular games and has lots of data in the e-sports scene. The dataset I will be analyzing is a data set handled by Oracle's Elixir from the professional scene from 2025. 

This dataset contains a massive amount of information that pertains but is not limited to a person's performance, a team's performance, and match statistics from a multitude of League of Legends matches. 

League of Legends has many in-game statistics that contribute to a player's performance in each match. One of these statistics is the idea of "vision score." In the game, vision is a key factor to a team, as vision allows a team to not only impede the enemies' strategies, but also create opportunities and create plays for themselves. Vision is vital in professional e-sports as knowing where the enemies are allows for strategies to put into fruition. 

This project is centered around the question "Is vision score an effective indication on individual performance and prediction?" This question and dataset helps understand what statistic is most predictive of team success, allowing for insights into training, evaluation, and what matters besides a higher number of kills. 

## Columns Introduction

This dataset involves a significant size array with a multitude of columns which includes in-game statistics (like goldat10, the amount of gold at 10 minutes) alongside post-game evaluations (like result, a binary output based on if a player/team won or lost a match). The dataset includes 118,932 rows, and 164 columns. The columns indicate in-game statistics that were produced from this dataset of professional League of Legends matches. Some of these columns include: 

* `gameid`: A unique identifier for each professional match in the dataset. 
* `datacompleteness`: A column designating whether data has been finished in the match, signified by 'complete' or 'partial'
* `side`: A column that designates which team each player is associated with in each match, known as 'Blue' side or 'Red' side.
* `position`: This column signifies which role each player was assigned to in the match, with roles including 'top', 'jungle', 'mid', 'bot', and 'sup'.
* `gamelength`: The amount of time each match lasted in seconds.
* `result`: The outcome of a match, with a win noted as '1' and a loss noted as '0'.
* `kills`: This column indicates the amount of champion eliminations a player or team contributed in a match as an integer.
* `deaths`: This column indicates the amount of champion deaths a player or team had in a match as an integer.
* `assists`: This column indicates the amount of champion assists a player or team had in a match as an integer, in which they contributed to the elimination but was not the final blow.
* `barons`: The amount of barons, a type of objective that a player or team can defeat to gain rewards, that were eliminated by a player or a team in a match.
* `visionscore`: A metric that captures how much vision a player or team controls in a map. Higher vision scores is usually an indicator of a higher indication of information for a player or team.
* `totalgold`: The total amount in-game currency per player in a match, earned by completing objectives in the game, such as eliminating enemy players or defeating baron. 

---

# Cleaning and Exploratory Data Analysis

## Data Cleaning

For this dataset, the process involved some data cleaning steps.

First, we keep the relevant columns of the dataset, `side`, `position`, `gamelength`, `result`, `kills`, `deaths`, `assists`, `barons`, `visionscore`, `totalgold`. 
Each match includes 12 rows, 10 for each player, and 2 for the team overall. I decided to keep all of these rows, and create separate dataframes to include or exclude the 'team' rows based on what is necessary.
Next, we check to see if any of the values of these columns are null or seem unusual. When analyzing the data, it seems that a column `datacompleteness` has two values: complete and partial. When a row has `datacompleteness` value of partial, many of the columns have null values, including column `baron` are null for individual players. Because of this, I created a helper function to fill the missing values in said rows. For each team where `baron` is null for individual players, the first player on each team gets the total baron eliminations, while the other players receive 0.

Here is the head of the cleaned dataset:

<div class="table-wrapper" markdown="1">

| gameid           | datacompleteness   | side   | position   |   gamelength |   result |   kills |   deaths |   assists |   barons |   visionscore |   totalgold |
|:-----------------|:-------------------|:-------|:-----------|-------------:|---------:|--------:|---------:|----------:|---------:|--------------:|------------:|
| LOLTMNT03_179647 | complete           | Blue   | top        |         1592 |        0 |       1 |        2 |         1 |        0 |            17 |       10668 |
| LOLTMNT03_179647 | complete           | Blue   | jng        |         1592 |        0 |       0 |        3 |         1 |        0 |            29 |        7429 |
| LOLTMNT03_179647 | complete           | Blue   | mid        |         1592 |        0 |       1 |        2 |         0 |        0 |            20 |        9032 |
| LOLTMNT03_179647 | complete           | Blue   | bot        |         1592 |        0 |       1 |        3 |         1 |        0 |            10 |        9407 |
| LOLTMNT03_179647 | complete           | Blue   | sup        |         1592 |        0 |       0 |        3 |         2 |        0 |            82 |        5719 |

</div>

## Univariate Analysis

I performed univariate analysis on two variables to further understand the distributions:

<iframe src="assets/visionscore_distribution.html" width=800 height=600 frameBorder=0></iframe>

This histogram shows the distribution of the total team vision scores across the dataset matches. Vision scores are distributed nearly normal, meaning it is balanced and can be analyzed as an indication of player performance.

I also looked at the distribution of total gold.

<iframe src="assets/gold_distribution.html" width=800 height=600 frameBorder=0></iframe>

This histogram shows that the distribution of total gold is roughly normal and slightly right skewed. This suggests that the progression of gold accumulation is balanced in professional matches and can follow patterns that can be analyzed.

## Bivariate Analysis

I performed bivariate analysis between the result of a team (whether they win or lose) and their vision score of the match.

<iframe src="assets/result_vs_vision_distribution.html" width=800 height=600 frameBorder=0></iframe>

This box plot helps to visualize this relationship, in which winning teams achieve higher vision scores than losing teams in professional games. This is an indicator that higher vision score is more likely to be attributed to winning than losing a match.

## Interesting Aggregates

Grouping the cleaned dataset without the 'team' rows in the `position` column, then sum the columns allows for some interesting aggregations to investigate:

<div class="table-wrapper" markdown="1">

|   result |   visionscore |   kills |   assists |   barons |   totalgold |
|---------:|--------------:|--------:|----------:|---------:|------------:|
|        0 |   2.35334e+06 |  105075 |    232555 |     1693 | 5.38138e+08 |
|        1 |   2.54024e+06 |  217971 |    512408 |     8808 | 6.39891e+08 |

</div>

It seem as though there is a distinct difference between winning and losing teams, in which winning teams not only have higher vision scores as shown by the visualization, but kills, assists, number of barons eliminated, and total gold as well. Winning teams seem to have better statistics in comparison to the losing teams.

---

# Assessment of Missingness

## NMAR Analysis

In the dataset, the columns of `ban1`, `ban2`, `ban3`, `ban4`, `ban5` could be Not Missing at Random (NMAR). This is because in League of Legends, players can ban a character, but are not required to. Therefore, these columns missingness seems to be dependent on the value itself for its missingness--it occurs when players choose to not ban. Data that would be interesting to investigate is the missingness in comparison to champions already played for each specific set of matches. This is because in 2025, League of Legends introduced 'fearless draft', in which a champion is removed from the series if they have already been picked. If missingness depended on this 'already played champions' column then the ban columns could be considered Missing at Random (MAR) because it could depend on this column.

## Missingness Dependency

Missingness can be tested, and in this section, I will be testing if the `barons` column is dependent on other columns. These columns include `result` and `datacompleteness`.

I conducted a permutation test to assess whether missingness of `barons` depends on these columns, using Total Variation Distance (TVD) as the test statistic, with a significance level of 0.05.

### Test 1: Barons Missingness vs. Result

Null Hypothesis: The distribution of `result` when `barons` is missing is the same as the distribution of `result` when `barons` is not missing.

Alternative Hypothesis: The distribution of `result` when `barons` is missing is not the same as the distribution of `result` when `barons` is not missing.

Test Statistic: TVD

This is the observed distribution of `result` when `barons` is missing vs. not:

|   result |   Prop (Missing Barons) |   Prop (Not Missing Barons) |
|---------:|------------------------:|----------------------------:|
|        0 |                     0.5 |                         0.5 |
|        1 |                     0.5 |                         0.5 |

After the permutation tests, the observed statistic was 0.0, while the p-value was 1.0.

Since our p-value, 1.0, is much greater than 0.05 level of significance, we fail to reject the null hypothesis, suggesting that the missingness of `barons` is not dependent on `result`. 

We then move on to the second permutation test:

### Test 2: Barons missingness vs. Data Completeness

Null Hypothesis: The distribution of `datacompleteness` when `barons` is missing is the same as the distribution when `barons` is not missing.

Alternative Hypothesis: The distribution of `datacompleteness` when `barons` is missing is NOT the same as the distribution when `barons` is not missing.

Test Statistic: Total Variation Distance (TVD)

This is the observed distribution of `datacompleteness` when `barons` is missing vs. not:

| datacompleteness   |   Prop Missing Barons |   Prop Not Missing Barons |   Difference |
|:-------------------|----------------------:|--------------------------:|-------------:|
| complete           |                     0 |                         1 |           -1 |
| partial            |                     1 |                         0 |            1 |

When computing the permutation tests, the observed TVD was 1.0, while the p-value was 0.0. Since the p-value was less than the significance level at 0.05, we reject the null hypothesis. This suggests that the missingness of `barons` column is strongly associated on the `datacompleteness` column. Only when `barons` are missing is when `datacompleteness` is 'partial'.

---

# Hypothesis Testing

I was curious to see whether vision score had an impact on the deaths in any match. More specifically, for this hypothesis test, I wanted to assess whether the distribution of deaths is the same or different based on whether the team has a higher or lower vision score. This significance is important as it can explain a individual's or team's performance in a match, allowing for analyzing their relationship of performance and vision, a crucial element in League of Legends because of the information given.

Null Hypothesis: The distributions of deaths is the same between the team with a higher vision score vs. the team with the lower vision score.

Alternative Hypothesis: The distribution of deaths is lower for the team with the higher vision score vs. the team with the lower vision score.

Test statistic: Mean difference in deaths (Higher vision score - Lower Vision Score)

Significance Level: 5%

This histogram shows the distribution of the test statistics after running the hypothesis test:

<iframe src="assets/new_fig_high_v_low.html" width=800 height=600 frameBorder=0></iframe>

The observed test statistic ended up being -7.37, with a p-value of 0.0. In the visualization, the observed test statistic isn't even distinguished, and falls outside of our distribution. Based on our p-value and hypothesis test, we reject the null hypothesis and accept the alternative. This suggests that teams with the higher vision score die significantly fewer than teams with the lower vision score. This supports the idea that vision and vision control is a vital factor for individual and team performance. 

---

# Framing a Prediction Problem

## Problem Identification

From the previous hypothesis test, we can see that a higher vision score relates to fewer deaths within a match, and from our aggregations, is part of a statistic that is higher based on a win or a loss in a match. Because of these observations, the next question I would want to ask is if this statistic, alongside other statistics in the dataset, is significant to team's performance. In other words, I want to find out if it is possible to see how related a 'good performance' is to a team winning a game.

For this section, I want to predict whether a team would win or lose any given match based on their post-game data. The prediction for this problem is of the classification type, because the response variable, or the variable I want to predict, is between two outcomes: a win or a loss. Being able to understand which statistics can strongly predict outcomes help to identify and train performance indicators, skills, and even change strategies in games based on metrics seen in real-time.

The model is based around this prediction problem: Can we predict if a team win or loses any given game based on their players' statistics? I will use select columns from our cleaned data set and use two evaluation metrics: accuracy and F1-Score. This is because I want to see both the overall correctness of our prediction, which makes sense as our dataset should be balanced with 50% win and 50% losses, alongside balancing precision and recall so our model can work for both wins and losses. Therefore, our model is a binary classification model, with a response variable of `result`.

I will only use columns that we will know at the time of making predictions, which involve kills, deaths, assists, vision score, total gold, baron, position, and game length. I will not be using result, as that is what I want to predict. These features are some statistics that are observable and allow us to analyze what assists to predict a match win.

The data will also be split into 75% training data and 25% test data to prevent overfitting.

---

# Baseline Model

In the baseline model, a Logistic Regression classifier was used with three simple features to create a benchmark of success.

This model includes three features, two of which are quantitative, and one categorical. We used features: `kills`, `visionscore`, and `position`.

The quantitative features, `kills`, `visionscore` were transformed using a StandardScaler Transformer that allows us to standardize them as the lengths of time could vary the statistics (50 kills in 10 minutes vs 100 minutes). The categorical feature `position` was encoded utilizing the OneHotEncoder Transformer, which allows us to one-hot encode a categorical column. In this instance, the column `position` transforms into five binary columns, `position_top`, `position_jng`, `position_mid`, `position_bot`, `position_sup`. We drop one of the columns as if all other columns have value 0, we know that column would have value 1.

From fitting the model to our data, the accuracy score was 0.6912: the model can predict 69.12% of data correctly. Our F1-Score was 0.6896, or 68.96%, meaning our model also balances precision and recall at 68.96%. This model works better than randomly guessing whether a team will win or lose a game, but not very high. It can be improved by both tuning hyperparameters and adding features tied closer to game-by-game performance. 

---

# Final Model

For the final model, I created improvements over the baseline to better capture the statistics and gameplay in a League of Legends Game.

### Feature Engineering

First, I added in Kill/Death/Assist Ratio, `kda`, measured as (kills + assists)/ deaths. This is because in League of Legends, it is not an individual game, nor do the statistics exist in a vacuum: statistics matter more when they are within the context of the others, especially combatative related ones. This helps to analyze one's combat effectiveness alongside helping to relay the fact that kills and assists matter when a player also strategizes to live. 

I also added in Vision per Minute, `vpm`, measured as visionscore / (gamelength / 60), and Gold per Minute, `gpm`, measured as totalgold / (gamelength / 60). These two features both normalizes the score by the game length, and indicates how much control or how 'active' a player is in a game respectively. In League of Legends, a player would want to maximize both of these statistics as it allows for higher performance and power spikes within the game.

All of these statistics are important to the model because a player doing 'well' in a given match would have a higher stat for all of these metrics, since these reflect a player's ability in terms of combat, micro/importance of an individual's performance, macro/importance of the team's performance, and how active a player is within a specific match. These features would be expected to be essential in both predictive power and analysis in practice toward a win in a League of Legends game.

The final model also switches from Logistic Regression to a Random Forest Classifier. This is because it helps to relate relationships that aren't linear, and can handle interactions between multple variables. The three features, `kda`, `vpm`, and `gpm`, all happen to be quantitative, so we use a StandardScaler Transformer when encoding the columns. I also decided to tune hyperparameters, include number of estimators for the random forest classifier, the max depth, the minimum samples to split a node, and the minimum samples required at leaf nodes. For number of estimators, test 50, 100, and 200. For max depth, test 5, 10, 15, and no depth. For minimum samples to split a node, test 2, 5, and 10. For minimum samples at leaf nodes, test 1, 2, and 4. To optimize them, I used a randomized search CV with a 5-fold cross-validation as the multiple parameters would be many combinations to test all of them. Then, the most optimal parameters were 200, 15, 10, and 1, respectively. 

From these improvements, the accuracy score of the model was 0.8963, meaning the model can correctly predict 89.63% of the data (better than the 69.12%). The F1 score was also 0.8963, meaning the balance of precision and recall is much higher than previously before (at 0.6896) This is a great improvement, and while there definitely is still room for improvement, this is a good suggestion about the model's effectiveness for predicting a win or a loss (around 9 of 10 matches)!

---

# Fairness Analysis

From our model, I then wondered if the model is accurate across different player positions. This is because some positions, such as support, focus on different objectives within a League of Legends game. For example, a support player would want to have a higher vision score than say a mid-lane player. In this way, I analyzed whether the model performs fairly across different player positions, specifically comparing support players to other positions. The question is: 'Does the model perform worse for players in support positions than it does for player in non-support positions. I create permutation test and observe the result of precision between the groups.

Group X is support players, or people with `position` = 'sup', while Group Y is non-support players, or people with `position` = 'top', 'jng', 'mid,' and 'bot'. The metric for evaluation is precision, as if the model has lower precision for supports, that means the model falsely predicts a win for a support player more often.

Null Hypothesis: The model is fair. Its precision for support vs. non-support players is around the same, and differences are due to random chance.

Alternative Hypothesis: The model is unfair. Its precision for support vs non-support players is not the same.

Test Statistic: Difference in precision (Support - Non-support)

Significance Level: 5%

Below is the histogram that shows the distribution of precision differences assuming the null hypothesis:

<iframe src="assets/fairness.html" width=800 height=600 frameBorder=0></iframe>

P-value: 0.0

The p-value is < 0.05, meaning that we reject the null hypothesis. Therefore, this suggests that our model does not seem to be fair, but not against supports, in favor of them, as the model shows higher precision for support players than non-support players. Inherently, there seems to have some type of bias for support players or statistics that are more closely related to the support position.

---