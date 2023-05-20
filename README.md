# Recipes-and-Ratings

By Pin-Hsuan Lai (pilai@ucsd.edu), Pin-Yeh Lai (p2lai@ucsd.edu)
## Introduction

These data sets come from food.com and contain recipes and ratings of recipes shared by food.com users. Due to the sheer volume of data, we only have recipes and reviews posted since 2008.

`RAW_recipes.csv` dataset contains 83782 rows of data of the recipe name, recipe ID number, recipe preparation time, User ID who submitted this recipe, recipe payment time, food.com tags for recipe, nutrition information, recipe step number, step content, and recipe description from the User. description.

`RAW_interactions` dataset contains 731927rows of data of the user ID, recipe ID, date of interaction, rating, and review text.

After we merged and cleaned the data, we created a new dataset with 23449 rows and 27 columns, which contains information on people's preferences, ratings and recipe nutrients, steps, etc. which can be further analysis.

After digging deeper, we found that there was a huge difference in the number of ratings between 2013 and 2014 in the overall data, and we wanted to learn more about this information. Therefore, we decided to study whether the distribution of people's ratings of recipes remained the same before and after 2014, which would allow us to better understand the difference in the overall satisfaction level of recipes before and after the huge rating gap and could be useful for considering the creation of new recipes in the future.

## Cleaning and EDA

After we have a clear understanding of the information contained in these two datasets, we need to do some data cleaning to be able to analyze the data better.

1.	Merge the two datasets together.
We first download the two required files RAW_recipes.csv and RAW_interactions and then merge them together with recipes as left using `merge` and with `how='left'`.

2.	In the new dataset, we than replace all 0s in column `rating` to np.nan. 
We select the target column `rating` and `apply` `lambda` to change all 0 to np.nan. 
      The reason we changed the value of the rating to 0 to np.nan is that we observed both positive and negative reviews of people with a rating of 0 in the data, which means that a rating of 0 does not strictly depend on the reviewer's evaluation of the recipe is lower than 1. This means that a rating of 0 is not only related to the rating itself; but may also be influenced by other columns. Therefore, the rating's missing type is not NMAR.
      
3.	Calculate the average of rating.
We used `gruopby` and `mean` to compute the average rating of each ids as a series and merge it back to the original dataset. This can create a more intuitive analysis for further use of investigation.

4.	Change datatype of some object columns to list columns.
We convert some object tpye columns that contain data in list form to list data type using `eval` and `apply`.

5.	Split the column `nutrition` into 7 corresponding columns.
We use `tolist` to breakdown column `nutrition` and assign each slice to its own column. There are totally 7 columns have been create, `calories`, `total_fat`, `sugar`, `sodium`, `protein`, `saturated_fat`, `carbohydrates`, respectively.
      
6.	Get time information from `submitted` and `date` columns, then make datatype to timestamp.
We use `pd.to_datetime` to transformed data in the two columns to timestamp.
      
### Head of the `merged` DataFrame:

| name                                 |     id |   minutes |   contributor_id | submitted           | tags                                               |   n_steps | steps                                              | description                                       | ingredients                                        |   n_ingredients |          user_id |   recipe_id | date                |   rating | review                                            |   ave_rating |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates |
|:-------------------------------------|-------:|----------:|-----------------:|:--------------------|:---------------------------------------------------|----------:|:---------------------------------------------------|:--------------------------------------------------|:---------------------------------------------------|----------------:|-----------------:|------------:|:--------------------|---------:|:--------------------------------------------------|-------------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course...] |        10 | ['heat the oven to 350f and arrange the rack i...] | these are the most; chocolatey, moist, rich, d... | ['bittersweet chocolate', 'unsalted butter', '...] |               9 | 386585           |      333281 | 2008-11-19 00:00:00 |        4 | These were pretty good, but took forever to ba... |            4 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'cuisin...] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixi...] | this is the recipe that we use at my school ca... | ['white sugar', 'brown sugar', 'salt', 'margar...] |              11 | 424680           |      453467 | 2012-01-26 00:00:00 |        5 | Originally I was gonna cut the recipe in half ... |            5 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course...] |         6 | ['preheat oven to 350 degrees', 'spray a 2 qua...] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken sou...]	|               9 |  29782           |      306168 | 2008-12-31 00:00:00 |        5 | This was one of the best broccoli casseroles t... |            5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course...] |         6 | ['preheat oven to 350 degrees', 'spray a 2 qua...] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken sou...] |               9 |      1.19628e+06 |      306168 | 2009-04-13 00:00:00 |        5 | I made this for my son's first birthday party ... |            5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course...] |         6 | ['preheat oven to 350 degrees', 'spray a 2 qua...] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken sou...] |               9 | 768828           |      306168 | 2013-08-02 00:00:00 |        5 | Loved this.  Be sure to completely thaw the br... |            5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |

## Univariate Analysis

## Bivariate Analysis

## Interesting Aggregates

## Assessment of Missingness

## Hypothesis Testing











This is a test:
<iframe src="assets/Number of ratings.html" width=800 height=600 frameBorder=0></iframe>

This is a test od merged head:

| rating_status   |   minutes |
|:----------------|----------:|
| False           |   103.49  |
| True            |   154.942 |