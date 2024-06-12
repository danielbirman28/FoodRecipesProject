# Investigating the relationship between dessert recipes and their total steps

By Daniel Birman and Megha Kataki

## Overview

This is a data science project for DSC80 at UCSD focusing on exploring the relationship between healthy vs non healthy recipes, and how many steps each recipe contains.

## Introduction

Recipes are an integral part of our daily lives, guiding us in preparing various dishes. Understanding what influences the complexity of a recipe can help both cooking enthusiasts and professionals alike. Whether it's for planning a quick weekday meal or preparing for a special occasion, knowing the factors affecting the number of steps in a recipe can streamline the cooking process and help achieve desired outcomes. So ultimately we aim to explore the question, **what factors influence the number of steps in a recipe?** To answer this question, we are using two datasets consisting of recipes and ratings that have been posted to the website [food.com](https://www.food.com/) since 2008. These datasets provide a wealth of information about various recipes, including ingredients, cooking steps, ratings, and nutritional details.

The first dataset is titled <code class="language-plaintext highlighter-rouge">recipe</code> and has 83,782 rows and 12 columns. These columns are designated as such:

| Column | Description |
| ----------- | ----------- |
| <code class="language-plaintext highlighter-rouge">'name'</code> | Recipe Name |
| <code class="language-plaintext highlighter-rouge">'id'</code> | Recipe ID |
| <code class="language-plaintext highlighter-rouge">'minutes'</code> | How many minutes it takes to prepare the recipe |
| <code class="language-plaintext highlighter-rouge">'contributed_id'</code> | The User ID of who submitted the recipe |
| <code class="language-plaintext highlighter-rouge">'submitted'</code> | Date the recipe was submitted |
| <code class="language-plaintext highlighter-rouge">'tags'</code> | A description of tags relevant to the recipe |
| <code class="language-plaintext highlighter-rouge">'nutrition'</code> | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; where PDV stands for “percentage of daily value” |
| <code class="language-plaintext highlighter-rouge">'n_steps'</code>  | The total number of steps in the recipe |
| <code class="language-plaintext highlighter-rouge">'steps'</code>  | The detailed steps for the recipe |
| <code class="language-plaintext highlighter-rouge">'description'</code>  | A user-submitted description |
| <code class="language-plaintext highlighter-rouge">'ingredients'</code>  | List of ingredients needed |
| <code class="language-plaintext highlighter-rouge">'n_ingredients'</code>  | Total number of ingredients needed |

The second dataset is titled <code class="language-plaintext highlighter-rouge">interactions</code> and it has 731,927 rows. Each row contains a rating and a review of someone who tried a specific recipe. The dataset has 4 columns and they are designated as such:

| Column | Description |
| ----------- | ----------- |
| <code class="language-plaintext highlighter-rouge">'user_id'</code> | User ID |
| <code class="language-plaintext highlighter-rouge">'recipe_id'</code> | Recipe ID |
| <code class="language-plaintext highlighter-rouge">'date'</code> | Date of interaction |
| <code class="language-plaintext highlighter-rouge">'rating'</code> | The rating given |
| <code class="language-plaintext highlighter-rouge">'review'</code> | The review given |

## Data Cleaning and Exploratory Data Analysis
To aid in our exploration, we cleaned our data set in the following ways:
1. Left merge the recipes and interactions datasets together
  - We did this to assign an interaction row to a recipe row.
2. Fill all ratings of 0 with np.nan
 - To avoid bias in the rating column, we replaced any 0 ratings with a NaN value, as ratings are only on a scale from 1 to 5, and a 0 would indicate bias.
3. Find the average rating per recipe, as a Series, and assign it a column <code class="language-plaintext highlighter-rouge">'avg_rating'</code>
 - This was done because the average rating per recipe is more valuable to look at compared to an individual rating left by a reviewer.
4. Assign columns into a logical order based off food.com
  - We reordered our dataframe to match the order in which information is presented on a recipe from food.com.
5. Convert the <code class="language-plaintext highlighter-rouge">'submitted'</code> and <code class="language-plaintext highlighter-rouge">'date'</code> column from objects to datetime objects
  - This was done in order to more easily perform analysis across dates as they were originally stored as an object.
6. Create an <code class="language-plaintext highlighter-rouge">'is_healthy'</code> column.
- We created this column in order to aid in our analysis of healthy versus non-healthy recipe items more easily.

After performing these data cleaning operations, our <code class="language-plaintext highlighter-rouge">cleaned</code> data frame has 234,428 rows and 23 columns. Here we did not show the columns <code class="language-plaintext highlighter-rouge">'review'</code>, <code class="language-plaintext highlighter-rouge">'tags'</code>, <code class="language-plaintext highlighter-rouge">'steps'</code>, <code class="language-plaintext highlighter-rouge">'directions'</code>, or <code class="language-plaintext highlighter-rouge">'ingredients'</code> because these rows became too large too display as some reviewers left lengthy reviews. We will show the first 5 rows for simplicity. Use the scroll bar below to see all of the columns.

|   user_id |   recipe_id | date       |   rating | name                                                           |   minutes |   contributor_id | submitted   |   n_steps |   n_ingredients |   avg_rating |   calories (#) |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|----------:|------------:|:-----------|---------:|:---------------------------------------------------------------|----------:|-----------------:|:------------|----------:|----------------:|-------------:|---------------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|
|    483827 |      306785 | 2008-07-15 |        5 | spicy nectarine and corn salsa                                 |        40 |           377499 | 2008-06-02  |         4 |               8 |        5     |           95.3 |                 1 |            50 |             16 |               5 |                     0 |                     7 |
|      5060 |      310237 | 2010-05-07 |        5 | french strawberry crepes                                       |        30 |           452576 | 2008-06-19  |         9 |              10 |        5     |          143.5 |                 5 |            25 |              3 |              10 |                     3 |                     7 |
|    935485 |      321038 | 2009-06-22 |        5 | hg s grab  n go breakfast cookies   weight watchers   2 points |        22 |           346383 | 2008-08-24  |        14 |              14 |        5     |          182.4 |                 2 |            50 |              7 |              11 |                     1 |                    13 |
|    539686 |      321038 | 2011-03-28 |        5 | hg s grab  n go breakfast cookies   weight watchers   2 points |        22 |           346383 | 2008-08-24  |        14 |              14 |        5     |          182.4 |                 2 |            50 |              7 |              11 |                     1 |                    13 |
|     22174 |      342209 | 2008-12-28 |        4 | banana oatmeal cookies                                         |        40 |           361931 | 2008-12-08  |         7 |              12 |        4.375 |          658.2 |                45 |           151 |             35 |              24 |                    72 |                    29 |

### Univariate Analysis

For this section, we analyzed the distribution of total steps that healthy recipes have. As shown in the plot, the distribution is skewed right, as the highest count of n-steps is 7. The graph shows us that most recipes fall within the 1-20 steps range, for healthy recipes.

<iframe
  src="assets/distribution-n-steps-healthy-recipes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

Here, we wanted to explore the distribution of N steps for healthy and unhealthy recipes, and plot these distributions over one another to compare them. Below we can see that there are more unhealthy recipes in general. Even though both distributions are skewed right, the unhealthy distributions is shifted to the right slightly, meaning that unhealthy recipes tend to have more steps in their recipes.

<iframe
  src="assets/distribution-n-steps-unhealthy-vs-healthy-recipes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
