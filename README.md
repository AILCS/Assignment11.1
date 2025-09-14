# Application of CRISP-DM Framework on Business Problem: What drives the price of a car?

**Assignment Notebook:** [[https://github.com](https://github.com/AILCS/Assignment11.1/blob/main/prompt_II%20(final).ipynb)](https://github.com/AILCS/Assignment11.1/blob/main/prompt_II%20(final).ipynb)

## Overview
In this application, we explore a dataset from Kaggle. The original dataset contained information on 3 million used cars. The provided dataset contains information on 426K cars to ensure speed of processing. The goal is to understand what factors make a car more or less expensive. The result of the analysis is to provide clear recommendations to a client -- a used car dealership -- as to what consumers value in a used car. 
  
## CRISP-DM Framework
To frame the task, a standard process in industry for data projects called CRISP-DM is referenced. This process provides a framework for working through a data problem. A brief overview of CRISP-DM ia available [here](https://mo-pcco.s3.us-east-1.amazonaws.com/BH-PCMLAI/module_11/readings_starter.zip). 
  
## Business Objective
1. Identify which of the following features to be used to predict the price of a car.
   - id
   - region
   - year
   - manufacturer
   - model
   - condition
   - cylinders
   - fuel
   - odometer
   - title_status
   - transmission
   - VIN
   - drive
   - size
   - type
   - paint_color
   - state 

2. Assess if the selected features can be used to predict the price of a car with reasonable accuracy.

## Data Understanding
Exploratory Data Analysis (EDA) was performed on the data before the following data assumptions were made.  
Data Assumptions and Findings:
1. Numeric columns:
   - The histograms of numeric columns from the raw data are plotted below.  
     - <img width="747" height="570" alt="image" src="https://github.com/user-attachments/assets/ea4c6a48-3d40-49a7-b5e6-f7881eb0e957" />  
   - The histograms show that outliers are skewing the plots. Further data analysis was done to reach the following data assumptions:
     - Price range of car lies within 1k and 300k.
     - Only cars from year 1990 have resale value.
     - Only cars with less than 500k clocked on the odometer have resale value.
   - After applying assumptions on the numeric columns, the histograms of the filtered data show more variation in the data counts.  
     <img width="730" height="537" alt="image" src="https://github.com/user-attachments/assets/a98df0e9-1eae-415f-b316-1f194d073cd9" />  
   - Positive correlation between car price and year. This correctly indicates that newer cars are more expensive.  
     <img width="770" height="565" alt="image" src="https://github.com/user-attachments/assets/862c54d4-4bff-467d-a0eb-a82acffd27a0" />  
   - Negative correlation between car price and distance travelled. This correctly captures that cars that have been used more are cheaper.  
     <img width="765" height="563" alt="image" src="https://github.com/user-attachments/assets/5e8f6cff-d9c6-4a19-a4d6-dc33025881cd" />
   - Selected Features:
     - Select key features as: 'year', 'odometer'
  
2. Categorical columns:  
   - Number of unique values for each categorical column is shown below: 
     - <img width="212" height="550" alt="image" src="https://github.com/user-attachments/assets/f1340de2-b5a8-48d5-a763-3eefc988bd42" />  
   - Columns dropped upfront:
     - Drop 'VIN', 'model' - too many distinct values to be useful in predictions.
   - Columns dropped after EDA:
     - Drop 'state', 'region' - assume that car prices don't vary significantly across states and regions.
     - Drop 'size' - significant number of NULL values.
     - Drop 'title_status' - data is dominated by a single value 'clean'.
     - Drop 'manufacturer', 'type', 'paint_colour' - these have a fair number of distinct values and are parked for future assessment.
   - Selected Features:
     - Select key features as: 'fuel', 'condition', 'transmission', 'drive', 'cylinders'.

## Data Preparation
1. Data Cleaning
  - All features other than the selected ones were dropped from the raw data.
  - Rows were filtered based on the numeric data assumptions (on 'price', 'year', 'odometer') listed above to remove outliers. Applying the filter removes NULL values for the selected numeric features.
  - For the remaining rows, NULL values for the following selected categorical features were filled based on the below:
    - 'fuel' - fill NULL with mode value i.e 'gas', which is reasonable as this is the case for majority of the cars
    - 'condition' - fill NULL with mode value i.e 'good', which is reasonable for a resale car
    - 'transmission' - fill NULL with mode value i.e 'automatic', which is reasonable as this is the case for majority of the cars
    - 'drive' - fill NULL with specific value 'fwd', which is reasonable as majority of the cars use front-wheel drive
    - 'cylinders' - fill NULL with specific value '4 cylinders', which is reasonable as majority of the cars run on 4 cylinders. Further, consolidate 3/5/10/12 cylinder counts to 'other' as these have relatively low counts.  
2. Data Transformation
  - The year that the car was manufactured was replaced with the age of the car (using current year as 2025).

**Cleaned Data**
Statistics of the car prices in the clean dataset is shown below. This would be helpful for comparison with the Root Mean Squared Errors (RMSE) results later.  
<img width="643" height="352" alt="image" src="https://github.com/user-attachments/assets/bf63d9ad-6451-4767-907c-85b2aa35e7ce" />

## Modelling
The clean data for modelling was shuffled before splitting into 80%/20% for train/test data. This prevents any ordering of the data from impacting the models.  

The following Column Transformations were applied:
- Numeric features: Polynomial Feature
- Categorical features: One Hot Encoding

**Model 1: Linear Regression**
- In this simple model, the polynomial degree was varied from 1 to 3 to assess if adding the polynomial features improved the model.
- Results: Polynomial degree 1 yielded the best results.
  - Polynomial Degree: [1, 2, 3]
  - Train RMSEs: ['9922.3432108642', '11437.6360049943', '12585.9282391401']
  - Test RMSEs: ['9636.3801039762', '11173.0485325417', '12354.9826787391']
- Polynomial degree 1 would be applied in subsequent models for simplicity.

**Model 2: Ridge Regression**
- In this L2 regularisation model, polynomial degree was kept at 1, while alpha was varied as [0.001, 0.1, 1.0, 10.0, 100.0, 1000.0] through a GridSearch. 5-fold cross validation was also used.
- Results: Alpha = 10 yielded the best result. 
  - Ridge (GridSearch) Train RMSE: 9922.3432149006
  - Ridge (GridSearch) Test RMSE: 9636.3796371393
  - Ridge (GridSearch) Best Alpha: 10.0
- Under Ridge Regression, the Train RMSE is marginally higher than Linear Regression, but Test RMSE is marginally lower. Based on Test RMSE, it is a slightly better model than Linear Regression.
- Ridge coefficients are shown below:  
  - <img width="452" height="537" alt="image" src="https://github.com/user-attachments/assets/7fe56ed7-c8b2-43f8-a636-a688adcf3844" />
  - Top 3 features contributing to higher prices: 'fuel_diesel', 'cylinders_8 cylinders', 'drive_4wd'
  - Top 3 features contributing to lower prices: 'age', 'odometer', 'fuel_gas'


**Model 3: Lasso Regression**
- In this L1 regularisation model, the same hyperparameters were used as for Ridge Regression.
- Results: Alpha = 0.1 yielded the best result. 
  - Lasso (GridSearch) Train RMSE: 9922.3432197779
  - Lasso (GridSearch) Test RMSE: 9636.3805397717
  - Lasso (GridSearch) Best Alpha: 0.1
- Under Lasso Regression, both Train RMSE and Test RMSE are marginally higher than both earlier models, indicating it is a slightly poorer model.
  - Lasso coefficients are shown below:  
    - <img width="460" height="540" alt="image" src="https://github.com/user-attachments/assets/65f5abce-1d13-4d1c-b79a-a2e76025d2da" />
    - Top 3 features contributing to higher prices: 'fuel_diesel', 'drive_4wd', 'cylinders_8 cylinders'
    - Top 3 features contributing to lower prices: 'age', 'odometer', 'cylinders_4 cylinders'

## Evaluation
**Features:**  
Selected features from the raw data included 'year' (or 'age'), 'odometer', 'fuel', 'condition', 'transmission', 'drive', and 'cylinders'. Together with One-Hot Encoding of categorical features, there are a total of 23 features.
  
**Models:**  
Three models were run on the cleaned dataset:
1. Linear Regression with polynomial features (degree: 1 to 3)
2. L2 regularisation using Ridge Regression with polynomial degree 1, alpha varied (0.001, 0.1, 1.0, 10.0, 100.0, 1000.0), and 5 fold cross validation.
3. L1 regularisation using Lasso Regression with polynomial degree 1, alpha varied (0.001, 0.1, 1.0, 10.0, 100.0, 1000.0), and 5 fold cross validation.

**Results:**  
Linear Regression (1 degree polynomial):
- Train RMSE: 9922.3432108642
- Test RMSE: 9636.3801039762

Ridge Regression (1 degree polynomial, alpha = 10):
- Train RMSE: 9922.3432149006
- Test RMSE: 9636.3796371393

Lasso Regression (1 degree polynomial, alpha = 0.1):
- Train RMSE: 9922.3432197779
- Test RMSE: 9636.3805397717
  
With the lowest Test RMSE, Ridge Regession performed the best (marginally) while Lasso Regression has the highest Test RMSE and performed the worst (marginally). Therefore Ridge Regression is the selected model.
  
Based on the selected Ridge Regression model:
- Top 3 features contributing to higher prices: 'fuel_diesel', 'cylinders_8 cylinders', 'drive_4wd'
- Top 3 features contributing to lower prices: 'age', 'odometer', 'fuel_gas'

The similarity of the Train and Test RMSE across all models also showed that the models were not overfit. 

Additionally, the RMSEs were within 1 standard deviation (15000) of the car prices of the training data. This is reasonable for a start and the model can be tweaked further to improve the prediction.


## Deployment
Dear Client,
  
I have identified a car price prediction model that estimates car prices using data you provided, specifically, 'price', 'year', 'odometer', 'fuel', 'condition', 'transmission', 'drive', and 'cylinders'.
  
Please note that the model is only good for estimating car prices ranging between 1k and 300k, cars from year 1990, and cars with less than 500k clocked on the odometer.
  
The prediction error based on your sample data is 9636, which is within a standard deviation (15000) of the car prices.
  
Additionally, below are the key findings:
- Top 3 features contributing to higher prices: diesel cars, 4-wheel drives, cars with 8 cylinders
- Top 3 features contributing to lower prices: older cars, cars used for longer distances, cars with 4 cylinders.
  
We can test the prediction model and continue to assess and refine it as we gather more data.
  
Please let me know if you have questions.
  
Regards,  
Chee Siong



