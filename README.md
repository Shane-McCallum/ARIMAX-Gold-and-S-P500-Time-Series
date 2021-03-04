![cover image](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/buy-gold-key-wo.jpg)

# ARIMAX Gold and S&P 500 Index Time Series

*Gold has been coveted by man since [2600 B.C.](https://nationwidecoins.com/how-was-gold-discovered/). Since it's discovery, we have treated it as currency. Today, the [gold standard](https://www.investopedia.com/ask/answers/09/gold-standard.asp) may be abandoned, yet it remains highly liquid and can be easily converted into any currency. Because of this, many have invested in gold as a security against inflation. These investments into gold are often through investment portfolios and are compared to stock. [Many have theorized and claimed](https://www.sunshineprofits.com/gold-silver/dictionary/gold-sp/) that the value of gold is significantly dependent on the health of the stock market; especially the indexes such as the S&P 500 and DOW. In this case study I implement complex ARIMA and ARIMAX models to test just how significant the influence of the S&P 500 is on gold.*

## 1. Problem Identification

[Problem Statement PDF](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/Capstone%20Problem%20Statement%20%5BShane%20McCallum%5D.pptx.pdf)

Before I write a model, I need to know what the problem I am looking to solve is. In a project like this, I have to come up with my own problem and hypothesis. Having no experience in the investment sector, I decided to ask my own financial advisor for some insight on how portfolio strategies are developed and balanced. After an informative conversation, I settled for a simple problem to investigate and solve:

*A client is developing an investment portfolio for their customers and wants to test the theory that the price of gold per ounce (GLD) can be predicted from the S&P 500 Index (SPX). Barrick Gold Mining, Corp (BARR) and the price of silver/ounce (SLV) has been included to compare the uniqueness of the relationship between SPX and GLD.*

From here, I developed the hypothesis for the project:

**Hypothesis:** The S&P 500 index (SPX) has a statistically significant, exogenous effect on the value of gold (GLD).

**Null Hypothesis:** The S&P 500 index does not have a statistically significant, exogenous effect on the value of gold.

## 2. Data Wrangling 

For this project I have collected a total of four different csv files. They are:
1. ["gold-price-last-ten-years.csv" as GLD(USD)](https://www.macrotrends.net/2627/gold-price-last-ten-years)
2. ["SP500 2007-2020.csv" as SPX(USD)](https://www.marketwatch.com/investing/index/spx)
3. ["silver history.csv" as SLV(USD)](https://www.macrotrends.net/1470/historical-silver-prices-100-year-chart)
4. ["Barrick Gold Corp 1985-2020.csv" as BARR(USD)](https://finance.yahoo.com/quote/GOLD/history?period1=476323200&period2=1614211200&interval=1d&filter=history&frequency=1d&includeAdjustedClose=true)

Again, the SLV and BARR datasets are not intended for creating a more accurate model, but instead to check if the relationship between GLD and SPX is unique!

In order to accurately train and test a model on this data, it needs to be properly cleaned.

*   **Step 1:** First, I had to decide on a time period for the time series! I was limited by the time frames of the individual CSV files as well as trends in the data. Stock and index values are heavily dependent on trends in the market. Trends in the stock market are periods of time [lasting 18 weeks or more](https://www.investors.com/how-to-invest/investors-corner/sell-rules-growth-stocks-break-uptrend-line/#:~:text=A%20properly%20drawn%20trend%20line,of%20at%20least%2018%20weeks). I chose August 21, 2017 as a start date for the data, as it will have just enough data in the test split (20% of the data) to include a trend within it.
*   **Step 2:** Next, I had to merge all of the CSV's together onto a common index. I am only concerned about predicting the closing price of GLD, so I only kept the closing value and date columns in each of the other data frames. Then I set the date column as the index for each of them. After choosing the time frame for the data frames, I used pd.merge() to merge all of them onto a common index.
*    **Step 3:** Finally, I had to go through and check for null values in the data. Since the stock market is closed on weekends and holidays, I had to figure out what I was going to do to fill the days where no closing price was recorded. I found that historically, other models have often foward filled the Nans for stock data. However, I wanted to be sure this didn't heavily affect the data. So I compared the standard deviations, means, min/max, and quartiles before and after using ffill on the data frame. The greatest change was that the standard deviation for SPX increased by about 68 cents.

## 3. Exploratory Data Analysis (EDA)

Now, for the fun part! My first priority was to visually explore the data to see if there was any support for a statistically significant relationship between SPX and GLD. If there isn't one, then there is no point in trying to develop a predictive model for GLD from SPX. However, time series are tricky; especially stocks, indexes, and values. What is easily derived on the surface is not always reality.

1. I wanted to look at the timeline of the data and see if there was any apparent trends.

![graphs](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/Graphs.png)

While GLD is not immune to market crashes or dips, it does seem to trend opposite in growth from SPX. This can be seen in BARR vs. SPX as well, since BARR is a gold mining company this is not all that surprising.

2. Seaborn's jointplot is one of my favorite methods for exploring the relationship between two features.

![jointplot](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/jp1.png)
![jointplot](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/jp2.png)
![jointplot](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/jp3.png)

Here, it's definitely apparent that there exists a positive, linear trend between GLD and SPX.

3. Next up was to get some concrete numbers in regards to correlation between each of the features. Seaborn's heatmap is a wonderful way to visualize this.

![heatmap](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/heatmap.png)

Right off the bat, it's clear that BARR and SLV are highly correlated with GLD. GLD and SPX, however, don't seem to have a statistically significant correlation.

4. Fret not though, for there exists some measurements of cointegration between time series. First, I tested their levels of cointegration with the Cointegrated Augmented Dickey Fuller ([CADF](https://pythonforfinance.net/2016/05/09/python-backtesting-mean-reversion-part-2/)) test. This test attempts to find a stationary, linear combination from time series that are not themselves stationary.

![CADF graph](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/CADF.png)
![cadf numbers](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/cadf%20numbers.png)

Well, the graph certainly does not look mean-reverting (stationary). Upon checking the ADF statistic for the graph, the value is -1.44. According to Statsmodels ADF function, a statistically significant ADF score would be less than or equal to -2.865. So again, there doesn't appear to be a significant level of cointegration between GLD and SPX. However, the CADF test is also sensitive to which columns we assign to X and y. The Johansen Cointegration test, though, is not sensitive to which features are assigned to X or Y, and can test for cointegration among 12 time series. [Here is the reference for a pre-written Johansen test](https://blog.quantinsti.com/johansen-test-cointegration-building-stationary-portfolio/). The Johansen test has been ruled as being a better test for cointegration; so I tried that as well.

![Johansen test](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/johanssen.png)

Well, the Eigenvectors statistic is 7.61, about half of a statistically significant score of 14.26. GLD and SPX certainly don't have a Johansen cointegration score of anything significant. 

However, I could try one more method: [Granger Causality](https://towardsdatascience.com/granger-causality-and-vector-auto-regressive-model-for-time-series-forecasting-3226a64889a6). The Granger Causality test, in short, simply tests time series to see if they are useful in forecasting other time series. However, using them for anything outside of economics, as warned by Clive Granger himself, is "ridiculous."

![Granger Causality](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/granger.png)

To understand what we have here:

Row 1, column 2 refers to the p-value of the Granger's Causality test for SPX(USD)_x causing GLD(USD)_y. What we see is a p-value of 0.0388. That is a significant p-value, as it is under the level of significance (0.05)! So, according to the Granger Causality, we could reject the null hypothesis and say with 95% confidence that SPX has a significantly causal relationship with GLD.

Additionally, it can be seen from this table that SPX has a strong causal relationship to BARR, which is not surprising as BARR is a stock value. However, SPX does not have a strong causal relationship to SLV, suggesting that the causal relationship SPX has with GLD is unique and not universal for other precious metals. 

## 4. Modeling with ARIMA and ARIMAX

To  begin, I wanted to get a baseline of how well I could use the history of GLD to  predict its future. So, to start off I split the data into a train-test split of  80%/20%. I then used a cross validation model to get the best p, d, and q values for an ARIMA model based off of the root of the mean squared error (RMSE).

![ARIMA RMSE](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/ARIMA%20cv.png)

In order to lower the time and complexity of the model and prevent overfitting, I settled on p, d, q  equal to 2, 1, 1. I then ran a forecasting model along the test data and got an RMSE of 22.837 on the test data. This is still low in comparison to the magnitude of the values for GLD and SPX. To understand this better I calculated the mean absolute percentage error (MAPE) to get the percentage in accuracy of predicting the value of GLD compared to the test data. The MAPE percentage was 8.374%, meaning the model was about 92% accurate in forecasting the values of GLD.

![ARIMA forecasting graph](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/ARIMA%20graph.png)

Now, to compare that to an ARIMAX model where SPX is the exogenous variable to GLD. Again, the cross-validation model showed that p, d, and q values of 2, 1, 1 were optimal in complexity and RMSE.

![ARIMAX CV](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/ARIMAX%20cv.png)

Testing the ARIMAX model resulted in an RMSE of 22.947 and a MAPE of 8.369%. Only 0.005% better than the ARIMA model.

![ARIMAX graph](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/ARIMAX%20graph.png)

## 5. Conclusion and Future Tests

The true value in a model like this stems from it's potential usefulness in the finance sector. Even with the COVID-19 market crash in March, 2020, the model was still accurate in it's predictions. A lot of trading and investment is determined with forecasting, such as "put" and "call" options. Being able to forecast the closing value of a stock tomorrow allows for investors to make more informed descisions about what to buy or sell today.

Overall, the difference between the ARIMA and ARIMAX models are not significant. So, while the ARIMAX model technically outperformed the ARIMA model, and the use of S&P 500 index for predicting the value of Gold could be argued, it is not necessarily any better than predicting the value of Gold without it. Reasons for this have been theorized, such as gold being used to offset decline in the stock market. However, when the USD was backed by gold, the causal relationship between the stock market and gold might have existed. Since the 1970's, though, when the US adopted a fiat currency system, this relationship has changed.

Perhaps a better test would be to see if the value of gold could be accurately predicted from the Barrick Gold Mining, Corp. stock. This could be significant as Barrick’s stock was shown to be caused by the value of the S&P 500 index as well.
