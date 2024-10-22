# supply-chain-forecasting

This repository contains the source code for the VN1 Supply Chain Forecasting competition. Overall, we utilized a combination of statistical and machine learning models to achieve our best possible results.

Our final submission is an ensemble of 4 different models: LightGBM, DynamicTheta and 2 variation of MFLES. The notebook used to train each respective models can be found in `notebooks/{model_name}.ipynb`. The file `notebook/ensemble.ipynb` should be ran after all the other notebooks have been ran, aggregating the output of all 4 of them.

We heavily utilized the nitxla library for our data processing and forecasting tasks.

## Training process and data use

The 3 statistical methods are univariate and we fitted each Client-Warehouse-Product combination to one model. 

For the LightGBM, we first dropped all data points before a first sale is made for each Client-Warehouse-Product combination. We experimented with multiple approachs, but in the end found that training one single global model using purely lagged `Sales` gave us the best result, what we tried but found to be unsuccessful include:
- Treating product `Price` as a historical feature, taking the lag values between 13 and 78 weeks.
- Training a separate model per Client.
- Forecast `Price` and use them as future exogeneous features.

The ensemble of 3 different architectures gave us the best result, achieving a 0.501 score on the Phase 1 data  
## Our takeaways and possible room for improvements.

- A combination of ML models statistical methods could yield very promising result, especially when only considering the endogeneous feature: `Sales`.
- Recursively forecasting the sales with LightGBM gave us worse result standalone compared to fitting a model for each horizon, but our ensemble result was better using a recursive approach.
- A change in price has a very significant impact on specific product sales, such as a decrease in price leading to an increase in sales, etc... However, we weren't able to utilize price as a feature in this competition, mainly due to working with a forecasting horizon of 13, and no future price available for those 13 weeks. Historic price were unreliable as features, and manually predicting the price created further variance in our prediction.
- We weren't able to experiment with deep learning methods, but we believe that it could also be quite promising, especially due to them being able to have multiple outputs, corresponding to each horizon.
