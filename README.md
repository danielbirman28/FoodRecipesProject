# Investigating the relationship between dessert recipes and their total steps

By Daniel Birman and Megha Kataki

## Overview

This is a data science project for DSC80 at UCSD focusing on exploring the relationship between healthy vs non healthy recipes, and how many steps each recipe contains.

## Introduction

Recipes are an integral part of our daily lives, guiding us in preparing various dishes. Understanding what influences the complexity of a recipe can help both cooking enthusiasts and professionals alike. Whether it's for planning a quick weekday meal or preparing for a special occasion, knowing the factors affecting the number of steps in a recipe can streamline the cooking process and help achieve desired outcomes. So ultimately we aim to explore the question, **what factors influence the number of steps in a recipe?** To answer this question, we are using two datasets consisting of recipes and ratings that have been posted to the website [Food.com](food.com) since 2008. These datasets provide a wealth of information about various recipes, including ingredients, cooking steps, ratings, and nutritional details.

The first dataset is titled ==recipe== and has 83782 rows and 12 columns. These columns are designated as such:

| Column | Description |
| ----------- | ----------- |
| =='name'== | text |
| =='id'== | text |
| =='minutes'== | text |
| =='contributor_id'== | text |
| =='submitted'== | text |
| =='tags'== | text |
| =='nutrition'== | text |
| =='n_steps'== | text |
| =='steps'== | text |
| =='description'== | text |
| =='ingredients'== | text |
| =='n_ingredients'== | text |

