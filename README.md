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

The project will investigate recipe popularity, meal types, and cuisines, and will be conducted as per the data analytics paradigm.

### Repository Structure

### Dataset
-  [Kaggle - Food.com Recipes and Interactions](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?)
    - `RAW_interactions.csv` from [https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_interactions.csv](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_interactions.csv)
    - `RAW_recipes.csv` from [https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_recipes.csv](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_recipes.csv)
- [Spoonacular API](https://spoonacular.com/food-api)

## Scope
### Research Questions
1.	Are more popular/higher-rated recipes healthier? What is the health rating of the highest-rated recipes?
2.	What meal type (i.e., breakfast, lunch, or dinner) have the healthiest/unhealthiest (percentage) recipes? What is the most popular ingredient for each meal?
3.	Which cuisine has the healthiest recipes?

### Question Decomposition
- __What is meant by "popularity"?__

    The project defines popularity by the highest rating (Food.com dataset) or the highest aggregate likes (Spoonacular API).

- __How is "healthiness" defined?__

    As per the project goal, "healthiness" is defined by the two market-implemented measures: Nutri-Scores and WW Smart Points.

    Nutri-Scores is based on a five colour-coded letter grade (A, B, C, D, E), where `A` is the highest and healthiest score.

    WW Smart Points is a scoring system using calories, saturated fat, sugar, and protein, where the lower the number the healthier the food is. Calories provides the baseline, saturated fat and sugar increase the score, and protein decreases the score.

- __What is considered as a "recipe"?__

    All submissions on both websites are considered as a "recipe", regardless if the recipe is a meal or a drink.

- __What is a meal type?__

    For simplicity, meal types have been narrowed to `breakfast`, `lunch`, and `dinner`, or equivalents if unavailable.

- __What cuisines will be considered?__

    As a minimum, the cuisine type must be available in both datasets.

### Nutritional Values
The nutritional values of interest will be reduced to the minimum required for calculating the "health" score: `calories`, `saturated fat`, `sugar`, and `protein`.

## Approach
### Fields
#### Minimum required
1. The recipe ID and name, for identification purposes.
2. Measure of recipe popularity, such as likes or ratings, converted to a common scoring system as required.
3. The minimum nutritional values outlined in the scope (`calories`, `saturated fat`, `sugar`, `protein`) with units preferably in `grams`. Or, if the absolute value is not available, the `Percent of Daily Values (PDV)`.
4. Meal type classified to a single meal type, i.e. `breakfast` OR `lunch` OR `dinner`. The type must be mutually exclusive.
5. Cuisine, as with the meal type.

#### Additional
1. 

### Food.com
1. Import `RAW_recipes.csv` and `RAW_interactions.csv` as `food_df` and `interactions_df` respectively.
2. Extract __ratings__ from `interactions_df` and merge the result with `food_df`.
3. Check for duplicate recipe IDs.
4. Parse the `tags` column:
    - Get a list of unique tags by stripping and splitting each row in the `tags` column.
    - The function `tag_check()` takes a list as an input, which it checks against the list of unique tags.
        - For meal types input, use:

            `breakfast, lunch, dinner`

        - For cuisines, use [Spoonacular supported cuisines](https://spoonacular.com/food-api/docs#Cuisines): `African, Asian, American, British, Cajun, Caribbean, Chinese, Eastern European, European, French, German, Greek, Indian, Irish, Italian, Japanese, Jewish, Korean, Latin American, Mediterranean, Mexican, Middle Eastern, Nordic, Southern, Spanish, Thai, Vietnamese` as the input.
    - Identify whether the minimum required tags exist in the `tags` column, otherwise, identify alternatives.
    - The function `parse_tags()` takes an input list (to search for in the `tags` column), the DataFrame to search, and the column name to save. This returns a count of each unique tag that exists in the DataFrame.
    - Reduce the merged DataFrame to rows with only one meal type.
5. Convert the `nutrition` column to nutritional values.
    - The list in the column corresponds to the following [Source](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/discussion/121778?select=RAW_recipes.csv&search=nutrition): `calories (#), total fat (PDV), sugar (PDV), sodium (PDV) , protein (PDV), saturated fat, carbohydrates (PDV)`.
    - The conversion units is based on a 2,000-calorie diet [Source](http://krupp.wcc.hawaii.edu/biol100l/nutrition/dailyval.pdf):
        - Total fat = 65g
        - Sugar = 50g
        - Sodium = 2400mg (2.4g)
        - Protein = 50g
        - Saturated fat = 20g
        - Carbohydrates = 300g
6. 

### Spoonacular API
1. 

### UNCATEGORISED
- Divide the nutritional values by the `servings` column.

## Decision Points
1. __Food.com meal types__. `dinner-party` is used in lieu of `dinner`, as it does not exist in the tag list.
2. __Negative WW Smart Points__. Although the WW Smart Points system does not allow for negative points, calculated negative values are allowed to present a wider range of values.


## Analysis

### Results

### Limitations


## Future Research
- __Longitudinal: Nutritional Value and Recipe Rating Evolution__
    Determine how recipe ratings and nutritional values change over time. The Food.com dataset has the date of each review, as well as the date the recipe is submitted. For Spoonacular, as the source URL for each recipe is provided, it is possible to determine the exact date using a few methods, such as Google's "inurl" functionality.

## References
- [1] Weight Watchers Smart Points Calculator [https://www.watcherspoint.com/weight-watchers-smart-points-calculator](https://www.watcherspoint.com/weight-watchers-smart-points-calculator)

- [2] Nutri-Score - A Simple Science-Based Nutritional Value Labelling System for the Food Industry [https://get.apicbase.com/nutri-score-science-based-nutritional-value-labelling-system/](https://get.apicbase.com/nutri-score-science-based-nutritional-value-labelling-system/)

- [3] Kaggle - Food.com Recipes and Interactions - Discussion [https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/discussion/121778?select=RAW_recipes.csv&search=nutrition](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/discussion/121778?select=RAW_recipes.csv&search=nutrition)







- [] How To Find When A Website Was First Published Or Launched [https://www.alphr.com/find-when-website-published-launched/](https://www.alphr.com/find-when-website-published-launched/)