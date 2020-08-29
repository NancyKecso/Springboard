![cover_photo](./figures/fig00.jpg)
# elementary OS Revenue Forecast

* Using Python-based machine learning models, this report details the examination of the 2016-2020 revenues and a prediction of future revenues.  *

## 1. Data

The data is availabe as CSV files that have been downloaded.  The files contain all of the transactions for two separate accounts that receive the revenues.  Contained within the files are fields that should be kept private (for example, email id of donor). The two files carry identical fields but unique data.


## 2. Data Cleaning 

The data came in very clean with minimal missing fields.  Some Sales records that did not find matches in the Characteristics file were dropped.


## 3. EDA

On the elementary download page, there are three preset amount buttons: $10, $20, and $30 as well as a "custom" button which allows a purchaser to input any amount (including $0).  A first look at the data was to see how the responses were distributed about these amounts.

When summed to a daily level, the revenue amounts stay relatively level except for some exceptionally high peaks.  Most of these peaks are closely tied to new version releases.

The peaks could be caused by higher than average purchases or an increased number of purchases, so both were examined:

While there were fluctuations in the mean daily amount, it appears that the actual number of purchases is what is driving the revenue peaks.

Finally, a look at how the different releases show over time:


## 4. Modeling

This is a time series with a continuous numeric target. In order to be able to predict the future from history, the time series must be stationary.  A KPSS (Kwiatkowski-Phillips-Schmidt-Shin) test can check for the null hypothesis that x is level or trend stationary. A KPSS test was run against the daily revenue total series and returned a value of 0.01. Since it was not greater than 0.05, the hypothesis that the series was stationary had to be rejected.  A second run against the log of the daily revenues, yielded .1. So, the target will be log(revenue) instead of revenue.
ARIMA Model
The data was run using an Autoregressive Integrated Moving Average (ARIMA) model. ARIMA combines autoregression (time sequence as a linear function of the time series at previous times) with moving average (time sequence as a linear function of the residual mean errors of the previous times). This integration of methods help to make the time series stationary.
Even after tuning of the hyperparameters, the model was not a good fit.  

The ARIMA model does not take seasonality into account, so the irregular peaks were difficult to fit.

FBProphet Model
A search for a model that would accommodate irregular seasonality, produced the Facebook Prophet model.  Prophet forecasts are based on an additive model that is fitted to yearly, weekly, and daily seasonality trends.  Most importantly for this case, it also allows the incorporation of holidays which are irregular. These can be used to mark the release dates and other events that boost the downloads.  Prophet works best with several seasons of historic data.

COVID-19 Effect
In addition to the irregular peaks, an examination of the daily revenues shows that there was a noticeable increase in the number of downloads in 2020.  This is probably caused by quarantining due to COVID-19.  Prophet automaticly calculates changepoints in trending but also allows for manual identification. Several different changepoints were tested 


![features_importance](./figures/fig03.png)

The 1/1/20 changepoint provided the best fit and was used for modeling and forecasting.
Testing Data
Since the model is working with annual seasonality, it is recommended that the test data include at least a full year of data. The changepoint had to be included in the training data, so that meant that the test data had to be taken from the start of the series instead of the end.  So, the first year's data was given to the test group and the final three years' went into training.

## 5. Predictions
Once the model was fitted and tested against the heldout test data, it could be used to predict future revenues. The model was run asking for an entire year's worth of forecast.

## 6. Conclusions

Future revenues are predicted to climb.  However, given the trending change caused by Covid, it is recommended that this model be rerun once things get back to a more normal basis.
elementary might want to experiment with different preselected amounts on the download page.  Since the majority of users have chosen the $10 preset button, perhaps raising this amount might encourage users to be more generous.
Not all of the revenue spikes have been identified.  Most of the peaks were correlated with new releases.  However, one very signficant boost occurred when elementary was featured very favorably in a Forbes article (December 2019).  Further study of these peaks might help the company to understand what is driving the increases and encourage future occurrences.
 sales. It does not include information about property that was put up for sale but never sold or information about how long the sale took.


Note:  data files have not been stored on github in order to protect user privacy