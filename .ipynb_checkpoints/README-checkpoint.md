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
#### Univariate Analysis 1: Distribution of Recipe Average Ratings
#### Univariate Analysis 2: Distribution of Number of Recipes Per User
#### Univariate Analysis 3: Distribution of Recipe Ratings

### 2C) Exploratory Data Analysis: Bivariate Analysis
#### Bivariate Analysis 1: Average Rating vs. Number of recipes per user
#### Bivariate Analysis 2: Recipe Duration vs. Average Rating 

### 2D) Interesting Aggregates
Column to be grouped by is `contributor_id`\
In continuation, it's grouped by again with `recipe_count_bin`

## Step 3: Assessment of Missingness
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

