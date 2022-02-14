# Demand Forecasting for hierarchical time series data

#### Installation Instructions:
1. Clone the repository or download the zip file and go inside the directory using Command Prompt/Terminal.
2. Make a python3 virtual environment.
```python
python3 -m venv time-env
```
3. Activate the virtual environment.
```python
source time-env/bin/activate
```
4. Install the requirements.
```python
pip install -r requirements.txt
```
5. Run jupyter notebook and explore the notebooks.
```python
jupyter notebook
```
## Conclusion

I trained 4 classes of models during Experiment:

**GROUPED TIME SERIES APPROACH** `DID NOT WORK`
1. RMSE of Bottom Up Approach: 376.75748084933656
2. Top Down Approach - Not used due to wrong assumption
3. Middle Out Approach - Not used due to lack of heirarchy

This approach performed worst because it was based on the wrong assumption that we had to forecast monthly sum of Sourcing cost. After realising the wrong assumption, I tried to forecast monthly mean which gave this result. Also, the bottom up approach used for Grouped Time Series utilises autoarima to fit the models. After outlier removals, the data at bottom level(i.e. individual product level) became stationary and therefore ARIMA models were too complex for data and performed poorly.

**DEEP LEARNING** `NOTEBOOK NOT INCLUDED`

LSTM are used to model long range dependencies and hence are suitable for time series forecasting. 

**TREE BASED MODELS** `NOTEBOOK INCLUDED`
1. RMSE of DecisionTreeRegressor: 34.12919305031901
2. RMSE of RandomForestRegressor: 34.1144924600023
3. RMSE of ExtraTreesRegressor': 34.024829092084374
4. RMSE of AdaBoostRegressor: 38.25561832984171
5. RMSE of GradientBoostingRegressor: 33.99121520896884
6. RMSE of VotingRegressor : 31.334522814414328
7. RMSE of LGBMRegressor : 33.16664421051016
8. RMSE of XGBRegressor : 33.956663530227814

Tree Based models were chosen as they perform well will categorical variables. 
Outliers were removed using IQR and categorical variables were one-hot encoded.
For outliers removal, there were three choices. Not removing outliers gave the best results. Removing outliers at top product level(i.e. NTM1, NTM2) gave intermediate results and removing outliers at bottom unit level(i.e NTM1_A10_X1_DIRECT_Large_Powder) gave worst results.

I would have tuned the hyperparameters of the model but was unable to do so due to time constraints. This would increase the performance(i.e. decrease RMSE)


**SIMPLE TIME SERIES BASED MODELS** `NOTEBOOK INCLUDED`

Models Used
1. RMSE of Simple Moving Average: 37.291999530367285
2. RMSE of Exponential Weighted Average: 36.49481290429226
3. RMSE of Simple Exponential Smoothing: 37.283745121556244
4. RMSE of Additive Exponential Smoothing: 36.173433451689036
5. RMSE of Holt's Method: 36.173433451689036

Models Not Used due to Complexity or lack of Seasonality
1. ARIMA
2. SARIMAX
3. Holt Winter's method

Outliers were removed at the at bottom unit level(i.e NTM1_A10_X1_DIRECT_Large_Powder) using IQR. Then stationarity was tested using Augmented Dickey Fuller Test. After that these simple models were fitted. Trying to fit complex models such as ARIMA using autoarima in pmdarima library result in ARIMA(0,0,0) which means that the model is too complex as the data points were 11 months and even less for some.

**FINAL CHOICE**
Given all these models, Voting Regressor performed best. But, I would go for a simpler time series model such as Exponential Smoothing because it is easy to understand and diagnose any problems if it happens in future. Given more time, I would like to experiment using LSTMs for the data.

**NOTE**
1. IQR -> Inter Quartile Range
2. All notebooks have more detailed explanations inside them. Please look them through.
3. Code would have been better organised given more time.
