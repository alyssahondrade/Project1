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
    1. [Fields](https://github.com/alyssahondrade/Project1#fields)
    2. [Food.com](https://github.com/alyssahondrade/Project1/tree/main#foodcom)
    3. [Spoonacular API](https://github.com/alyssahondrade/Project1/tree/main#spoonacular-api)
4. [Decision Points](https://github.com/alyssahondrade/Project1#decision-points)
5. [Analysis](https://github.com/alyssahondrade/Project1#analysis)
    1. [Results](https://github.com/alyssahondrade/Project1/tree/main#results)
    2. [Limitations](https://github.com/alyssahondrade/Project1/tree/main#limitations)
    3. [Conclusion](https://github.com/alyssahondrade/Project1/tree/main#conclusion)
6. [Future Research](https://github.com/alyssahondrade/Project1#future-research)
7. [References](https://github.com/alyssahondrade/Project1#references)
   1. [Research Concept](https://github.com/alyssahondrade/Project1/tree/main#research-concept)
   2. [Python Coding](https://github.com/alyssahondrade/Project1/tree/main#python-coding)
   3. [Data Visualisation](https://github.com/alyssahondrade/Project1/tree/main#data-visualisation)

## Introduction
### Goal
The goal of the project is to compare recipes from two popular recipe websites, Spoonacular and Food.com, and identify "healthy" recipes using two market-implemented measures:
1. Nutri-Scores: [https://www.mangiu.ch/index/](https://www.mangiu.ch/index/)
2. Weight Watchers (WW) Smart Points [https://www.weightwatchers.com/us/how-it-works/smartpoints](https://www.weightwatchers.com/us/how-it-works/smartpoints)

The project will investigate recipe popularity, meal types, and cuisines, and will be conducted as per the data analytics paradigm.

### Repository Structure

- `Images/` subdirectory contains the output figures as well as the flowchart images.
- `Output/` subdirectory contains the cleaned data, as a csv, for input to `recipe_exploration.ipynb`
- `Resources/` subdirectory contains the raw datasets, and the following subdirectories:
    - `Resources/01_recipe_IDs` contains the first Spoonacular API requests, containing recipe IDs.
    - `Resources/02_raw_data` contains the second Spoonacular API requests, containing raw recipe information data.
    - `Resources/03_simplified_data` contains the parsed data.
    - `Resources/04_complex_test` contains the extra data requested, given recipe IDs which had missing information.

### Dataset
-  [Kaggle - Food.com Recipes and Interactions](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?)

    The dataset consists of more than 200K recipes and over 1M recipe reviews over 18 years of user interactions and recipe submissions.
    - `RAW_interactions.csv` available from [[Source](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_interactions.csv)]
    - `RAW_recipes.csv` available from [[Source](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_recipes.csv)]

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

    As per the project goal, "healthiness" is defined by the two market-implemented measures: Nutri-Scores and WW Smart Points. Nutri-Scores is based on a five colour-coded letter grade (A, B, C, D, E), where `A` is the highest and healthiest score. WW Smart Points is a scoring system using calories, saturated fat, sugar, and protein, where the lower the number the healthier the food is. Calories provides the baseline, saturated fat and sugar increase the score, and protein decreases the score.

- __What is considered as a "recipe"?__

    All submissions on both websites are considered as a "recipe", regardless if the recipe is a meal or a drink.

- __What is a meal type?__

    For simplicity, meal types have been narrowed to `breakfast`, `lunch`, and `dinner`, or equivalents if unavailable.

- __What cuisines will be considered?__

    As a minimum, the cuisine type must be available in both datasets.

### Nutritional Values
The nutritional values of interest will be reduced to the minimum required for calculating the "health" score:

`calories, saturated fat, sugar, protein`

Both datasets provide `Percent of Daily Values (PDV)`, which assumes a single serving [[Source](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/discussion/121778?select=RAW_recipes.csv&search=nutrition)], a necessary consideration when using the values for calculations.

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

The flowchart below summarises the process.

![Data Cleaning Flowchart](https://github.com/alyssahondrade/Project1/blob/main/Images/data_cleaning_flowchart.png)

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
    - Explore outliers in the `wws_points` column:
        - Create a histogram to gain an understanding of the data distribution.
        - 

8. Nutritional Values Correlation
    - Calculate the correlation matrix between:

        `'Rating', 'Calories', 'Total Fat (PDV)', 'Sugar (PDV)', 'Sodium (PDV)', 'Protein (PDV)', 'Saturated Fat (PDV)', 'Carbohydrates (PDV)'`

9. Initial Visualisation
    - Meal Types, for each type:
        - Create a groupby `meal_type` DataFrame, calculating the median as it is a skewed dataset.
        - Create a bin based on the minimum and maximum `wws_points`, to bin the DataFrame to these ranges.
        - Create a plot with the:
            - Binned `wws_points` as the x-axis
            - `rating` as the y-axis
            - `protein_g` as the bubble size
            - A nutritional value as the bubble colour
        - Calculate the median `wws_points`, set this as the limit for "healthiness".
            - Calculate the percentages of each meal type that sit above this limit.
            - Create a bar chart of the average `wws_points` for each meal type.
            - Sort the dataset by the percentage value, as it is visually easier to understand and mentall sort the bar size compared to the written percentage value.
            - Annotate the percentage value over the corresponding meal type for clarity.
    - Cuisines
        - As with meal types, repeat the process for cuisines.
        - Using Geoapify's Geocoding API, request the latitude and longitude for the following cuisine regions as equivalents:

             `Congo, 'Louisiana, USA', 'Caribbean', 'France', 'German', 'Greece', 'India',
       'Ireland', 'Japan', 'Korea', 'Mexico', 'Spain', 'Thailand', 'Vietnam'`
        
        - Append the acquired latitudes and longitudes as a new column to the binned DataFrame.
        - Create a map visualisation given:
            - `rating` as the bubble colour
            - `wws_points` as the bubble sizes
            - `CartoLight` for the tiles
    - Popularity
    - 5-Star Ratings
    - Lowest WW Smart Points

10. 
11. 
12. 

### Spoonacular API

The diagram below summarises the API testing workflow.

![API Testing](https://github.com/alyssahondrade/Project1/blob/main/Images/spoonacular_apiTesting.png)

The diagram below summarises the Spoonacular cleaning process until the point where it was decided it was no longer feasible to continue cleaning the dataset.

![API Data Collection](https://github.com/alyssahondrade/Project1/blob/main/Images/spoonacular_datacollection.png)

1. API Testing
    - Test types and outcomes:
        - `Complex Search` and `Recipe Information`, returns the same recipes each time.
        - `Random Recipes` and `Recipe Information`, returns random recipes but not cost-effective.
        - `Random Recipes` and `Nutrition by ID`, identified as the more cost-effective method.
        - `Complex Search` with comprehensive parameters, to ensure `cuisines` and `meal type` are relevant. Note: this still returns the same recipes each time, unless the parameters are updated. This method intended to bulk up the existing datasets.
    - The general process is:
        - Get a list of recipe IDs, often with generic metadata that lacks nutritional value information.
        - Get a comprehensive report, with the nutritional value information.
        - Export the raw data to a csv.
        - Whilst the API response is active, parse information that would otherwise be converted to a list in the csv.

2. Data Collection
    - Functions:
        - __Method 1 - `spoonacular_v1(type)`__: The `type` parameter distinguishes between the `Recipe Information` and `Nutrition by ID`.
        - __Method 2 - `spoonacular_v2()`__: The most cost-efficient method, returning a single request with all the necessary details.
        - __Method 3 - `spoonacular_v3()`__: A new data collection function created to collect the recipe information only.
    - Due to the iterative process with testing and limited API requests, the raw data folder had collected datasets of various formats. This means that the cleaning process needed to be staged, depending on raw dataset formatting.
    - Get recipe IDs from raw data: As the process is conducted in 2 stages, recipe information requests in the second stage was often incomplete. To optimise API requests, a list of recipe IDs with missing information is extracted from the raw datasets.
        - Look through `Resources/01_recipe_IDs` and `Resources/02_raw_data` subdirectories for a list of all recipe IDs collected.
        - Look through `Resources/03_simplified_data` and `Resources/04_complex_test` subdirectories for a list of all recipes with recipe information.
        - Identify the recipes that are missing recipe information using the `difference()` method given sets.
        - Use `spoonacular_v3()` to get the data, given recipe IDs.
    - Append the missing data to the simplified datasets, given two sources:
        - From another column of the same dataset, OR
        - From the raw datasets.
    - Use the function `get_nutrition(string, nutritional_value)` to parse the correct nutritional values from the string.

3. 

## Decision Points
1. __Food.com meal types__.

    `dinner-party` is used in lieu of `dinner`, as it does not exist in the tag list.

2. __Negative WW Smart Points__.

    Although the WW Smart Points system does not allow for negative points, calculated negative values are allowed to present a wider range of values.

3. __Cuisine Match__.

- Removed `Asian` and `European` from the list of cuisines to match. Doing so improved the range of available cuisines for the cuisine analysis.
- Also removed `American` as it accounted for greater than the rest of the cuisines put together.

4. __Ingredients__.

    Explored whether it was feasible to use the ingredients, grouped by food groups, as another measure of healthiness or point of interest. Although it was possible to parse the unique list of ingredients, a machine learning model is required to classify this. A brief search online identified there is no API or online tool that solves this problem, and due to project resourcing and time restraints, this has been identified as future research instead.

5. 

## Analysis

### Results
#### Question 1

__Are more popular/higher-rated recipes healthier? What is the health rating of the highest-rated recipes?__

There is no apparent correlation between health rating and recipe popularity, this is denoted in the figure below.

If the health rating increased with popularity, the plot would be right-skewed, as the majority of the points would have a lower WW Smart Points. This figure is normally-shaped, with a peak in the range of -4 to 6 points.

![Question 1 - Distribution of 5-star Recipes](https://github.com/alyssahondrade/Project1/blob/main/Images/Question%201%20-%20Distribution%20of%205-star%20Recipes.png)

The figure below shows the table of results for the top 10 recipes with a 5-star rating, sorted by the WW Smart Point. This shows the healthiest recipes with the highest ratings.

![Question 1 - Healthiest Recipes](https://github.com/alyssahondrade/Project1/blob/main/Images/Question%201%20-%20Healthiest%20Recipes.png)

---

#### Question 2
__What meal type (i.e., breakfast, lunch, or dinner) have the healthiest/unhealthiest (percentage) recipes?__

The figure below shows the percentage of healthy recipes per meal. The bar plot is sorted, from left to right, by the percentage of healthy recipes for a given meal type. This was done delibately, as it is easier to understand the bar height visually, as compared to understanding the order of percentages.

The figure demonstrates that "Lunch" has the healthiest percentage of recipes, as well as the lowest average WW Smart Points. It is important to note that both "Breakfast" and "Lunch" have the same average WW Smart Points. This means, that "Breakfast" and "Lunch" recipes tend to be healthier compared to "Dinner" recipes.

![Question 2 - Percentage of Healthy Recipes per Meal](https://github.com/alyssahondrade/Project1/blob/main/Images/Question%202%20-%20Percentage%20of%20Healthy%20Recipes%20per%20Meal%20Type.png)

The following three figures will explore this further, by analysing the nutritional values.

To provide context, the WW Smart Points have been binned, to create a line-plot rate of change in the health rating, as opposed to a scatter plot that will plot each datapoint. The y-axis is the average rating, while the x-axis is the binned WW Smart Points range.
- Instead of a line plot, a scatter plot was used to add an extra dimension of data: protein in grams. The bigger the size of the bubble, the greater the protein content.
- A color bar is also used to add another dimension of data, in this case the nutritional value of interest. The colour purple denotes lower values, whilst the colour yellow denotes higher values.

It is important to note that the location of the datapoints for all three figures are the same, it is the colour bar values that differ. Recall that the lower the WW Smart Point value, the healthier the recipe is.
- For "Lunch", there appears to be a slightly negative trend in rating as the WW Smart Point value increases. This means that the popularity of the recipe decreases as it becomes less healthy.
- For "Dinner", although there is a slightly positive trend, there is a decrease followed by an increase in popularity as the recipes traverse from healthiest to least healthy. This could be interpreted as: people are more likely to rate a recipe very high if it is very healthy or very unhealthy. There could be two factors explaining this:
    - For the 5-star healthy recipes, these could have been posted on websites that are health-focused, hence people would rate healthy AND delicious recipes very high.
    - For the 5-star unhealthy recipes, these are likely a result from people who rated the recipe very high for taste, rather than how healthy the recipe is.

The figure below demonstrates that across all three meal types, there tends to be a higher protein for healthier recipes. As expected, the protein content decreases as the recipe becomes less healthy. It is also reasonable that the sugar content increases as the recipe becomes less healthy. This is in line with the equation used to calculate the WW Smart Point, with a positive factor for sugar, and a negative factor for protein.

![Question 2 - WW Smart Points & Sugar](https://github.com/alyssahondrade/Project1/blob/main/Images/Question%202%20-%20WW%20Smart%20Points%20%26%20sugar_g.png)

The figure below demonstrates the differences in how saturated fat is distributed across the meal types.
- For "Breakfast", there tends to be a high saturated fat content for recipes considered "healthy", with the saturated fat decreasing as the health rating decreases (an increase in WW Smart Points).
- For "Lunch", there is a pocket in the middle where the saturated fat contents fluctuates for 4 data points, before decreasing for 2 data points (purple), then increasing for the last (yellow). This somewhat follows the trend in "Breakfast", with the exception of the low saturated fat content for the healthiest recipes, and the final data point where there is a significant increase in saturated fat. This could be interpreted as: a limit exists in the increase of WW Smart Points and saturated fat, where recipes in the unhealthy end of the range take on a higher saturated fat, which also subsequently results in a decrease in recipe popularity.
- For "Dinner", the pattern appears to be relatively middle-range saturated fat for healthier recipes (blue), followed by a decrease (purple), then a sharp spike to a high content (yellow), similar to the "Lunch" recipes.

![Question 2 - WW Smart Points & Saturated Fat](https://github.com/alyssahondrade/Project1/blob/main/Images/Question%202%20-%20WW%20Smart%20Points%20%26%20sat_fat_g.png)

The figure below demonstrates the differences in calories across the meal types. For all three, it is apparent that the less healthy recipes tend to have higher caloric content. However, this is most evident for "Breakfast" and "Dinner". Calories are not inherently a "poor" measure of health, in that, a high-calorie meal is not "unhealthy" or "bad". It is more important to understand when the high-calorie meal is eaten, as extra calories consumed without using up the energy results in it being stored as fat [[Source](https://www.chubb.com/hk-en/chubblifebalance/biggest-meal.html#:~:text=Not%20only%20does%20a%20big,will%20be%20stored%20as%20fat.)]. Hence, this figure demonstrates that healthy "Dinner" recipes tend to have the highest calorie content, which could be contrary to weight loss despite eating "healthy".

![Question 2 - WW Smart Points & Calories](https://github.com/alyssahondrade/Project1/blob/main/Images/Question%202%20-%20WW%20Smart%20Points%20%26%20Calories.png)

__What is the most popular ingredient for each meal?__

This question has been interpreted as what is the most used ingredient for each meal. The table below shows the top 3 ingredients, and its percentage of use per meal type.
- For "Breakfast", "Eggs" is used in 67% of recipes, followed by "Butter" then "Baking Powder".
- For "Lunch", "Salt" is used in 37% of recipes, followed by "Onion" then "Olive Oil".
- For "Dinner", "Salt" is also the most used ingredient, followed by "Olive Oil" then "Garlic Cloves".

A more interesting analysis for this would be to identify the most used fruit, vegetable, protein, etc., per food group. However, a machine learning algorithm is required to classify the ingredients to each category - a topic for future research to extend this project.

![Question 2 - Most Popular Ingredient](https://github.com/alyssahondrade/Project1/blob/main/Images/Question%202%20-%20Most%20Popular%20Ingredient.png)

---

#### Question 3 - Which cuisine has the healthiest recipes?

The figure below denotes the count of recipes per cuisine. This is important to keep in mind when the cuisines are ranked, to identify the percentages healthy recipes per cuisine. The weighting will be significant for cuisines with less recipes in the database, such as: `Thai and Vietnamese`.

![Question 3 - Number of Recipes per Cuisine](https://github.com/alyssahondrade/Project1/assets/129482049/959299df-2a65-49bf-aa80-fdbc49c31c5a)

The figure below shows a plot of the average WW Smart Points for each cuisine. As with the meal type plot previously, this bar chart is organised by increasing percentages. This is because it is visually easier to see changes in the heights of the bar, compared to reading percentage values. `Cajun` cuisine has the healthiest recipes (followed by `Japanese` cuisine), both in terms of the average WW Smart Points, and the percentageof healthy recipes submitted.

![Question 3 - Percentage of Healthy Recipes per Cuisine](https://github.com/alyssahondrade/Project1/assets/129482049/3791cd47-9f07-437c-acc4-0d8376d61a40)


Finally, the figure below is a visual representation of where each cuisine is located around the world.
- The size of the bubble represents the average WW Smart Point, meaning the bigger the bubble the less healthy recipes tend to be.
- The colour of the bubble represents the average rating, meaning the darker the colour, the higher the average rating.

![Question 3 - Cuisine Map](https://github.com/alyssahondrade/Project1/blob/main/Images/Question%203%20-%20Cuisine%20Map.jpg)

### Limitations
- The dataset is limited to recipes available on Food.com, over an 18-year period. The data was crawled and there have been no updates, meaning it is only current to a certain date.


### Conclusion
The one recipe to rule them all:



## Future Research
- __Longitudinal: Nutritional Value and Recipe Rating Evolution__

    Determine how recipe ratings and nutritional values change over time. The Food.com dataset has the date of each review, as well as the date the recipe is submitted. For Spoonacular, as the source URL for each recipe is provided, it is possible to determine the exact date using a few methods, such as Google's "inurl" functionality [[Source](https://www.alphr.com/find-when-website-published-launched/)].

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

- [] Nested dictionary to multiindex dataframe where dictionary keys are column labels [https://stackoverflow.com/questions/24988131/nested-dictionary-to-multiindex-dataframe-where-dictionary-keys-are-column-label](https://stackoverflow.com/questions/24988131/nested-dictionary-to-multiindex-dataframe-where-dictionary-keys-are-column-label)

### Data Visualisation
- [] Matplotlib - Iterate Subplot Axis Array through Single List [https://stackoverflow.com/questions/20288842/matplotlib-iterate-subplot-axis-array-through-single-list](https://stackoverflow.com/questions/20288842/matplotlib-iterate-subplot-axis-array-through-single-list)

- [] How to set spacing between subplots in Matplotlib [https://www.geeksforgeeks.org/how-to-set-the-spacing-between-subplots-in-matplotlib-in-python/](https://www.geeksforgeeks.org/how-to-set-the-spacing-between-subplots-in-matplotlib-in-python/)

- [] plt.show shows full graph but savefig is cropping the image [https://stackoverflow.com/questions/37427362/plt-show-shows-full-graph-but-savefig-is-cropping-the-image](https://stackoverflow.com/questions/37427362/plt-show-shows-full-graph-but-savefig-is-cropping-the-image)

- [] Save a pandas dataframe as an image (module: dataframe-image) [https://predictivehacks.com/?all-tips=save-a-pandas-dataframe-as-an-image](https://predictivehacks.com/?all-tips=save-a-pandas-dataframe-as-an-image)

- [] Bubble Plots in Python [https://www.askpython.com/python/examples/bubble-plots-in-python](https://www.askpython.com/python/examples/bubble-plots-in-python)

- [] Making a Heatmap from Pandas DataFrame [https://stackoverflow.com/questions/12286607/making-heatmap-from-pandas-dataframe](https://stackoverflow.com/questions/12286607/making-heatmap-from-pandas-dataframe)

- [] Change Space between Bars when drawing multiple Barplots in Pandas [https://stackoverflow.com/questions/34674558/how-to-change-space-between-bars-when-drawing-multiple-barplots-in-pandas](https://stackoverflow.com/questions/34674558/how-to-change-space-between-bars-when-drawing-multiple-barplots-in-pandas)

- [] Geoapify's Geocoding API [https://www.geoapify.com/geocoding-api](https://www.geoapify.com/geocoding-api)

- [] Holoviews Tiles [https://holoviews.org/reference/elements/bokeh/Tiles.html](https://holoviews.org/reference/elements/bokeh/Tiles.html)

- [] Setting Different Bar Color in Matplotlib [https://www.tutorialspoint.com/setting-different-bar-color-in-matplotlib](https://www.tutorialspoint.com/setting-different-bar-color-in-matplotlib)
