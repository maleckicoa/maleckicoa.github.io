---
layout: posts_wide
date:   2024-04-15
title:  "Superior ETF performance comparison"
permalink:  /compare_etf_performance/
categories: 
- Data
tags:
- Data
---
<br><br>
If you are an ETF investor like myself, you've probably searched for the performance of your favorite ETF before making a purchase. It's common to base investment decisions on the published annual rate of return—for example, QQQ at 9.44% per annum. Hopefully, you also considered the fund's risk, often expressed in terms of beta or volatility."

But what do these number tell us? In this post we will focus on the rate of return.

Annualized rate of return is a geometric mean of annual returns, where *r* is the ETFs increase in value within a particular year *n*.

<img src= "/assets/images/2024-04-15-ETF-compare/annual_return.png"  width="300"/>

Annualized rate of return can also be expressed as the n-th root of the ETFs value increase accross the whole holding period. 

<img src= "/assets/images/2024-04-15-ETF-compare/annual_return_2.png" width="200"/>

<br>
This rate of return is based on the assumption that an ETF is bought on a specific date (usually January 1st) and sold on the exact same date several years later. This scenario is quite unrealistic, given that an investor can buy or sell assets on any trading day within the year. With this in mind, we will demonstrate how the specific dates of purchase and sale can significantly influence the ETF's return rate. Let's examine the performance of a very popular ETF - QQQ. The blue line represents a five-year holding period from January 1, 2018, to January 1, 2023, while the red line shows a five-year period from June 1, 2018, to June 1, 2023."

<img src= "/assets/images/2024-04-15-ETF-compare/qqq.png" width="300" class="center_70"/>

The blue and red time frames each span five years, overlapping for four and a half years. Despite this, the annualized rates of return, calculated using the second formula above, are 12.4% and 17.3% . This is a huge difference. Essentially, if you are considering purchasing this ETF based on its annual return over the last five years, your decision could be heavily influenced by the most recent six-month period.

In practice, an investor is likely to buy or sell her ETF on any random day of the year, and the holding period may not align perfectly with a full year. Additionally, a typical investor often has limited knowledge about how long they will hold onto their investment—it could range from months to decades, and this is not usually known in advance.

Considering all of the points discussed, I have attempted to develop a more realistic method for assessing ETF performance. This approach ensures that:

The start and end dates of the analysis period are weighted equally with any other data points.
The investor has no preference for either short-term or long-term holding periods.

To implement this, I am collecting a series of returns between data points that are 12 months apart. These returns are calculated as a simple fraction, dividing the ETF price at time *t+12* by the ETF price at time *t*. The median of these returns represents the median one-year return for our ETF. This procedure is similarly applied to other holding periods, using intervals of 24 months, 60 months, and so forth.

Returning to our QQQ example, using this method, the blue and red time frames yield median annual returns of 18.4% and 18.7%, respectively. These returns are much more stable (compared to the previous 12.4% and 17.3%), and less dependent on the specific time frame chosen. This approach provides a clearer and more reliable comparison of the ETF's performance against other competing ETFs.

One important aspect to remember is that the one-year return rate is not meant to be compounded.  For instance, the five-year return should not be calculated as (1,184%)<sup>5</sup>-1. Rather, to determine the five-year (median) return, we would apply the same procedure using a 60-month interval.

fter defining the implementation, the next step is to apply this procedure across a broad range of ETFs and various holding periods. The goal is to conduct a thorough comparison and obtain a clear indication of which ETFs have shown the best historical performance. To achieve this, I processed a pool of approximately 21,000 ETF tickers available on the [internet](https://github.com/MichaelDz6/Yahoo_Finance_ETFs_Web_Scraper/blob/master/ETFs_Tickers.csv), from which around 5,500 were still active and had adequate and current data on Yahoo Finance.

Below is a series of ordered boxplots that summarize the performance of ETFs across holding periods of 1, 2, 5, 7, and 10 years. The plots on the left display the highest performance in terms of median return. The legend includes a description for each ETF, as available from the Yahoo Finance API.

<img src= "/assets/images/2024-04-15-ETF-compare/boxplots.png" >

Unsurprisingly, many of the top-performing ETFs belong to the technology and semiconductor sectors. Several ETFs show consistent high median returns across various holding periods. The table at the bottom provides a comprehensive summary of the boxplots — the higher the median return an ETF achieves within a holding period, the more points it is allocated. For instance, if there are 15 ETFs/boxplots within a holding period, the ETF with the highest median return would receive 15 points. The best-performing ETFs overall are those with the highest total points across all holding periods. However, caution is needed as ETFs exhibit different return distributions, evident in the varying shapes of their boxplots.

Based on the information available, it is clear that SOXX and SMH consistently yield the highest median returns overall. TNOW and TNOG are close contenders but lack sufficient data for the 10-year holding period. These two ETFs are relatively new, having been established around 2010, and thus do not yet possess the minimum data history of 15 years required for a comprehensive long-term analysis. Further details can be found in the Notes section.

ith this, we have come to the end of the post. I've made an effort to keep it brief and to the point, and I hope you have found it helpful. Please note that the topic of risk, specifically ETF price volatility, was not covered here, yet it is crucial for providing investment advice. The main objective of this post was to highlight the limitations of using traditional rates of return as indicators of ETF performance and to propose an alternative measure.


Notes:

Notes about the implementation:
- Data points are collected monthly, based on ETF prices at the beginning of each month. While daily data collection could improve precision, it would also require significantly more resources.
- To facilitate accurate comparisons between ETFs, the timeframe for calculating median returns is set as the holding period plus an additional five years. For example, a 10-year holding period would involve data stretching back 15 years from the most recent month, while a 1-year holding period would use data from the past six years.
- An ETF must have complete data for the past six months to be considered for analysis.
- An ETF with a data gap larger than five months is excluded.
- Smaller data gaps of less than five months are linearly interpolated to fill in missing values.
- An ETF must have at least 20 data points to be considered in the analysis.
- By default, leveraged ETFs are not included; however, users can choose to include them if desired.


Notes about the code:
- The project and instructions how to run can be found [here](https://github.com/maleckicoa/ETF_compare)
- The code runs on top of the [yfinance](https://pypi.org/project/yfinance/) library.
- The ETF_Ticker.csv file is included with the code and serves as the initial data source for the implementation. Users can add new tickers to the ETF_Ticker.csv to expand the dataset.
- Users can include additional ETFs for comparison against the best performers, provided that the ticker symbols are listed in the ETF_Ticker.csv file.
- Some tickers display inconsistent data, such as unexplainable jumps in values, and have thus been excluded from consideration.
- The Yahoo Finance API might occasionally return corrupted data for a given Ticker, it is important to perform a sanity check 





