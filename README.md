![cover image](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/buy-gold-key-wo.jpg)

# ARIMAX Gold and S&P 500 Index Time Series

*Gold has been coveted by man since [2600 B.C.](https://nationwidecoins.com/how-was-gold-discovered/). Since it's discovery, we have treated it as currency. Today, the [gold standard](https://www.investopedia.com/ask/answers/09/gold-standard.asp) may be abandoned, it remains highly liquid and can be easily converted into any currency. Because of this, many have used an investment into gold as a security against inflation. These investments into gold are often through investment portfolios and are compared to stock. [Many have theorized and claimed](https://www.sunshineprofits.com/gold-silver/dictionary/gold-sp/) that the value of gold is significantly dependent on the health of the stock market; especially the indexes such as the S&P 500 and DOW. In this case study I implement complex ARIMA and ARIMAX models to test just how significant the influence of the S&P 500 is on gold.*

## 1. Problem Identification

[Problem Statement PDF](https://github.com/Shane-McCallum/ARIMAX-Gold-and-S-P500-Time-Series/blob/master/2.%20README%20files/Capstone%20Problem%20Statement%20%5BShane%20McCallum%5D.pptx.pdf)

Before I write a model, I need to know what the problem I am looking to solve is. In a capstone project like this, I have to come up with  my own problem and hypothesis. Having no experience in the investment sector, I decided to ask my own financial advisor for some insight on how portfolio strategies are developed and balanced. After an informative conversation, I settled for a simple problem to investigate and solve:

*A client is developing an investment portfolio for their customers and wants to test the theory that the price of gold per ounce (GLD) can be predicted from the S&P 500 Index (SPX). Barrick Gold Mining, Corp (BARR) and the price of silver/ounce (SLV) has been included to compare the uniqueness of the relationship between SPX and GLD.*

From here, I developed the hypothesis for the project:

*Hypothesis: The S&P 500 index has a statistically significant, exogenous effect on the value of gold(GLD).*

*Null Hypothesis: S&P 500 index does not have a statistically significant, exogenous effect on the value of gold.*

For this capstone I have collected a total of four different csv files. They are:
1. "gold-price-last-ten-years.csv" as GLD(USD)
2. "SP500 2007-2020.csv" as SPX(USD)
3. "silver history.csv" as SLV(USD)
4. "Barrick Gold Corp 1985-2020.csv" as BARR(USD)
