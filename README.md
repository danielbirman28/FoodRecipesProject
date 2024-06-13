# Investigating the relationship between food recipes and their total steps

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
  - The tags we used to determine this included: ['healthy', 'health', 'low-calorie', 'low calorie', 'low-sugar', 'low sugar', 'nutritious', 'whole-grain', 'vegetarian', 'vegan', 'organic', 'fresh', 'lean', 'high-fiber', 'low-fat', 'gluten-free']
7. Create a <code class="language-plaintext highlighter-rouge">'is_dessert'</code> column.
  - This column can be used in comparing dessert recipes versus meal recipes.

After performing these data cleaning operations, our <code class="language-plaintext highlighter-rouge">cleaned</code> data frame has 234,428 rows and 23 columns. Here we did not show the columns <code class="language-plaintext highlighter-rouge">'review'</code>, <code class="language-plaintext highlighter-rouge">'tags'</code>, <code class="language-plaintext highlighter-rouge">'steps'</code>, <code class="language-plaintext highlighter-rouge">'directions'</code>, or <code class="language-plaintext highlighter-rouge">'ingredients'</code> because these rows became too large too display as some reviewers left lengthy reviews. We will show the first 5 rows for simplicity. Use the scroll bar below to see all of the columns.

|   recipe_id | name                                                           |   avg_rating |   contributor_id | submitted           |   n_ingredients |   minutes |   n_steps |   calories (#) |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |   user_id | date                |   rating | is_dessert   | is_healthy   |
|------------:|:---------------------------------------------------------------|-------------:|-----------------:|:--------------------|----------------:|----------:|----------:|---------------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|----------:|:--------------------|---------:|:-------------|:-------------|
|      306785 | spicy nectarine and corn salsa                                 |        5     |           377499 | 2008-06-02 00:00:00 |               8 |        40 |         4 |           95.3 |                 1 |            50 |             16 |               5 |                     0 |                     7 |    483827 | 2008-07-15 00:00:00 |        5 | False        | True         |
|      310237 | french strawberry crepes                                       |        5     |           452576 | 2008-06-19 00:00:00 |              10 |        30 |         9 |          143.5 |                 5 |            25 |              3 |              10 |                     3 |                     7 |      5060 | 2010-05-07 00:00:00 |        5 | False        | False        |
|      321038 | hg s grab  n go breakfast cookies   weight watchers   2 points |        5     |           346383 | 2008-08-24 00:00:00 |              14 |        22 |        14 |          182.4 |                 2 |            50 |              7 |              11 |                     1 |                    13 |    935485 | 2009-06-22 00:00:00 |        5 | True         | False        |
|      321038 | hg s grab  n go breakfast cookies   weight watchers   2 points |        5     |           346383 | 2008-08-24 00:00:00 |              14 |        22 |        14 |          182.4 |                 2 |            50 |              7 |              11 |                     1 |                    13 |    539686 | 2011-03-28 00:00:00 |        5 | True         | False        |
|      342209 | banana oatmeal cookies                                         |        4.375 |           361931 | 2008-12-08 00:00:00 |              12 |        40 |         7 |          658.2 |                45 |           151 |             35 |              24 |                    72 |                    29 |     22174 | 2008-12-28 00:00:00 |        4 | True         | False        |

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

### Interesting Aggegrates

Here we created a dataframe called <code class="language-plaintext highlighter-rouge">contributor_aggregates</code> where we grouped by <code class="language-plaintext highlighter-rouge">'is_dessert'</code> and <code class="language-plaintext highlighter-rouge">'is_healthy'</code> and calculated aggregate statistics to analyze the difference between these two reciupe types.  By examining the average rating, steps, and cooking time for dessert and healthy recipes, we can gain insights into how these types of recipes differ in terms of user preferences and preparation requirements. The dataframe below for <code class="language-plaintext highlighter-rouge">contributor_aggregates</code> shows the first 5 rows of which contributors of recipes tend to have the highest total number of recipes submitted, highest average cook times, and higest average total recipe steps. As we can see, the contributor with contributor ID 37449 has the most recipes submitted at 3060 total recipes. Contributor 226863 has the highest average steps per recipe at over 12 average steps and the highest average minutes required at 112 minutes! Could it be that having a higher average steps means the recipe will take longer to make?

|   contributor_id |   avg_rating |   total_recipes |   avg_steps |   avg_minutes |
|-----------------:|-------------:|----------------:|------------:|--------------:|
|            37449 |      4.78761 |            3060 |    10.9562  |       59.2291 |
|           226863 |      4.84693 |            2754 |    12.5425  |      112.537  |
|           424680 |      4.80217 |            2503 |     6.0004  |       33.6336 |
|            89831 |      4.78994 |            2436 |     9.35591 |       97.3001 |
|           383346 |      4.76688 |            2368 |     8.86318 |       41.0249 |


## Assessment of Missingness

During our data cleaning step, we found that three columns have significant missing values: <code class="language-plaintext highlighter-rouge">'rating'</code>, <code class="language-plaintext highlighter-rouge">'review'</code>, and <code class="language-plaintext highlighter-rouge">'date'</code>.

### NMAR Analysis

The missing data in the 'rating' column may be NMAR (Not Missing At Random) if user experiences and platform design influence the likelihood of providing a rating. Users who had extremely positive or negative experiences might be more inclined to leave a rating, while those with moderate or indifferent experiences might skip the step to leave a rating. Additionally, if the platform's design makes rating optional, the missingness could be related to user interface elements rather than random factors.

## Missingness Dependency

Here we wanted to investigate whether the missingness of the <code class="language-plaintext highlighter-rouge">'rating'</code> column was dependent on the <code class="language-plaintext highlighter-rouge">'minutes'</code> column or the <code class="language-plaintext highlighter-rouge">'n_steps'</code> column.

> Minutes and Rating

**Null Hypothesis:** The missingness in the 'rating' column is independent of the 'minutes' column.

**Alternate Hypothesis:** The missingness in the 'rating' column depends on the 'minutes' column.

**Test Statistic:** The test statistic is the absolute difference in the mean 'minutes' between the groups with missing and non-missing 'rating' values.

**Significance Level:** 0.05

We then peformed a permutation test by shuffling the missingness of minutes 1000 times which we could then use for finding the absolute mean difference.

<iframe
  src="assets/emp-dist-mean-diff-mins.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As show in the graph above, the **observed statistic** is **51.4597** which is designated by the red line. We found our **p-value** to be **0.129** which is greater than our significance level of 0.05. Since this is the case, we **fail to reject** the null hypothesis that the missingness in the 'rating' column is independent of the 'minutes' column. Therefore, the missingness in 'rating' does not depend on 'minutes'

> N Steps and Rating

**Null Hypothesis:** The missingness in the 'rating' column is independent of the 'n_steps' column.

**Alternate Hypothesis:** The missingness in the 'rating' column depends on the 'n_steps' column.

**Test Statistic:** The test statistic is the difference in the mean 'n_steps' between the groups with missing and non-missing 'rating' values.

**Significance Level:** 0.05

Again we performed a permutation test but for these columns by shuffling the missingness of n_steps 1000 times which helps us to find our test statistic, the difference in mean.

<iframe
  src="assets/emp-dist-mean-diff-steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that from our graph of distributions, the our **observed statistic** (the red line), is significantly further away and holds a value of **1.339**. The green line is our **p-value** and has a value of **0.0**. Since our p-value is less than the significance level (0.0 < 0.05), we will **reject the null hypothesis**. Hence, The missingness in 'rating' depends on 'n_steps'.

## Hypothesis Testing

Like we mentioned in the intro, we want to explore whether healthy or nonhealthy recipes tend to have a higher number of steps. To investigate this, we ran a **permutation test** with these hypotheses and test statistic:

**Null Hypothesis:** There is no significant difference in steps between healthy and non-healthy recipes

**Alternate Hypothesis:** There is a significant difference in steps between healthy and non-healthy recipes

**Test Statistic:** The mean difference in healthy steps versus non healthy steps

**Significance Level:** 0.05

We chose a permutation test because we do not have any information of the population, and want to see if the distribution of healthy and non-healthy steps come from the same population. We believe that the number of steps for healthy recipes may be higher because, for example, say you have lots of fresh produce, there would be more steps to prep each ingredient whereas say a microwave dinner would be less healthy but have less steps to make. We chose this test statistic because we want to see the difference in steps between healthy and non-healthy recipes. We can see which recipes tend to have higher number of steps.

Before running the test, we filtered for healthy and non-healthy recipes. Then filtered those values to extract steps and handle NaN values. We used these points to run our permutation test, and resulted in an **observed statistic** of **-1.34**.

Then, we shuiffled the steps 1000 times simulating 1000 mean differences between our two distriburtions resulting in a **p-value** of **0.0**.

<iframe
  src="assets/hypothesis-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Conclusion of Permutation Test

Since our **p-value** of **0.0** is less than our significance level of 0.05, we **reject the null hypothesis**. This supports the alternative hypothesis: There is a significant difference in steps between healthy and non-healthy recipes. The negative observed difference indicates that, on average, the "healthy_steps" group takes fewer steps than the "non_healthy_steps" group. The extremely low p-value indicates that it's highly unlikely to observe such a large difference in mean steps between the groups if there were truly no difference between them. This could be because the unhealthy recipes could be desserts that have many steps, like baking an intricate cake or dessert item.

## Framing a Prediction Problem

Our goal is to predict the number of steps in a recipe, utilizing a regression model since we're predicting a discrete quantitative variable, given that recipe steps are whole numbers. To address this problem effectively, we'll first construct a linear regression model, considering the wide range of step values recipes can encompass. Later we will expand upon our baseline model to employ a Random Forrest Regressor which can better tackle our problem of predicting recipe steps. We will use the root mean squared error (we will refer to it as **RMSE** from now on) as a metric to evaluate the model since it measures the average magnitude of errors in continuous predictions, aligning with the algorithm's objective of minimizing mean squared error during training.

## Baseline Model

Before creating our baseline model, we noticed that some entries in the minutes column were in the 100,000s if not millions for some values. So to deal with these values, we used the IQR method to remove these outlier, and removed 24,291 outliers. Now for the actual model, we started off with a simple linear regression model. We used only one feature, <code class="language-plaintext highlighter-rouge">'minutes'</code>, in order to predict our target <code class="language-plaintext highlighter-rouge">'n_steps'</code>. Here, <code class="language-plaintext highlighter-rouge">'minutes'</code> is a quantitative discrete varaible, meaning it is a whole number. We did not perform any encodings yet because we wanted to keep our baseline model very simple. The baseline model resulted in an RMSE of 5.28, meaning its predictions were 5.28 steps off from a recipes actual steps. The R^2, or the coefficient of determination, reflects how accurately the model predicts the outcome between 0 and 1. We resulted in an R^2 of 0.184, meaning it inaccurately predicts the outcome most of the time since it is on the lower end. So since our RMSE is high and our R^2 is low, we can greatly improve our baseline model in the final model.

## Final Model

#### Features

For our final model, the features we used include: <code class="language-plaintext highlighter-rouge">'minutes'</code>, <code class="language-plaintext highlighter-rouge">'tag_length'</code>, <code class="language-plaintext highlighter-rouge">'is_dessert'</code>, <code class="language-plaintext highlighter-rouge">'is_healthy'</code>, <code class="language-plaintext highlighter-rouge">'calories (#)'</code>, and finally <code class="language-plaintext highlighter-rouge">'n_ingredients'</code>

<code class="language-plaintext highlighter-rouge">'minutes'</code>

We chose the feature for minutes because in our EDA we saw that as minutes increases, steps also increase. Naturally a correlation between the two features was prevalent. This would be helping in predicting total steps because if minutes increases, steps is likely to increase as well.

<code class="language-plaintext highlighter-rouge">'tag_length'</code>

Tag Length could be a noteworthy feature as recipes with more tags could have longer minutes associated to the recipe because there could be more complex steps needed to make the recipe. This could be helpful because if a recipe has more steps it could inicate a lengthier amount of steps.

<code class="language-plaintext highlighter-rouge">'is_dessert'</code>

We wanted to use the feature of <code class="language-plaintext highlighter-rouge">'is_dessert'</code> to see if desserts had more steps in their recipe. In our hypothesis test, we conluded that unhealthy recipes tend to have more steps, so using <code class="language-plaintext highlighter-rouge">'is_dessert'</code> could be a useful feature.

<code class="language-plaintext highlighter-rouge">'is_healthy'</code>

Contrary to the above point, we wanted to use <code class="language-plaintext highlighter-rouge">'is_healthy'</code> to see if healthier recipes have less steps. As we learned in our hypothesis test, healthier recipes tend to have less steps than unhealthy recipes, so this feature could be helpful in predicting steps for healthy recipes.

<code class="language-plaintext highlighter-rouge">'calories (#)'</code>

The calorie count of a recipe could be used because the higher calories a recipe has, the unhealthier it tends to be. So this could be used to explore the relationship between calories and recipe steps.

<code class="language-plaintext highlighter-rouge">'n_ingredients'</code>

The number of ingredients a recipe has is correlated with total number of steps as the more ingredients present, the more time it will take to prepare each ingredient. So, the more ingredients a recipe has, the more steps the recipe will likely have as well.

#### Modeling Algorithm

For our modeling algorithm, we used a <code class="language-plaintext highlighter-rouge">RandomForrestRegressor</code> and a <code class="language-plaintext highlighter-rouge">RandomizedSearchCV</code> to tune our hyperparameters of <code class="language-plaintext highlighter-rouge">'regressor__n_estimators'</code>, <code class="language-plaintext highlighter-rouge">'regressor__max_depth'</code>, <code class="language-plaintext highlighter-rouge">'regressor__min_samples_split'</code>, and <code class="language-plaintext highlighter-rouge">'regressor__min_samples_leaf'</code>. We chose a Random Forest Regressor for the model because it effectively captures non-linear relationships, is robust to overfitting, and requires minimal data preprocessing. In tandem with the model, we used a RandomizedSearchCV, which in contrast to GridSearchCV, doesn't try out all parameter values, but rather a fixed number of parameter settings is sampled from the specified distributions. After running the RandomizedSearchCV, we found that the best hyperparamaters were <code class="language-plaintext highlighter-rouge">{'regressor__n_estimators': 50, 'regressor__min_samples_split': 2, 'regressor__min_samples_leaf': 1, 'regressor__max_depth': None}</code>. Our model produce a RMSE of **3.539**, which is 1.75 units better than our original baseline model. Additionally, the R^2 increased from the baseline's 0.184 to our new model's R^2 of 0.632, nearly a **0.45** increase, and a 243% improvement in the how well the model predicts an outcome.
