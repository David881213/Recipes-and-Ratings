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
#### 1.	We generate the histogram that state the distribution of total rating.

<iframe src="assets/Number of ratings.html" width=800 height=600 frameBorder=0></iframe>

We can clearly find that rating of 5 star is the most rated, which is far more than others. So, we may should focus on the distribution of the ratings.

#### 2.	We generate the histogram of the number of total reviews in each year.

<iframe src="assets/Number of review in each year.html" width=800 height=600 frameBorder=0></iframe>

We can find that the number of reviews per year is declining almost every year since 2009, overall showing a decreasing trend.

## Bivariate Analysis

#### 1.	We like to see the distribution of rating changes over time, so we generate line chart that ratings of 1 to 5 star and year.

<iframe src="assets/Multiple Rating Chart.html" width=800 height=600 frameBorder=0></iframe>

From the line chart, we can find out that ranting 5 star is always the most rating within the 5 ratings, but the total number of rating decline in a rapid speed each year. This help to observe the distribution of rating and consider to generate hypothesis test questions.

#### 2.	We plot the scatter plot of the relationship between total fat and calories.

<iframe src="assets/Scatter plot of total fat on calories.html" width=800 height=600 frameBorder=0></iframe>

We can see that there is a positive correlation between total fat and calories.

## Interesting Aggregates

#### We create pivot table by 'date year', 'rating' column and exam aggregate statistics of mean calories.

|   date year |     1.0 |     2.0 |     3.0 |     4.0 |     5.0 |
|------------:|--------:|--------:|--------:|--------:|--------:|
|        2008 | 423.078 | 439.531 | 399.924 | 393.426 | 381.724 |
|        2009 | 402.172 | 412.855 | 406.237 | 412.033 | 411.16  |
|        2010 | 374.292 | 407.845 | 448.419 | 386.521 | 392.315 |
|        2011 | 424.43  | 458.717 | 420.413 | 393.102 | 405.219 |
|        2012 | 485.634 | 443.383 | 377.45  | 404.506 | 412.093 |
|        2013 | 467.188 | 464.784 | 430.592 | 406.745 | 438.165 |
|        2014 | 478.428 | 419.141 | 483.446 | 406.731 | 443.62  |
|        2015 | 574.179 | 464.92  | 494.436 | 464.476 | 487.102 |
|        2016 | 483.974 | 501.667 | 392.359 | 454.129 | 481.53  |
|        2017 | 567.373 | 470.976 | 509.53  | 465.268 | 457.456 |
|        2018 | 581.357 | 520.778 | 467.675 | 456.574 | 491.634 |

We can find from the pivot table that basically 5-star rating always had the lowest mean calories, on the contrary, 1-star has the highest. At the same time, we also found that the overall mean calories are increasing year by year. 

## Assessment of Missingness
First, let's take a look at the missing values in the dataset:

|             |   missing |
|:------------|----------:|
| name        |         1 |
| description |       114 |
| user_id     |         1 |
| recipe_id   |         1 |
| date        |         1 |
| rating      |     15036 |
| review      |        58 |
| ave_rating  |      2777 |
| date year   |         1 |

1. The missing values of `user_id`, `recipe_id`, `date` and `date year` are missing values due to merging two data frames, and the reason of missing is not related to their own column themselves are not related. Therefore, we believe that their missing type is not NMAR.

2. We observe that some users are used to not taking names or writing descriptions when submitting recipes, and the missing `name` and `description` may be related to the `user_id`, so we also believe that the missing type of `name` and `description` is not NMAR.

3. For the missing value of the `review`, we observe that the `review` is a missing value when the reviewer uses only pictures in the review. the missingness is caused by the rules in the data collection process rather than by the specific properties of the missing value itself, so we consider that the missing type of the `review` is not NMAR but MD.

4. `ave_rating` is the column we derived from the rating, so we believe the missing type of `ave_rating` is not NMAR.

5. According to what we have mentioned in Cleaning and EDA, the `rating`'s missing value does not depend on its own specific properties, so we believe that `ratiing`'s missing type is not NMAR.

Overall, we believe that all columns that contain a missing value have a missing type that is not NMAR.

|             |   missing | NMAR   |
|:------------|----------:|:-------|
| name        |         1 | False  |
| description |       114 | False  |
| user_id     |         1 | False  |
| recipe_id   |         1 | False  |
| date        |         1 | False  |
| rating      |     15036 | False  |
| review      |        58 | False  |
| ave_rating  |      2777 | False  |
| date year   |         1 | False  |

### MAR Dependent determination

#### 1. Use permutation test to determine if values in a `rating` column are MAR dependent on `calories` column.

To test whether the `rating` column is MAR dependent on `calories` column:
We have our

**Null hypothesiss**: *H<sub>0</sub>*: the distribution of `calories` when `rating` is missing is the same as the distribution of `calories` when `rating` is not missing. And 

**Alternative hypothesis**: *H<sub>a</sub>*: the distribution of `calories` when `rating` is missing is not the same as the distribution of `calories` when `rating` is not missing.

First, let us looked at the mean of the calories based on the missingness of the rating values.

| rating_status   |   calories |
|:----------------|-----------:|
| False           |    415.103 |
| True            |    484.11  |

Calories by Rating Status:

<iframe src="assets/Calories by Rating Status.html" width=800 height=600 frameBorder=0></iframe>


Since `rating` is categorical and `calories` is numerical, we use mean difference as test statistics to run the permutation test. Then, we perform the permutation test of mean difference and find out the p-value.

The result of permutation test and p-value:
<iframe src="assets/Permutation test of rating and calories.html" width=800 height=600 frameBorder=0></iframe>

Hence, we **reject the null hypothesis** that the distribution of `calories` when `rating` is missing is the same as the distribution of `calories` when `rating` is not missing. Hence, we conclude that the missingness in the `rating` column is MAR dependent on `calories` .

#### 2. Use permutation test to determine if values in a `rating` column are MAR dependent on `minutes` column.

To test whether the `rating` column is MAR dependent on `minutes` column:
We have our

**Null hypothesiss**: *H<sub>0</sub>*: the distribution of `minutes` when `rating` is missing is the same as the distribution of `minutes` when `rating` is not missing. And 

**Alternative hypothesis**: *H<sub>a</sub>*: the distribution of `minutes` when `rating` is missing is not the same as the distribution of `minutes` when `rating` is not missing.

First, let us looked at the mean of the `minutes` based on the missingness of the `rating` values.

| rating_status   |   minutes |
|:----------------|----------:|
| False           |   103.49  |
| True            |   154.942 |

Minutes by Rating Status:

<iframe src="assets/Minutes by Rating Status.html" width=800 height=600 frameBorder=0></iframe>

Since `rating` is categorical and `minutes` is numerical, we use mean difference as test statistics to run the permutation test.
Then, we perform the permutation test of mean difference and find out the p-value.

The result of permutation test and p-value:
<iframe src="assets/Permutation test of rating and minutes.html" width=800 height=600 frameBorder=0></iframe>

Hence, we **fail to reject the null hypothesis** that the distribution of `minutes` when `rating` is missing is the same as the distribution of `minutes` when `rating` is not missing. Hence, we conclude that the missingness in the `rating` column is not MAR dependent on `minutes` .
## Hypothesis Testing

Pivot table between year and rating:

|   date year |   1.0 |   2.0 |   3.0 |   4.0 |   5.0 |
|------------:|------:|------:|------:|------:|------:|
|        2008 |   205 |   276 |  1138 |  6808 | 22209 |
|        2009 |   267 |   398 |  1529 |  8659 | 36053 |
|        2010 |   244 |   302 |  1051 |  5984 | 27943 |
|        2011 |   205 |   244 |   818 |  4581 | 21717 |
|        2012 |   204 |   181 |   597 |  3363 | 18114 |
|        2013 |   259 |   197 |   607 |  3104 | 16156 |
|        2014 |   288 |   157 |   359 |  1606 |  8983 |
|        2015 |   320 |   166 |   339 |   978 |  5571 |
|        2016 |   240 |   141 |   256 |   721 |  3908 |
|        2017 |   327 |   162 |   250 |   817 |  5027 |
|        2018 |   311 |   144 |   228 |   686 |  3995 |

We create the pivot table between year and rating and found out that there is a huge gap of number of ratings between 2013 and 2014. Thus, we like to investigate that whether the distribution of ratings is the same before and after the gap, which is 2014.

Distribution of rating before and after 2014:

|   rating |     After |     Before |
|---------:|----------:|-----------:|
|        1 | 0.0413007 | 0.00754581 |
|        2 | 0.0214008 | 0.00871258 |
|        3 | 0.0397999 | 0.0312955  |
|        4 | 0.13363   | 0.17719    |
|        5 | 0.763869  | 0.775256   |

#### 1. The null and alternative hypothesis: 

Under the significance level &#945; = 0.05.

**Null hypothesiss**: *H<sub>0</sub>*: the distributions of `rating` of each star number is the same before and after 2014. 

**Alternative hypothesis**: *H<sub>a</sub>*: the distributions of `rating` of each star number is not the same before and after 2014.

#### 2. Select test statistics:

Since both `rating` and `after shrinkage`(’Before’ for ratings before 2014, ‘After’ for ratings after 2014) columns we have use are categorical, we choose TVD as the test statistic.

#### 3. Simulation steps:

Under the null hypothesis, we assume that the distribution of each rating is the same before and after 2014, thus, the distribution of ratings won't be effect whether it is before or after 2014.

#### 4. Test and p-value:

We do the test under &#945; = 0.05 since its commonly used, simulate for 1000 times and collect test statistics to generate p-value.

<iframe src="assets/Hypothesis test of year and rating distribution.html" width=800 height=600 frameBorder=0></iframe>

#### 5. Conclusion:

Since the test p-value is 0.0 which is smaller than &#945; = 0.05, we **reject the null hypothesis** that the distribution of each rating is the same before and after 2014.

Since we do not have randomized controlled trials, we can only conclude that people may change their rating standard after a period.