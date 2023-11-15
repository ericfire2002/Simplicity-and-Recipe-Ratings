# Simplicity and Time in Recipe Ratings

by: Eric Mei, Vivek Srinivasan

--- 

## Introduction

We chose to analyze two datasets –recipes and interactions– for this project. The recipes dataframe contains information on recipes, as would be seen on a website or blog, while interactions contains ratings for the recipes within the recipes Dataframe. After exploring the frames’ information, we ran into our central question; Are simpler and faster recipes more likely to be highly rated? We chose this question because of its significance in the real world. For people who do not have time to cook complicated dishes, a Dataframe such as recipes, with quick, easy, and well-liked meals can go a long way in promoting self-support.

### Do Quicker/Simpler Recipes Result in Different Ratings?**

Many people do not have the time to prepare longer or more involved meals, so quicker or simpler recipes can make a big difference in the quality of life for many people. In this project, we define simple as having fewest number of steps.

---

## Cleaning and EDA

### Data Cleaning

We first extracted both Dataframes, and then got the average rating per recipe from interactions, and added it to recipes via a Dataframe merge. We then converted the ‘nutrition’ column from a single series of lists to their own individual columns of floats, one for each nutritional value, and converted the ‘submitted’ column to DateTimes. We then created a categorical distribution for the ratings for each recipe based on the number of stars to get a better picture of the distribution of minutes and steps for our analysis. Finally, we omitted all recipes that took longer than a day. These recipes were either not for meals (pickles or preserves), or were not meant to be quick or easy. Therefore, they had no relevance to our central question. Once this was finished, we were ready to begin analysis. 

| name                                 | submitted   |   minutes |   n_steps | description                                                                                                                                                                                                                                                                                                                                                                       |   avg_rating |
|:-------------------------------------|:------------|----------:|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------:|
| 1 brownies in the world    best ever | 2008-10-27  |        40 |        10 | these are the most; ...              |            4 |
| 1 in canada chocolate chip cookies   | 2011-04-11  |        45 |        12 | this is the recipe t...                                                                                                                                           |            5 |
| 412 broccoli casserole               | 2008-05-30  |        40 |         6 | since there are alre... |            5 |
| millionaire pound cake               | 2008-02-12  |       120 |         7 | why a millionaire po...                                                                                                        |            5 |
| 2000 meatloaf                        | 2012-03-06  |        90 |        17 | ready, set, cook! sp...                                                                                                         |            5 |

### Univariate Analysis

In this project, we defined simplicity through a single variable, the number of steps in the recipe. We also defined quickness through the minutes required for each recipe. Due to this, our analyses were focused on the distributions of step numbers and minutes across our dataframe. We used histograms to represent the frequencies of these quantitative variables. 



A distribution of the frequency of the number of steps, skewed to the right. This shows that most of the recipes are relatively simple, as expected. 

<iframe src="assets/fig_1.html" width=800 height=600 frameBorder=0></iframe>




A distribution of the frequency of the number of minutes, skewed heavily to the right, as recipes are usually completed in an hour, as expected. 

<iframe src="assets/fig_2.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

Once the variables of interest were explored on their own, we wanted to see their relationships with the average recipe ratings. We constructed two scatterplots, one for minutes and number of steps, in order to see if there was an obvious correlation based on either variable.  




The number of steps vs. the average rating of each recipe. While there is no obvious trend in the data, there seems to be more recipes clustered at higher ratings and lower number of steps, suggesting a slightly negative correlation in the data. 

<iframe src="assets/fig_3.html" width=800 height=600 frameBorder=0></iframe>



The time of each recipe vs. the average rating. As with the number of steps, there seems to be an extremely loose correlation towards low minutes and high ratings, though there is a large range in the time for the recipes. 

<iframe src="assets/fig_4.html" width=800 height=600 frameBorder=0></iframe>

---

### Interesting Aggregates

We chose to find the mean number of steps and minutes for each categorical rating, to again determine if there were any obvious differences between low and highly-rated recipes. This table shows a slight negative correlation, consistent with our bivariate analyses, between average ratings and both minutes and steps. However, we were unsure if this was due to chance. 

| rate_intervals   |   n_steps |   minutes |
|:-----------------|----------:|----------:|
| 0 - 1            |  11.4629  |   73.7275 |
| 1 - 2            |  10.7183  |   68.9882 |
| 2 - 3            |  10.7524  |   69.4882 |
| 3 - 4            |  10.3165  |   71.1551 |
| 4 - 5            |   9.96339 |   59.9275 |

This table shows the trend that lower number of steps and minutes tend to result in higher ratings.

## Assessment of Missingness

### NMAR Analysis

There were only 3 columns with missing values: ‘name’, ‘avg_rating’, and ‘description’. Of these columns, both ‘name’ and ‘avg_rating’ had only 1 missing value, and were therefore too small of samples to confirm a type of missingness. Therefore, we looked to recipe descriptions to analyze missingness in our dataset. We determined that there was no difference in the missing descriptions and the non-missing descriptions, therefore the data was not NMAR, as the missingness did not depend on the values in the ‘description’ column itself. Furthermore, the missing values were seemingly at random, so we concluded that they were not missing by design. 

---

### Missingness Dependency

To assess the missingness of the data, we hypothesized that the missing values were related to date, as there were no other columns that showed a visible difference in means between recipes groups of missing and nonmissing descriptions. Our null hypothesis was that the missing and nonmissing descriptions would have no absolute difference in mean submission dates. We used the absolute difference of time submitted between missing and nonmissing description groups as our test statistic, and conducted 1000 random permutations, resulting in a p-value of 0.027. This value is low enough to reject the null hypothesis at a 0.05 significance, but not a 0.01 significance level. Therefore, we treated the data as MAR at 0.05 significance, but as MCAR at a 0.01 significance level. 

<iframe src="assets/fig_5.html" width=800 height=600 frameBorder=0></iframe>

## Hypothesis Testing

Because our data was based on three quantitative variables, we used a linear regression for each independent variable to get a sense of the relationship between the number of steps and minutes, and the average recipe rating. We opted for single regression to determine the correlation between each variable; in the event that there was no correlation between one variable, but a correlation between the other, multiple regression would not show the effect of each variable. We therefore used the r-value as our test statistic. Our null hypothesis was that across all ratings, the number of minutes and steps would be the same, suggesting a correlation of 0. With initial statistics of -0.04355 for the number of minutes, and -0.04115 for the number of steps, we wanted to determine whether this negative correlation was due to chance. We performed 10000 trials, each time shuffling the average rating, and getting the r-value for each independent variable. This resulted in p-values of 0.0 for both the number of steps and minutes. This allowed us to reject the null hypothesis, and strongly suggests that our initial hypothesis was correct. There is very likely a correlation between both lower numbers of steps and minutes, and a higher average rating.  