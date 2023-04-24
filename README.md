# House-Price-Prediction
- This repository is dedicated to analyse the features that affect housing prices and predict them for 10 different counties: Wise, Denton, Collin, Parker, Tarrant, Dallas, Rockwall, Kaufman, Johnson, Ellis.
- Predicted price can last to at least one quarter to a year with small deviations.

### Data Cleaning and Processing
- Pull data from the Rapid-API Zillow.com to get the housing data that contains houses that are currently on sale.
  -   Disclamer: You need to buy the API Key to pull down the data. Paths in the Python files need to be changed to ensure that the codes are running.
- Clean the datasets to ensure that it contains only important features.
- End up with location and properties of houses data. (For more preferences: please look into the Overall_progress.pptx in Time Series Analysis Folder.)
- This is the features and target (price/lotsqft) for the location data.
![Location_data](https://user-images.githubusercontent.com/89664955/234097468-0d8e703a-e48a-40f7-9037-32e4bdf83e2f.JPG)
- This is the number of features for properties data with target living_price. (living_price = liv/lot_ratio * price if liv/lot_ratio <= 1)

![Properties_features](https://user-images.githubusercontent.com/89664955/234098142-b5ba6454-f207-4fad-8f2d-a95b06ee110e.JPG)

