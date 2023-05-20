# Recipe Steps VS Taste Triumph: 

## Investigating the Influence of Number of Steps on Average Ratings

By Vicky Li (<yil164@ucsd.edu>)

## Introduction

### Question of Focus

Many people naturally assume that a rating for a recipe represents how good the food **really** tastes. However, will the ratings be influenced by other factors, for example, the number of steps that are involved in the process? This project aims to explore whether or not there is a relationship, and what is the relationship, between **the number of steps in each recipe** and **their average ratings**.

### Dataset

I will use a dataset named *recipes* to explore the relationship. The dataset contains 83782 recipes and 13 columns that each contains some infomation about each recipe. Our question is relevant to columns: "*n_steps*" and "*average_rating*". "*n_steps*" represents the number of steps that are shown in a recipe, and "*average_rating*" represents the average ratings for a recipe if there exists any ratings.

## Cleaning and EDA

### Data Cleaning

- Column "*n_steps*": it has been stored as an integer type already in the dataset, and there are no missing values/values that are used to impute missing values. As a result, this column is ready to be utilized efficiently without causing any bias due to missingness!

- Column "*average_rating*": Before calculating the average of rating, we identified an *individual* rating of 0 as a missing value since the minimum value for rating is 1 (1 star), so a 0 means that a user does not rate the recipe. Therefore, before we calculated the *average* rating, we replace all 0s with Null so that they don't contribute to the average rating of a recipe.
    * Without this cleaning step, the average ratings will be biased low.

Cleaned *Recipes* Dataset:

Note that there is a column named "steps" are being ignored for readability purpose. It contains detailed strings of descriptions of every step in a recipe.


| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                                                                                               | nutrition                                     |   n_steps | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                                                                             |   n_ingredients |   average_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings']                                                                        | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]      |        10 | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour']                                                          |               9 |                4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                                                                                                      | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]  |        12 | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                                                                             |              11 |                5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                                                                                               | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]     |         6 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']                                                                   |               9 |                5 |
| millionaire pound cake               | 286009 |       120 |           461724 | 2008-02-12  | ['time-to-make', 'course', 'cuisine', 'preparation', 'occasion', 'north-american', 'desserts', 'american', 'southern-united-states', 'dinner-party', 'holiday-event', 'cakes', 'dietary', 'christmas', 'thanksgiving', 'low-sodium', 'low-in-something', 'taste-mood', 'sweet', '4-hours-or-less'] | [878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0] |         7 | why a millionaire pound cake?  because it's super rich!  this scrumptious cake is the pride of an elderly belle from jackson, mississippi.  the recipe comes from "the glory of southern cooking" by james villas.                                                                                                                                                                | ['butter', 'sugar', 'eggs', 'all-purpose flour', 'whole milk', 'pure vanilla extract', 'almond extract']                                                                                                                                |               7 |                5 |
| 2000 meatloaf                        | 475785 |        90 |          2202916 | 2012-03-06  | ['time-to-make', 'course', 'main-ingredient', 'preparation', 'main-dish', 'potatoes', 'vegetables', '4-hours-or-less', 'meatloaf', 'simply-potatoes2']                                                                                                                                             | [267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]    |        17 | ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese.                                                                                                                                                                         | ['meatloaf mixture', 'unsmoked bacon', 'goat cheese', 'unsalted butter', 'eggs', 'baby spinach', 'yellow onion', 'red bell pepper', 'simply potatoes shredded hash browns', 'fresh garlic', 'kosher salt', 'white pepper', 'olive oil'] |              13 |                5 |

### Univariate Analysis

##### Distribution of Number of Steps:
<iframe src="assets/step_distribution.html" width=1000 height=600 frameBorder=0></iframe>

As shown, most of the recipes have number of steps <= 20, with the mode being 7. There are indeed some recipes that has above 20 steps, but it is not very likely. Notice that when you browse over [Food.com](https://www.food.com/), you will notice that some recipes, especially the ones with many steps, contains **unnecessary steps** such as, "[Take a deep breath. Make sure that you will have several uninterrupted hours to cook...](https://www.food.com/recipe/death-by-chocolate-a-la-trellis-356724)". Therefore, we need to be aware that the number of steps collected may not be reflective of what the real steps that are required.

<iframe src="assets/step_distribution_box.html" width=1000 height=600 frameBorder=0></iframe>

Notice that the median of number of steps is 9, but the mean is 10. This means the number of steps is biased high to some extent. One possible reason can be the one we walked about above -- necessary steps! However, despite this potential phenomenon, most of the recipes have less steps and any recipe that has > 23 steps is considered to be an outlier.

##### Distribution of Average Ratings
<iframe src="assets/rating_distribution.html" width=1000 height=600 frameBorder=0></iframe>

Most recipes have average ratings >= 4, making the distribution heavily negatively skewed.

<iframe src="assets/rating_distribution2.html" width=1000 height=600 frameBorder=0></iframe>

Any rating under 3.75 is considered to be an outlier.
The lower 25% of ratings are found in [1, 4.5], which shows how rare it is for a rating to be in this already wide range

### Bivariate Analysis

<iframe src="assets/bi_distribution.html" width=1000 height=600 frameBorder=0></iframe>

A few things to emphasize:
- The distribution is positively skewed for most of the range of average ratings (if not all)
- Despite that there are more data points that fall into higher ranges of average ratings, the outlier thresholds for number of steps are similar (around 21 steps). In fact, the more data points, the more consistent the threshold will be throughout various ranges
    * Similarly, the middle 50% data points are consistent throughout ranges of average ratings that have many data points

### Interesting Aggregates

The dataframe below shows aggregated statistics between average rating of a recipe and the steps it takes. Since average rating is continuous, I group them into several ranges for better analysis.

| Average Rating Range   |     mean |   median |       std |   count |
|:-----------------------|---------:|---------:|----------:|--------:|
| [1, 1.25)              | 10.489   |        9 |   6.19485 |     589 |
| [1.25, 1.5)            | 11       |       11 | nan       |       1 |
| [1.5, 1.75)            | 11.3889  |       10 |   8.73895 |      18 |
| [1.75, 2.0)            |  5       |        5 |   1.41421 |       2 |
| [2.0, 2.25)            | 10.7758  |       10 |   6.5605  |     620 |
| [2.25, 2.5)            | 10.1     |        8 |   6.62452 |      20 |
| [2.5, 2.75)            | 10.0952  |        9 |   6.20244 |     147 |
| [2.75, 3.0)            |  7.07692 |        6 |   4.07148 |      13 |
| [3.0, 3.25)            |  9.98221 |        9 |   6.26911 |    2530 |
| [3.25, 3.5)            |  9.51351 |        8 |   5.0874  |     185 |
| [3.5, 3.75)            | 10.1984  |        9 |   6.28899 |    1008 |
| [3.75, 4.0)            | 10.3963  |       10 |   6.17714 |     217 |
| [4.0, 4.25)            |  9.98307 |        9 |   5.97365 |   12640 |
| [4.25, 4.5)            |  9.54066 |        8 |   5.88854 |    2238 |
| [4.5, 4.75)            |  9.70374 |        9 |   5.80111 |    8604 |
| [4.75, 5.0)            |  9.32083 |        8 |   5.69882 |    4557 |
| 5                      | 10.2251  |        9 |   6.59071 |   47784 |

##### Discovery: 
- In ranges where many data points (recipes) fall in, the std tends to be smaller and is more consistent across different ranges.
- It appears that there is no obvious trend between ratings and  **the mean and median of number of steps**. However, there is an obvious trend that most of the recipes are associated with a high average rating. It also implies that recipes that have high ratings may be our focus of interest. 


