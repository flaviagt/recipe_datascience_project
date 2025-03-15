# Recipe Rating & User Analysis Project
By: Flavia Gabriella Tedjarutjianta

## Overview
This project focuses on analyzing the relationship between ratings of recipes and other factors. Specifically, relationship between a cook's recipe count and their recipe rating. 

## Introduction
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

columns that will be important are:
- id: for merging
- minutes: in prediction
- contributor_id: a lot revolves around user recipe count
- submitted: to test fairness
- nutrition: mainly for prediction
- n_steps: mainly for prediction
   
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

columns that will be important are:
- recipe_id: for merging the datasets
- rating: will be used to find average rating per recipe, which is core of a lot of parts within this project



## Data Cleaning and Exploratory Data Analysis
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
<iframe
  src="assets/plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
As seen from the distribution, most of the recipe's average ratings are 5 and generally decreases when the average rating decreases. 

### 2C) Exploratory Data Analysis: Bivariate Analysis
#### Recipe Duration vs. Average Rating 
**PLOT**
As seen from the graph, most of boxes are high up. Remembering from the distribution of recipe average rating, most of them are in the 5-ratings. This means that in general, no matter the time needed to cook the recipe, there is general high rating for the recipes and distributed quite equally. 

### 2D) Interesting Aggregates
Column to be grouped by is `contributor_id`\
**TABLE**
Even from the head of the table, it is seen that there is a lot of variety in teh number of total recipes that a contributor uploads. Can be over 100 or as little as 1, where the ratings are also generall above 4. 

## Assessment of Missingness
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
Reason: Writing a review requires more effort than just rating a recipe on a scale of 1-5. Therefore, missing reviews may be due to the recipe not compelling the user to leave a written response, only a rating.

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

## Hypothesis Testing
Null Hypothesis: Number of recipe a contributors submitted does not the contributor's average rating\
Alternative Hypothesis: Contributors with multiple submission have higher average rating per recipe.\
Test Statistic: Difference in average rating between contributors with single and multiple recipes\
\
Significance level: 0.05\
This number is used because it's the most common one and there is no risk of if hypothesis testing is not as intricate\

The significance level (α) is 0.05, meaning that there is a high chance that we reject the null hypothesis if the p-value is less than 0.05.\
Since p-value = 0.0 < 0.05, we will **reject the null hypothesis**\

There is strong statistical evidence that contributors with multiple submissions tend to receive higher average ratings than those with only one submission. This suggests that more experienced or frequent contributors tend to perform better in terms of ratings.

## Framing a Prediction Problem
We're predicting the **average rating of recipes**, which will be a *regression* prediction problem.\ 
This will not be a classification of 1,2,3,4,5 as the rating, but rather a continuous, becuase the average rating per-recipe will be used, not per user reiew/rating\
variable being predicted: `avg_rating`\
RMSE is chosen to be the value to evaluate the model, because it's the sees how far off the predictions are based on the model from the actual values. 

We assume that the recipes given/that will be predicted, has never been reviewed or rated before by anyone. Therefore, `review` and `rating` columns will not be a part of the features. 

##  Baseline Model
### BASELINE
The 2 features (columns) that will be used to craft the base model are: 
1. minutes (quantitative)
2. calories (quantitative)

for this base model, we will use only one row per recipe, because minutes and calories do not change per review. 

Linear Regression is used for the base model, because it's the most *straightforward* model and it makes sense that there is a linear relationship between the 2 features and the average rating. As minutes go up, rating also goes up linearly (making something good does take time) and usually, the better tasting something is, the more unhealthy it will be (more calories). 

RMSE is used as the metric. Because, it's built for the regression model's performance. We're not using the "normal" accuracy, F-1 score, ebcasue these metrics are used for classification prediction problems

After the evaluation, I think the model is *okay* but not the best, because it means that the model is off by less than 1 rating above or less than 1 rating below.

##  Final Model
These are the features added along with their reasons of why they improve the model:\
- minutes: relates to how much *care* and time is invested to teh food, which relates to the depth of the flavor. usually the longer food marinates, the deeper the flavor and etc
- n_steps: relates to how much *care* and time is invested to teh food, which relates to the depth of the flavor
- n_ingredients: relates with teh complexity of the flavors in the recipe
- calories: usually, there's a strong correlation for healthy food (lower calories) with bland flavour, and higher calories for yummier foods
- total fat": fat also relates a lot with the flaours of a recipe
- sugar: sweetness is one of the main flavours that makes or breaks a recipe

The modeling algorithm chosen is: Linear Regression with Polynomial Features of 2

The process that led to this decision of using this model and features was: 
1. creating baseline model
2. doing k-fold cross-validation to get features
3. tries baseline model with the features recommended from the k-fold cross-validation
4. improve the base model with a scaler
5. try other types of model
6. do k-fold cross-validation for the *new* types of models
7. compare all of the RMSE to get the best result (focusing on the test RMSE performeance)

The final model's performance was:\ 
train rmse:  0.6388763461704926\
test rmse:  0.6433070346049465

Meanwhile the baseline performance was:\
train rmse (base model):  0.6938029224262099\
test rmse (base model):  0.6999782415603966

It's not that significant of an improvement, but improvement is seen from the decrease of the test and train RMSE. 

##  Fairness Analysis
2 Groups of choice: 
- Older Recipes (posted from 2008- to 2009)
- Newer Recipes (posted from 2010- to 2018)

Note: 2009 was the median

Evaluation Metric: RMSE

Null Hypothesis:The model is fair, the precision for older and newer recipes  are roughly the same and any differences are due to random chance. \
Alternative Hypothesis:The model is unfair, the precision for older recipe is lower than the precision for younger recipes

test statistic: Difference of RMSE between the 2 groups
significance level:  0.05 
resulting p-value: 0
conclusion:\
p-value is lesser than 0.05\
Therefore, reject the null hypothesis.\
Hence, The model can be considered as unfair due to this analysis