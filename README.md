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
    * Without this cleaning step, the average rating will be biased low.

Cleaned *Recipes* Dataset:

``` py
clean_recipes = pd.read_csv('/Users/vickyli/Dropbox/My Mac (Vicky的MacBook Air)/Desktop/Recipes-Rating-Analysis/cleaned_recipes')
print(clean_recipes.head().to_markdown(index=False))
```