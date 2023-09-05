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
The nutritional values of interest will be reduced to the minimum required for calculating the "health" score:

`calories, saturated fat, sugar, protein`

Both datasets provide `Percent of Daily Values (PDV)`, which assumes a single serving [Source](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/discussion/121778?select=RAW_recipes.csv&search=nutrition), a necessary consideration when using the values for calculations.

## Approach
### Fields
#### Minimum required
1. The recipe ID and name, for identification purposes.

2. Measure of recipe popularity, such as likes or ratings, converted to a common scoring system as required.

3. The minimum nutritional values outlined in the scope (`calories`, `saturated fat`, `sugar`, `protein`) with units preferably in `grams`. Or, if the absolute value is not available, the `PDV`.

4. Meal type classified to a single meal type, i.e. `breakfast` OR `lunch` OR `dinner`. The type must be mutually exclusive.

5. Cuisine, as with the meal type.

#### Additional
1. 

### Food.com
1. Import the Food.com datasets as Pandas DataFrames:

    `RAW_recipes.csv` as `food_df`
   
    `RAW_interactions.csv` as `interactions_df`

2. Extract __ratings__ from `interactions_df` and merge the result with `food_df`.

3. Check for duplicate recipe IDs.

4. Parse the `tags` column:
    - Get a list of unique tags by stripping and splitting each row in the `tags` column.
    - The function `tag_check()` takes a list as an input, which it checks against the list of unique tags.
        - For meal types input list, use:

            `breakfast, lunch, dinner`

        - For cuisines input list, use [Spoonacular supported cuisines](https://spoonacular.com/food-api/docs#Cuisines):

            `African, Asian, American, British, Cajun, Caribbean, Chinese, Eastern European, European, French, German, Greek, Indian, Irish, Italian, Japanese, Jewish, Korean, Latin American, Mediterranean, Mexican, Middle Eastern, Nordic, Southern, Spanish, Thai, Vietnamese`

    - Identify whether the minimum required tags exist in the `tags` column, otherwise, identify alternatives.
    - The function `parse_tags()` takes an input list (to search for in the `tags` column), the DataFrame to search, and the column name to save. This returns a count of each unique tag that exists in the DataFrame.
    - Reduce the merged DataFrame to rows with only one meal or cuisine type.
    - Add columns for the derived meal or cuisine value.

5. Convert the `nutrition` column to nutritional values.
    - The list in the column corresponds to the following [Source](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/discussion/121778?select=RAW_recipes.csv&search=nutrition):

        `calories (#), total fat (PDV), sugar (PDV), sodium (PDV) , protein (PDV), saturated fat, carbohydrates (PDV)`.
      
    - The conversion units is based on a 2,000-calorie diet [Source](http://krupp.wcc.hawaii.edu/biol100l/nutrition/dailyval.pdf):
        - Total fat = 65g
        - Sugar = 50g
        - Sodium = 2400mg (2.4g)
        - Protein = 50g
        - Saturated fat = 20g
        - Carbohydrates = 300g
    - Use `iterrows()` to loop through the DataFrame and parse each row to the nutritional values as a float.
    - Use `iterrows()` to append the absolute value of each nutritional value (in grams) to its corresponding column.

6. Calculate the WW Smart Points
    - WW Smart Points Equation:

        $$SmartPoint = (Calories * 0.0305) + (Saturated Fat * 0.275) + (Sugar * 1.2) - (Protein * 0.98)$$

    - User `iterrows()` to loop through the DataFrame, get each value, and calculate the points.
    - Convert the calculated value to an integer.

7. Outlier identification.
    - Calculate the descriptive statistics for the cleaned DataFrame.
    - Use `describe()` to get the baseline statistics:
  
        `mean, std, min, 25%, 75%, max`

    - The function `add_iqr()` takes the `describe()` output (Series or DataFrame) as the input, to append values for:

        `iqr, lower_bounds, upper_bounds`

    - Explore outliers in the `PDV` columns, no need to remove these values as the PDV is for a single serving [Source](https://www.fda.gov/food/new-nutrition-facts-label/how-understand-and-use-nutrition-facts-label).
    - Explore outliers in the `minutes` column, and remove:
        - Recipes with `minutes` greater than the calculated upper bounds, and
        - Recipes with `0` minutes
8. 
9. 

### Spoonacular API
1. 



## Decision Points
1. __Food.com meal types__. `dinner-party` is used in lieu of `dinner`, as it does not exist in the tag list.
2. __Negative WW Smart Points__. Although the WW Smart Points system does not allow for negative points, calculated negative values are allowed to present a wider range of values.
3. __Cuisine Match__. Removed `Asian` and `European` from the list of cuisines to match. Doing so improved the range of available cuisines for the cuisine analysis. Finally, also removed `American` as it account for greater than the rest of the cuisines put together.


## Analysis

### Results

### Limitations


## Future Research
- __Longitudinal: Nutritional Value and Recipe Rating Evolution__
    Determine how recipe ratings and nutritional values change over time. The Food.com dataset has the date of each review, as well as the date the recipe is submitted. For Spoonacular, as the source URL for each recipe is provided, it is possible to determine the exact date using a few methods, such as Google's "inurl" functionality.

- __Ingredients Classification: Food Group Percentage as Measure of Healthiness__
    Given a recipe, identify the percentage breakdown per food group. This requires an ML classification model to categorise to the food groups. A pie chart can be used to visually display the breakdown and inform decision making around food and diet.

- __Spoonacular's 'Taste by ID' and Preferences__
    Spoonacular's widget scores each recipe's:

        `sweetness, saltiness, sourness, bitterness, savoriness, fattiness, spiciness`

    Based on the user's preferences and tastes, return healthy recipes by percentage of similarity. This tool can be used to improve diets whilst also considering the user's preferences and tastes.

- __Regional Recipe Investigation__
    Since cuisines for `American, Asian, European` were removed, can investigate these broad cuisines more closely.


## References

### Research Concept
- [1] Weight Watchers Smart Points Calculator [https://www.watcherspoint.com/weight-watchers-smart-points-calculator](https://www.watcherspoint.com/weight-watchers-smart-points-calculator)

- [2] Nutri-Score - A Simple Science-Based Nutritional Value Labelling System for the Food Industry [https://get.apicbase.com/nutri-score-science-based-nutritional-value-labelling-system/](https://get.apicbase.com/nutri-score-science-based-nutritional-value-labelling-system/)

- [3] Kaggle - Food.com Recipes and Interactions - Discussion [https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/discussion/121778?select=RAW_recipes.csv&search=nutrition](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/discussion/121778?select=RAW_recipes.csv&search=nutrition)

- [4] How to Understand and Use the Nutrition Facts Label [https://www.fda.gov/food/new-nutrition-facts-label/how-understand-and-use-nutrition-facts-label](https://www.fda.gov/food/new-nutrition-facts-label/how-understand-and-use-nutrition-facts-label)

### Python Coding
- [] List all Subdirectories in a Directory Python [https://www.techiedelight.com/list-all-subdirectories-in-directory-python/](https://www.techiedelight.com/list-all-subdirectories-in-directory-python/)

- [] Iterate over Files in a Directory [https://www.geeksforgeeks.org/how-to-iterate-over-files-in-directory-using-python/](https://www.geeksforgeeks.org/how-to-iterate-over-files-in-directory-using-python/)

- [] Python `os direntry` name attribute [https://www.geeksforgeeks.org/python-os-direntry-name-attribute/](https://www.geeksforgeeks.org/python-os-direntry-name-attribute/)

- [] Python string `find()` with examples [https://sparkbyexamples.com/python/python-string-find-with-examples/](https://sparkbyexamples.com/python/python-string-find-with-examples/)

- [] Print common elements between two lists [https://www.geeksforgeeks.org/python-print-common-elements-two-lists/](https://www.geeksforgeeks.org/python-print-common-elements-two-lists/)

- [] How to Import Functions from Another Jupyter Notebook [https://saturncloud.io/blog/how-to-import-functions-from-another-jupyter-notebook/](https://saturncloud.io/blog/how-to-import-functions-from-another-jupyter-notebook/)

- [] Python function documentation example [https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)

### Data Visualisation
- [] Bubble Plots in Python [https://www.askpython.com/python/examples/bubble-plots-in-python](https://www.askpython.com/python/examples/bubble-plots-in-python)

- [] Making a Heatmap from Pandas DataFrame [https://stackoverflow.com/questions/12286607/making-heatmap-from-pandas-dataframe](https://stackoverflow.com/questions/12286607/making-heatmap-from-pandas-dataframe)

- [] Change Space between Bars when drawing multiple Barplots in Pandas [https://stackoverflow.com/questions/34674558/how-to-change-space-between-bars-when-drawing-multiple-barplots-in-pandas](https://stackoverflow.com/questions/34674558/how-to-change-space-between-bars-when-drawing-multiple-barplots-in-pandas)

### Future Research
- [] How To Find When A Website Was First Published Or Launched [https://www.alphr.com/find-when-website-published-launched/](https://www.alphr.com/find-when-website-published-launched/)