# CAX Mortgage Competition

### Entry in Data Science Competition

This project was completed as part of Module 3 for the Flatiron School Data Science Immersive, March 11 2019 Cohort.

### Project Objective

<i>From the contest organizer:</i>

Propensity to fund mortgages describes the natural tendency for a mortgage to be funded based on certain factors in a customer’s application data.

To predict whether a mortgage will be funded using only this application data, certain leading factors driving the loan’s ultimate status will be identified. Solvers will discover the specific aspects of the dataset that have the greatest impact, and build a model based on this information.

### Methods Used

* Data Cleaning
* Exploratory Data Analysis
* Feature Engineering
* Random Over Sampling to adjust for Class Imbalance
* SMOTE & ADASYN for Class Imbalance (excluded from Final Model)
* Principal Component Analysis (excluded from Final Model)
* Logistic Regression (excluded from Final Model)
* Decision Tree Classifier
* Random Forest Classifier
* XGBoost
* Extra Trees (excluded from Final Model)
* AdaBoost
* GridSearch for Parameter Tuning
* VotingClassifier

### Dataset Summary

* 45,642 observations
* Target Variable: Result (Funded or Not Funded)
* 22 Features
* Class Imbalance: Funded: 36,023 ( 78.9% ) / Not Funded:	9,619 ( 21.1% )

### Outliers

* Dropped 1,195 observations (0.32%) based on Mortgage Amount or Property Value more than 3 Standard Deviations higher than Mean, or Rate, LTV, TDS, or GDS above a certain threshold.

## Exploratory Data Analysis

Where applicable:

* Funded: 1
* Not Funded: 0

#### Property Value Histogram:

![download (4)](https://user-images.githubusercontent.com/42282874/58130012-14ea3300-7be9-11e9-8049-31675060373d.png)

<i>Slightly right-skewed, Property Value will be log-transformed.</i>


#### Result by Mortgage Loan Rate:

![rate](https://user-images.githubusercontent.com/42282874/58130026-1c114100-7be9-11e9-8ea5-3413413f0494.png)


#### Result by Territory (Canada):

![territory](https://user-images.githubusercontent.com/42282874/58130031-1c114100-7be9-11e9-9aa8-4b863f008eae.png)

<i> Only 1 observation found in Territory 'W', was removed. Only 9 in Territory 'X' (Northwest Territories).</i>


#### Result by Mortgage Purpose:

![mortgage_purpose](https://user-images.githubusercontent.com/42282874/58130032-1c114100-7be9-11e9-94db-313bf1886ee9.png)

#### Result by Age Range:

![age_range](https://user-images.githubusercontent.com/42282874/58130034-1c114100-7be9-11e9-9d2f-6b136dd201d8.png)

#### Rate + Credit Score:

![credit_x_rate](https://user-images.githubusercontent.com/42282874/58130024-1b78aa80-7be9-11e9-9a70-fff284fee533.png)

#### LTV by Result:

![result_ltv](https://user-images.githubusercontent.com/42282874/58180172-9ee0dd00-7c77-11e9-8f72-fd9f9859a092.png)

#### Correlation Matrix (prior to Feature Engineering):

![corr_1](https://user-images.githubusercontent.com/42282874/58131617-1e759a00-7bed-11e9-8d4b-9a180cc0c22f.png)

## Encoding & Feature Engineering

Primarily utilized LabelEncoder to encode categorical features. 

Sample features engineered:

* 'Zeroes' for Credit Score, GDS, TDS
* Territories (Extracted from FSA) & Urban / Rural split
* Payment Income Ratio (Income / Mortgage Payment)
* Mortgage Constant
* Age Adjusted Income & Credit Score (Income or Credit Score over Age Range Encoding)

After Encoding & Featuring Engineer, the DataFrame contains 55 features.

#### Correlation Matrix (post Feature Engineering):

![corr_2](https://user-images.githubusercontent.com/42282874/58176683-d1d3a280-7c70-11e9-976c-1917c8475169.png)

## Modeling

Final Model utilizes AdaBoost (Decision Tree Base Estimator), Random Forests and XGBoost. Experimented with, but did not include Logistic Regression + Extra Trees. Parameters were tuned with GridSearchCV. Final submission was made using a VotingClassifier() with equal weights to the three models.

Random Over Sampling was used to adjust for Class Imbalance. Experimented with, but did not use SMOTE, ADASYN and Random Under Sampling methods.

Data was scaled with StandardScaler().

### AdaBoost with Decision Tree Base Estimator
criterion: entropy, max_depth: 25, n_estimators: 200, learning_rate: 0.015

* Train Accuracy score:  0.6661
* Test Accuracy score:  0.6547

* Train F1 score:  0.6618
* Test F1 score:  0.7502

![cm_ada](https://user-images.githubusercontent.com/42282874/58177099-b5843580-7c71-11e9-95a8-c8ae7f10ec67.png)

![roc_ada](https://user-images.githubusercontent.com/42282874/58177098-b4eb9f00-7c71-11e9-93b2-b2137f139116.png)

<i>AUC:  0.7258</i>

### Feature Importanace - AdaBoost (DecisionTree)
![ada_feature_impor](https://user-images.githubusercontent.com/42282874/58177360-3e9b6c80-7c72-11e9-92ad-0f2864822938.png)


## Random Forest Classifier
criterion: gini, max_features: 0.15, n_estimators: 350, max_depth: 30, min_samples_leaf: 5

* Train Accuracy score:  0.9948
* Test Accuracy score:  0.7927

* Train F1 score:  0.9948
* Test F1 score:  0.8750

![cmrfc](https://user-images.githubusercontent.com/42282874/58177464-6ee30b00-7c72-11e9-8134-7f64a5629295.png)

![roc_rfc](https://user-images.githubusercontent.com/42282874/58177463-6ee30b00-7c72-11e9-8da1-eedabdc56597.png)

<i>AUC:  0.7306</i>

### Feature Importanace - RFC
![featrure_import_rfc](https://user-images.githubusercontent.com/42282874/58177462-6ee30b00-7c72-11e9-8924-67a70c2e8a17.png)

## XGBoost
colsample_bytree: 0.75, learning_rate: 0.10, max_depth: 5, alpha: 10, n_estimators: 500, scale_pos_weight: 0.25

* Train Accuracy score:  0.8434
* Test Accuracy score:  0.7449

* Train F1 score:  0.8454
* Test F1 score:  0.8326

![xgb_cm](https://user-images.githubusercontent.com/42282874/58177856-1b24f180-7c73-11e9-85fb-c0cf78fc1b5f.png)

![roc_Xgb](https://user-images.githubusercontent.com/42282874/58177855-1b24f180-7c73-11e9-95d2-2246f7cf1bff.png)

<i>AUC:  0.7438</i>

## Voting Classifier

* Train Accuracy score:  0.9532
* Test Accuracy score:  0.7777

* Train F1 score:  0.9527
* Test F1 score:  0.8593

![cm_vc](https://user-images.githubusercontent.com/42282874/58181506-eb2d1c80-7c79-11e9-87c2-0c9f7cfb561e.png)

![roc_vc](https://user-images.githubusercontent.com/42282874/58181505-eb2d1c80-7c79-11e9-812c-bd774719e438.png)

<i>AUC:  0.7498</i>

## Conclusions

Random Forest achieved the best F1-Score and XGBoost achieved the best AUC. AdaBoost did the best job of identifying True Negatives (True Not Funded), so I included the model in my Voting Classifier. In addition, AdaBoost tended to underfit, which helped balance Random Forest + XGBoost, which both overfit.

My best submission score was 0.6605, which placed 49th out of 293 in the CAX competition. The top score was 0.7524, and the 10th-best score was 0.7066. 

<img width="1159" alt="Screen Shot 2019-05-22 at 9 27 25 AM" src="https://user-images.githubusercontent.com/42282874/58178244-d51c5d80-7c73-11e9-8668-4399a0bc7552.png">
