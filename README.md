# cax_mortgage

## Entry in Data Science Competition

This project was completed as part of Module 3 for the Flatiron School Data Science Immersive, March 11 2019 Cohort.

## Project Objective

From the contest organizer:

Propensity to fund mortgages describes the natural tendency for a mortgage to be funded based on certain factors in a customer’s application data.

To predict whether a mortgage will be funded using only this application data, certain leading factors driving the loan’s ultimate status will be identified. Solvers will discover the specific aspects of the dataset that have the greatest impact, and build a model based on this information.

## Methods Used

* Data Cleaning
* Exploratory Data Analysis
* Feature Engineering
* Random Over Sampling to adjust for Class Imbalance
* Logistic Regression
* Decision Tree Classifier
* Random Forest Classifier
* XGBoost
* RandomizedSearch + GridSearch for Hyper Parameter Tuning
* VotingClassifier

## Dataset Summary

* 45,642 observations
* Target Variable: Result (Funded or Not Funded)
* 22 Features
* Class Imbalance: Funded: 36,023 ( 78.9% ) / Not Funded:	9,619 ( 21.1% )

## Outliers

* Dropped 1,348 observations (0.30%) based on Mortgage Amount or Property Value more than 3 Standard Deviations higher than Mean.

# Exploratory Data Analysis
