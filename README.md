# Simplicity and Time in Recipe Ratings

by: Eric Mei, Vivek Srinivasan

---

## Introduction

This is a project at UCSD for DSC 80. 

### Do Quicker/Simpler Recipes Result in Different Ratings?**

Many people do not have the time to prepare longer or more involved meals, so quicker or simpler recipes can make a big difference in the quality of life for many people. In this project, we define simple as having fewest number of steps.

---

## Cleaning and EDA

### Data Cleaning

We get the recipes dataset and merge to get average rating per recipe. Then, we elimnate recipes that take more than a day, since they are not recipes for meals necessarily, or they are intended to take over a day, so people are unlikely to consider the value of simplicity/quickness when making these recipes.

| name                                 | submitted   |   minutes |   n_steps | description                                                                                                                                                                                                                                                                                                                                                                       |   avg_rating |
|:-------------------------------------|:------------|----------:|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------:|
| 1 brownies in the world    best ever | 2008-10-27  |        40 |        10 | these are the most; ...              |            4 |
| 1 in canada chocolate chip cookies   | 2011-04-11  |        45 |        12 | this is the recipe t...                                                                                                                                           |            5 |
| 412 broccoli casserole               | 2008-05-30  |        40 |         6 | since there are alre... |            5 |
| millionaire pound cake               | 2008-02-12  |       120 |         7 | why a millionaire po...                                                                                                        |            5 |
| 2000 meatloaf                        | 2012-03-06  |        90 |        17 | ready, set, cook! sp...                                                                                                         |            5 |

### Univariate Analysis

This is a distribution of the number of steps it takes to make each recipe. The distribution is skewed right, with a majority values between 0 and 30 steps.

<iframe src="file_1.html" width=800 height=600 frameBorder=0></iframe>

This is a distribution of the number of minutes it takes to make each recipe. The distribution is skewed right, with a majority of values between 0 and 150 minutes.

<iframe src="file_2.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

This is a distribution of the number of steps vs. average rating for each recipe. There seems to be a higher density of number of steps with lower ratings, than with the density of number of steps with high ratings.

<iframe src="file_3.html" width=800 height=600 frameBorder=0></iframe>

This is a distribution of the number of minutes vs. average rating for each recipe. There seems to be a higher density of number of minutes with lower ratings, than with the density of number of minutes with high ratings.

<iframe src="file_4.html" width=800 height=600 frameBorder=0></iframe>

---

### Interesting Aggregates

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

No evidence that the data is NMAR - nothing differentiates these recipes from the others that are given (missingness is independent of the values w/in the column).

---

### Missingness Dependency

There are 3 columns with missing values. Only one column has more than 1 missing value, so we cannot conclude about the missingness in the other two columns, since the sample is too small. Missingness is present in the 'description' column. Our hypothesis is that it is dependent on the date of the recipe. We used a test statistic of diff of means to measure missingness. 

At a significance of 0.05, the descriptions are MAR based on the date the recipe was submitted. However, at a 0.01 significance, we cannot reject the null hypothesis and therefore we must label the data as MCAR. 

<iframe src="assets/file_5.html" width=800 height=600 frameBorder=0></iframe>

## Hypothesis Testing

We are using linear regression with each category to determine whether ratings are dependent on number of steps, minutes, or both. We are using the r-value as our test statistic for each category. The null hypothesis is that there is no observable correlation between either of the categories. Therefore r_min, r_steps = 0 as a null hypothesis.

We reject the null hypothesis in the case of both variables. The extremely low p-values of 0.0 for both cases show that there is a negative association between ratings and number of steps/minutes. This means that our initial hypothesis surrounding our driving question was correct; simpler/quicker recipes do tend to have higher ratings. 