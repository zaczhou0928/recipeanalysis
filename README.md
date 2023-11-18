# Recipe and Rating Data Analysis Project
by Qianjin Zhou and Haohan Zou

# Introduction
Welcome to our exploration of food recipes and ratings, a fascinating collection of data that delves into the world of delicious food. Inspired by the relationship between user satisfaction and the nutrition factors of recipes, we have dived deep into the datasets and developed our research question on:

**What is the relationship between the amount of calories and average rating of recipes?**

Investigating this question can give us an idea regarding the impact of the amount of calories on peoples’ enjoyment of the food. By identifying a potential relationship between calories and rating for a recipe, we can possibly help food recipe creators, chefs, restaurant owners, etc. to make food that is more appealing and satisfactory.

The data we are using is sourced from food.com, a website of user community that thrives in authentic food recipes that are shared by food enthusiasts all around the world. 

The data is divided into two sets, each focused on recipes and ratings respectively. The recipe dataset contains information of recipe name, ID, preparation time, contributor ID, submission date, tags, nutrition information, number of steps, steps text, and description. Meanwhile, the rating dataset includes data of user ID, recipe ID, date of interaction, rating, and review text.

After merging and cleaning, the dataset we will primarily use for the course of the research is a single dataframe that contains 83698 rows and 14 columns, with each row representing a recipe and its corresponding information. For the purpose of our analysis, we will mainly focus on the following columns.

'id': The unique identifier of a recipe, formatted as a 6-digit int.
‘average rating’: the average rating of a recipe.
‘nutrition’: Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” (For easier access to the data, we have splitted the data and created a column for each nutritional value, and we will mainly focus on the calories column.) 
‘n_steps’: number of steps in recipe
‘n_ingredients’: number of ingredients in recipe

In the following parts, we will show the sections of Data Cleaning and EDA (Exploratory Data Analysis), Assessment of Missingness, and Hypothesis Testing.


# Data Cleaning and EDA

## Data Cleaning

Our data cleaning process was meticulous, ensuring the integrity of our analysis.


### Checking Data Type

First, we observe the data type of each column from the recipe dataset to understand the data.

We also look at the data type of each column from the ratings dataset.

### Merging Dataset

We left merged the recipes and ratings dataset together and filled all ratings of 0 in the rating dataset with np.nan. 


Filling is a necessary step because through observing the website, we found that the minimum value for rating is 1. In this way, we recognized that 0 does not mean numerically a rating of 0 but that the reviewer simply didn’t provide a rating. Therefore, dropping those ratings of 0 can help us accurately calculate the average rating for each recipe in the later cleaning process.

### Assigning New Column

We calculated the average rating for each recipe and assigned a new column to the original recipe dataset.

### Converting the Nutrition Column

We find that the nutrition column contains several nutritional values in the format: ['calories', 'total fat (PDV)', 'sugar (PDV)', 'sodium (PDV)', 'protein (PDV)', 'saturated fat (PDV)', 'carbohydrates (PDV)']. We splitted the values in the nutrition column and assigned each resulting series of values with its own column. This improves the accessibility and readability of the dataframe.

### Dropping Columns

After merging, ‘recipe_id’ and ‘id’ are duplicated, therefore we drop the ‘recipe_id’ column.

Nutrition column is also dropped because it is no longer needed as we have extracted out each individual value and formed new columns.

Finally we dropped other columns, 'name', 'contributor_id', 'submitted', 'tags', that are irrelevant for our analysis.

### Cleaning Outliers

We found that there are some recipes containing significantly unusual values(more than 10,000 calories) for the amount of calories. Therefore, we drop the outliers(calories data that is greater than the 99.9 percentiles) in calories.

### Cleaning Result


**Below is the head of our cleaned dataframe**:

|     id |   minutes |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | ingredients                                                                                                                                                                                                                                                                                                                                               |   n_ingredients |   rating |   calories |   total fat |   sugar |   sodium |   protein |   saturated fat |   carbohydrates | calories_category   |
|-------:|----------:|----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|---------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|:--------------------|
| 486161 |        60 |         7 | ['heat oil in a 4-quart dutch oven', 'add celery , onion , sweet pepper and garlic', 'cook for 5 minutes over medium heat', 'stir in ham , paprika , sugar , dry mustard , cumin , basil , oregano , thyme , cloves , black pepper and cayenne pepper', 'cook for 5 more minutes , stirring frequently', 'stir in black-eyed peas , hominy , undrained tomatoes , chicken broth , parsley and molasses', 'bring to boil , then reduce heat and cover dutch over and let soup simmer for 30 minutes']                                                                                                                                                                                                                                                                                                                                         | this is a delicious soup that i originally found on the better homes and gardens website, but i've seen it reprinted on a variety of other cooking and recipe sites.  i tried to research the background so i could accurately list its origin when posting this recipe.  but there doesn't seem to be any sort of consensus whether this soup is more accurately classified as a cajun or creole recipe. it seems to be one of those age-old questions that has no definite answer one way or the other, though most sites seem to agree that its roots are firmly planted in louisiana.  so, i'll leave it up to each individual to decide if this is cajun or creole. | ['celery', 'onion', 'green sweet pepper', 'garlic cloves', 'olive oil', 'cooked ham', 'paprika', 'sugar', 'dry mustard', 'ground cumin', 'dried basil', 'dried oregano', 'dried thyme', 'ground cloves', 'black pepper', 'cayenne pepper', 'black-eyed peas', 'yellow hominy', 'diced tomatoes', 'low sodium chicken broth', 'fresh parsley', 'molasses'] |              22 |        5 |      415.2 |          26 |      34 |       26 |        44 |              21 |              15 | 0-1000              |
| 493372 |         5 |         1 | ['mix all ingredients together thoroughly']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | this spice mix will make your taste buds dance!                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | ['paprika', 'salt', 'garlic powder', 'onion powder', 'dried basil', 'dried oregano', 'dried tarragon', 'dried thyme', 'powdered sugar', 'black pepper', 'cayenne pepper', 'red pepper flakes', 'celery seed']                                                                                                                                             |              13 |        5 |       14.8 |           0 |       2 |       58 |         1 |               0 |               1 | 0-1000              |
| 308080 |        40 |         7 | ['in a bowl , combine the mashed yolks and mayonnaise', 'stir in the mustard', 'stir in the cajun seasoning and tabasco', 'taste , then season with salt and pepper', 'fill the whites evenly with the mixture', 'garnish with parsley', 'chill a few hours before serving']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | deviled eggs, cajun-style                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | ['hard-cooked eggs', 'mayonnaise', 'dijon mustard', 'salt-free cajun seasoning', 'tabasco sauce', 'salt', 'black pepper', 'fresh italian parsley']                                                                                                                                                                                                        |               8 |        5 |       59.2 |           6 |       2 |        3 |         6 |               5 |               0 | 0-1000              |
| 298512 |        29 |         9 | ['place melted butter in a large mixing bowl and add each remaining ingredient as listed , beating well after each addition to achieve a smooth dough', 'let dough stand uncovered 10 minutes', 'make golf-ball-sized shapes of the dough and insert a flat , wooden popsicle stick into one side of each ball', 'carefully flatten each ball of dough to a 1 / 2 inch thick patty', 'place 3 inches apart on greased baking sheets', 'bake at 400f for 6-8 minutes or til just browned around the edges , but still white on tops of cookies', 'remove from the oven and smear the top of each cookie generously with heated , canned , ready-to-spread frosting', 'frostings may be tinted with few drops food coloring once it has been heated', 'let cookies cool on baking sheet 10 minutes before carefully removing to paper towels'] | i've heard of the 'cookies by design' company, but have never tried their cookies. this recipe is supposed to be like theirs.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | ['butter', 'eagle brand condensed milk', 'light brown sugar', 'sour cream', 'egg', 'extract', 'nutmeg', 'self-rising flour', 'bisquick', 'wooden popsicle sticks']                                                                                                                                                                                        |              10 |        1 |      188   |          11 |      57 |       11 |         7 |              21 |               9 | 0-1000              |
| 298509 |        20 |         5 | ['whip sugar and shortening in a large bowl , add eggs and beat well', 'add remaining ingredients and mix', 'knead dough into ball , roll out and cut', 'place on cookie sheet', 'bake at 350 degrees f for 10 to 12 minutes']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | i've heard of the 'cookies by design' company, but have never tried their cookies. this is supposed to be a copy-cat of their sugar shortbread cookie.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | ['granulated sugar', 'shortening', 'eggs', 'flour', 'cream of tartar', 'baking soda', 'vanilla extract']                                                                                                                                                                                                                                                  |               7 |        3 |      174.9 |          14 |      33 |        4 |         4 |              11 |               6 | 0-1000              |
## Univariate Analysis:

### We examined the distribution of calories:



From the graph, we can conclude that the distribution of calories follows Gaussian distribution and is skewed right. We also conclude that the distribution centers around 300-400 calories, and most data is distributed in the range [0,1000]. Hence, we can say that most of the recipes contain less than or equal to 1000 calories.

<iframe src="assests/histgram_avg_rating.html" width=1000 height=800 frameBorder=0></iframe>

### Distribution of Rating

We also looked at the distribution of ratings, and found that ratings are concentrated between the range from (4 to 5), indicating that the average ratings of most recipes are very high.

<iframe src="assests/histgram_calories.html" width=1000 height=800 frameBorder=0></iframe>

## Bivariate Analysis:

We performed bivariate analysis by plotting the amount of calories against average rating.

<iframe src="assests/scatter_plot_calories_rating.html" width=1000 height=800 frameBorder=0></iframe>


From the plot, it seems that most recipes are clustered in the lower calorie range (close to 0 to around 2000 calories) and have a wide range of ratings, from 1 to 5. There is a dense concentration of points with ratings of 4 and above, indicating that many recipes with a lower calorie count have higher ratings.

As the calorie count increases, the number of recipes decreases, which is evident from the fewer data points beyond 2000 calories. Interestingly, recipes with very high calorie counts (4000+) still maintain ratings mostly between 3 and 5, but there are much fewer of them.

The overall trend is not suggesting a clear positive or negative correlation between the number of calories and the average rating. Therefore, further investigation and analysis are needed.



We also categorized the amount of calories into 8 values ['0-1000', '1000-2000', '2000-3000', '3000-4000', '4000-5000', '5000-6000', '6000-7000', '7000-8000'], then we plotted a line of categorized amount of calories against average rating.

<iframe src="assests/line_plot_avg_rating_cate_calories.html" width=1000 height=800 frameBorder=0></iframe>


The result is very interesting and counterintuitive, we found that the average rating is fluctuating a lot around different categories, and there is a significant drop in rating at 4000-5000 and 5000-6000 calories. Afterward, there is a rise in rating at 6000-7000 and 7000-8000 calories. Indicating that high calories recipes are having a slightly higher average rating overall.

## Interesting Aggregates

We performed aggregate analysis to investigate the relationship between number of ingredients and number of steps on amount of calories. Specifically, we created a pivot table. On the row, there is the distribution of the number of steps. On the columns, there is the distribution of the number of ingredients. We calculate the average amount of calories within each combination of categories.





## Table Info

The columns categorize recipes based on the number of ingredients: fewer than 10, 10-20, 20-30, and 30 or more.
The rows categorize recipes based on the number of steps in the recipe: fewer than 20, 20-40, 40-60, and 60 or more.

|     <10 |   10-20 |    20-30 |   >=30 |
|--------:|--------:|---------:|-------:|
| 369.846 | 476.219 |  624.134 | 518.55 |
| 505.313 | 626.355 |  716.679 | 820.6  |
| 653.253 | 609.726 |  932.337 | nan    |
| 806.33  | 942.835 | 2021.28  | nan    |

## Observations

For recipes with fewer than 10 ingredients, as the number of steps increases, so does the average calorie count. Recipes with fewer than 20 steps have the lowest average calories (approximately 372.51), whereas those with 60 or more steps have the highest (approximately 821.24).

A similar trend is observed for recipes with 10-20 ingredients, starting from an average of approximately 477.63 calories for recipes with fewer than 20 steps, peaking at approximately 880.50 calories for recipes with 60 or more steps.

For recipes with 20-30 ingredients, the calories start higher (approximately 617.00) for recipes with fewer than 20 steps and significantly jump for recipes with 60 or more steps (approximately 1699.82).

The NaN (Not a Number) values indicate missing data, possibly because there are no recipes in the dataset that have 40-60 or more than 60 steps with 30 or more ingredients.

Overall, this table suggests that there may be a general trend where recipes with a greater number of steps and ingredients tend to have higher average calories.

# Assessment of Missingness:

## NMAR Analysis:

In our dataframe, we observed that some recipes do not have descriptions, and we believe that the missingness of “description” is NMAR. This is because we believe that those recipes that are self-explanatory and require little amount of descriptions might end up not having descriptions at all. To explain the missingness, we could possibly obtain information on user demographics. For example, if we could collect the recipe creator’s age, we might be able to conclude that older creators don’t like to write descriptions.

## Missingness Dependency:

Now, we will assess the missingness of ‘average rating’ in our dataframe. We will test the dependency of the missingness of ‘average rating’ on ‘calories’ and ‘sodium’.

## Calories and Rating

Null hypothesis: the distribution of the calories when average rating is missing is the same as the distribution of the calories when average rating is not missing.

Alternative hypothesis: the distribution of the calories when average rating is missing is different from the distribution of the calories when average rating is not missing 

Observed Statistics: we choose k-s as our test statistic.


### Distribution of Column ‘calories’ when Column ‘average rating’ is Missing and not Missing

<iframe src="assests/missingness1.html" width=1000 height=800 frameBorder=0></iframe>

### Result Interpretation

After performing the k-s test, we found the p-value is approximately equal to 8.92e-9, which is significantly lower than the p-value threshold of 0.05. Therefore, we determined that it is very unlikely that the null hypothesis is true. We reject the null hypothesis and conclude that average rating is dependent on the amount of calories, and missingness of average rating is the MAR. 

## Sodium and Rating:

### Null hypothesis

The distribution of the sodium when average rating is missing is the same as the distribution of the sodium when average rating is not missing.

### Alternative hypothesis

The distribution of the sodium when average rating is missing is different from the distribution of the sodium when average rating is not missing 

### Observed Statistics

We choose k-s as our test statistic.

### Distribution of Column ‘sodium’ when Column ‘average rating’ is Missing and not Missing

<iframe src="assests/missingness2.html" width=1000 height=800 frameBorder=0></iframe>

### Result Interpretation

After performing the k-s test, we found the p-value is approximately equal to 0.113, which is higher than the p-value threshold of 0.05. Therefore, we determined that it is likely that the null hypothesis is true. We fail to reject the null hypothesis that average rating is not dependent on the amount of sodium. And the missingness of average rating is the MCAR. 


# Hypothesis Testing

The question we would like to address is:

What is the relationship between the amount of calories and average rating of recipes?


## Null Hypothesis:

Our research question led us to the hypothesis that:
**There doesn't exists a linear relationship between variable ratings and variable calories.**

### Alternative Hypothesis
**There exists a linear relationship between variable ratings and variable calories.**


### Test Statistic

Our choice of test statistic is correlation coefficient since correlation coefficient measures the linear relationship between two variables.

### Significance Level
0.05

### P-values
From our experiment, the p_value we received is around 0.078.

### Justification
In the hypothesis, we randomly shuffled the values in calories columns in order to do a permutation test. Each time, after we permuated the values of calories, we use 'corr' function from panda to calculate the correlation coefficient between variable ratings and variable calories. After iterating same procedures for 10000 times, we calculate the p_value stated above, and our experiment result fails to reject our null hypothesis.

<iframe src="assests/hypothesis_test.html" width=1000 height=800 frameBorder=0></iframe>

### Conclusion

From our experiment, we can see that we fail to reject the null hypothesis that there does not exist a linear relationship between variable ratings and variable calories.
