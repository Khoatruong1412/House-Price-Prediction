# House-Price-Prediction
- This repository is dedicated to analyse the features that affect housing prices and predict them for 10 different counties: Wise, Denton, Collin, Parker, Tarrant, Dallas, Rockwall, Kaufman, Johnson, Ellis.
- Predicted price can last to at least one quarter to a year with small deviations.
- The predictions can give better estimate than an average home buyer.
- With a DL model, you can save less time on the picking the house you want to buy.

### Data Cleaning and Processing
- Pull data from the Rapid-API Zillow.com to get the housing data that contains houses that are currently on sale.
  -   Disclamer: You need to buy the API Key to pull down the data. Paths in the Python files need to be changed to ensure that the codes are running.
- Clean the datasets to ensure that it contains only important features. Put some restrictions on the datasets for better generalization.
- End up with location and properties of houses data. 
  -   (**For more preferences: please look into the Overall_progress.pptx in Time Series Analysis Folder.**)
- This is the features and target (price/lotsqft) for the location data.
![Location_data](https://user-images.githubusercontent.com/89664955/234097468-0d8e703a-e48a-40f7-9037-32e4bdf83e2f.JPG)
- This is the number of features for properties data with target living_price. (living_price = liv/lot_ratio * price if liv/lot_ratio <= 1).

![Properties_features](https://user-images.githubusercontent.com/89664955/234098142-b5ba6454-f207-4fad-8f2d-a95b06ee110e.JPG)
- After finished processing, we performed EDA on this data.

### Data Analysis
##### Unsupervised learning TSNE 2D
- Using TSNE 2D, a dimensionality reduction technique, to see any impacts of high-dimensional features on the housing prices.
![image](https://user-images.githubusercontent.com/89664955/234099183-9489bc52-f521-4a10-8889-51506d70f36a.png)
- There are some features (without location features) that are more important than the others can really distinguish housing prices.
- Expensive houses and cheap houses are distinct groups while moderately priced houses are in a mix. 

##### Map of liv/lot_ratio distribution and price/lot_sqft distribution
- liv/lot_ratio = living Area (sqft) / lot Area (sqft)
- The Map of liv/lot_ratio across the 10 counties. Counties with more cities such as Dallas and Tarrant have a bigger liv/lot_ratio than 'countryside' counties such as Parker and Wise.
![image](https://user-images.githubusercontent.com/89664955/234101606-d0093ed6-e879-4195-89c2-045e9ac240c4.png)
- Cities have more price/lot_sqft than countrysides => Location of these houses matters.
![image](https://user-images.githubusercontent.com/89664955/234103593-6c1a14a3-d03f-43b9-9027-7f5fa1399189.png)


##### Liv/lot_ratio vs. price/lotsqft and living_price
- Fit a regression line through the scatter plot. As Liv/lot_ratio increase, price/lotsqft also increases.
![image](https://user-images.githubusercontent.com/89664955/234101844-b76ab96a-f128-43fc-a61e-4c35a7ab3fe1.png)
<img src="https://user-images.githubusercontent.com/89664955/234101844-b76ab96a-f128-43fc-a61e-4c35a7ab3fe1.png" width = 500 height = 460>

- The correlation matrix between the 2 variables.
![image](https://user-images.githubusercontent.com/89664955/234101879-3267d462-6745-4600-92e1-ef0d296b5fdc.png)
<img src="https://user-images.githubusercontent.com/89664955/234101879-3267d462-6745-4600-92e1-ef0d296b5fdc.png" width = 500 height = 460>

- liv/lot_ratio and other numerical features also have effects on housing prices.
![image](https://user-images.githubusercontent.com/89664955/234104050-0f22c903-e489-4eae-ab28-fa834cf1c3f3.png)
![image](https://user-images.githubusercontent.com/89664955/234104064-50eeea34-9368-4aff-8fd8-39b36e9075d9.png)

### Supervised learning (Housing prices prediction)
- Cross 5 Folds Validation to evaluate the mode performance.
- Build 3 ANN models for housing price prediction:
  - ANN_location: the model is built to predict the price/lotsqft based on location.
  - ANN_properties: the model is built to predice the living_price based on the inner part (interior) of the house.
  - ANN_price: Use the other 2 outputs of these models and with other raw features to predict the housing price.
    - Raw features: LivingAreaValue, LotArea.
    - Derived features: Liv/lot_ratio, Predicted_orice/lotsqft, Predicted_living_price, predicted_price1 (ANN_location's output * LotArea), predicted_price2 (ANN_properties_output * (1/liv_lot_ratio)).
    - Target: Actual housing prices.
- Model performance (R-squared) comparison:
![image](https://user-images.githubusercontent.com/89664955/234106776-13590f04-0ded-4932-94e1-84f66904c703.png)
![image](https://user-images.githubusercontent.com/89664955/234106805-a7f3ae69-2d6c-4e63-b51b-34d7df10c430.png)
- The model generalizes well. I also looking into the distribution of errors when they are in the test dataset.
![image](https://user-images.githubusercontent.com/89664955/235806606-3ad629ad-c18f-4bf4-8c05-598ab304ad2d.png)
![image](https://user-images.githubusercontent.com/89664955/235806638-6b502de2-70c5-4603-ac72-e8dfd20ec3f3.png)

- We can see that the monthly mean errors are around $1636.33 to $14,686.50 and the standard deviation of the errors are around $77638.48 to $98124.65. This shows that even though most of the time the model made accurate results, they are also inconsistent and this could due to many extreme outliers existing in these monthly datasets.

![image](https://user-images.githubusercontent.com/89664955/234107385-985019bb-d6dc-443b-b968-ef2049d58dba.png)
- Houses that are in the range between $250,000 to $1,000,000, which are most of our samples, have predicted values that are closer to the actual prices while houses that are above $1,000,000 are usually underestimated. 
- I also plot the residuals of all the monthly predictions to check if there exists any biases in the datasets.
![image](https://user-images.githubusercontent.com/89664955/235808444-bb64cea3-869f-4269-a464-76a272ff9863.png)
- Even if the residual plots are plumbed together, this is due to some extreme outliers. For instances, houses that are only $300000 but the model predicts it to be a $2 million or even $4 million even though we capped our target to $2 million.

## Conclusions:
- Location of houses are important to the point that we need a separate model to predict the price correctly same for the inner part of the house.
- To check why ANN models performed better, please look for the powerpoints in Time_Series_Analysis/progress_presentation to understand why I pick ANN models for predicting all of these targets.

## Future Work:
- Rapid API has created a new command line that you can pull recently Sold homes' last posted prices. Need these data to improve the model performance.
- Incorporate Time Series Analysis to get better housing predictions. For instance, is there a relationship between CPI or fund with housing price in DFW.
- Features such as median household income based on zipcode area, hospitals, etc. can affect the housing prices.

# How to reproduce results:
- To get housing data from the beginning, you need to subscribe to Rapid API Zillow.com to get housing data. Moreover, redirect your data with different paths as you see fit. Website: https://rapidapi.com/apimaker/api/zillow-com1/.
- Using the 3 files to pull these data and have clean data for analysis: Data pulling from api.ipynb, Cleaning the data from API propertyExtendedSeach.ipynb, Cleaning the houses properties data_part_3.ipynb.
- You also need the government data from US Census Bureau. Website: https://www2.census.gov/geo/tiger/TIGER_RD18/LAYER/.
- EDA can be found in the Data Analysis folder.
- Supervised Learning and other traditional models' performance can be found in Time_Series_Analysis.
- File that build the 3 complete ANN models: Combination_DLs_final.ipynb.
**Disclaimer: Most of the training_data or raw data are not published in the github repository because they are larger than 100MB in total. But with the instructions above and comments in the notebook, you will be able to replicate with what I have!**

# Packages:
- Pandas, numpy, etc: Essentials packages for data cleaning and analysis
- Tensorflow and Sklearn: Deep learning (ANN) with tensorflow and traditional models such as Random Forest with Sklearn.



