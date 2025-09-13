# Application of CRISP-DM Framework on Business Problem: What drives the price of a car?

**Assignment Notebook:** https://github.com

## Overview
In this application, we explore a dataset from Kaggle. The original dataset contained information on 3 million used cars. The provided dataset contains information on 426K cars to ensure speed of processing. The goal is to understand what factors make a car more or less expensive. As a result of the analysis is to provide clear recommendations to your client -- a used car dealership -- as to what consumers value in a used car. 
  
## CRISP-DM Framework
To frame the task, we will refer back to a standard process in industry for data projects called CRISP-DM. This process provides a framework for working through a data problem. Your first step in this application will be to read through a brief overview of CRISP-DM [here](https://mo-pcco.s3.us-east-1.amazonaws.com/BH-PCMLAI/module_11/readings_starter.zip). 
  
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
2. Categorical columns:  
   - <img width="212" height="550" alt="image" src="https://github.com/user-attachments/assets/f1340de2-b5a8-48d5-a763-3eefc988bd42" />  
   - Columns dropped upfront:
     - Drop 'VIN', 'model' - too many distinct values to be useful in predictions.
   - Columns dropped after EDA:
     - Drop 'state', 'region' - assume that car prices don't vary significantly across states and regions.
     - Drop 'size' - significant number of NULL values.
     - Drop 'title_status' - data is dominated by a single value 'clean'.
     - Drop 'manufacturer', 'type', 'paint_colour' - these have a fair number of distinct values and are parked for future assessment.
   - Selected Features:
     - Select key features as: 'fuel', 'condition', 'transmission', 'drive', 'cylinders' - these are the key features potential car buyers will look our for when they purchase a car.



   




## Data Cleaning
Numeric columns:
  - Rows were filtered based on the data assumptions listed above.
  - The year that the car was manufactured can also be replaced with the age of the car (using current year as 2025).
