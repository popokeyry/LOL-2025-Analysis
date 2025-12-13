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

<div class="table-wrapper" markdown="1">

| gameid           | side   | position   |   gamelength |   result |   kills |   deaths |   assists |   barons |   visionscore |   totalgold |
|:-----------------|:-------|:-----------|-------------:|---------:|--------:|---------:|----------:|---------:|--------------:|------------:|
| LOLTMNT03_179647 | Blue   | top        |         1592 |        0 |       1 |        2 |         1 |        0 |            17 |       10668 |
| LOLTMNT03_179647 | Blue   | jng        |         1592 |        0 |       0 |        3 |         1 |        0 |            29 |        7429 |
| LOLTMNT03_179647 | Blue   | mid        |         1592 |        0 |       1 |        2 |         0 |        0 |            20 |        9032 |
| LOLTMNT03_179647 | Blue   | bot        |         1592 |        0 |       1 |        3 |         1 |        0 |            10 |        9407 |
| LOLTMNT03_179647 | Blue   | sup        |         1592 |        0 |       0 |        3 |         2 |        0 |            82 |        5719 |

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

---

# Hypothesis Testing

<iframe src="assets/observed_high_v_low.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/new_fig_high_v_low.html" width=800 height=600 frameBorder=0></iframe>

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis
---