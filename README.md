# Recipe Rating & User Analysis Project
By: Flavia Gabriella Tedjarutjianta

## Overview
This project focuses on analyzing the relationship between ratings of recipes and other factors. Specifically, relationship between a cook's recipe count and their recipe rating. 

## PART 1. Introduction
As a big foodie and someone who likes to cook, recipes are always the way to go. Especially, when trying out new recipes. Personally, I look for recipes with a lot of reviews and if the cook has multiple other high-rated recipes. Only then, I trust and will try out their recipe myself. (Well, if the food turns out bad, I need to sharpen my cooking skills more!)\
However, this makes me wonder, **is there actually a relationship between a cook's recipe count and their recipe rating?**

Not only limited to the relationship between a cook's recipe count, but what other factors affect a recipe's average rating?\
Hence, this project is here to answer that very question using the power of data!

There are 2 datasets to be used in this project, the ratings and recipe. Ratings dataset consist of the review and/or ratings that users have given to recipes. Recipe dataset consists of the details of the recipes that someone posted (called contributors, users of the web that uploads recipes) along with details on the instructions and nutritional information. 

Dataset Source:\
This dataset contains recipes and ratings from food.com.\
It was originally scraped and used by the authors of a research paper titled, "Generating Personalized Recipes from Historical User Preferences" by Bodhisattwa Prasad Majumder, Shuyang Li, Jianmo Ni, Julian McAuley

### Understanding the Data sets
#### **Recipe Data Set**  
**Number of rows:**  83,782  
**Number of columns:**  12  

**Recipe Dataset Column Details**  

| Column Name       | Description |
|------------------|-------------|
| `name`          | Recipe name |
| `id`            | Recipe ID |
| `minutes`       | Minutes to prepare the recipe |
| `contributor_id` | User ID who submitted this recipe |
| `submitted`     | Date recipe was submitted |
| `tags`          | Food.com tags for the recipe |
| `nutrition`     | Nutrition information in the form `[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]`; PDV stands for “percentage of daily value” |
| `n_steps`       | Number of steps in the recipe |
| `steps`         | Text for recipe steps, in order |
| `description`   | User-provided description |

   
#### **Ratings Data Set**  
**Number of rows:**  731,927  
**Number of columns:**  5  

**Rating Dataset Column Details**  

| Column Name  | Description        |
|-------------|--------------------|
| `user_id`   | User ID            |
| `recipe_id` | Recipe ID          |
| `date`      | Date of interaction |
| `rating`    | Rating given       |
| `review`    | Review text        |


## PART 2: Data Cleaning and Exploratory Data Analysis
### 2A) Data Cleaning
1. **merging the 2 datasets**\
based on the dataset, left merge the recipes and ratings dataset so analysis will be based on the recipe (whether or not it has a review and/or rating
2. **replacing 0 values in `rating` column to NaN**\
This is done because the lowest rating that can be given is 1, hence 0 indicates a missing value. Therefore, changing 0s to NaN is important to ensure that these missing ratings are considered missing and not included in calculations related to the ratings.\
Unique values of the `rating`  column will be: 1, 2, 3, 4, 5, np.nan
3. **Finding the average rating per recipe & storing values in new column**\
creating new column by getting the average rating based on the recipe_id
4. **Breaking down "nutrition" columns into individual columns**\
default nutrition column's values is a list of different information\
Nutrition information in the form:\
[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)] (PDV stands for “percentage of daily value")

### 2B) Exploratory Data Analysis: Univariate Analysis
#### Distribution of Recipe Average Ratings

### 2C) Exploratory Data Analysis: Bivariate Analysis
#### Recipe Duration vs. Average Rating 

### 2D) Interesting Aggregates
Column to be grouped by is `contributor_id`\
In continuation, it's grouped by again with `recipe_count_bin`

## PART 3: Assessment of Missingness
These are the columns in the dataset with missing values:  
`['name', 'description', 'user_id', 'date', 'rating', 'review', 'avg_rating']`  

This is the number of rows that have missing values for each of the columns:  

- **name** : 1  
- **description** : 114  
- **user_id** : 1  
- **date** : 1  
- **rating** : 15,036  
- **review** : 58  
- **avg_rating** : 2,777  

**Note:** There are missing values in the `avg_rating` column because some recipes have missing or no ratings.

#### NMAR
`review` column is NMAR\
Writing a review requires more effort than just rating a recipe on a scale of 1-5. Therefore, missing reviews may be due to the recipe not compelling the user to leave a written response, only a rating.

#### Missingness Dependency
Missingness Dependency
chosen column : `rating` 

depends on: `year_submitted`\
does not depend on: `sodium (PDV)`

#### Proportion of `year_submitted` and `Rating`
Null Hypothesis: The missingness of ratings does NOT depend on the year that the recipe was submitted on\
Alternate Hypothesis: The missinness of ratings depends on the year that the recipe was submitted on 

Test Statistic: permutation test using the Total Variation Distance (TVD)
Significant Level: 0.05

**PLOT**

OBSERVED TVD VALUE: 0.008775371248423168

**PLOT**

p-value is  0.308, which is greater than 0.05\
Therefore, **fail to reject the null hypothesis** of "The missingness of ratings does NOT depend on the year that the recipe was submitted on"
then we can conclude 'rating' is **MCAR**.

#### Proportion of sodium (PDV) and Rating
Null Hypothesis: The missingness of ratings does not depend on the proportion of sodium in the recipe\
Alternate Hypothesis: The missingness of ratings depends on the proportion of sodium in the recipe

Test Statistic: The absolute difference of mean in the proportion of the group without missing ratings\
Significance Level:0.05

**2 plots**
Observed Statistic: 4.603758453968997\
P-value: 0.0\

P-value, which is 0, is smaller than 0.05\
Therefore, reject the null.\
The missingness of `rating` does depend on the `proportion of sodium`

## PART 4: Hypothesis Testing
Null Hypothesis: Number of recipe a contributors submitted does not the contributor's average rating\
Alternative Hypothesis: Contributors with multiple submission have higher average rating per recipe.\
Test Statistic: Difference in average rating between contributors with single and multiple recipes\
\
Significance level: 0.05\
This number is used because it's the most common one and there is no risk of if hypothesis testing is not as intricate\

**PLOT**

The significance level (α) is 0.05, meaning we reject the null hypothesis if the p-value is less than 0.05.\
Since p-value = 0.0 < 0.05, we **reject the null hypothesis**\

There is strong statistical evidence that contributors with multiple submissions tend to receive higher average ratings than those with only one submission. This suggests that more experienced or frequent contributors tend to perform better in terms of ratings.

## PART 5: Framing a Prediction Problem
We're predicting the average rating of recipes, which will be a regression prediction problem.\ 
This will not be a classification of 1,2,3,4,5 as the rating, but rather a continuous, becuase the average rating per-recipe will be used, not per user reiew/rating

## PART 6: Baseline Model
### BASELINE

The 2 features (columns) that will be used to craft the base model are: 
1. minutes
2. calories

for this base model, we will use only one row per recipe, because minutes and calories do not change per review. 

RMSE is used as the metric. Because, it's built for the regression model's performance. We're not using the "normal" accuracy, F-1 score, ebcasue these metrics are used for classification prediction problems

