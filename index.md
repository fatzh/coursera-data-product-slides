---
title       : Child height predictor
subtitle    : Predict the child height using historical data from Galton.
author      : Fabrice Tereszkiewicz
job         : 
framework   : io2012     # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

--- .class #id 

## Introduction

This presentation will show how the Galton historical data on children heights can be used to predict the height of a children based on the child gender and its parents heights.

### The Galton dataset

The Galton dataset gives data based on the famous 1885 study of Francis Galton exploring the relationship between the heights of adult children and the heights of their parents.

### The prediction application

We will use this dataset to create a model and use it in an applicaiton where the user will be able to enter the child gender and its parents height, and the system will return a prediction for the height of this child based on the input parameters.

--- .class #id

## Exploratory analysis



The Galton dataset comes in multiple flavours in R. We will use the most complete one, called `GaltonFamilies`. This dataset contains 934 observations and 8 features, and it includes the mother and father heights as separate values which is useful for our application.


```
## Observations: 934
## Variables: 8
## $ family          (fctr) 001, 001, 001, 001, 002, 002, 002, 002, 003, ...
## $ father          (dbl) 78.5, 78.5, 78.5, 78.5, 75.5, 75.5, 75.5, 75.5...
## $ mother          (dbl) 67.0, 67.0, 67.0, 67.0, 66.5, 66.5, 66.5, 66.5...
## $ midparentHeight (dbl) 75.43, 75.43, 75.43, 75.43, 73.66, 73.66, 73.6...
## $ children        (int) 4, 4, 4, 4, 4, 4, 4, 4, 2, 2, 5, 5, 5, 5, 5, 6...
## $ childNum        (int) 1, 2, 3, 4, 1, 2, 3, 4, 1, 2, 1, 2, 3, 4, 5, 1...
## $ gender          (fctr) male, female, female, female, male, male, fem...
## $ childHeight     (dbl) 73.2, 69.2, 69.0, 69.0, 73.5, 72.5, 65.5, 65.5...
```

--- .class #id
## Data Preprocessing

We only need the child gender and the parent heights for our model, also we would like the heights to be in centimeters.


```r
df <- select(GaltonFamilies, childHeight, father, mother, gender)
df$father <- df$father * 2.54
df$mother <- df$mother * 2.54
df$childHeight <- df$childHeight * 2.54
glimpse(df)
```

```
## Observations: 934
## Variables: 4
## $ childHeight (dbl) 185.928, 175.768, 175.260, 175.260, 186.690, 184.1...
## $ father      (dbl) 199.39, 199.39, 199.39, 199.39, 191.77, 191.77, 19...
## $ mother      (dbl) 170.18, 170.18, 170.18, 170.18, 168.91, 168.91, 16...
## $ gender      (fctr) male, female, female, female, male, male, female,...
```

--- .class #id
## The model

We will use a linear regression model based on the features we elicited on the previous slide.


```
##               Estimate Std. Error  t value      Pr(>|t|)
## (Intercept) 41.9639494 6.92709835  6.05794  1.999906e-09
## father       0.3928433 0.02867682 13.69899  4.818605e-39
## mother       0.3176101 0.03100038 10.24536  2.061354e-23
## gendermale  13.2460730 0.36019472 36.77476 1.678202e-183
```

We can see that the selected features are all highly significant (P-values very close to 0), and they explain 63.54% of the variance in the child height. This is good enough for this application.

## The application

The application has been developped using Shiny and can be accessed online [here](https://fatz.shinyapps.io/child_height_predictor/).
