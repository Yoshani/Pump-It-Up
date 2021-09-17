# Pump it Up: Data Mining the Water Table

Github Link: https://github.com/Yoshani/Pump-It-Up

This repository contains the code used for submissions made on the competition Pump it Up: Data Mining the Water Table, hosted by DrivenData.

For the competition, two main submission codes are presented. The benchmark code mainly consists of all the preprocessing and feature engineering approaches that were tried for the dataset. With this, the random forest model only was able to achieve a score of 0.8143 in the competition.
The best submission code consists of the simple code where only the best fitting approaches were carried out, and it was able to achieve my personal best score of 0.8255. 
Apart from these, the EDA, outlier analysis, feature selection, and post processing that was conducted on the benchmark code are presented as separate files for clarity. 

## Exploratory data analysis

* Pandas profiling was conducted. It revealed that some columns are highly skewed, some have a high cardinality, and some have a very high missing value count. 
* Analysis of the target revealed that the dataset is highly imbalanced. 
* Some columns were identified to have unnatural zero values, these were replaced with NaN to conduct an analysis of missing values. 
* An analysis of some columns vs the target was conducted to identify how the columns affect the target. Some revelations: 
  * There are a lot of wells with sufficient water that are not functioning. These should be repaired first.
  * All of the dry water pumps are deemed not working. Probably only because it is dry, but the pump may actually be working. The dryness should be attended to in this case.
  * Older wells are mostly non functioning
  * Mostly salty water and water with the unknown quality have affected the pumps to be nonfunctional
  * Pumps which get a payment for water are more functional
* A further analysis on numeric columns revealed that in many columns there exist a dominant number of zeros. The zeros in most cases must be unknown values since columns like construction year cannot have a 0
* A correlation analysis on the numeric attributes revealed:
  * District code and region code have a strong positive correlation
  * Construction year and gps height also have a strong positive correlation
  * Longitude and latitude are very strongly negatively correlated
  * District code and region code are strongly negatively correlated with gps height and latitude

## Outlier analysis

Several boxplots were plotted to identify outliers in the data.

* The zeros in longitude column were identified as outliers
* Gps height, latitude, and construction year showed no significant outliers.
* However amount tsh, population, region code and district code showed to have many outliers

## Feature selection

* Mutual information scores plot was plotted
* K best selections were also done separately on numeric and categorical attributes to select the best features

## Handling missing values

* Columns ‘funder’ and ‘permit’ were filled with dummy value ‘unknown’
* Column ‘construction year’ was filled with dummy value 1950
* Columns ‘installer’, ‘public meeting’, and ‘scheme management’ were filled with mode
* Column ‘population’ was filled by median grouped by region code
* Zero values of ‘longitude’, ‘latitude’ and ‘gps height’ were filled with mean
* All remaining values were filled with means of respective columns

## Feature Engineering

* Feature ‘operation time’ was newly formed using ‘date recorded’ and ‘construction year’
* Log of ‘population’ was added to normalize the population column
* Feature splitting was done on ‘date recorded’ to extract year, month and day
* PCA was tried on ‘longitude’ and ‘latitude’ columns to see the effect of other axes of variation

## Encoding

* Binary encoding was done on high cardinality columns
* Ordinal encoding was done to permit column
* Label encoding was done on remaining columns having cardinality greater than 10
* Remaining columns were one hot encoded

## Normalization

* Both min max normalization and standard normalization were tried. Min max normalization performed better

## Hyperparameter tuning

* This was done using grid search for models Random Forest and XGBoost to identify the best hyperparameters

## Models

The following models were used:

* RandomForestClassifier
* ExtraTreesClassifier
* XGBClassifier
* GradientBoostingClassifier
* LGBMClassifier
* SVM
* CatBoostClassifier
* MLPClassifier

Ensembling was conducted using the following models

* StackingClassifier with LogisticRegression as final estimator
* VotingClassifier

## Post processing

* Model inbuilt feature importance, eli5 permutation importance and SHAP summary plots were used for plotting on models Random Forest, Extra Trees Classifier, and XGBoost to identify the most important features for each model
  * The dry quality of water is a very important feature to all models
  * Longitude seemed to matter more than latitude

* Partial dependence plots on XGBoost model revealed that:
  * Up to about gps height 500 the model's likeliness to predict functional decreases, but increases after that, there doesn't seem to be much effect on predicting functional but needs repair, predicting non functional is as expected, roughly the reverse of predicting functional
  * If operation time is more, it becomes more likely to predict it's functional, it becomes less likely to predict it's functional but needs repair, it becomes less likely to predict it's non functional
  * When population increases, chances of being functional increases and being non functional decreases
  * Population has no effect on predicting if it is functional but needs repair
  * If the pipe was built after around 2000, it becomes more likely that the model predicts it is functional
  * Similarly for pipes built after around 2000, the pipes are less likely to be predicted functional needs repair or non functional
* SHAP dependence contribution plots revealed that:
  * The nearer the construction year, the more likely it is to be functional. Also, the likelihood of recently built pipes to be dry is very low.
  * Towards the middle of the year, the pipes are less likely to be dry, probably since Tanzania is reported to have the monsoon season roughly between March-May. Also during this period the tendency for the prediction to be functional is more. This observation may be somehow related to the fact that some pipes may be reported to be non-functional only because the well is dried up, and they are now functional due to rain.

## Proof of Submission

Drivendata user name: moracse_170494F

![rank](https://user-images.githubusercontent.com/46979289/133734792-b16b76f7-9e14-4463-b5d6-83a1c18d7253.png)
![submissions](https://user-images.githubusercontent.com/46979289/133734821-96f1465c-404a-42b4-8aa1-b9208c179ad1.png)

