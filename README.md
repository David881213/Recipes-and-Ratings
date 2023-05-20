# Recipes-and-Ratings

By Pin-Hsuan Lai (pilai@ucsd.edu), Pin-Yeh Lai (p2lai@ucsd.edu)
## Introduction

These data sets come from food.com and contain recipes and ratings of recipes shared by food.com users. Due to the sheer volume of data, we only have recipes and reviews posted since 2008.

`RAW_recipes.csv` dataset contains 83782 rows of data of the recipe name, recipe ID number, recipe preparation time, User ID who submitted this recipe, recipe payment time, food.com tags for recipe, nutrition information, recipe step number, step content, and recipe description from the User. description.

`RAW_interactions` dataset contains 731927rows of data of the user ID, recipe ID, date of interaction, rating, and review text.

After we merged and cleaned the data, we created a new dataset with 23449 rows and 27 columns, which contains information on people's preferences, ratings and recipe nutrients, steps, etc. which can be further analysis.

After digging deeper, we found that there was a huge difference in the number of ratings between 2013 and 2014 in the overall data, and we wanted to learn more about this information. Therefore, we decided to study whether the distribution of people's ratings of recipes remained the same before and after 2014, which would allow us to better understand the difference in the overall satisfaction level of recipes before and after the huge rating gap and could be useful for considering the creation of new recipes in the future.

## Cleaning and EDA

















This is a test:
<iframe src="assets/Number of ratings.html" width=800 height=600 frameBorder=0></iframe>

This is a test od merged head:

| rating_status   |   minutes |
|:----------------|----------:|
| False           |   103.49  |
| True            |   154.942 |