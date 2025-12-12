# LOL-2025-Analysis

Project for DSC80 at UCSD on League of Legends

by Ryan Conrad Atienza (ratienza@ucsd.edu)

---

## Introduction

# Dataset Introduction

League of Legends is a video game created by Riot Games. It is one of the most popular games and has lots of data in the e-sports scene. The dataset I will be analyzing is a data set handled by Oracle's Elixir from the professional scene from 2025. 

This dataset contains a massive amount of information that pertains but is not limited to a person's performance, a team's performance, and match statistics from a multitude of League of Legends matches. 

League of Legends has many in-game statistics that contribute to a player's performance in each match. One of these statistics is the idea of "vision score." In the game, vision is a key factor to a team, as vision allows a team to not only impede the enemies' strategies, but also create opportunities and create plays for themselves. Vision is vital in professional e-sports as knowing where the enemies are allows for strategies to put into fruition. 

This project is centered around the question "Is vision score an effective indication on individual performance and prediction?" This question and dataset helps understand what statistic is most predictive of team success, allowing for insights into training, evaluation, and what matters besides a higher number of kills. 

# Columns Introduction

This dataset involves a significant size array with a multitude of columns which includes in-game statistics (like goldat10, the amount of gold at 10 minutes) alongside post-game evaluations (like result, a binary output based on if a player/team won or lost a match). The dataset includes 118,932 rows, and 164 columns. I will be limiting the columns to relevant columns that are  These columns include: 

* `gameid`: 
* `side`:
* `position`:
* `gamelength`:
* `result`:
* `kills`:
* `deaths`:
* `assists`:
* `visionscore`:
* `totalgold`: 

---

## Cleaning and EDA

<iframe src="assets/visionscore_distribution.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/result_vs_vision_distribution.html" width=800 height=600 frameBorder=0></iframe>

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing

<iframe src="assets/observed_high_v_low.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/new_fig_high_v_low.html" width=800 height=600 frameBorder=0></iframe>

## Framing a Prediction Problem

## Baseline Model

## Final Model
---