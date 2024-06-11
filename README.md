# Investigating the relationship between dessert recipes and their total steps

By Daniel Birman and Megha Kataki

## Overview

This is a data science project for DSC80 at UCSD focusing on exploring the relationship between healthy vs non healthy recipes, and how many steps each recipe contains.

## Introduction

Recipes are an integral part of our daily lives, guiding us in preparing various dishes. Understanding what influences the complexity of a recipe can help both cooking enthusiasts and professionals alike. Whether it's for planning a quick weekday meal or preparing for a special occasion, knowing the factors affecting the number of steps in a recipe can streamline the cooking process and help achieve desired outcomes. So ultimately we aim to explore the question, **what factors influence the number of steps in a recipe?** To answer this question, we are using two datasets consisting of recipes and ratings that have been posted to the website [food.com](https://www.food.com/) since 2008. These datasets provide a wealth of information about various recipes, including ingredients, cooking steps, ratings, and nutritional details.

The first dataset is titled <code class="language-plaintext highlighter-rouge">recipe</code> and has 83782 rows and 12 columns. These columns are designated as such:

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

The second dataset is titled <code class="language-plaintext highlighter-rouge">recipe</code> interactions and it has 731,927 rows. Each row contains a rating and a review of someone who tried a specific recipe. The dataset has 4 columns and they are designated as such:

| Column | Description |
| ----------- | ----------- |
| <code class="language-plaintext highlighter-rouge">'user_id'</code> | User ID |
| <code class="language-plaintext highlighter-rouge">'recipe_id'</code> | Recipe ID |
| <code class="language-plaintext highlighter-rouge">'date'</code> | Date of interaction |
| <code class="language-plaintext highlighter-rouge">'rating'</code> | The rating given |
| <code class="language-plaintext highlighter-rouge">'review'</code> | The review given |

## Data Cleaning and Exploratory Data Analysis
