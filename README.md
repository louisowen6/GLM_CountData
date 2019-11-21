# Mortality Rate Prediction based on Age and Cigarette Types in Canada using Generalized Linear Model for Count Data 

You can find the powerpoint slides in Bahasa Indonesia [here](https://github.com/louisowen6/GLM_CountData/blob/master/PPT.pptx)

You can find the notebook [here](https://github.com/louisowen6/GLM_CountData/blob/master/Model)

## Background

- Smoking is an activity that is commonly done for some groups, especially men
- Smoking does not give any positive impacts

This project aims to:
1. Build a regression model that can model the number of male deaths based on age categories and types of cigarettes consumed
2. Knowing the age categories of men and the types of cigarettes consumed which results in the highest and lowest mortality rates

## Dataset

The data source is from "Daly, Fergus, Hand, David.J, and McConway D. (1993) “A Handbook of Small Data Sets”, CRC Press, 390."

This data is a grouped data which has 36 levels with Dead (the number of men dead) as the response variable and Age (age of men), Smoke (type of cigarettes) as the predictor variables. Age variable has 9 levels and Smoke variable has 4 levels.

You can see the data [here](https://github.com/louisowen6/GLM_CountData/blob/master/Smoking.xlsx)

## Model

There are 2 types of GLM for count data which tested: **Poisson Regression** and **Negative Binomial Regression**

## Hypothesis Testing

There are several hypothesis testing used in this project, including:
  - Wald Test
  
    This test aims to test the parameter significance in GLM regression. 

