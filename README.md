# One "Healthy" Recipe to Rule Them All
Project 1 - UWA/edX Data Analytics Bootcamp

Github Repository at: [https://github.com/alyssahondrade/Project1.git](https://github.com/alyssahondrade/Project1.git)

## Table of Contents
1. [Introduction](https://github.com/alyssahondrade/Project1#introduction)
    1. [Goal](https://github.com/alyssahondrade/Project1#goal)
    2. [Repository Structure](https://github.com/alyssahondrade/Project1#repository-structure)
    3. [Dataset](https://github.com/alyssahondrade/Project1#dataset)
2. [Scope](https://github.com/alyssahondrade/Project1#scope)
3. [Approach](https://github.com/alyssahondrade/Project1#approach)
    1. [Decompose the Ask + Identify Data Sources]
    2. [Define Strategy and Metrics]
    3. [Build a Data Retrieval Plan]
    4. [Retrieve the Data]
    5. [Assemble and Clean]
    6. [Analyse or Trends]
    7. [Acknowledge Limitations]
    8.    
4. [Decision Points](https://github.com/alyssahondrade/Project1#decision-points)
5. [Analysis](https://github.com/alyssahondrade/Project1#analysis)
6. [Future Research](https://github.com/alyssahondrade/Project1#future-research)
7. [References](https://github.com/alyssahondrade/Project1#references)

## Introduction
### Goal
The goal of the project is to compare recipes from two popular recipe websites, Spoonacular and Food.com, and identify "healthy" recipes using two market-implemented measures:
1. Nutri-Scores: [https://www.mangiu.ch/index/](https://www.mangiu.ch/index/)
2. Weight Watchers (WW) Smart Points [https://www.weightwatchers.com/us/how-it-works/smartpoints](https://www.weightwatchers.com/us/how-it-works/smartpoints)

The project will investigate recipe popularity, meal types, and cuisines.

### Repository Structure

### Dataset
- `RAW_interactions.csv` and `RAW_recipes.csv` from [Kaggle - Food.com Recipes and Interactions](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?)
- [Spoonacular API](https://spoonacular.com/food-api)

## Scope
### Research Questions
1.	Are more popular/higher-rated recipes healthier? What is the health rating of the highest-rated recipes?
2.	What meal type (i.e., breakfast, lunch, or dinner) have the healthiest/unhealthiest (percentage) recipes? What is the most popular ingredient for each meal?
3.	Which cuisine has the healthiest recipes?

### Question Decomposition
- What is meant by "popularity"?
The project defines popularity by the highest rating (Food.com dataset) or the highest aggregate likes (Spoonacular API).

- How is "healthiness" defined?
As per the project goal, "healthiness" is defined by the two market-implemented measures: Nutri-Scores and WW Smart Points.

    Nutri-Scores is based on a five colour-coded letter grade (A, B, C, D, E), where `A` is the highest and healthiest score.

    WW Smart Points is a scoring system using calories, saturated fat, sugar, and protein, where the lower the number the healthier the food is. Calories provides the baseline, saturated fat and sugar increase the score, and protein decreases the score.

- What is considered as a "recipe"?
All submissions on both websites are considered as a "recipe", regardless if the recipe is a meal or a drink.

- What is a meal type?
For simplicity, meal types have been narrowed to `breakfast`, `lunch`, and `dinner`, or equivalents if unavailable.

- What cuisines will be considered?
As a minimum, the cuisine type must be available in both datasets.

### Nutritional Values
THe nutritional values of interest will be reduced to the minimum required for calculating the "health" score: `calories`, `saturated fat`, `sugar`, and `protein`.

## Approach
### Data Analytics Paradigm
The project is structured around the data analytics paradigm.



#### 2. Define Strategy and Metrics

#### 3. Build a Data Retrieval Plan

#### 4. Retrieve the Data

#### 5. Assemble and Clean

#### 6. Analyse or Trends

#### 7. Acknowledge Limitations

#### 8. Tell the Story

#### Food.com

## Decision Points
1. __Food.com meal types__. `dinner-party` is used in lieu of `dinner`, as it does not exist in the tag list.
2. __Negative WW Smart Points__. Although the WW Smart Points system does not allow for negative points, calculated negative values are allowed to present a wider range of values.

## Analysis

### Results

### Limitations


## Future Research

## References
- [1] Weight Watchers Smart Points Calculator [https://www.watcherspoint.com/weight-watchers-smart-points-calculator](https://www.watcherspoint.com/weight-watchers-smart-points-calculator)

- [2] Nutri-Score - A Simple Science-Based Nutritional Value Labelling System for the Food Industry [https://get.apicbase.com/nutri-score-science-based-nutritional-value-labelling-system/](https://get.apicbase.com/nutri-score-science-based-nutritional-value-labelling-system/)