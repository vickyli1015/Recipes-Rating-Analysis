

## Investigating the Influence of Number of Steps on Average Ratings

By Vicky Li (<yil164@ucsd.edu>)

## Introduction

### Question of Focus

Many people naturally assume that a rating for a recipe represents how good the food **really** tastes. However, will the ratings be influenced by other factors, say, the number of steps that are required to make the dish? This project aims to explore whether or not there is a relationship, and what is the relationship, between **the number of steps in a recipe** and **its average ratings**.

### Dataset

I will use a dataset named *Recipes* to explore the relationships.

The dataset contains 83782 recipes and 13 columns in which each contains some infomation about each recipe. 

The dataset is formed by merging the original *Recipes* dataset with the averaged **rating** column of the *Interactions* dataset, in which there is one row for each interaction (rating and/or commenting). 

Our question is relevant to columns: "*n_steps*" and "*average_rating*". 
- "*n_steps*": the number of steps that are shown in a recipe 
- "*average_rating*": average rating for a recipe, if any.

## Cleaning and EDA

### Data Cleaning

- Column "*n_steps*": It has been stored as an integer type in the dataset, and there are no missing values/values that are used to impute missing values. As a result, this column is ready to be utilized efficiently without causing any bias due to missingness!

- Column "*average_rating*": Before calculating the average of rating, we identified an **individual rating** of 0 as a missing value since the minimum value for rating is 1 (1 star), so a 0 means that a user did not rate the recipe. Therefore, before we calculated the **average rating**, we replace all 0s with Null so that they don't contribute to the average rating of a recipe.
    * Without this cleaning step, the average ratings will be biased low.

Cleaned Recipes Dataset:

*Note*: Some entries of columns composed of strings are being shorten for the readability purpose. 

| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                  | nutrition                                     |   n_steps | steps                                                 | description                                           | ingredients                                           |   n_ingredients |   average_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:------------------------------------------------------|:----------------------------------------------|----------:|:------------------------------------------------------|:------------------------------------------------------|:------------------------------------------------------|----------------:|-----------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', '... | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]      |        10 | ['heat the oven to 350f and arrange the rack in th... | these are the most; chocolatey, moist, rich, dense... | ['bittersweet chocolate', 'unsalted butter', 'eggs... |               9 |                4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', ... | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]  |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing b... | this is the recipe that we use at my school cafete... | ['white sugar', 'brown sugar', 'salt', 'margarine'... |              11 |                5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', '... | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]     |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart b... | since there are already 411 recipes for broccoli c... | ['frozen broccoli cuts', 'cream of chicken soup', ... |               9 |                5 |
| millionaire pound cake               | 286009 |       120 |           461724 | 2008-02-12  | ['time-to-make', 'course', 'cuisine', 'preparation... | [878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0] |         7 | ['freheat the oven to 300 degrees', 'grease a 10-i... | why a millionaire pound cake?  because it's super ... | ['butter', 'sugar', 'eggs', 'all-purpose flour', '... |               7 |                5 |
| 2000 meatloaf                        | 475785 |        90 |          2202916 | 2012-03-06  | ['time-to-make', 'course', 'main-ingredient', 'pre... | [267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]    |        17 | ['pan fry bacon , and set aside on a paper towel t... | ready, set, cook! special edition contest entry: a... | ['meatloaf mixture', 'unsmoked bacon', 'goat chees... |              13 |                5 |


### Univariate Analysis

<iframe src="assets/step_distribution.html" width=1000 height=600 frameBorder=0></iframe>

As shown, most of the recipes have number of steps <= 20, with the mode being 7. There are indeed some recipes that has above 20 steps, but it is very unlikely. Notice that when you browse over [Food.com](https://www.food.com/), you will notice that some recipes, especially the ones with many steps, contains **unnecessary steps** such as, "[Take a deep breath. Make sure that you will have several uninterrupted hours to cook...](https://www.food.com/recipe/death-by-chocolate-a-la-trellis-356724)". Therefore, we need to be aware that the number of steps collected may not be reflective of what the real required steps are.

<iframe src="assets/step_distribution_box.html" width=1000 height=600 frameBorder=0></iframe>

Notice that the median of number of steps is 9, but the mean is 10. This means the number of steps is **biased high to some extent**. One possible reason can be the one we walked about above -- unnecessary steps! However, despite this potential phenomenon, most of the recipes have less steps and any recipe that has > 23 steps is considered to be an outlier (interact with the plot to see!).

<iframe src="assets/rating_distribution.html" width=1000 height=600 frameBorder=0></iframe>

Most recipes have average ratings >= 4, making the distribution of average ratings **heavily negatively skewed**.

<iframe src="assets/rating_distribution_box.html" width=1000 height=600 frameBorder=0></iframe>

Any rating under 3.75 is considered to be an outlier.
The lower 25% of ratings are found in [1, 4.5], which shows **how rare it is for a rating to be in this already wide range.**

---


### Bivariate Analysis

<iframe src="assets/bi_distribution_box.html" width=1000 height=600 frameBorder=0></iframe>

#### There are a few things to emphasize:
- The distribution is **positively skewed** for most of the ranges of average ratings (if not all).
- Despite that there are more data points that fall into higher ranges of average ratings while some ranges have only a few data points, the **outlier thresholds for number of steps are similar** (around 21 steps). In fact, the more data points there are within a range, the more similar the threshold will be throughout various ranges
    * Similarly, the **middle 50%** data points are consistent throughout ranges of average ratings that have many data points.

---

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

#### Discovery: 
- In rating ranges where many data points (recipes) fall in, **the standard deviation (std) tends to be smaller** and is more consistent with other different ranges that both have many data points.
- It appears that there is no obvious trend between ratings and **the mean and median of number of steps**. However, there is an obvious trend that **most of the recipes are associated with a high average rating**. It also implies that recipes that have high ratings may be our focus of interest. 


## Assessment of Missingness

### NMAR Analysis

Refer back the cleaned *Recipes* Dataset, there is a possibility that the column "*average_rating*" is not missing at random (NMAR). Why? Think about the following scenarios:
1. Notice that *Interactions* dataset has one row for each rating and/or comment. Imagine that you have questions regarding a recipe, do you choose to rate on it or comment on it to ask?
2. Imagine that you are a user that just **do** something instead of **disclosing opinions about** it, would you rate a recipe on a regular basis, especially when you are making food every single day?
3. For every bad recipe you've seen, will you rate on each of them or are you more inclined to just forget about it after you eat it?

Overall, Since rating a recipe is completely voluntary and is also subject to many factors such as **users not rating recipes they haven't tried, users choosing not to show their opinion, being kind, or even just that they're too lazy to rate**, they may contribute to the missiness of "*average_rating*". Therefore, it can be NMAR. However, we can obtain data like users' review habits (how often do they rate), their profile data (personality), and the time it takes to rate to make it MAR, since they all determine whether a rating will be missing to some extent!

---


### Missingness Dependency
#### Target Missing Column: 'description'

1. It is not missing by design (MD) because other columns do not provide any information sufficient enough for a recipe to miss detailed descriptions.
2. It is not MNAR because as we will see, its missingness will depend on other column.

So we test whether it is MAR or MCAR below:

#### Test whether missingness of 'description' depends on the column: 'minutes':

Null Hypothesis: distribution of *minutes* is the same for recipes that have missing descriptions and recipes that do not.

Alternative Hypothesis: The distribution is not the same.

<iframe src="assets/minutes_description.html" width=1000 height=600 frameBorder=0></iframe>

Since the p-value > 0.05 by a lot, we **fail to reject** the null hypothesis that the distribution of *minutes* is the same for recipes that have missing *average rating* and recipes that do not at 5% significance level. Therefore we say that **the column *description* is NOT MAR depending on *minutes***.

*Note*: If you stuck on here, think about the goal of giving an description: focus on the qualities or characteristics of the dishes, such as the flavors or healthiness, and why is it very **unlikely** for it be related to time spent on making it.
   * *Note-Note*: Because regardless of how much time a dish spends, it ALWAYS has some characteristics that one may want to share. Fast-making food can be delicious that the contributor may want to describe, and delicate food can also be delicious!


#### Test whether missingness of 'description' depends on the column: 'contributor_id':

Null Hypothesis: distribution of *contributor_id* is the same for recipes that have missing descriptions and recipes that do not.

Alternative Hypothesis: The distribution is not the same.

<iframe src="assets/contributor_description.html" width=1000 height=600 frameBorder=0></iframe>

Since the p-value < 0.01 , we **reject** the null hypothesis that the distribution of *minutes* is the same for recipes that have missing *descriptions* and recipes that do not at 1% significance level. Therefore we say that **the column *description* is MAR depending on *contributor id***.

*Note*: This is intuitive when you think about the personality of different recipe contributors. Some may LOVE giving detailed descriptions while some may be so straight forward that they believe that the descriptions in *steps* or recipe *name* is sufficient.

---

## Hypothesis Testing

Recall the Question of Interest: **whether or not there is a relationship, and what is the relationship, between the number of steps in recipes and their average ratings?** That is, from a broader sense, is it possible that the average ratings for recipes does not only depend on how the food tastes?

#### small exploration
It appears that the recipes with middle/high ranges of number of steps often (not always though) tend to have slightly higher average rating than the recipes that have lower ranges of number of steps.

<iframe src="assets/stepsx_mean_ratingsy.html" width=1000 height=600 frameBorder=0></iframe>>

**Null Hypothesis**: The number of steps in a recipe is **not related** to its average rating. i.e., the differences in means of average ratings we observe above are **due to chance only**.
- For example, given that there are 20 recipes that fall into [60, 70.3), and they have a mean of average ratings of around 4.80 (since this range has relatively larger counts, I am going to use this as an example we test on), if we pick 20 recipes from the **whole population**, it is reasonable to see a mean this high.

**Alternative Hypothesis**: The number of steps in a recipe **is related** to its average rating.
- In other words, the randomly chosen 20 recipes are very unlikely to have a mean this high if they are not related.

#### Plan:
Repeatably sample 20 recipes 100000 times from the population and compute their **mean of average ratings**, and see where the observed mean lies in this empirical distribution.

Test Statistic: Means of average ratings
This statistic is good because it is  

Significance level: 5%
5% is strong enough compared to 10% to minimize the rate of rejecting a null hypothesis when it is true, while also allowing for reasonable sensitivity to detect meaningful/necessary relationships compared to 1%. It balances between the 2 other options.

<iframe src="assets/hypo_test.html" width=1000 height=600 frameBorder=0></iframe>>
As you can see, the observed mean (4.80) is almost sitting at the margin of the histogram! That means ...  

#### Conclusion:
The chance that the observed mean of average ratings came from the distribution of mean under the null is less than 5%.
Under the null hypothesis, we rarely see an mean of average ratings this large, therefore, we **reject the null hypothesis that the number of steps in a recipe is NOT related to its average rating** at 5% significance level.