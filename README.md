# ARIMAX Gold and S&P500 Time Series:
*Gold has been coveted by man since [2600 B.C.](https://nationwidecoins.com/how-was-gold-discovered/). Since it's discovery, we have treated it as currency. Today, the [gold standard](https://www.investopedia.com/ask/answers/09/gold-standard.asp) may be abandoned, it remains highly liquid and can be easily converted into any currency. Because of this, many have used an investment into gold as a security against inflation. These investments into gold are often through investment portfolios and are compared to stock. [Many have theorized and claimed](https://www.sunshineprofits.com/gold-silver/dictionary/gold-sp/) that the value of gold is significantly dependent on the health of the stock market; especially the indexes such as the S&P 500 and DOW. In this case study I implement complex models and tests to really see just how significant the influence of the S&P 500 is on gold.*

For this capstone I have collected a total of five different csv files. They are:
1. "gold-price-last-ten-years.csv"
2. "SP500 2007-2020.csv"
3. "silver history.csv"
4. "Barrick Gold Corp 1985-2020.csv"

The purpose of this capstone is to conduct a small scale test on the belief that the value of GLD increases when the value of stock market indexes decreases (represented by the S&P500 index). To add some additional insight and food for thought, I have included several other sets of data: the value of silver and the stock value of Barrick Gold Corp (a gold mining company). The reasoning for including silver is to see if this trend is unique to gold, or if other precious metals increase in value as the stock market declines. The stock value of Barrick Gold Corp. is included in order to see if the stock value of gold mining companies also increases in value in correlation to the value of gold and the S&P500 index.

Hypothesis: The S&P 500 index has a statistically significant, exogenous effect on the value of gold(GLD).

Null Hypothesis: S&P 500 index does not have a statistically significant, exogenous effect on the value of gold.

After discovering any significant relationships between the indexes/stocks, I will write a time series in order to see how well we can predict one based off of another.

To begin, I will clean the data in order to properly merge and compare it in a single dataframe.
