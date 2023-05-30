# Project Title

Used Car Prices Prediction Model

## Description

The goal of this project is to derive a used car pricing model by understanding what factors make a car more or less expensive based on what consumers value in a used car.

## Getting Started

## Process
The original dataset of used car contain information on 3 million used cars. Then it is updated contains information on 426K cars to ensure speed of processing. 
1. Dataset is revied for null values and missing values
2. Checking for pricing abnormality on the extreme end for car more than 30 yrs with pricing above 100,000. This would consider as a "collectible" catagory. This portion of data is dropped as it has different set of valuation method than a traditional used car model.
3. Checking for pricing abnormality on the extreme end for car less than 30 yrs with pricing above 200,000. This would consider as a "exotic" catagory. This portion of data is dropped to avoid brand bias.
4. Certain Data is dropped such as invalid title, salvage condition, vehicle type such as as bus and truck. 
5. Certain model with pricing over 100000 has incorrect cyclinders data where it has updated to 8 cyclinder from 4 cyclinder for Ferrari and Porsche. Abnormality such as used car that has high are removed as well.
5. Price and Year that are zero are dropped as well.
6. Some parameters are dropped such as 'region','state','VIN','id','manufacturer','model','paint_color','title_status'.
7. Dataset is coded to remove all catagorical features.
8. Set X,y as coded with y as prices which is the target
9. Split data into Training and Test data
10. Run 3 different regression models using Pipeline and Gridsearch to find the optimal parameters such as alpha and coefficient

### Dependencies

* The python code muct be compiled with the libraries in the beginning of the code.
* Vehicle.csv should be upload and direction should be updated accordingly.


## CRISP-DM

# Business Understanding

The dataset contain data for used car of wide range from 1920s to 2022s with over 30 different OEMs and models. The model is not sliced specifically a single brand as this prediction model can be applied to marketplace company such as CarMax, Carvana & TrueCar where multiple brand of cars are hosted under a single platform. 

Succesful critieria include a well refined model that predict use car prices with a good RMSE rate. Based on the coefficient of the model, one can understand the importance of the features that contribute to the pricing. 

In terms of risk, the data model doesn't cover all type of vehicle where the model is further segmented to not include collectible car as it has a different valuation method compare to traditional used car. Exotic car are excluded as exotic car might have strong brand bias. Vehicle type such as bus and trucks are exlucded in the model as commerical vehicle likely have differet set of attribute comapre to consumer vehicles. 

# Data Understanding

Data is reviewed with observation as following
1. Ford, Chevrolet, Toyota, Nissan and Honda are the top 5 OEMs in user car listings data
2. Sedan, SUV and Pickup are the top 3 most popular choices of car type. 
3. Sedan & Mid-size are the most common sizes
4. White, Black and Silver are the top 3 colors
5. In terms of pricing, most # of listing has a price point around $10000
6. In terms of Year, the most listing with model year is 2013 which skew toward recent years and trend down for the lower year model. This make sense as most consumer wont prefer to buy model that are made in the past 10 yrs. A hypothesis is that car that are older than 15 yrs, parts may be discontinued and it would be harder to get service due to lack of support by technician. 
7. Full-size car has the most listing with range up to $150,000. Coupe and SUV also commend the higher pricing.

# Data Preparation

The dataset contain data for used car of wide range from 1920s to 2022s. Based on the initial of the price range, it may have some abnormlity. After investigation, the model will be focusing on car priced under $200,000 that has year model between 2022 dated back 30 yrs. Older than 30 yrs are considered as collectible

Data Check
- On cyclinders, 4 cyclinder engine are the most common among used car. Further investigation have shown that the data is incorrectly label for some of the highend car such as Ferrari and Porsche as the models should have V8 instead of 4 Cyclinders. After relabeling, it is shown that V8 and V6 cars typically has higher pricing.
- Check for data anormality on extreme pricing with incorrect attribute such as manufacturers and type (ie. truck)
- Several columns of data are dropped for the Pipeline setup. Columns such as VIN, ID, Manufacturer, Model, Region, State, Color, Title are dropped for the model. However it is retained to check for Feature Importance.

Catagorial Columns are convert to Numerical Columns using Get_Dummies. This is similar to OnehotCoder function.

# Modeling

3 variation of Linear Regression are performed on the dataset:
1) Sequential Selector with Linear Regression
2) Ridge Regression with Polynomial Features and Scaler
3) Lasso Regressio with Polynomial Features and Scaler

The model contain the following Features from best selector: 
Index(['year', 'odometer', 'condition_new', 'cylinders_10 cylinders',
       'cylinders_6 cylinders', 'cylinders_8 cylinders', 'fuel_diesel',
       'transmission_other', 'drive_fwd', 'type_wagon'],
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       # Coefficient values: 
===================
	year	odometer	condition_new	cylinders_10 cylinders	cylinders_6 cylinders	cylinders_8 cylinders	fuel_diesel	transmission_other	drive_fwd	type_wagon
model	2053.586764	-0.048171	9729.98916	-9275.467702	5585.434816	11795.49979	16142.386238	-21427.797458	-6410.940579	-5996.247501  

Based on the model, it shows:
1. Year
2. Odoemeter
3. Condition NEW
4. Cyclinders 10
5. Cyclinders 8
6. Cyclinders 6
7. Fuel Diesel
8. Transmission Other
9. Drive FWD
10. Wagon Type

Note that Odoemeter, Fuel Disesel, Wagan Type, FWD Drive has a negative impact to pricing where the rest are positive correlated. This make sense since more mileage on the car would reduce the resale value. Diseal, Wagan and FWD Drive has a better alterative of technology such as hybrid, AWD and SUV/Coupe type that command higher prices.

# Feature Importance

Based on Feature Importance techqniue, we are able to calculate a score to the predictive model that indiate the importance of each of the feature that contribute to the used car pricing.

Feature Importance based on Random Forest and Permutation Importance for regression are performed. Here is the summary:
1. Odometer
2. Condition New
3. Condition Excellent
4. Size Full Size
5. Size Compact
6. Size Mid Size
7. Transmission Automatic
8. Cyclinder 8
9. Drive FWD
10. Cyclinder 6

For Feature Importance:
1. Odometer
2. Condition Good
3. Drive Fwd
4. Size Mid
5. Condition New
6. Drive 4WD
7. Cyclinder 6
8. Cyclinder 4
9. SUV TYpe
10. Size Full Size

As a summry on the importance above, I can conclude that the following 10 features are essential to what it drive the used car pricing or  :
1. Mileage (Odometer)
2. Condition
3. Size
4. Transmission
5. Engine Size
6. Drive (AWD over FWD)

# Conclusion

For Tradition Used Car (Newer than 2003)
Based on the model and feature importance, Used Car dealership owners should consider following factors when acquiring inventory flee for their stock:
1. Odometer
2. Condition (New/Excellent)
3. Size (Full Size/Compact/Mid Size)
4. Transmission (Automatic)
5. Cyclinder (6/8)
6. Drive FWD/4WD

For Collectible Car (Older than 2003), the dataset doesn't have enough data to do a seperate model for collectible / classic cars. 


# Supporting Data

# Error Rate

The best rate are as following:

Mean Absolute Error      :  6025.397484314429
Root Mean Squared  Error :  9980.506509296576
R Squared Error          :  0.5459114884600689

An R-Squared value shows how well the model predicts the outcome of the dependent variable. The R2 Score is 0.546. What this mean is that the model is able to predict 55% of the time.
As for Mean Absolute Error is the average

# Recsidul  Plot
Based on the graph, most of the result are randomly cluster around the zero line. What this mean that regression model is good. 
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
