# Recipe Rating & User Analysis Project
By: Flavia Gabriella Tedjarutjianta

## Overview
This project focuses on analyzing the relationship between ratings of recipes and other factors. Specifically, relationship between a cook's recipe count and their recipe rating. 

## 1. Introduction
As a big foodie and someone who likes to cook, recipes are always the way to go. Especially, when trying out new recipes. Personally, I look for recipes with a lot of reviews and if the cook has multiple other high-rated recipes. Only then, I trust and will try out their recipe myself. (Well, if the food turns out bad, I need to sharpen my cooking skills more!)\
However, this makes me wonder, **is there actually a relationship between a cook's recipe count and their recipe rating?**

Not only limited to the relationship between a cook's recipe count, but what other factors affect a recipe's average rating?\
Hence, this project is here to answer that very question using the power of data!

There are 2 datasets to be used in this project, the ratings and recipe. Ratings dataset consist of the review and/or ratings that users have given to recipes. Recipe dataset consists of the details of the recipes that someone posted (called contributors, users of the web that uploads recipes) along with details on the instructions and nutritional information. 

Dataset Source:\
This dataset contains recipes and ratings from food.com.\
It was originally scraped and used by the authors of a research paper titled, "Generating Personalized Recipes from Historical User Preferences" by Bodhisattwa Prasad Majumder, Shuyang Li, Jianmo Ni, Julian McAuley

### Understanding the Data sets
1. **Recipe Data Set**
   number of rows:  83782\
   number of columns:  12

   RECIPE DATASET COLUMN DETAILS  
| Column Name      | Description |
|-----------------|-------------|
| `name`         | Recipe name |
| `id`           | Recipe ID |
| `minutes`      | Minutes to prepare the recipe |
| `contributor_id` | User ID who submitted this recipe |
| `submitted`    | Date recipe was submitted |
| `tags`         | Food.com tags for recipe |
| `nutrition`    | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `n_steps`      | Number of steps in recipe |
| `steps`        | Text for recipe steps, in order |
| `description`  | User-provided description |
   
2. **Ratings Data Set**
   number of rows:  731927
   number of columns:  5

   RATING DATASET COLUMN DETAILS 
| Column Name    | Description        |
|------------|--------------------|
| `user_id`  | User ID            |
| `recipe_id` | Recipe ID         |
| `date`     | Date of interaction |
| `rating`   | Rating given       |
| `review`   | Review text        |


