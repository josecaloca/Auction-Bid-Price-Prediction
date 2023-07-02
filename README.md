# Digital Turbine's Auction Bid Price Prediction

## Background
To optimize our Ad Exchange's performance against competitors, we must predict their bids and the winning bid for each request.

## Goal
The goal of the task is to predict the winning bid price (winBid) in $ CPM for a given auction, based on a set of auction attributes (features) and the information about whether we (DT) won the auction (has_won).

## Problem type
Regression

## Solution
Random Forest Regressor with tuned hyperparameters:
- max_depth = 10
- min_samples_leaf = 0.03
- min_samples_split = 0.05
- n_estimators = 219

## Model performance (train sample)
- Mean Squared Error: 15.52
- R-squared: 0.074
- Mean Absolute Error: 0.278
- Root Mean Squared Error: 3.94

## Data Preparation
1. The training dataset includes a flag variable called 'has_won' which indicates whether DT Exchange won the auction (1) or not (0).
2. The training dataset is filtered to focus on auction instances that were won by DT Exchange. i.e has_won = 1
3. The data is analysed and planed a data cleaning strategy.

## Feature Engineering
The following steps were performed for data handling and creating a final dataset for training the algorithm:
- Creation of new time-related features using the eventTimestamp column.
- Identification of the top 8 popular brands based on the brandName column and addition of a new column indicating whether the brand is popular or not.
- Extraction of the app category, major version, and minor version from the bundleId and appVersion columns.
- Aggregation of bid floor price and sent price statistics per deviceId.
- Aggregation of bid floor price and sent price statistics per day, hour, and minute.
- Aggregation of unitDisplayType counts per day, hour, and minute.
- Extraction of the OS and version from the osAndVersion column.
- Addition of a new column continent indicating the continent based on the countryCode.
- Extraction of the device width and height from the size column.
- Splitting the mediationProviderVersion into three parts and converting them to integers.
- Modification of the os column using the check_os_type() function.
- Filling missing values in specific columns with default values and dropping rows with missing values in the continent column.
- Performing one-hot encoding for categorical variables.

## Modeling + Evaluation
Given that the modeling approach involves using a random forest regression algorithm, this particular type of algorithm is capable of handling variables that come from various distributions, have different scales, contain outliers, and exhibit high correlation among features. Hence, there is no need for additional pre-processing steps to be performed.

Variable selection: A default random forest model was used to select variables with variable importance higher than the mean of importances.

Hyperparameter tuning was performed using Bayesian optimization over hyperparameters and 5-fold cross-validation on 5 iterations.

A final model was trained on filtered variables and final tuned parameters.

Final predictions were made on the test set for final submission.

