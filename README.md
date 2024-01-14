# Feature Engineering for a time series on daily traded Volume
The aim of this project is to create a complete database starting from a Time Series of a security's daily traded Volume. 

The final Database can be used to forecast an approximation of the Volume that will be traded in the near future for the chosen financial security. 

The starting point for this project is a time series composed by:

Day     |    Volume   |   Security

01/01/23|   100,000   |   Equity1

02/01/23|   110,000   |   Equity1

03/01/23|   130,000   |   Equity1

The aim of feature engineering on time series is to create some relevant exogenous variables that will provide more information to the model, but that will still maintain the "time order" logic of the time series.
For this reason, I will create features based on the Volume, or on other time-dependent variables that can impact the change in volume over the time series.

In the specific, I will create 8 Variables based on our financial domain knowledge, and then I will use python's TSFresh package to generate other variables starting strictly related to the Volume's changes over time.

To recap some of the main features:
1. Volume 1 day Lag: To accurately forecast the next day's volume I must take into consideration the previous day Volume
2. Rolling Weighted Volume Average: A nice way to capture a Volume trend is to use a weighted moving average. By doing so I take into account the most recent trend of the volume and I give more importance to the closest days as they are strongest indicators of the current trend. In the code, I consider a moving average of 2 weeks, where the weights decay following an exponential distribution.
3. Turning points: The Time series of financial instruments are usually volatile and are characterized by sudden changes in trends. For this reason, I use a change_point feature to identify the days that played the role of pivot in switching from a growing trend, to a decreasing trend.
4. As time series forecasting benefits from having columns displaying an ordered sequence of values, I created as well a trendgroup feature. This feature is used to identify all the upward or downward trends of more than 2 days that happened in the past and to assign an increasing count for all the days belonging to that specific trend. The result is that for each trend identified, I will know which day was the first day of the trend, which one was the seconds, ... , up to the last day of that trend.
5. Expanding AVerage Volume: This is a complete moving average of the volume that gradually includes the values of all the previous days considered
6. Closeprice: The traded volume may depend on the price's features as well, so I include the security's closing price of the day
7. RSI: Another useful price feature to define the security trend. It is usually used as a momentum oscillator that measures the speed and change of price movements. A high RSI can be used to understand if a security is overbought, while a really low one is an indicator of oversold.
8. MA difference: A useful way To understand if we are in a bullish or bearish scenario is to use the 20 days moving average and compute how much the price is above/below that value.

TSFresh:
This is an open-source Python package that automatically calculates hundreds of time series features from sequential data such as time-series data. Tsfresh also includes methods to calculate the feature importance and assists in feature selection.  

https://tsfresh.readthedocs.io/en/latest/text/list_of_features.html

Out of all the features created with this package, I used statistical tests to determine the 20 most impactful ones, and then I appended the 8 features explained above. 

The final database contained 28 features + the Volume

Thanks :)



