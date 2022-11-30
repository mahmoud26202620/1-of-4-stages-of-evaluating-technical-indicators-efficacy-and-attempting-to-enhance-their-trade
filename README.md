# 1 of 4 stages of evaluating technical indicators efficacy and attempting to enhance their trade


The effectiveness of some MACD, RSI, and stochastic oscillator trading strategies is evaluated using historical data from the stock, bond, currency, and commodities markets.

# Introduction

A wide range of factors obviously influences the price of the securities. the state of the global economy, the industries it touches, the board of directors' capacity to steer the businesses they invested in, and business outcomes like sales and earnings as well as
On the other hand, a significant portion of investors rely on past data and prices of the securities and some previously identified patterns that help them predict future prices of the assets.

They refer to this process as "technical analysis," which is a methodology for analyzing and forecasting the direction of prices through the study of past market data, primarily price and volume.
Chart patterns and statistical indicators are the two main categories under which technical analysis. Chart patterns such as support, resistance, and trendlines and they are subjective and can be identified by looking at the chart at the specific patterns.
Technical indicators are statistical tools that technicians use to analyze pricing and volume using a variety of mathematical calculations. Moving averages, the MACD (moving average convergence divergence), the RSI, and the stochastic oscillator are among the most commonly used technical indicators.

Our goal in this phase is to track the technical indicators by assuming that traders act on their buy or sell signals over time and compare the results to one another as well as the price of the security itself, We assume that our beginning investment amount will be the same as the price of the security itself in the first buy signal, Following that, we will reinvest the exiting result in the following buy signal as a cumulative investment during the course of our study.

There will be several performance metrics used to assess each indicator technique. Later, we'll go into more detail about the seven key metrics of the evaluation report.
In order to do that, we will be using historical data for the Dow Jones, Gold, USD Dollar Index, EUR/USD Rate, and US Bond 10 Year Yield.

# Gather and Prepare Data

I'll begin by loading some of the packages I'll need for the analysis.

```
library(tidyverse)
library(lubridate)
```

**Reading the data**

I'll read the data from the Stooq.com website.

```
##Dow Jones Index
##Beginning in 1970, the data spans until November 20, 2022.
##Dow Jones daily
Dow_Jones_daily<-read.csv("https://stooq.com/q/d/l/?s=^dji&d1=19700101&d2=20221120&i=d")
##the structure of the data
str(Dow_Jones_daily)
```

```
'data.frame':	13342 obs. of  6 variables:
 $ Date  : chr  "1970-01-02" "1970-01-05" "1970-01-06" "1970-01-07" ...
 $ Open  : num  800 809 811 804 802 ...
 $ High  : num  814 819 814 808 809 ...
 $ Low   : num  797 805 799 796 797 ...
 $ Close : num  809 811 804 802 802 ...
 $ Volume: num  907895 1295865 1292481 1128948 1203384 ...
 ```
 ```
##convert character date variable to dates
Dow_Jones_daily$Date<-as.Date(Dow_Jones_daily$Date)
##the first part of a data object 
head(Dow_Jones_daily)
```

```
        Date  Open  High   Low Close  Volume
1 1970-01-02 800.4 813.6 797.3 809.2  907895
2 1970-01-05 809.2 819.2 804.8 811.3 1295865
3 1970-01-06 811.3 814.1 798.7 803.7 1292481
4 1970-01-07 803.7 808.5 796.2 801.8 1128948
5 1970-01-08 801.8 809.3 796.9 802.1 1203384
6 1970-01-09 802.1 805.7 794.4 798.1 1057895
```

**Checking for NAs**

```
colSums(is.na(Dow_Jones_daily))
```

```
  Date   Open   High    Low  Close Volume 
     0      0      0      0      0      0 
```

**reading the remainder of the data**

```
##Dow Jones weekly
Dow_Jones_weekly<-read.csv("https://stooq.com/q/d/l/?s=^dji&d1=19700101&d2=20221120&i=w")
##convert character data to dates
Dow_Jones_weekly$Date<-as.Date(Dow_Jones_weekly$Date)

##Dow Jones monthly
Dow_Jones_monthly<-read.csv("https://stooq.com/q/d/l/?s=^dji&d1=19700101&d2=20221220&i=m")
##convert character data to dates
Dow_Jones_monthly$Date<-as.Date(Dow_Jones_monthly$Date)

##The EUR/USD Rate
Beginning in 1971, the data spans until November 20, 2022.

##Eur/Usd_daily
EurUsd_daily<-read.csv("https://stooq.com/q/d/l/?s=eurusd&d1=19700101&d2=20221120&i=d")
##convert character data to dates
EurUsd_daily$Date<-as.Date(EurUsd_daily$Date)

##Eur/Usd weekly
EurUsd_weekly<-read.csv("https://stooq.com/q/d/l/?s=eurusd&d1=19700101&d2=20221120&i=w")
##convert character data to dates
EurUsd_weekly$Date<-as.Date(EurUsd_weekly$Date)

##EurUsd monthly
EurUsd_monthly<-read.csv("https://stooq.com/q/d/l/?s=eurusd&d1=19700101&d2=20221220&i=m")
##convert character data to dates
EurUsd_monthly$Date<-as.Date(EurUsd_monthly$Date)

##Gold (ozt) / U.S. Dollar 
Beginning in 1970, the data spans until November 20, 2022.

##Gold daily
Gold_daily<-read.csv("https://stooq.com/q/d/l/?s=xauusd&d1=19700101&d2=20221120&i=d")
##convert character data to dates
Gold_daily$Date<- as.Date(Gold_daily$Date)

##Gold weekly
Gold_weekly<-read.csv("https://stooq.com/q/d/l/?s=xauusd&d1=19700101&d2=20221120&i=w")
##convert character data to dates
Gold_weekly$Date<- as.Date(Gold_weekly$Date)

##Gold monthly
Gold_monthly<-read.csv("https://stooq.com/q/d/l/?s=xauusd&d1=19700101&d2=20221220&i=m")
##convert character data to dates
Gold_monthly$Date<- as.Date(Gold_monthly$Date)

##10-Year U.S. Bond Yield 
Beginning in 1970, the data spans until November 20, 2022.

##10-Year U.S. Bond daily
US_10YB_daily<-read.csv("https://stooq.com/q/d/l/?s=10usy.b&d1=19700101&d2=20221120&i=d")
##convert character data to dates
US_10YB_daily$Date<- as.Date(US_10YB_daily$Date)

##US_10YB weekly
US_10YB_weekly<-read.csv("https://stooq.com/q/d/l/?s=10usy.b&d1=19700101&d2=20221120&i=w")
##convert character data to dates
US_10YB_weekly$Date<- as.Date(US_10YB_weekly$Date)

##US_10YB monthly
US_10YB_monthly<-read.csv("https://stooq.com/q/d/l/?s=10usy.b&d1=19700101&d2=20221220&i=m")
##convert character data to dates
US_10YB_monthly$Date<- as.Date(US_10YB_monthly$Date)
```
US Dollar/USDX - Index

I'll read the USD Dollar Index from finance.yahoo.com

```
##US Dollar/USDX - Index - Cash (DX-Y.NYB)
Beginning in 1970, the data spans until November 20, 2022.

##USDX daily
USDX_daily<-read.csv("http://bit.ly/3OoN0Sy")

##the first part of a data object 
head(USDX_daily)
```

```
        Date       Open       High        Low      Close  Adj.Close Volume
1 1971-01-05 120.519997 120.519997 120.519997 120.519997 120.519997      0
2 1971-01-06 120.489998 120.489998 120.489998 120.489998 120.489998      0
3 1971-01-07 120.550003 120.550003 120.550003 120.550003 120.550003      0
4 1971-01-08 120.529999 120.529999 120.529999 120.529999 120.529999      0
5 1971-01-10       null       null       null       null       null   null
6 1971-01-11 120.529999 120.529999 120.529999 120.529999 120.529999      0
```

the data include weekend dates on which there are no trades

```
##deleting the weekend rows
USDX_daily<-USDX_daily[!USDX_daily$Close=="null",]
```

```
##the structure of the data
str(USDX_daily)
```

```
'data.frame':	13187 obs. of  7 variables:
 $ Date     : chr  "1971-01-05" "1971-01-06" "1971-01-07" "1971-01-08" ...
 $ Open     : chr  "120.519997" "120.489998" "120.550003" "120.529999" ...
 $ High     : chr  "120.519997" "120.489998" "120.550003" "120.529999" ...
 $ Low      : chr  "120.519997" "120.489998" "120.550003" "120.529999" ...
 $ Close    : chr  "120.519997" "120.489998" "120.550003" "120.529999" ...
 $ Adj.Close: chr  "120.519997" "120.489998" "120.550003" "120.529999" ...
 $ Volume   : chr  "0" "0" "0" "0" ...
 ```
 
 ```
##convert character data to dates
USDX_daily$Date<- as.Date(USDX_daily$Date)

##convert the open, close, High, and Low into numeric
USDX_daily$Open<-as.numeric(USDX_daily$Open)
USDX_daily$Close<-as.numeric(USDX_daily$Close)
USDX_daily$High<-as.numeric(USDX_daily$High)
USDX_daily$Low<-as.numeric(USDX_daily$Low)
```

```
##USDX weekly
USDX_weekly<-read.csv("http://bit.ly/3giHJiy")
##convert character data to dates
USDX_weekly$Date<- as.Date(USDX_weekly$Date)
##convert the open, close, High, and Low into numeric
USDX_weekly$Open<-as.numeric(USDX_weekly$Open)
USDX_weekly$Close<-as.numeric(USDX_weekly$Close)
USDX_weekly$High<-as.numeric(USDX_weekly$High)
USDX_weekly$Low<-as.numeric(USDX_weekly$Low)

##USDX monthly
USDX_monthly<-read.csv("http://bit.ly/3Xeib6V")
##convert character data to dates
USDX_monthly$Date<- as.Date(USDX_monthly$Date)
##convert the open, close, High, and Low into numeric
USDX_monthly$Open<-as.numeric(USDX_monthly$Open)
USDX_monthly$Close<-as.numeric(USDX_monthly$Close)
USDX_monthly$High<-as.numeric(USDX_monthly$High)
USDX_monthly$Low<-as.numeric(USDX_monthly$Low)
```

# Analyze phase

According to Wikipedia, in technical analysis in finance, a "technical indicator" is a mathematical calculation based on historic price, volume, or (in the case of futures contracts) open interest information that aims to forecast financial market direction. Technical indicators are a fundamental part of technical analysis and are typically plotted as a chart pattern to try to predict the market trend. Indicators generally overlay on price chart data to indicate where the price is going, or whether the price is in an "overbought" or "oversold" condition.

The plan is to calculate the investment price if we followed each indicator over the years(Only long trades will be flown; the analysis of short trades will follow.). Every indicator, of course, has a variety of settings and strategies, but in this phase we will determine the most common settings and the most well-known strategies. Later, we'll try to come up with a range of solutions for each indication and see if we can modify them for the greatest outcomes.

# Elements of a Strategy Performance Report

Seven performance metrics will be our main emphasis. While all performance metrics are significant, it is advantageous to start by focusing on these seven fundamental and crucial metrics:

1-Total Net Profit

2-Profit Factor

3-%Profitable(Winning Trades)

4-%Average Revenue

5-Avg. Win: Avg Loss

6-%Time in the Market

7-%M D D(intraday peak to valley)

and in our performance report, we will include another metrics that is related to them.

**Total Net Profit**

The gross profit of all winning trades is subtracted from the gross loss of all losing trades to determine the The total net profit.

$$Total Net Profit=The Gross Profit-The Gross Loss$$

Of course, total net profit profit is the primary goal of any trading strategy. 

**Profit Factor**

the profit factor is calculated by dividing the gross profit by the gross loss

$$Profit Factor=\frac{The Gross Profit}{The Gross Loss}$$

According to the equation above, the profit factor will grow whenever the gross profit increases and will decrease whenever the gross loss increases. A profit factor greater than one will identify The profitable strategy.

**Percent Profitable**

the percent profitable is determined by dividing the number of winning trades by the total number of trades

$$Percent Profitable=\frac{nWinning Trades}{Total Trades Number}$$

The percent profitable result alone did not reveal much about the strategy's profitability; a very low percent profitable strategy can still be successful because it can make a large profit from a small number of winning trades, and the total number of losing trades are with a small loss.

**Percent Average Revenue**

The percent average revenue is calculated by dividing the sum of all percentage of trading revenue (winning and losing) by the number of all trading

$$Percent Average Revenue=\frac{SUM(Percent Revenue)}{Total Trades Number}$$

The percent average revenue is the arithmetic mean of all trades' revenue, and it also refers to the probability of revenue that will be gotten from each trade, and it has the same problem as the arithmetic mean, which can be affected by a single large amount of revenue (winning or losing).

**Avg. Win: Avg Loss**

the Avg. Win: Avg Loss is the ratio between the average points in winning trades and the average points in losing trades

**Percent Time in the Market**

the percentage of time we hold our position, including the one you sell your position in, to the total time of the study (in daily trading, we were going to consider only the session days in our ratio, canceling out the effect of Friday and Saturday).

At this point, we are prepared to begin our analysis with the first indicator.

**Maximum Drawdown (MDD)**

According to the "robeco" site Maximum drawdown is defined as the peak-to-trough decline of an investment during a specific period. It is usually quoted as a percentage of the peak value. The maximum drawdown can be calculated based on absolute returns, in order to identify strategies that suffer less during market downturns, such as low-volatility strategies. However, the maximum drawdown can also be calculated based on returns relative to a benchmark index, for identifying strategies that show steady outperformance over time.

The Formula for Maximum Drawdown Is

$$MMD=\frac{Trough Value−Peak Value}{Peak Value}$$

# Moving average convergence/divergence (MACD)

According to Wikipedia, MACD is a trading indicator used in technical analysis of stock prices that was created by Gerald Appel in the late 1970s. It is designed to reveal changes in the strength, direction, momentum, and duration of a trend in a stock's price.The MACD indicator is a collection of three time series calculated from historical price data, most often the closing price. These three series are: the MACD series proper, the "signal" or "average" series, and the "divergence" series which is the difference between the two. The MACD series is the difference between a "fast" (short period) exponential moving average (EMA), and a "slow" (longer period) EMA of the price series. The average series is an EMA of the MACD series itself.

![MACD](https://user-images.githubusercontent.com/41892582/202749396-0e417819-2986-4ea1-b5c8-f51c63de82c5.png)

# MACD Formula

**The MACD line** is simply the 12 Period EMA minus the 26 Period EMA.

$$MACD line=12 Period EMA − 26 Period EMA$$

**The signal line** This is a 9-Period EMA of the MACD

$$SIGNAL line=9 Period EMA(MACD line)$$

A buy signal is generated when a MACD crosses over the the signal line. A sell signal, is generated when a MACD crosses below the signal line.

**calculate the MACD for DOW Jones**

according to Wikipedia, The Dow Jones Industrial Average (DJIA), Dow Jones, or simply the Dow, is a stock market index of 30 prominent companies listed on stock exchanges in the United States.

**DOW Jones Daily**

```
##get a historical data of DOW Jones Daily, open and close price
Macd_Dow_daily<-Dow_Jones_daily%>%
  select(Date,Open,Close)
```  

**calculate the 12-day EMA of the close prices** 
The first value is the simple moving average of the first 12 days; all other values are given by the formula:



$$EMA_n=Closing Price_n(\frac{2}{Time Period+1})+EMA_{n-1}(1-\frac{2}{Time Period+1})$$


```
Macd_Dow_daily$EMA_12Day<-NA
Macd_Dow_daily$EMA_12Day[12]<-mean(Macd_Dow_daily$Close[1:12])
for (i in 13:length(Macd_Dow_daily$Close))
     {
       Macd_Dow_daily$EMA_12Day[i]<-(Macd_Dow_daily$Close[i]*(2/(1+12)))+Macd_Dow_daily$EMA_12Day[i-1]*(1-(2/(1+12)))
     }
```

**calculate the 26-day EMA of the close prices**

```
Macd_Dow_daily$EMA_26Day<-NA
Macd_Dow_daily$EMA_26Day[26]<-mean(Macd_Dow_daily$Close[1:26])

for (j in 27:length(Macd_Dow_daily$Close))
     {
       Macd_Dow_daily$EMA_26Day[j]<-(Macd_Dow_daily$Close[j]*(2/(1+26)))+Macd_Dow_daily$EMA_26Day[j-1]*(1-(2/(1+26)))
     }
```

**MACD line**

The MACD line is the 12 day EMA minus the 26 day EMA.

```
Macd_Dow_daily$MACD<-(Macd_Dow_daily$EMA_12Day)-(Macd_Dow_daily$EMA_26Day)
```

**Signal line**

is a 9-day EMA of the MACD,the first value of the signal line is the Simple Moving Average of the first 9 values of the MACD line.

```
Macd_Dow_daily$Signal<-NA
Macd_Dow_daily$Signal[34]<-mean(Macd_Dow_daily$MACD[26:34])
for (k in 35:length(Macd_Dow_daily$Close))
     {
       Macd_Dow_daily$Signal[k]<-(Macd_Dow_daily$MACD[k]*(2/(1+9)))+Macd_Dow_daily$Signal[k-1]*(1-(2/(1+9)))
     }
```

**Action**  

There are four distinct types of activity. "Buy" is the action to take when the MACD line crosses over the signal line; "Hold" is the action to take during the time between the MACD line crossing over the signal line and before crossing below it; "Sell" is the action to take when the MACD line crosses below the signal line; and "Wait" is the action to take when the MACD line is below the signal line.

```
Macd_Dow_daily$Action<-NA
Macd_Dow_daily$Action[34]<-case_when(
  Macd_Dow_daily$MACD[34]>Macd_Dow_daily$Signal[34]~"Buy",
  Macd_Dow_daily$MACD[34]<Macd_Dow_daily$Signal[34]~"Wait"
                              )

for (l in 35:length(Macd_Dow_daily$Close))
   {
     Macd_Dow_daily$Action[l]<-case_when(
       Macd_Dow_daily$MACD[l]>Macd_Dow_daily$Signal[l]&Macd_Dow_daily$MACD[l-1]<Macd_Dow_daily$Signal[l-1]~"Buy",
       Macd_Dow_daily$MACD[l]>Macd_Dow_daily$Signal[l]&Macd_Dow_daily$MACD[l-1]>Macd_Dow_daily$Signal[l-1]~"Hold",
       Macd_Dow_daily$MACD[l]<Macd_Dow_daily$Signal[l]&Macd_Dow_daily$MACD[l-1]>Macd_Dow_daily$Signal[l-1]~"Sell",
       Macd_Dow_daily$MACD[l]<Macd_Dow_daily$Signal[l]&Macd_Dow_daily$MACD[l-1]<Macd_Dow_daily$Signal[l-1]~"Wait"
                                       )
   }
```

**Investment price**

The investment price is what our portfolio would be worth if we bought and sold in accordance with the MACD signals.

```
Macd_Dow_daily$investment_price<-NA
##The starting price of a portfolio is the price of the asset when the first buy signal occurs.
Macd_Dow_daily$investment_price[match("Buy",Macd_Dow_daily$Action)]<-Macd_Dow_daily$Close[match("Buy",Macd_Dow_daily$Action)]

for (m in (match("Buy",Macd_Dow_daily$Action)+1):length(Macd_Dow_daily$Close))
{
  Macd_Dow_daily$investment_price[m]<-case_when(
    Macd_Dow_daily$Action[m]=="Buy"~Macd_Dow_daily$investment_price[m-1],
    Macd_Dow_daily$Action[m]=="Wait"~Macd_Dow_daily$investment_price[m-1],
    Macd_Dow_daily$Action[m]=="Hold"~(Macd_Dow_daily$investment_price[m-1]/Macd_Dow_daily$Close[m-1])*Macd_Dow_daily$Close[m],
    Macd_Dow_daily$Action[m]=="Sell"~(Macd_Dow_daily$investment_price[m-1]/Macd_Dow_daily$Close[m-1])*Macd_Dow_daily$Close[m]
  )
}
```

tidying the data

```
##new data frame contains only the date, close price, and investment price.
##tidying the data
Macd_Dow_daily2<-Macd_Dow_daily%>%
  select(Date,Close,investment_price)%>%
  pivot_longer(c(`Close`, `investment_price`), names_to = "MACD_vs_DOW", values_to = "Price")  
```

**The result**

```
##plotting the result
ggplot(Macd_Dow_daily2,aes(x=Date,y=Price,col=MACD_vs_DOW))+
  geom_line()+
  theme(legend.title=element_blank())+
  theme(legend.position=c(0.1,0.9))+
  scale_color_manual(labels = c("Dow Jones price", "MACD investing price"),
                     values = c( "red", "blue"))+
  scale_x_date(date_breaks = "4 years",
               date_labels = "%b-%y")+
  ggtitle("Using the daily MACD buy-and-sell technique versus holding the Dow Jones")+
  xlab("Saturday, November 19, 2022")+
  ylab("")
```

![macd dow daily](https://user-images.githubusercontent.com/41892582/202853970-28817add-e5c3-4e90-ad6c-4b9dcf15ea86.jpg)

Evidently, investing in the Dow Jones over the long term was considerably preferable to trading using the MACD strategy. It might have happened as a result of the Dow Jones uptrend over the years, It is obvious that it is ineffective to exit our investment due to a lack of momentum in the hopes of returning to it at a lesser cost during a rally (uptrend).

Let's take every year on its own to see how MACD is doing versus DOW Jones.

```
## new dataframe to calculate investment yield by year
Dow_daily_revenue_by_years<-matrix(nrow = 53,ncol = 7)
Dow_daily_revenue_by_years<-as.data.frame(Dow_daily_revenue_by_years)
names(Dow_daily_revenue_by_years)=c("Year","Open","Close","MACD_open","MACD_close","DOW_revenue","MACD_revenue")
Dow_daily_revenue_by_years$Year<-c(1970:2022)

for(n in 1970:2022)
{
  Dow_daily_revenue_by_years$Open[n-1969]<-first(Macd_Dow_daily$Open[year(Macd_Dow_daily$Date)==n])
  Dow_daily_revenue_by_years$Close[n-1969]<-last(Macd_Dow_daily$Close[year(Macd_Dow_daily$Date)==n])
  Dow_daily_revenue_by_years$MACD_open[n-1969]<-first(Macd_Dow_daily$investment_price[year(Macd_Dow_daily$Date)==n])
  Dow_daily_revenue_by_years$MACD_close[n-1969]<-last(Macd_Dow_daily$investment_price[year(Macd_Dow_daily$Date)==n])
}

Dow_daily_revenue_by_years$MACD_open[1]<-Macd_Dow_daily$investment_price[34]

Dow_daily_revenue_by_years$DOW_revenue<-((Dow_daily_revenue_by_years$Close-Dow_daily_revenue_by_years$Open)/(Dow_daily_revenue_by_years$Open))*100
Dow_daily_revenue_by_years$MACD_revenue<-((Dow_daily_revenue_by_years$MACD_close-Dow_daily_revenue_by_years$MACD_open)/(Dow_daily_revenue_by_years$MACD_open))*100

##tidying the data

Dow_daily_revenue_by_years2<-Dow_daily_revenue_by_years %>%
  pivot_longer(c(`DOW_revenue`, `MACD_revenue`), names_to = "MACD_vs_DOW", values_to = "Revenue")
```

plotting the result

```
ggplot(data = Dow_daily_revenue_by_years2,aes(x=Year,y=Revenue,fill=MACD_vs_DOW))+
  geom_col(width = 0.6,position = position_dodge(0.7))+
  theme(legend.title=element_blank())+
  theme(legend.position=c(0.9,0.9))+
  scale_color_manual(labels = c("DOW", "MACD"),
                     values = c( "red", "blue"))+
  ggtitle("Every year, compare Dow revenue to trade with MACD (daily) revenue")+
  xlab("Saturday, November 19, 2022")+
  ylab("%Revenue")
```

![macd dow daily2](https://user-images.githubusercontent.com/41892582/202853988-848c1f66-1297-4c7d-8c3c-88b2880a089e.jpg)

As we've already seen, MACD's performance is significantly far away from Dow Jones, but as we also observed, it performs better during the most bearish years,The challenge now is to determine the false sell signal during the uptrend.

**Functions**

To make it easier to use with other security types, we're going to incorporate the prior code into three functions.

**The "MACD_file" function** is a function that takes only one argument, a file containing the date and the close price. The output will be a file containing the calculation of the signal and MACD line, the action taken according to the MACD strategy, and the investment price as a result of the trading strategy we use.

**The "investment_price_MACD" function** is a function that plots the price of investing if we use the MACD buy-and-sell technique versus holding the security over time.

**The "yield by year" function** compares the yield of a security over each year to the yield if we utilize the MACD approach for purchasing and selling.


The first technical indicator function is the "MACD file"; afterwards, we will develop functions for each indicator we utilize as well as functions for each individual strategy inside each indicator.
```
MACD_file<-function(file_name)
{ 
    ##get a historical data of the security, open and close price
    Macd_file_name<-file_name%>%
        select(Date,Open,Close)
    
    ##calculate the 12-period EMA of the close prices
    Macd_file_name$EMA_12period<-NA
    Macd_file_name$EMA_12period[12]<-mean(Macd_file_name$Close[1:12])
    for (i in 13:length(Macd_file_name$Close))
    {
        Macd_file_name$EMA_12period[i]<-(Macd_file_name$Close[i]*(2/(1+12)))+Macd_file_name$EMA_12period[i-1]*(1-(2/(1+12)))
    }
    
    ##calculate the 26-period EMA of the close prices
    Macd_file_name$EMA_26period<-NA
    Macd_file_name$EMA_26period[26]<-mean(Macd_file_name$Close[1:26])
    
    for (j in 27:length(Macd_file_name$Close))
    {
        Macd_file_name$EMA_26period[j]<-(Macd_file_name$Close[j]*(2/(1+26)))+Macd_file_name$EMA_26period[j-1]*(1-(2/(1+26)))
    }
    
    ##MACD line
    Macd_file_name$MACD<-(Macd_file_name$EMA_12period)-(Macd_file_name$EMA_26period)
    
    ##Signal line
    Macd_file_name$Signal<-NA
    Macd_file_name$Signal[34]<-mean(Macd_file_name$MACD[26:34])
    for (k in 35:length(Macd_file_name$Close))
    {
        Macd_file_name$Signal[k]<-(Macd_file_name$MACD[k]*(2/(1+9)))+Macd_file_name$Signal[k-1]*(1-(2/(1+9)))
    }
    
    ##Action
    Macd_file_name$Action<-NA

    
    for (l in 35:length(Macd_file_name$Close))
    {
        Macd_file_name$Action[l]<-case_when(
            Macd_file_name$MACD[l]>Macd_file_name$Signal[l]&Macd_file_name$MACD[l-1]<Macd_file_name$Signal[l-1]~"Buy",
            Macd_file_name$MACD[l]>Macd_file_name$Signal[l]&Macd_file_name$MACD[l-1]>Macd_file_name$Signal[l-1]~"Hold",
            Macd_file_name$MACD[l]<Macd_file_name$Signal[l]&Macd_file_name$MACD[l-1]>Macd_file_name$Signal[l-1]~"Sell",
            Macd_file_name$MACD[l]<Macd_file_name$Signal[l]&Macd_file_name$MACD[l-1]<Macd_file_name$Signal[l-1]~"Wait"
        )
    }
    
    ##Investment price
    Macd_file_name$investment_price<-NA
    Macd_file_name$investment_price[match("Buy",Macd_file_name$Action)]<-Macd_file_name$Close[match("Buy",Macd_file_name$Action)]
    
    for (m in (match("Buy",Macd_file_name$Action)+1):length(Macd_file_name$Close))
    {
        Macd_file_name$investment_price[m]<-case_when(
            Macd_file_name$Action[m]=="Buy"~Macd_file_name$investment_price[m-1],
            Macd_file_name$Action[m]=="Wait"~Macd_file_name$investment_price[m-1],
            Macd_file_name$Action[m]=="Hold"~(Macd_file_name$investment_price[m-1]/Macd_file_name$Close[m-1])*Macd_file_name$Close[m],
            Macd_file_name$Action[m]=="Sell"~(Macd_file_name$investment_price[m-1]/Macd_file_name$Close[m-1])*Macd_file_name$Close[m]
        )
    }
    return(Macd_file_name)
}

```

"investment_price" is a function to plot the comparison between the price of the security over time and the investment price if we trade according to the technical indicator strategy. 

The first input is the file containing the close of the price for a given time period, the price if we trade using the indicator, and the date. 

The function then accepts the other three arguments. The indecator's name is the second argument. The name of the security itself is the third argument, and by default it is given the value "security". The final input is a time frame, which can be daily, weekly, or monthly. By default, we gave this argument a blank value.

```
##"investment_price" is a function that plots the price of investing if we use the specific Technical Indicator buy-and-sell technique versus holding the security over time.  
investment_price<-function (Technical_Indicator_file,Indecator_name,the_security_name="security",a_time_frame="")
{
  ##tidying the data
  ##new data frame contains only the date, close price, and investment price.
  Technical_Indicator_file2<-Technical_Indicator_file%>%
    select(Date,Close,investment_price)%>%
    pivot_longer(c(`Close`, `investment_price`), names_to = "security_vs_indicator", values_to = "Price")  
  
  ##The result
  ##plotting the result
  holding_vs_indicator_trading<-ggplot(Technical_Indicator_file2,aes(x=Date,y=Price,col=security_vs_indicator))+
    geom_line()+
    theme(legend.title=element_blank())+
    theme(legend.position=c(0.1,0.9))+
    scale_color_manual(labels = c(paste0(the_security_name," price"), paste0(Indecator_name," investing price")),
                       values = c( "red", "blue"))+
    scale_x_date(date_breaks = "4 years",
                 date_labels = "%b-%y")+
    eval(parse(text =paste0("ggtitle(","'","Using the",Indecator_name," buy-and-sell technique(",a_time_frame, ")versus holding the ", the_security_name,"'",")"))) +
    xlab(paste0(format(last(Technical_Indicator_file$Date),"%a %b %d %Y")))+
    ylab("")
  
  return(plot(holding_vs_indicator_trading))
}
```

The "yield_by_year" function is a function that takes the same arguments as "investment_price", and the output will be the plotting of the percentage of gain or loss either to the security and our portfolio if we trade according to the technical indicator strategy.

```
yield_by_year<-function(Technical_Indicator_file,Indecator_name,the_security_name="security",a_time_frame="")
{
  ## new dataframe to calculate investment yield by year
  revenue_by_years<-matrix(nrow = (last(year(Technical_Indicator_file$Date))-first(year(Technical_Indicator_file$Date))+1),ncol = 7)
  revenue_by_years<-as.data.frame(revenue_by_years)
  names(revenue_by_years)=c("Year","Open","Close",paste0(Indecator_name,"_open"),paste0(Indecator_name,"_close"),paste0(the_security_name,"_revenue"),paste0(Indecator_name,"_revenue"))
  revenue_by_years$Year<-c(first(year(Technical_Indicator_file$Date)):last(year(Technical_Indicator_file$Date)))
  
  for(n in first(year(Technical_Indicator_file$Date)):last(year(Technical_Indicator_file$Date)))
  {
    revenue_by_years$Open[n-(first(year(Technical_Indicator_file$Date))-1)]<-first(Technical_Indicator_file$Open[year(Technical_Indicator_file$Date)==n])
    revenue_by_years$Close[n-(first(year(Technical_Indicator_file$Date))-1)]<-last(Technical_Indicator_file$Close[year(Technical_Indicator_file$Date)==n])
    eval(parse(text =paste0('revenue_by_years$',Indecator_name,'_open[n-(first(year(Technical_Indicator_file$Date))-1)]<-first(Technical_Indicator_file$investment_price[year(Technical_Indicator_file$Date)==n])')))
    eval(parse(text =paste0('revenue_by_years$',Indecator_name,'_close[n-(first(year(Technical_Indicator_file$Date))-1)]<-last(Technical_Indicator_file$investment_price[year(Technical_Indicator_file$Date)==n])')))
  }
  
  eval(parse(text =paste0('revenue_by_years$',Indecator_name,'_open[1]<-Technical_Indicator_file$investment_price[which(!is.na(Technical_Indicator_file$investment_price))[1]]')))
  
  eval(parse(text =paste0('revenue_by_years$',the_security_name,'_revenue<-((revenue_by_years$Close-revenue_by_years$Open)/(revenue_by_years$Open))*100')))
  eval(parse(text =paste0('revenue_by_years$',Indecator_name,'_revenue<-((revenue_by_years$',Indecator_name,'_close-revenue_by_years$',Indecator_name,'_open)/(revenue_by_years$',Indecator_name,'_open))*100')))
  
  ##tidying the data
  
  revenue_by_years2<-revenue_by_years %>%
    pivot_longer(c(paste0(the_security_name,"_revenue"),paste0(Indecator_name,"_revenue")), names_to = "Indecator_vs_security", values_to = "Revenue")
  
  ##plotting the result
  
  every_year_revenue<-ggplot(data = revenue_by_years2,aes(x=Year,y=Revenue,fill=Indecator_vs_security))+
    geom_col(width = 0.6,position = position_dodge(0.7))+
    theme(legend.title=element_blank())+
    theme(legend.position=c(0.9,0.9))+
    scale_color_manual(labels = c(paste0(the_security_name,"revenue"),paste0(Indecator_name," revenue")),
                       values = c( "red", "blue"))+
    ggtitle(paste0("Every year, compare ",the_security_name," revenue to trade with", Indecator_name,"(",a_time_frame,")revenue"))+
    xlab(paste0(format(last(Technical_Indicator_file$Date),"%a %b %d %Y")))+
    ylab("%Revenue")
  
  return(plot(every_year_revenue))
  
}
```

We are going to create a function called "buy_performance" to create the performance metrics.

```
buy_performance<-function(file_name)
{
  
##creating new variables "outcome","Profit","Loss2",and "Revenue"
  file_name$outcome<-NA
  file_name$Profit<-NA
  file_name$Loss2<-NA
  file_name$Revenue<-NA
  
##calculating the outcome (which is the difference between the sell price and the buy price)
##The outcome is profit when it's positive and Loss2 when it's negative  
  for (i in 1:length(file_name$Close))
    if (file_name$Action[i]=="Buy"& !is.na(file_name$Action[i]))
    {
      for (j in i+1:length(file_name$Close))
      {
        if (file_name$Action[j]=="Sell"& !is.na(file_name$Action[j]))
        {
          file_name$outcome[j]<-file_name$investment_price[j]-file_name$investment_price[i]
          file_name$Revenue[j]<-((file_name$investment_price[j]-file_name$investment_price[i])/file_name$investment_price[i])*100
          break
        }
      }
    }
  file_name$Profit<-file_name$outcome
  file_name$Profit[file_name$Profit<0]<-NA
  file_name$Loss2<-file_name$outcome
  file_name$Loss2[file_name$Loss2>0]<-NA

##Draw down
  
  file_name$Draw_down_value<-NA
  file_name$Draw_down_percentage<-NA
  peak_value<-file_name$investment_price[match("Buy",file_name$Action)]
  trough_value<-file_name$investment_price[match("Buy",file_name$Action)]
  draw_down<-0
  
  for (i in (match("Buy",file_name$Action)+1):length(file_name$Close))
  {
    if (peak_value>file_name$investment_price[i])
    {
      if (trough_value>file_name$investment_price[i])
      {
        trough_value<-file_name$investment_price[i]
        new_draw_down<-peak_value-trough_value
        if(new_draw_down>draw_down)
        {
          draw_down<-new_draw_down
          file_name$Draw_down_value[i]<-draw_down
          file_name$Draw_down_percentage[i]<-((file_name$Draw_down_value[i])/peak_value)*100
        }
      }
    }
    else
    {
      peak_value<-file_name$investment_price[i]
      trough_value<-file_name$investment_price[i]
      draw_down<-0
    }
  }   
  
##performance  
  performance<-matrix(nrow = 19,ncol = 2)
  performance<-as.data.frame(performance)
  names(performance)<-c("Statistics","Long Trades")
  performance[,1]<-c("Total Net Profit","Profit Factor","Total Number of Trades","%Profitable(Winning Trades)",
                     "Average Trade Net Profit","%Average Revenue",
                     "Avg. Winning Trade","Avg. Losing Trade","Avg. Win: Avg Loss","Largest Winning Trade",
                     "Largest Losing Trade","Trading Period","Time in the Market","%M D D(intraday peak to valley)",
                     "portfolio starting price","portfolio close price","%change","highest price","lowest price")
  
  ##Tootal Net Profit
  performance[1,2]<-paste0(round(sum(file_name$Profit,na.rm = TRUE)-sum(-file_name$Loss2,na.rm = TRUE),digits = 5)," price")
  
  ##Profit Factor
  performance[2,2]<-round(sum(file_name$Profit,na.rm = TRUE)/sum(-file_name$Loss2,na.rm = TRUE),digits=4)
  
  #Total Number of Trades
  performance[3,2]<-length(na.omit(file_name$outcome))
  
  ##%Profitable
  performance[4,2]<-paste0(round(((length(na.omit(file_name$Profit))/length(na.omit(file_name$outcome)))*100),digits=1),"%")
  
  ##Average Trade Net Profit
  performance[5,2]<-paste0(round((sum(file_name$Profit,na.rm = TRUE)-sum(-file_name$Loss2,na.rm = TRUE))/length(na.omit(file_name$outcome)),digits = 5)," price")
  
  ##%Average Revenue
  performance[6,2]<-paste0(round(mean(file_name$Revenue,na.rm = TRUE),digits = 2),"%")
  
  ##Avg. Winning Trade
  performance[7,2]<-paste0(round(mean(file_name$Profit,na.rm = TRUE),digits = 5)," price")
  
  ##Avg. Losing Trade
  performance[8,2]<-paste0(round(abs(mean(file_name$Loss2,na.rm = TRUE)),digits=5)," price")
  
  ##Avg. Win: Avg Loss
  performance[9,2]<-round(abs(mean(file_name$Profit,na.rm = TRUE)/mean(file_name$Loss2,na.rm = TRUE)),digits=3)
  
  ##Largest Winning Trade
  performance[10,2]<-paste0(round(max(file_name$Profit,na.rm = TRUE),digits = 5)," price")
  
  ##Largest Losing Trade
  performance[11,2]<-paste0(round(abs(min(file_name$Loss2,na.rm = TRUE)),digits = 5)," Price")
  
  ##Trading Period
  performance[12,2]<-paste0(round(time_length(difftime(last(file_name$Date), first(file_name$Date)), "years"),digits=2)," years")
  
  ##Time in the Market
  performance[13,2]<-paste0(round(((length(na.omit(file_name$Action[file_name$Action=="Hold"]))+length(na.omit(file_name$Action[file_name$Action=="Sell"])))/length(file_name$Date))*100,digits = 2),"%")
  
  ##%M D D(intraday peak to valley)
  performance[14,2]<-paste0(-round(max(file_name$Draw_down_percentage,na.rm = TRUE),digits = 2),"%")
  
  ##portfolio starting price
  performance[15,2]<-paste0(round(file_name$investment_price[match("Buy",file_name$Action)],digits = 5)," price")
  
  ##portfolio close price
  performance[16,2]<-paste0(round(last(file_name$investment_price),digits = 5)," price")
  
  ##%change
  performance[17,2]<-paste0(round(((last(file_name$investment_price)-file_name$investment_price[match("Buy",file_name$Action)])/file_name$investment_price[match("Buy",file_name$Action)])*100,digits = 2),"%")
  
  ##highest price
  performance[18,2]<-paste0(round(max(file_name$investment_price,na.rm = TRUE),digits = 5)," price")
   
  ##lowest price 
  performance[19,2]<-paste0(round(min(file_name$investment_price,na.rm = TRUE),digits = 5)," price")

  return(head(performance,19))
  }
```

Now let's see the performance metrics of the MACD indicator when it's trading the daily Dow Jones

```
buy_performance(MACD_file(Macd_Dow_daily))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 5573.93074 price
2                    Profit Factor           1.4648
3           Total Number of Trades              537
4      %Profitable(Winning Trades)            43.6%
5         Average Trade Net Profit   10.37976 price
6                 %Average Revenue            0.45%
7               Avg. Winning Trade   75.06792 price
8                Avg. Losing Trade   39.57743 price
9               Avg. Win: Avg Loss            1.897
10           Largest Winning Trade  490.74575 price
11            Largest Losing Trade  247.09516 Price
12                  Trading Period      52.88 years
13              Time in the Market           50.51%
14 %M D D(intraday peak to valley)          -32.25%
15        portfolio starting price      790.1 price
16           portfolio close price 7093.86043 price
17                         %change          797.84%
18                   highest price  7094.3166 price
19                    lowest price  685.79726 price
```

low average revenue per trade (0.45%), with a plausibility profit factor, but a high volume of trades that mislead us with sell signals prohibit us from making more money, especially given the Dow Jones's strong upswing.

Let's apply the same processing for DOW Jones weekly chart.

**DOW Jones Weekly**

```
investment_price(MACD_file(Dow_Jones_weekly),"MACD","Dow_Jones","weekly")
```

![macd dow weekly](https://user-images.githubusercontent.com/41892582/202857160-5f8790e2-8bbf-4fb0-8af4-68427b3c822c.jpg)

```
yield_by_year(MACD_file(Dow_Jones_weekly),"MACD","Dow_Jones","weekly")
```

![macd dow weekly2](https://user-images.githubusercontent.com/41892582/202857182-429f0076-8304-45fb-a76d-e72b299e927d.jpg)

```
buy_performance(MACD_file(Dow_Jones_weekly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 3989.15573 price
2                    Profit Factor           1.6103
3           Total Number of Trades              115
4      %Profitable(Winning Trades)            44.3%
5         Average Trade Net Profit   34.68831 price
6                 %Average Revenue            1.85%
7               Avg. Winning Trade  206.39275 price
8                Avg. Losing Trade  102.13866 price
9               Avg. Win: Avg Loss            2.021
10           Largest Winning Trade  930.62526 price
11            Largest Losing Trade  799.50587 Price
12                  Trading Period      52.88 years
13              Time in the Market           50.91%
14 %M D D(intraday peak to valley)          -45.31%
15        portfolio starting price      881.2 price
16           portfolio close price  5001.3546 price
17                         %change          467.56%
18                   highest price 5556.81125 price
19                    lowest price  635.07207 price
```
The MACD indicator (weekly time frame) makes less money with almost the same amount of trading time than the daily Macd time frame, which has a higher average revenue per transaction and fewer trades.

**DOW Jones monthly**

```
investment_price(MACD_file(Dow_Jones_monthly),"MACD","Dow_Jones","monthly")
```

![macd dow monthly](https://user-images.githubusercontent.com/41892582/202857210-0bafd81e-2f05-47ac-8283-a6baecbbd2f0.jpg)

```
yield_by_year(MACD_file(Dow_Jones_monthly),"MACD","Dow_Jones","monthly")
```

![macd dow monthly2](https://user-images.githubusercontent.com/41892582/202857226-0cd68649-fa79-4ea5-9586-61cf5fa8f6d8.jpg)

```
buy_performance(MACD_file(Dow_Jones_monthly))
```

```
                        Statistics       Long Trades
1                 Total Net Profit   9995.1448 price
2                    Profit Factor            7.3507
3           Total Number of Trades                22
4      %Profitable(Winning Trades)             72.7%
5         Average Trade Net Profit   454.32476 price
6                 %Average Revenue            13.15%
7               Avg. Winning Trade    723.0626 price
8                Avg. Losing Trade   262.30948 price
9               Avg. Win: Avg Loss             2.757
10           Largest Winning Trade  2842.66396 price
11            Largest Losing Trade  1001.42846 Price
12                  Trading Period       52.83 years
13              Time in the Market             54.8%
14 %M D D(intraday peak to valley)           -25.14%
15        portfolio starting price      1018.2 price
16           portfolio close price  11013.3448 price
17                         %change           981.65%
18                   highest price 11808.07101 price
19                    lowest price    923.2362 price
```

It appears that MACD (monthly) is the preferred timeframe among the others (initially with an uptrend) due to its greater net profit, larger profitability ratio (72.7), high average revenue per transaction (13.15%), and lower MDD, but it still lagged the DOW Jones. It had the advantage of letting you out during the most major bearish periods, but needed more time to give you a buy signal again.

**Gold (ozt) / U.S. Dollar (XAUUSD)**

The definition of XAU is derived from the ISO 4217 standard code for a troy ounce of gold. Gold, or XAU, is regarded as a form of money.

XAU used to stand for a currency used to purchase products. Because of this, there was a golden age when many people began to look for gold and become wealthy.

According to Gemforex, XAU/USD is a trading method to buy and sell gold and US dollars.

It deals with things that are not gold currency, but it can also trade in a currency pair like the dollar yen. However, it conducts trade in units of the Troy ounce, not units such as 100,000 currencies.

**Gold daily**

```
investment_price(MACD_file(Gold_daily),"MACD","Gold","daily")
```

![macd gold daily](https://user-images.githubusercontent.com/41892582/202857983-9e6ef710-b894-4139-8958-abb4d55103e4.jpg)

At the end of the time period under consideration, the MACD indicator lags the price of gold; this lag started in almost 2009, which may be a result of the gold price's sharp uptrend, as happened with the Dow Jones.

```
yield_by_year(MACD_file(Gold_daily),"MACD","Gold","daily")
```

![macd gold daily2](https://user-images.githubusercontent.com/41892582/202857996-a805afbe-b256-43fb-ad8b-5721e42cc17e.jpg)

```
buy_performance(MACD_file(Gold_daily))
```

```
                        Statistics      Long Trades
1                 Total Net Profit  1295.6611 price
2                    Profit Factor           1.3942
3           Total Number of Trades              501
4      %Profitable(Winning Trades)            37.5%
5         Average Trade Net Profit    2.58615 price
6                 %Average Revenue            0.86%
7               Avg. Winning Trade   24.37676 price
8                Avg. Losing Trade    10.4687 price
9               Avg. Win: Avg Loss            2.329
10           Largest Winning Trade   163.7889 price
11            Largest Losing Trade   71.95931 Price
12                  Trading Period      52.87 years
13              Time in the Market           51.41%
14 %M D D(intraday peak to valley)           -43.4%
15        portfolio starting price       35.2 price
16           portfolio close price 1384.18973 price
17                         %change         3832.36%
18                   highest price  1538.6072 price
19                    lowest price       35.2 price
```

**Gold weekly**

```
investment_price(MACD_file(Gold_weekly),"MACD","Gold","weekly")
```

![macd gold weekly](https://user-images.githubusercontent.com/41892582/202857933-0e29b536-a3ee-487b-b720-c1e1accf4d75.jpg)

```
yield_by_year(MACD_file(Gold_weekly),"MACD","Gold","weekly")
```

![macd gold weekly2](https://user-images.githubusercontent.com/41892582/202857958-5af68868-ce41-4821-b6d4-15e0243db921.jpg)

When compared to the price of the assessed itself, weekly trade in this case is incredibly slow.

```
buy_performance(MACD_file(Gold_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit 433.38164 price
2                    Profit Factor          1.5685
3           Total Number of Trades             119
4      %Profitable(Winning Trades)           42.9%
5         Average Trade Net Profit   3.64186 price
6                 %Average Revenue           3.05%
7               Avg. Winning Trade  23.44406 price
8                Avg. Losing Trade  11.04733 price
9               Avg. Win: Avg Loss           2.122
10           Largest Winning Trade 214.38198 price
11            Largest Losing Trade  43.22187 Price
12                  Trading Period     52.88 years
13              Time in the Market          49.33%
14 %M D D(intraday peak to valley)         -67.09%
15        portfolio starting price      36.5 price
16           portfolio close price  464.8077 price
17                         %change        1173.45%
18                   highest price 547.95152 price
19                    lowest price      36.4 price
```

**Gold monthly**

```
investment_price(MACD_file(Gold_monthly),"MACD","Gold","monthly")
```

![macd gold monthly](https://user-images.githubusercontent.com/41892582/202857892-df1a2ce1-59ee-4b8a-9f50-3a53603a467d.jpg)

```
yield_by_year(MACD_file(Gold_monthly),"MACD","Gold","monthly")
```

![macd gold monthly2](https://user-images.githubusercontent.com/41892582/202857915-294b804f-e812-47ac-8651-8d84ae6141b6.jpg)

monthly trading with MACD outperforming weekly and daily trading, like in the case of Dow Jones

```
buy_performance(MACD_file(Gold_monthly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 1235.54103 price
2                    Profit Factor             2.73
3           Total Number of Trades               21
4      %Profitable(Winning Trades)            42.9%
5         Average Trade Net Profit   58.83529 price
6                 %Average Revenue           20.96%
7               Avg. Winning Trade  216.63467 price
8                Avg. Losing Trade   59.51425 price
9               Avg. Win: Avg Loss             3.64
10           Largest Winning Trade  529.44196 price
11            Largest Losing Trade  191.76387 Price
12                  Trading Period      52.83 years
13              Time in the Market           54.33%
14 %M D D(intraday peak to valley)          -53.47%
15        portfolio starting price        146 price
16           portfolio close price 1381.54103 price
17                         %change          846.26%
18                   highest price 1578.85377 price
19                    lowest price        146 price
```

With the exception of the weekly on the level of the overall net profit, trading on a daily and monthly basis is doing well, with net changes of 3832% and 2343%, respectively. Daily revenue on average is just 0.86% and monthly revenue is 27.2%. also As before, the MACD indicator lagged in bullish years and performed better in bearish ones. 

**EUR/USD Rate**

According to Investopedia, The Currency Pair (EUR/USD) is the shortened term for the euro against the U.S. dollar pair, or cross, for the currencies of the European Union (EU) and the United States (USD). The currency pair indicates how many U.S. dollars (the quote currency) are needed to purchase one euro (the base currency). Trading the EUR/USD currency pair is also known as trading the "euro." The value of the EUR/USD pair is quoted as 1 euro per x U.S. dollars. For example, if the pair is trading at 1.50, it means it takes 1.5 U.S. dollars to buy 1 euro.

**EUR/USD daily**

```
investment_price(MACD_file(EurUsd_daily),"MACD","EurUsd","daily")
```

![macd eurusd daily](https://user-images.githubusercontent.com/41892582/202859867-6f5a361d-1272-4393-b343-6478ebd07849.jpg)

Here we go. The MACD is performing sufficiently better than the EUR/USD pair. Obviously, the return on investment has more than doubled, but considering the high leverage this pair is trading with, is that sufficient?

```
yield_by_year(MACD_file(EurUsd_daily),"MACD","EurUsd","daily")
```

![macd eurusd daily2](https://user-images.githubusercontent.com/41892582/202859844-455f56f8-c69b-40e5-8abf-09ddc7b26a42.jpg)

Surprisingly, MACD is also underperforming over the majority of the bullish years, even if he performed better overall during the time period under review. it's is significant during the bearish years.

```
buy_performance(MACD_file(EurUsd_daily))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 1.01635 price
2                    Profit Factor        1.1627
3           Total Number of Trades           512
4      %Profitable(Winning Trades)         35.4%
5         Average Trade Net Profit 0.00199 price
6                 %Average Revenue         0.24%
7               Avg. Winning Trade 0.04012 price
8                Avg. Losing Trade 0.01887 price
9               Avg. Win: Avg Loss         2.126
10           Largest Winning Trade 0.16717 price
11            Largest Losing Trade 0.09766 Price
12                  Trading Period   51.87 years
13              Time in the Market        50.61%
14 %M D D(intraday peak to valley)        -34.2%
15        portfolio starting price  0.5373 price
16           portfolio close price 1.61061 price
17                         %change       199.76%
18                   highest price 2.35524 price
19                    lowest price  0.5369 price
```

**EUR/USD weekly**

```
investment_price(MACD_file(EurUsd_weekly),"MACD","EurUsd","weekly")
```

![macd eurusd weekly](https://user-images.githubusercontent.com/41892582/202859813-6fa4ecc8-1b9f-456e-bba4-cce23e334efd.jpg)

```
yield_by_year(MACD_file(EurUsd_weekly),"MACD","EurUsd","weekly")
```

![macd eurusd weekly2](https://user-images.githubusercontent.com/41892582/202859794-c544a9e0-1048-45d9-a0e5-e2599709a93f.jpg)


```
buy_performance(MACD_file(EurUsd_weekly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 1.02512 price
2                    Profit Factor        1.4921
3           Total Number of Trades           100
4      %Profitable(Winning Trades)           47%
5         Average Trade Net Profit 0.01025 price
6                 %Average Revenue         1.16%
7               Avg. Winning Trade 0.06613 price
8                Avg. Losing Trade  0.0393 price
9               Avg. Win: Avg Loss         1.683
10           Largest Winning Trade 0.25733 price
11            Largest Losing Trade 0.10218 Price
12                  Trading Period   51.86 years
13              Time in the Market        46.84%
14 %M D D(intraday peak to valley)       -24.17%
15        portfolio starting price  0.5985 price
16           portfolio close price 1.69972 price
17                         %change          184%
18                   highest price 2.14113 price
19                    lowest price 0.58461 price
```

With a slight advantage over daily trading in terms of total net profit, weekly trades outperform everything else.
Having less MDD, less time in the market, and fewer trades, with 47% successful trades compared to 35.4%, and an average revenue per trade of 1.16% as opposed to 0.24%.

**EUR/USD monthly**

```
investment_price(MACD_file(EurUsd_monthly),"MACD","EurUsd","monthly")
```

![macd eurusd monthly](https://user-images.githubusercontent.com/41892582/202859777-2361975d-aa59-48d5-b1dc-5841be440e23.jpg)

```
yield_by_year(MACD_file(EurUsd_monthly),"MACD","EurUsd","monthly")
```

![macd eurusd monthly2](https://user-images.githubusercontent.com/41892582/202859757-0abf1a5b-e339-414e-b4ed-8a7efdcb65a0.jpg)

```
buy_performance(MACD_file(EurUsd_monthly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 0.70044 price
2                    Profit Factor        2.0869
3           Total Number of Trades            22
4      %Profitable(Winning Trades)         36.4%
5         Average Trade Net Profit 0.03184 price
6                 %Average Revenue          3.9%
7               Avg. Winning Trade 0.16811 price
8                Avg. Losing Trade 0.04603 price
9               Avg. Win: Avg Loss         3.652
10           Largest Winning Trade 0.47188 price
11            Largest Losing Trade 0.11001 Price
12                  Trading Period   51.83 years
13              Time in the Market        47.35%
14 %M D D(intraday peak to valley)       -31.19%
15        portfolio starting price  0.8335 price
16           portfolio close price 1.53394 price
17                         %change        84.04%
18                   highest price 1.78879 price
19                    lowest price 0.71489 price
```

Despite having a high average revenue percent and a strong profit factor, it appears that the weekly and daily time frames are outperforming the monthly time frame when examining time in market and total net profit.

**10-Year U.S. Bond Yield (10USY.B)**

**10USY.B daily**

According to Investopedia The 10-year Treasury yield is the yield that the government pays investors that purchase the specific security. Purchase of the 10-year note is essentially a loan made to the U.S. government. The yield is considered a marker for investor confidence in the markets, shining a light on whether investors feel they can make a higher return than the yield offered on a 10-year note by investing in stocks, ETFs, or other riskier securities.

```
investment_price(MACD_file(US_10YB_daily),"MACD","US_10YB","daily")
```

![macd US_10YB daily](https://user-images.githubusercontent.com/41892582/202860992-00da1d85-01e2-47b3-bac8-33d08e7d958f.jpg)

simply amazing here; not surprising the major trend of the asset here is down trend

```
yield_by_year(MACD_file(US_10YB_daily),"MACD","US_10YB","daily")
```

![macd US_10YB daily2](https://user-images.githubusercontent.com/41892582/202861004-4180e434-d3a4-4235-a1fe-3ac0abdbbe66.jpg)

Maybe it's going to be a trademark here about MACD and its performance in bearish years.

```
buy_performance(MACD_file(US_10YB_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit 10.96862 price
2                    Profit Factor         1.0647
3           Total Number of Trades            479
4      %Profitable(Winning Trades)          37.2%
5         Average Trade Net Profit   0.0229 price
6                 %Average Revenue           0.4%
7               Avg. Winning Trade  1.01347 price
8                Avg. Losing Trade  0.55917 price
9               Avg. Win: Avg Loss          1.812
10           Largest Winning Trade  5.76768 price
11            Largest Losing Trade   4.2685 Price
12                  Trading Period    52.88 years
13              Time in the Market         49.38%
14 %M D D(intraday peak to valley)        -81.89%
15        portfolio starting price     6.97 price
16           portfolio close price 17.93862 price
17                         %change        157.37%
18                   highest price  32.2167 price
19                    lowest price  5.83494 price
```
This MACD method appears to be tradable in downtrends but has a fundamental flaw: excessive MDD.

**10USY.B weekly**

```
investment_price(MACD_file(US_10YB_weekly),"MACD","US_10YB","weekly")
```

![macd US_10YB weekly](https://user-images.githubusercontent.com/41892582/202861018-80374c18-bfe7-4c99-9ee1-e00ed3c5a96f.jpg)

The price of investment over the years didn't take a downtrend as much as it did for a time with the daily time frame

```
yield_by_year(MACD_file(US_10YB_weekly),"MACD","US_10YB","weekly")
```

![macd US_10YB weekly2](https://user-images.githubusercontent.com/41892582/202861031-cef18006-0de1-4709-aecf-21bde959bad0.jpg)

```
buy_performance(MACD_file(US_10YB_weekly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit 59.63622 price
2                    Profit Factor         2.6551
3           Total Number of Trades             93
4      %Profitable(Winning Trades)          48.4%
5         Average Trade Net Profit  0.64125 price
6                 %Average Revenue          3.47%
7               Avg. Winning Trade  2.12596 price
8                Avg. Losing Trade  0.75067 price
9               Avg. Win: Avg Loss          2.832
10           Largest Winning Trade 25.54045 price
11            Largest Losing Trade  4.00603 Price
12                  Trading Period    52.88 years
13              Time in the Market         50.62%
14 %M D D(intraday peak to valley)        -51.64%
15        portfolio starting price     6.01 price
16           portfolio close price 72.78816 price
17                         %change       1111.12%
18                   highest price 80.20195 price
19                    lowest price     6.01 price
```

It's an amazing result when trading an asset with a major downtrend.

**10USY.B monthly**

```
investment_price(MACD_file(US_10YB_monthly),"MACD","US_10YB","monthly")
```

![macd US_10YB monthly](https://user-images.githubusercontent.com/41892582/202861043-d4cb4a4f-3603-4865-9e19-1d236afa718b.jpg)

Monthly trading with MACD outperforms the asset, but appears to underperform daily and weekly trading.

```
yield_by_year(MACD_file(US_10YB_monthly),"MACD","US_10YB","monthly")
```

![macd US_10YB monthly2](https://user-images.githubusercontent.com/41892582/202861052-e37577ae-b89a-4a3c-b8a9-a6e68f6760d8.jpg)

```
buy_performance(MACD_file(US_10YB_monthly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -2.46902 price
2                    Profit Factor         0.7923
3           Total Number of Trades             20
4      %Profitable(Winning Trades)            45%
5         Average Trade Net Profit -0.12345 price
6                 %Average Revenue          -0.6%
7               Avg. Winning Trade  1.04637 price
8                Avg. Losing Trade  1.08058 price
9               Avg. Win: Avg Loss          0.968
10           Largest Winning Trade  1.51116 price
11            Largest Losing Trade  2.38455 Price
12                  Trading Period    52.83 years
13              Time in the Market         48.98%
14 %M D D(intraday peak to valley)         -73.6%
15        portfolio starting price     8.31 price
16           portfolio close price  23.9633 price
17                         %change        188.37%
18                   highest price 25.82527 price
19                    lowest price  4.34912 price
```
The strategy does not appear to be profitable unless the last trade is completed (which has not yet been closed by a sell). 

**US Dollar/USDX - Index - Cash (DX-Y.NYB)**

According to Wikipedia The U.S. Dollar Index is an index (or measure) of the value of the United States dollar relative to a basket of foreign currencies, often referred to as a basket of U.S. trade partners' currencies. The Index goes up when the U.S. dollar gains "strength" (value) when compared to other currencies.

The index is designed, maintained, and published by ICE (Intercontinental Exchange, Inc.), with the name "U.S. Dollar Index" a registered trademark.

It is a weighted geometric mean of the dollar's value relative to following select currencies:

Euro (EUR), 57.6% weight
Japanese yen (JPY), 13.6% weight
Pound sterling (GBP), 11.9% weight
Canadian dollar (CAD), 9.1% weight
Swedish krona (SEK), 4.2% weight
Swiss franc (CHF), 3.6% weight

**USDX Daily**

```
investment_price(MACD_file(USDX_daily),"MACD","USDX","daily")
```

![MACD USDX_daily](https://user-images.githubusercontent.com/41892582/202862491-96f7fa13-953f-4951-a957-387465de1d6c.png)

```
yield_by_year(MACD_file(USDX_daily),"MACD","USDX","daily")
```

![MACD USDX_daily2](https://user-images.githubusercontent.com/41892582/202862504-3a379d33-c5b7-44f2-84b2-0860a5b17c87.png)

```
buy_performance(MACD_file(USDX_daily))
```

```
                        Statistics     Long Trades
1                 Total Net Profit 140.54813 price
2                    Profit Factor          1.2418
3           Total Number of Trades             491
4      %Profitable(Winning Trades)           36.9%
5         Average Trade Net Profit   0.28625 price
6                 %Average Revenue           0.17%
7               Avg. Winning Trade   3.98783 price
8                Avg. Losing Trade   1.86897 price
9               Avg. Win: Avg Loss           2.134
10           Largest Winning Trade  19.86781 price
11            Largest Losing Trade   9.93642 Price
12                  Trading Period     51.87 years
13              Time in the Market          49.97%
14 %M D D(intraday peak to valley)         -25.72%
15        portfolio starting price     120.2 price
16           portfolio close price 260.74813 price
17                         %change         116.93%
18                   highest price  272.6282 price
19                    lowest price 113.24092 price
```

**USDX Weekly**

```
investment_price(MACD_file(USDX_weekly),"MACD","USDX","weekly")
```

![MACD USDX_weekly](https://user-images.githubusercontent.com/41892582/202862513-6f5b5a28-f1c5-408b-b3ae-615a400c10d5.png)

```
yield_by_year(MACD_file(USDX_weekly),"MACD","USDX","weekly")
```
![MACD USDX_weekly2](https://user-images.githubusercontent.com/41892582/202862530-ef3f85a5-877f-4222-8e3a-ad60cd93858f.png)

```
buy_performance(MACD_file(USDX_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit 119.22571 price
2                    Profit Factor          1.5884
3           Total Number of Trades              94
4      %Profitable(Winning Trades)           43.6%
5         Average Trade Net Profit   1.26836 price
6                 %Average Revenue           0.92%
7               Avg. Winning Trade   7.85047 price
8                Avg. Losing Trade   3.82346 price
9               Avg. Win: Avg Loss           2.053
10           Largest Winning Trade  37.31657 price
11            Largest Losing Trade  18.18435 Price
12                  Trading Period     51.86 years
13              Time in the Market          51.94%
14 %M D D(intraday peak to valley)          -29.2%
15        portfolio starting price    108.65 price
16           portfolio close price 227.87571 price
17                         %change         109.73%
18                   highest price 233.31162 price
19                    lowest price 106.81732 price
```

**USDX Monthly**

```
investment_price(MACD_file(USDX_monthly),"MACD","USDX","monthly")
```

![MACD USDX_monthly](https://user-images.githubusercontent.com/41892582/202862549-a5236aa7-4a52-4f9a-9d90-53a9fc45ad6c.png)

```
yield_by_year(MACD_file(USDX_monthly),"MACD","USDX","monthly")
```

![MACD USDX_monthly2](https://user-images.githubusercontent.com/41892582/202862556-b76d720c-bf79-4448-aa65-8eca431cc2c7.png)


```
buy_performance(MACD_file(USDX_monthly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  15.01824 price
2                    Profit Factor          1.5029
3           Total Number of Trades              14
4      %Profitable(Winning Trades)           57.1%
5         Average Trade Net Profit   1.07273 price
6                 %Average Revenue           1.36%
7               Avg. Winning Trade   5.61004 price
8                Avg. Losing Trade   4.97702 price
9               Avg. Win: Avg Loss           1.127
10           Largest Winning Trade  15.46544 price
11            Largest Losing Trade   9.55294 Price
12                  Trading Period     37.83 years
13              Time in the Market          50.99%
14 %M D D(intraday peak to valley)         -15.85%
15        portfolio starting price     92.02 price
16           portfolio close price 121.76806 price
17                         %change          32.33%
18                   highest price 127.33291 price
19                    lowest price  82.09795 price
```

# Relative Strength Index (RSI)

According to Wikipedia, the relative strength index (RSI) is a technical indicator used in the analysis of financial markets. It is intended to chart the current and historical strength or weakness of a stock or market based on the closing prices of a recent trading period. 

The RSI is classified as a momentum oscillator, measuring the velocity and magnitude of price movements. Momentum is the rate of the rise or fall in price. The relative strength RS is given as the ratio of higher closes to lower closes, with "closes" here meaning averages of absolute values of price changes. The RSI computes momentum as the ratio of higher closes to overall closes: stocks that have had more or stronger positive changes have a higher RSI than stocks that have had more or stronger negative changes.

The RSI is most typically used on a 14-day timeframe, measured on a scale from 0 to 100, with high and low levels marked at 70 and 30, respectively. Short or longer timeframes are used for alternately shorter or longer outlooks. High and low levels—80 and 20, or 90 and 10—occur less frequently.

![rsi](https://user-images.githubusercontent.com/41892582/202812126-3a223c88-52f4-48ed-99d0-a9eaf977b561.png)

# RSI Formula

This formula provides RSI definition.

$$RSI=100-\frac{100}{100+RS}$$

where RS stands for relative strength. RS is a moving average, which can be either an equally-weighted mean or an exponential moving average. The window for average is typically 14 days, although it can also be longer or shorter. The initial value of RS is derived using an equally weighted mean.

First average gain: you take note of the days when the stock finished higher over the previous two weeks. You figure out the overall gain on each of these days.

First average loss: you take note of the days when the stock closed lower during the previous two weeks. You figure out the overall loss on a daily basis. 

First average gain minus initial average loss equals the first value of RS.

For any other average gain figures, use the formula [(prior average gain *13 + current gain) / 14].

All other average loss figures are calculated as follows: [(prior average loss* 13 + current loss) / 14].

Simply dividing the first value by the second value yields all other values of RS.

The impact of significant price changes is mitigated by the moving average.

We will test RSI on the Dow Jones daily first. Buying was the plan when the RSI crossed above the 30 line, and selling was the plan when the RSI crossed below the 70 line.

```
##RSI
##read the daily Dow Jones data starting in 1970 and ending on November 19, 2022.
RSI_Dow_daily<-Dow_Jones_daily%>%
  select(Date,Open,Close)

##to calculate the daily changes, a new variable is needed
RSI_Dow_daily$change<-NA
for(i in 2:length(RSI_Dow_daily$Close))
{RSI_Dow_daily$change[i]<-RSI_Dow_daily$Close[i]-RSI_Dow_daily$Close[i-1]}

##to calculate the percentage of daily changes
RSI_Dow_daily$change_percent<-NA
for(i in 2:length(RSI_Dow_daily$Close))
{RSI_Dow_daily$change_percent[i]<-RSI_Dow_daily$change[i]/RSI_Dow_daily$Close[i-1]*100}

##the loss variable occurs when the changes is less than zero
RSI_Dow_daily$Loss<-RSI_Dow_daily$change_percent
RSI_Dow_daily$Loss[RSI_Dow_daily$Loss>0]<-0

##When the changes are more than zero, the gain variable happens
RSI_Dow_daily$Gain<-RSI_Dow_daily$change_percent
RSI_Dow_daily$Gain[RSI_Dow_daily$Gain<0]<-0

##variables for the 14-day gain average and the absolute value of the 14-day loss average
RSI_Dow_daily$Gains_Ave<-NA
RSI_Dow_daily$Losses_Ave<-NA

##the first gain and loss average is the simple moving average of the previous 13 days and the price of this day itself
RSI_Dow_daily$Gains_Ave[15]<-mean(RSI_Dow_daily$Gain[2:15])
RSI_Dow_daily$Losses_Ave[15]<--mean(RSI_Dow_daily$Loss[2:15])

for(i in 16:length(RSI_Dow_daily$Close))
{
  RSI_Dow_daily$Gains_Ave[i]<-(RSI_Dow_daily$Gains_Ave[i-1]*13+RSI_Dow_daily$Gain[i])/14
  RSI_Dow_daily$Losses_Ave[i]<-(RSI_Dow_daily$Losses_Ave[i-1]*13+(-RSI_Dow_daily$Loss[i]))/14
}

##calculating the RS
RSI_Dow_daily$RS<-RSI_Dow_daily$Gains_Ave/RSI_Dow_daily$Losses_Ave
##calculating the RSI
RSI_Dow_daily$RSI<-100-(100/(1+RSI_Dow_daily$RS))

##the Action variable
RSI_Dow_daily$Action<-0

##Calculating the action to take, whether to buy or sell, according to the previous strategy
for(i in 16:length(RSI_Dow_daily$Close))
{
  RSI_Dow_daily$Action[i]<-case_when(
    RSI_Dow_daily$RSI[i-1]<30&RSI_Dow_daily$RSI[i]>30~"Buy",
    RSI_Dow_daily$RSI[i-1]>70&RSI_Dow_daily$RSI[i]<70~"Sell")
}

##fill the NAs cells in the action variable with zeros to be able to determine the hold and wait periods
RSI_Dow_daily$Action[is.na(RSI_Dow_daily$Action)]<-0

##determine the hold and wait periods after correcting the consecutive buying and selling signals
##due to the RSI crossing over the 30 line consecutively before crossing the 70 line and vice versa

##Hold
for(i in 16:length(RSI_Dow_daily$Close))
{
  if (RSI_Dow_daily$Action[i]=="Buy")
  {
    for (j in i+1:length(RSI_Dow_daily$Close))
    {
      if (RSI_Dow_daily$Action[j]!="Sell"&!is.na(RSI_Dow_daily$Action[j]))
      {
        RSI_Dow_daily$Action[j]<-"Hold"
      }
      else break
    }
  }
}

##Wait
for(i in 16:length(RSI_Dow_daily$Close))
{
  if (RSI_Dow_daily$Action[i]=="Sell")
  {
    for (j in i+1:length(RSI_Dow_daily$Close))
    {
      if (RSI_Dow_daily$Action[j]!="Buy"&!is.na(RSI_Dow_daily$Action[j]))
      {
        RSI_Dow_daily$Action[j]<-"Wait"
      }
      else break
    }
  }
}

##calculating the investment price over the time
RSI_Dow_daily$investment_price<-NA
RSI_Dow_daily$investment_price[match("Buy",RSI_Dow_daily$Action)]<-RSI_Dow_daily$Close[match("Buy",RSI_Dow_daily$Action)]

for (m in (match("Buy",RSI_Dow_daily$Action)+1):length(RSI_Dow_daily$Close))
{
  RSI_Dow_daily$investment_price[m]<-case_when(
    RSI_Dow_daily$Action[m]=="Buy"~RSI_Dow_daily$investment_price[m-1],
    RSI_Dow_daily$Action[m]=="Wait"~RSI_Dow_daily$investment_price[m-1],
    RSI_Dow_daily$Action[m]=="Hold"~(RSI_Dow_daily$investment_price[m-1]/RSI_Dow_daily$Close[m-1])*RSI_Dow_daily$Close[m],
    RSI_Dow_daily$Action[m]=="Sell"~(RSI_Dow_daily$investment_price[m-1]/RSI_Dow_daily$Close[m-1])*RSI_Dow_daily$Close[m]
  )
}
```

Now that the data has been prepared, we will compare the RSI performance with Dow Jones using the previous two functions.

```
investment_price(RSI_Dow_daily,"RSI","Dow_Jonse","daily")
```

![rsi dow daily](https://user-images.githubusercontent.com/41892582/202873359-14f24776-7121-4e94-98a9-a34b580f79d2.jpg)

```
yield_by_year(RSI_Dow_daily,"RSI","Dow_Jonse","daily")
```

![rsi dow daily2](https://user-images.githubusercontent.com/41892582/202873365-a84066d3-7c1d-4a7b-be16-17c6bbdc6e13.jpg)


```
buy_performance(RSI_Dow_daily)
```

```
                        Statistics      Long Trades
1                 Total Net Profit 4161.21375 price
2                    Profit Factor           3.8976
3           Total Number of Trades               50
4      %Profitable(Winning Trades)              82%
5         Average Trade Net Profit   83.22427 price
6                 %Average Revenue            4.28%
7               Avg. Winning Trade  136.51997 price
8                Avg. Losing Trade  159.56722 price
9               Avg. Win: Avg Loss            0.856
10           Largest Winning Trade  458.19858 price
11            Largest Losing Trade  856.33358 Price
12                  Trading Period      52.88 years
13              Time in the Market           44.03%
14 %M D D(intraday peak to valley)          -52.31%
15        portfolio starting price      757.5 price
16           portfolio close price 4918.71375 price
17                         %change          549.34%
18                   highest price 4938.07577 price
19                    lowest price  481.26404 price
```


for the first look at the RSI performance in comparison with the Dow Jones, it seems RSI has nothing to do with trading the sharp uptrend, but by looking deep at his performance the RSI signals seems to be Irresistible with 82 Profitable percentage with average revenue 4.28% per trade, its faults the long time it's spend in the market to achieve that with high MDD.
and also the time spent waiting for the RSI to pass below the 30 line before reentering the market when RSI crossed over it again.

In a later section of this collection, we will explore a reentry method that enables us to enter the market again before waiting for the RSI to drop below the 30 line in an effort to enhance the RSI's performance, particularly in an upswing like the Dow Jones.


We will now use the previously created code to construct a function named "RSI_file" that will be used to prepare the data in the files for plotting using the "investment_price" function according to the same RSI strategy.

**RSI function**

```
RSI_file<-function(file_name)
{
  
RSI_file_name<-select(file_name,Date,Open,Close)
RSI_file_name$Date<-as.Date(RSI_file_name$Date)


RSI_file_name$change<-NA
for(i in 2:length(RSI_file_name$Close))
{RSI_file_name$change[i]<-RSI_file_name$Close[i]-RSI_file_name$Close[i-1]}

RSI_file_name$change_percent<-NA
for(i in 2:length(RSI_file_name$Close))
{RSI_file_name$change_percent[i]<-RSI_file_name$change[i]/RSI_file_name$Close[i-1]*100}

RSI_file_name$Loss<-RSI_file_name$change_percent
RSI_file_name$Loss[RSI_file_name$Loss>0]<-0

RSI_file_name$Gain<-RSI_file_name$change_percent
RSI_file_name$Gain[RSI_file_name$Gain<0]<-0

RSI_file_name$Gains_Ave<-NA
RSI_file_name$Losses_Ave<-NA

RSI_file_name$Gains_Ave[16]<-mean(RSI_file_name$Gain[2:15])
RSI_file_name$Losses_Ave[16]<--mean(RSI_file_name$Loss[2:15])

for(i in 17:length(RSI_file_name$Close))
{
  RSI_file_name$Gains_Ave[i]<-(RSI_file_name$Gains_Ave[i-1]*13+RSI_file_name$Gain[i])/14
  RSI_file_name$Losses_Ave[i]<-(RSI_file_name$Losses_Ave[i-1]*13+(-RSI_file_name$Loss[i]))/14
}

RSI_file_name$RS<-RSI_file_name$Gains_Ave/RSI_file_name$Losses_Ave
RSI_file_name$RSI<-100-(100/(1+RSI_file_name$RS))

RSI_file_name$Action<-0

for(i in 17:length(RSI_file_name$Close))
{
  RSI_file_name$Action[i]<-case_when(
    RSI_file_name$RSI[i-1]<30&RSI_file_name$RSI[i]>30~"Buy",
    RSI_file_name$RSI[i-1]>70&RSI_file_name$RSI[i]<70~"Sell")
}

RSI_file_name$Action[is.na(RSI_file_name$Action)]<-0
for(i in 16:length(RSI_file_name$Close))
{
  if (RSI_file_name$Action[i]=="Buy")
  {
    for (j in i+1:length(RSI_file_name$Close))
    {
      if (RSI_file_name$Action[j]!="Sell"&!is.na(RSI_file_name$Action[j]))
      {
        RSI_file_name$Action[j]<-"Hold"
      }
      else break
    }
  }
}
##Wait

for(i in 16:length(RSI_file_name$Close))
{
  if (RSI_file_name$Action[i]=="Sell")
  {
    for (j in i+1:length(RSI_file_name$Close))
    {
      if (RSI_file_name$Action[j]!="Buy"&!is.na(RSI_file_name$Action[j]))
      {
        RSI_file_name$Action[j]<-"Wait"
      }
      else break
    }
  }
}

##investment price
RSI_file_name$investment_price<-NA
RSI_file_name$investment_price[match("Buy",RSI_file_name$Action)]<-RSI_file_name$Close[match("Buy",RSI_file_name$Action)]

for (m in ((match("Buy",RSI_file_name$Action))+1):length(RSI_file_name$Close))
{
  RSI_file_name$investment_price[m]<-case_when(
    RSI_file_name$Action[m]=="Buy"~RSI_file_name$investment_price[m-1],
    RSI_file_name$Action[m]=="Wait"~RSI_file_name$investment_price[m-1],
    RSI_file_name$Action[m]=="Hold"~(RSI_file_name$investment_price[m-1]/RSI_file_name$Close[m-1])*RSI_file_name$Close[m],
    RSI_file_name$Action[m]=="Sell"~(RSI_file_name$investment_price[m-1]/RSI_file_name$Close[m-1])*RSI_file_name$Close[m]
  )
}
return(RSI_file_name)
}
```


**DOW Jones Weekly**
  
```
investment_price(RSI_file(Dow_Jones_weekly),"RSI","Dow_Jones","weekly")
```

![RSI Dow_Jones_weekly](https://user-images.githubusercontent.com/41892582/202877094-d0d4fc23-728a-4d17-aa51-d28b32611c48.png)

```
yield_by_year(RSI_file(Dow_Jones_weekly),"RSI","Dow_Jones","weekly")
```

![RSI Dow_Jones_weekly2](https://user-images.githubusercontent.com/41892582/202877101-84eefb43-c29e-4386-adf9-c9141c3ae6b3.png)

```
buy_performance(RSI_file(Dow_Jones_weekly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 2651.34648 price
2                    Profit Factor              Inf
3           Total Number of Trades                8
4      %Profitable(Winning Trades)             100%
5         Average Trade Net Profit  331.41831 price
6                 %Average Revenue           22.34%
7               Avg. Winning Trade  331.41831 price
8                Avg. Losing Trade        NaN price
9               Avg. Win: Avg Loss              NaN
10           Largest Winning Trade 1242.48213 price
11            Largest Losing Trade        Inf Price
12                  Trading Period      52.88 years
13              Time in the Market           22.83%
14 %M D D(intraday peak to valley)          -31.53%
15        portfolio starting price      700.4 price
16           portfolio close price  3405.5079 price
17                         %change          386.22%
18                   highest price 3407.05798 price
19                    lowest price      684.2 price
```

**DOW Jones monthly**
  
  ```
investment_price(RSI_file(Dow_Jones_monthly),"RSI","Dow_Jones","monthly")
```

![RSI Dow_Jones_monthly](https://user-images.githubusercontent.com/41892582/202877136-c7ec3e9c-477c-44c3-b925-b6a4310f0ab5.png)

```
yield_by_year(RSI_file(Dow_Jones_monthly),"RSI","Dow_Jones","monthly")
```

![RSI Dow_Jones_monthly2](https://user-images.githubusercontent.com/41892582/202877193-66e7d4a8-facd-44c3-9262-364ad80c59bd.png)


```
buy_performance(RSI_file(Dow_Jones_monthly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 1547.67321 price
2                    Profit Factor              Inf
3           Total Number of Trades                2
4      %Profitable(Winning Trades)             100%
5         Average Trade Net Profit  773.83661 price
6                 %Average Revenue           82.36%
7               Avg. Winning Trade  773.83661 price
8                Avg. Losing Trade        NaN price
9               Avg. Win: Avg Loss              NaN
10           Largest Winning Trade  992.57321 price
11            Largest Losing Trade        Inf Price
12                  Trading Period      52.83 years
13              Time in the Market           25.83%
14 %M D D(intraday peak to valley)          -26.14%
15        portfolio starting price      665.5 price
16           portfolio close price 2213.17321 price
17                         %change          232.56%
18                   highest price 2316.16804 price
19                    lowest price      616.2 price
```

On a weekly and monthly basis, the RSI buy signal appears to be unstoppable, with both being 100% profitable and having average trade profits of 22 and 82 percent, respectively.

Also, after the RSI fell below the 70 level and we sold our position, it could be years before it falls below the 30 level and allows us to reenter the market, and this could be the case even if there is an uptrend. 

**Gold**
  
**Gold daily**
  
```
investment_price(RSI_file(Gold_daily),"RSI","Gold","daily")
```

![RSI Gold_daily](https://user-images.githubusercontent.com/41892582/202877247-bae99859-c454-45b9-83a5-4edd41818b93.png)

```
yield_by_year(RSI_file(Gold_daily),"RSI","Gold","daily")
```

![RSI Gold_daily2](https://user-images.githubusercontent.com/41892582/202877259-a1d6ea2e-e7a7-4fd6-a5da-dbdd22cff4cf.png)

```
buy_performance(RSI_file(Gold_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit 38.13791 price
2                    Profit Factor         1.6852
3           Total Number of Trades             46
4      %Profitable(Winning Trades)          71.7%
5         Average Trade Net Profit  0.82908 price
6                 %Average Revenue           2.3%
7               Avg. Winning Trade  2.84234 price
8                Avg. Losing Trade  4.28148 price
9               Avg. Win: Avg Loss          0.664
10           Largest Winning Trade  9.81843 price
11            Largest Losing Trade 20.04539 Price
12                  Trading Period    52.87 years
13              Time in the Market         43.24%
14 %M D D(intraday peak to valley)         -66.9%
15        portfolio starting price     35.7 price
16           portfolio close price 73.83791 price
17                         %change        106.83%
18                   highest price 77.92053 price
19                    lowest price 17.08665 price
```

**Gold weekly**
  
  ```
investment_price(RSI_file(Gold_weekly),"RSI","Gold","weekly")
```

![RSI Gold_weekly](https://user-images.githubusercontent.com/41892582/202877263-87c12975-7939-4f78-a303-a4296e2ec5c5.png)

```
yield_by_year(RSI_file(Gold_weekly),"RSI","Gold","weekly")
```

![RSI Gold_weekly2](https://user-images.githubusercontent.com/41892582/202877266-bb4a4278-7ded-43ff-89ef-4eb8a1a6c77f.png)

```
buy_performance(RSI_file(Gold_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit -20.62418 price
2                    Profit Factor          0.6567
3           Total Number of Trades               8
4      %Profitable(Winning Trades)           37.5%
5         Average Trade Net Profit  -2.57802 price
6                 %Average Revenue          -1.46%
7               Avg. Winning Trade   13.1518 price
8                Avg. Losing Trade  12.01592 price
9               Avg. Win: Avg Loss           1.095
10           Largest Winning Trade      18.4 price
11            Largest Losing Trade  20.58838 Price
12                  Trading Period     52.88 years
13              Time in the Market          25.64%
14 %M D D(intraday peak to valley)         -50.06%
15        portfolio starting price     143.6 price
16           portfolio close price 122.97582 price
17                         %change         -14.36%
18                   highest price 179.90788 price
19                    lowest price  89.84765 price
```

**Gold monthly**
  
```
investment_price(RSI_file(Gold_monthly),"RSI","Gold","monthly")
```

![RSI Gold_monthly](https://user-images.githubusercontent.com/41892582/202877272-117ee249-18b2-406e-aa2c-0fa8cf240fa2.png)

The RSI curve is practically flat since it kept us out of the market for the majority of the period

```
yield_by_year(RSI_file(Gold_monthly),"RSI","Gold","monthly")
```

![RSI Gold_monthly2](https://user-images.githubusercontent.com/41892582/202877290-26b8807c-c5b3-4458-a452-c5ff2a882ef7.png)


```
buy_performance(RSI_file(Gold_monthly))
```

```
                        Statistics  Long Trades
1                 Total Net Profit  -15.6 price
2                    Profit Factor            0
3           Total Number of Trades            1
4      %Profitable(Winning Trades)           0%
5         Average Trade Net Profit  -15.6 price
6                 %Average Revenue       -4.29%
7               Avg. Winning Trade    NaN price
8                Avg. Losing Trade   15.6 price
9               Avg. Win: Avg Loss          NaN
10           Largest Winning Trade   -Inf price
11            Largest Losing Trade   15.6 Price
12                  Trading Period  52.83 years
13              Time in the Market        11.5%
14 %M D D(intraday peak to valley)      -29.59%
15        portfolio starting price 363.25 price
16           portfolio close price 347.65 price
17                         %change       -4.29%
18                   highest price  367.5 price
19                    lowest price 255.75 price
```

Daily gold trading can be profitable, but the net gain is little compared to the time invested in the market and the asset's price increase. It's also seems that not just the upswing should be considered when interpreting RSI indications.

it's frustrated when it loses in monthly and weekly time frame with price explode that happened in the asset itself


**EUR/USD Rate**
  
**EUR/USD daily**
  
```
investment_price(RSI_file(EurUsd_daily),"RSI","EurUsd","daily")
```

![RSI EurUsd_daily](https://user-images.githubusercontent.com/41892582/202877306-372f2cd5-e4b6-4399-9a0f-cb50888f5230.png)

```
yield_by_year(RSI_file(EurUsd_daily),"RSI","EurUsd","daily")
```

![RSI EurUsd_daily2](https://user-images.githubusercontent.com/41892582/202877312-c65695b7-3b03-49b9-b5cc-5bab0be3a38d.png)

```
buy_performance(RSI_file(EurUsd_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -0.26677 price
2                    Profit Factor         0.5731
3           Total Number of Trades             41
4      %Profitable(Winning Trades)          53.7%
5         Average Trade Net Profit -0.00651 price
6                 %Average Revenue         -1.39%
7               Avg. Winning Trade  0.01628 price
8                Avg. Losing Trade  0.03289 price
9               Avg. Win: Avg Loss          0.495
10           Largest Winning Trade  0.04394 price
11            Largest Losing Trade  0.13264 Price
12                  Trading Period    51.87 years
13              Time in the Market         43.82%
14 %M D D(intraday peak to valley)        -64.72%
15        portfolio starting price   0.5358 price
16           portfolio close price  0.23335 price
17                         %change        -56.45%
18                   highest price  0.61483 price
19                    lowest price  0.21689 price
```

**EUR/USD weekly**
  
  ```
investment_price(RSI_file(EurUsd_weekly),"RSI","EurUsd","weekly")
```

![RSI EurUsd_weekly](https://user-images.githubusercontent.com/41892582/202877318-fca753b0-f0cc-4b96-9c15-7f02d95e622e.png)

```
yield_by_year(RSI_file(EurUsd_weekly),"RSI","EurUsd","weekly")
```

![RSI EurUsd_weekly2](https://user-images.githubusercontent.com/41892582/202877324-a6ba4819-55f7-4380-8856-55279c813133.png)

```
buy_performance(RSI_file(EurUsd_weekly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 0.08711 price
2                    Profit Factor        1.2131
3           Total Number of Trades            11
4      %Profitable(Winning Trades)         72.7%
5         Average Trade Net Profit 0.00792 price
6                 %Average Revenue         2.11%
7               Avg. Winning Trade 0.06197 price
8                Avg. Losing Trade 0.13623 price
9               Avg. Win: Avg Loss         0.455
10           Largest Winning Trade 0.11504 price
11            Largest Losing Trade 0.28568 Price
12                  Trading Period   51.86 years
13              Time in the Market        40.16%
14 %M D D(intraday peak to valley)       -49.09%
15        portfolio starting price  0.7042 price
16           portfolio close price 0.72142 price
17                         %change         2.44%
18                   highest price 0.92758 price
19                    lowest price 0.47222 price
```

**EUR/USD monthly**
  
  ```
investment_price(RSI_file(EurUsd_monthly),"RSI","EurUsd","monthly")
```

![RSI EurUsd_monthly](https://user-images.githubusercontent.com/41892582/202877334-e7909c66-468b-4040-b71f-051e66918254.png)

```
yield_by_year(RSI_file(EurUsd_monthly),"RSI","EurUsd","monthly")
```

![RSI EurUsd_monthly2](https://user-images.githubusercontent.com/41892582/202877337-1aa90d3c-38f2-459f-899c-952eda42372f.png)

the majority of the time still off the market, and it increases in larger time frames.

```
buy_performance(RSI_file(EurUsd_monthly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 0.02008 price
2                    Profit Factor        3.7508
3           Total Number of Trades             2
4      %Profitable(Winning Trades)           50%
5         Average Trade Net Profit 0.01004 price
6                 %Average Revenue         1.21%
7               Avg. Winning Trade 0.02738 price
8                Avg. Losing Trade  0.0073 price
9               Avg. Win: Avg Loss         3.751
10           Largest Winning Trade 0.02738 price
11            Largest Losing Trade  0.0073 Price
12                  Trading Period   51.83 years
13              Time in the Market        34.83%
14 %M D D(intraday peak to valley)       -33.79%
15        portfolio starting price  0.8389 price
16           portfolio close price  0.7893 price
17                         %change        -5.91%
18                   highest price 0.95078 price
19                    lowest price  0.5832 price
```

**10-Year U.S. Bond Yield (10USY.B)**
  
**10USY.B daily**
  
```
investment_price(RSI_file(US_10YB_daily),"RSI","US_10YB","daily")
```

![RSI US_10YB_daily](https://user-images.githubusercontent.com/41892582/202877349-95b58595-e3cd-4aa4-a7ca-ba4fd8354d16.png)

```
yield_by_year(RSI_file(US_10YB_daily),"RSI","US_10YB","daily")
```

![RSI US_10YB_daily2](https://user-images.githubusercontent.com/41892582/202877355-a1c00a19-7eb9-4ad7-8591-1a05ea4cb914.png)

```
buy_performance(RSI_file(US_10YB_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -6.24077 price
2                    Profit Factor         0.5004
3           Total Number of Trades             53
4      %Profitable(Winning Trades)          50.9%
5         Average Trade Net Profit -0.11775 price
6                 %Average Revenue         -2.42%
7               Avg. Winning Trade  0.23152 price
8                Avg. Losing Trade  0.48045 price
9               Avg. Win: Avg Loss          0.482
10           Largest Winning Trade     0.79 price
11            Largest Losing Trade  1.72299 Price
12                  Trading Period    52.88 years
13              Time in the Market         53.82%
14 %M D D(intraday peak to valley)        -95.37%
15        portfolio starting price     6.99 price
16           portfolio close price  0.74923 price
17                         %change        -89.28%
18                   highest price  8.12039 price
19                    lowest price  0.37619 price
```

**10USY.B weekly**
  
```
investment_price(RSI_file(US_10YB_weekly),"RSI","US_10YB","weekly")
```

![RSI US_10YB_weekly](https://user-images.githubusercontent.com/41892582/202877364-faa4cd10-bb7d-4142-b6f5-9a7f4aa9589e.png)

```
yield_by_year(RSI_file(US_10YB_weekly),"RSI","US_10YB","weekly")
```

![RSI US_10YB_weekly2](https://user-images.githubusercontent.com/41892582/202877367-35bb97db-4e7e-43c0-abea-47fbac14e816.png)

```
buy_performance(RSI_file(US_10YB_weekly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit  -3.0895 price
2                    Profit Factor         0.5213
3           Total Number of Trades             12
4      %Profitable(Winning Trades)            50%
5         Average Trade Net Profit -0.25746 price
6                 %Average Revenue         -3.31%
7               Avg. Winning Trade  0.56069 price
8                Avg. Losing Trade  1.07561 price
9               Avg. Win: Avg Loss          0.521
10           Largest Winning Trade  0.96924 price
11            Largest Losing Trade  2.42409 Price
12                  Trading Period    52.88 years
13              Time in the Market          54.2%
14 %M D D(intraday peak to valley)        -85.44%
15        portfolio starting price     6.53 price
16           portfolio close price   3.4405 price
17                         %change        -47.31%
18                   highest price  8.06772 price
19                    lowest price  1.17452 price
```

**10USY.B monthly**
  
 ```
investment_price(RSI_file(US_10YB_monthly),"RSI","US_10YB","monthly")
```

![RSI US_10YB_monthly](https://user-images.githubusercontent.com/41892582/202877377-f16c35ea-c7c5-4651-9cbe-37f271881592.png)

```
yield_by_year(RSI_file(US_10YB_monthly),"RSI","US_10YB","monthly")
```

![RSI US_10YB_monthly2](https://user-images.githubusercontent.com/41892582/202877379-7a1af8d2-fe34-4a3f-b9c5-25b92861195d.png)

Because it could keep us trapped in the market before the RSI goes over the 70 level and sends us a sell signal, this method didn't work with the downtrend either.

```
buy_performance(RSI_file(US_10YB_monthly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit -3.7864 price
2                    Profit Factor        0.3958
3           Total Number of Trades             2
4      %Profitable(Winning Trades)           50%
5         Average Trade Net Profit -1.8932 price
6                 %Average Revenue       -15.93%
7               Avg. Winning Trade    2.48 price
8                Avg. Losing Trade  6.2664 price
9               Avg. Win: Avg Loss         0.396
10           Largest Winning Trade    2.48 price
11            Largest Losing Trade  6.2664 Price
12                  Trading Period   52.83 years
13              Time in the Market        83.15%
14 %M D D(intraday peak to valley)       -94.47%
15        portfolio starting price    6.38 price
16           portfolio close price  2.5936 price
17                         %change       -59.35%
18                   highest price 9.39667 price
19                    lowest price 0.52009 price
```

**US Dollar/USDX - Index - Cash (DX-Y.NYB)**
  
**USDX Daily**
  
```
investment_price(RSI_file(USDX_daily),"RSI","USDX","daily")
```

![RSI USDX_daily](https://user-images.githubusercontent.com/41892582/202877386-a76bf402-2967-49b3-95ea-c70ae32e5544.png)

```
yield_by_year(RSI_file(USDX_daily),"RSI","USDX","daily")
```

![RSI USDX_daily2](https://user-images.githubusercontent.com/41892582/202877392-b3bad157-aa76-4388-be48-f1db8c28cfcf.png)

```
buy_performance(RSI_file(USDX_daily))
```

```
                        Statistics     Long Trades
1                 Total Net Profit -67.53519 price
2                    Profit Factor          0.4164
3           Total Number of Trades              45
4      %Profitable(Winning Trades)           55.6%
5         Average Trade Net Profit  -1.50078 price
6                 %Average Revenue          -1.56%
7               Avg. Winning Trade   1.92751 price
8                Avg. Losing Trade   5.78615 price
9               Avg. Win: Avg Loss           0.333
10           Largest Winning Trade   5.72798 price
11            Largest Losing Trade  31.76674 Price
12                  Trading Period     51.87 years
13              Time in the Market          53.85%
14 %M D D(intraday peak to valley)          -61.1%
15        portfolio starting price    120.28 price
16           portfolio close price  52.74481 price
17                         %change         -56.15%
18                   highest price  120.4208 price
19                    lowest price  46.84244 price
```

**USDX Weekly**
  
```
investment_price(RSI_file(USDX_weekly),"RSI","USDX","weekly")
```

![RSI USDX_weekly](https://user-images.githubusercontent.com/41892582/202877402-355b590a-2312-4915-9340-7293abe8597c.png)

```
yield_by_year(RSI_file(USDX_weekly),"RSI","USDX","weekly")
```

![RSI USDX_weekly2](https://user-images.githubusercontent.com/41892582/202877414-e6aa511c-7aca-4fdb-9a84-65faad1760d8.png)

```
buy_performance(RSI_file(USDX_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  -3.98052 price
2                    Profit Factor          0.9509
3           Total Number of Trades              12
4      %Profitable(Winning Trades)           58.3%
5         Average Trade Net Profit  -0.33171 price
6                 %Average Revenue           0.69%
7               Avg. Winning Trade  11.01189 price
8                Avg. Losing Trade  16.21274 price
9               Avg. Win: Avg Loss           0.679
10           Largest Winning Trade  15.32363 price
11            Largest Losing Trade  29.41207 Price
12                  Trading Period     51.86 years
13              Time in the Market          56.89%
14 %M D D(intraday peak to valley)         -45.53%
15        portfolio starting price    119.43 price
16           portfolio close price 115.44948 price
17                         %change          -3.33%
18                   highest price 138.19469 price
19                    lowest price  75.27805 price
```

**USDX Monthly**
  
```
investment_price(RSI_file(USDX_monthly),"RSI","USDX","monthly")
```

![RSI USDX_monthly](https://user-images.githubusercontent.com/41892582/202877430-d2e32e6d-3eeb-4d9d-bebd-a25a25524a61.png)

```
yield_by_year(RSI_file(USDX_monthly),"RSI","USDX","monthly")
```

![RSI USDX_monthly2](https://user-images.githubusercontent.com/41892582/202877435-d2332120-e798-431d-8848-d23341be03b5.png)

```
buy_performance(RSI_file(USDX_monthly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -5.09053 price
2                    Profit Factor              0
3           Total Number of Trades              2
4      %Profitable(Winning Trades)             0%
5         Average Trade Net Profit -2.54526 price
6                 %Average Revenue         -2.61%
7               Avg. Winning Trade      NaN price
8                Avg. Losing Trade  2.54526 price
9               Avg. Win: Avg Loss            NaN
10           Largest Winning Trade     -Inf price
11            Largest Losing Trade     3.72 Price
12                  Trading Period    37.83 years
13              Time in the Market         56.92%
14 %M D D(intraday peak to valley)        -31.75%
15        portfolio starting price    98.57 price
16           portfolio close price 93.47947 price
17                         %change         -5.16%
18                   highest price   102.98 price
19                    lowest price 70.28827 price
```

In an uptrending market, maybe it remains flat and out of the market; if it gave us a buy signal, it didn't last long before giving us a sell signal, letting us out of the market; and in a downtrending market, it keeps us in the market for a long time before giving us a sell signal. This strategy by itself is frustrating.

Of course, the RSI indicator has various uses and more complex strategies, some of which overlap with those of other indicators and other types of analysis like chart analysis.

We will go more deeply into the RSI indicator later on in this process, but for now, let's try to adjust our RSI strategy so that we can re-enter an uptrending market before we wait for the 30 line to cross and a downtrending market before we wait for the 70 line to cross.

Our new strategy will be creating the 3-period moving average of the RSI. The buy signal will be created when the RSI crosses over the 30 line or when it crosses its moving average at any area below the 70 line. The sell signal will be created when the RSI crosses below the 70 line area or when the RSI crosses below its moving average at any area above the 30 line.

On the daily Dow Jones, we will test the new strategy first.

```
##RSI Modify
##Dow Jones daily
RSI2_Dow_daily<-select(Dow_Jones_daily,-High,-Low,-Volume)

RSI2_Dow_daily$change<-NA
for(i in 2:length(RSI2_Dow_daily$Close))
{RSI2_Dow_daily$change[i]<-RSI2_Dow_daily$Close[i]-RSI2_Dow_daily$Close[i-1]}

RSI2_Dow_daily$change_percent<-NA
for(i in 2:length(RSI2_Dow_daily$Close))
{RSI2_Dow_daily$change_percent[i]<-RSI2_Dow_daily$change[i]/RSI2_Dow_daily$Close[i-1]*100}

RSI2_Dow_daily$Loss<-RSI2_Dow_daily$change_percent
RSI2_Dow_daily$Loss[RSI2_Dow_daily$Loss>0]<-0

RSI2_Dow_daily$Gain<-RSI2_Dow_daily$change_percent
RSI2_Dow_daily$Gain[RSI2_Dow_daily$Gain<0]<-0

RSI2_Dow_daily$Gains_Ave<-NA
RSI2_Dow_daily$Losses_Ave<-NA

RSI2_Dow_daily$Gains_Ave[16]<-mean(RSI2_Dow_daily$Gain[2:15])
RSI2_Dow_daily$Losses_Ave[16]<--mean(RSI2_Dow_daily$Loss[2:15])

for(i in 17:length(RSI2_Dow_daily$Close))
{
  RSI2_Dow_daily$Gains_Ave[i]<-(RSI2_Dow_daily$Gains_Ave[i-1]*13+RSI2_Dow_daily$Gain[i])/14
  RSI2_Dow_daily$Losses_Ave[i]<-(RSI2_Dow_daily$Losses_Ave[i-1]*13+(-RSI2_Dow_daily$Loss[i]))/14
}

RSI2_Dow_daily$RS<-RSI2_Dow_daily$Gains_Ave/RSI2_Dow_daily$Losses_Ave
RSI2_Dow_daily$RSI<-100-(100/(1+RSI2_Dow_daily$RS))

##the 3-day moving average of the RSI
RSI2_Dow_daily$S<-NA
for(i in 18:length(RSI2_Dow_daily$Close))
{
  RSI2_Dow_daily$S[i]<-mean(RSI2_Dow_daily$RSI[(i-2):i])
}

RSI2_Dow_daily$Action<-0

for(i in 17:length(RSI2_Dow_daily$Close))
{
  RSI2_Dow_daily$Action[i]<-case_when(
    RSI2_Dow_daily$RSI[i-1]<30&RSI2_Dow_daily$RSI[i]>30~"Buy",
    RSI2_Dow_daily$RSI[i-1]>70&RSI2_Dow_daily$RSI[i]<70~"Sell")
}


RSI2_Dow_daily$Action[is.na(RSI2_Dow_daily$Action)]<-0

for(i in 19:length(RSI2_Dow_daily$Close))
{
  if (RSI2_Dow_daily$RSI[i]<70&RSI2_Dow_daily$RSI[i]>RSI2_Dow_daily$S[i]& RSI2_Dow_daily$RSI[i-1]<RSI2_Dow_daily$S[i-1])
  {
    RSI2_Dow_daily$Action[i]<-"Buy"
  }
}

for(i in 19:length(RSI2_Dow_daily$Close))
{
  if (RSI2_Dow_daily$RSI[i]>30&RSI2_Dow_daily$RSI[i]<RSI2_Dow_daily$S[i]& RSI2_Dow_daily$RSI[i-1]>RSI2_Dow_daily$S[i-1])
  {
    RSI2_Dow_daily$Action[i]<-"Sell"
  }
}


##Hold
RSI2_Dow_daily$Action[is.na(RSI2_Dow_daily$Action)]<-0
for(i in 16:length(RSI2_Dow_daily$Close))
{
  if (RSI2_Dow_daily$Action[i]=="Buy")
  {
    for (j in i+1:length(RSI2_Dow_daily$Close))
    {
      if (RSI2_Dow_daily$Action[j]!="Sell"&!is.na(RSI2_Dow_daily$Action[j]))
      {
        RSI2_Dow_daily$Action[j]<-"Hold"
      }
      else break
    }
  }
}
##Wait

for(i in 16:length(RSI2_Dow_daily$Close))
{
  if (RSI2_Dow_daily$Action[i]=="Sell")
  {
    for (j in i+1:length(RSI2_Dow_daily$Close))
    {
      if (RSI2_Dow_daily$Action[j]!="Buy"&!is.na(RSI2_Dow_daily$Action[j]))
      {
        RSI2_Dow_daily$Action[j]<-"Wait"
      }
      else break
    }
  }
}

##investment price
RSI2_Dow_daily$investment_price<-NA
RSI2_Dow_daily$investment_price[match("Buy",RSI2_Dow_daily$Action)]<-RSI2_Dow_daily$Close[match("Buy",RSI2_Dow_daily$Action)]

for (m in (match("Buy",RSI2_Dow_daily$Action)+1):length(RSI2_Dow_daily$Close))
{
  RSI2_Dow_daily$investment_price[m]<-case_when(
    RSI2_Dow_daily$Action[m]=="Buy"~RSI2_Dow_daily$investment_price[m-1],
    RSI2_Dow_daily$Action[m]=="Wait"~RSI2_Dow_daily$investment_price[m-1],
    RSI2_Dow_daily$Action[m]=="Hold"~(RSI2_Dow_daily$investment_price[m-1]/RSI2_Dow_daily$Close[m-1])*RSI2_Dow_daily$Close[m],
    RSI2_Dow_daily$Action[m]=="Sell"~(RSI2_Dow_daily$investment_price[m-1]/RSI2_Dow_daily$Close[m-1])*RSI2_Dow_daily$Close[m]
  )
}
```

**DOW Jones daily**

```
investment_price(RSI2_Dow_daily," Rsi 2nd strategy","Dow_Jones","daily")
```

![rsi2nd](https://user-images.githubusercontent.com/41892582/202907323-6c3b7be8-9d05-414c-aef4-fca4b364ee34.png)

surprisingly the RSI rises up the Dow jones

```
yield_by_year(RSI2_Dow_daily,"Rsi_2nd_strategy","Dow_Jones","daily")
```

![rsi2nd2](https://user-images.githubusercontent.com/41892582/202907345-12aa8aa8-d648-4639-ab0b-2ef5fe676336.png)

```
buy_performance(RSI2_Dow_daily)
```

```
                        Statistics       Long Trades
1                 Total Net Profit 36913.36475 price
2                    Profit Factor            1.1275
3           Total Number of Trades              2182
4      %Profitable(Winning Trades)             44.2%
5         Average Trade Net Profit    16.91722 price
6                 %Average Revenue              0.2%
7               Avg. Winning Trade   338.23067 price
8                Avg. Losing Trade   236.88972 price
9               Avg. Win: Avg Loss             1.428
10           Largest Winning Trade  4679.07346 price
11            Largest Losing Trade  4458.62716 Price
12                  Trading Period       52.88 years
13              Time in the Market             49.3%
14 %M D D(intraday peak to valley)           -69.62%
15        portfolio starting price       746.4 price
16           portfolio close price 37659.76475 price
17                         %change          4945.52%
18                   highest price 63009.58368 price
19                    lowest price   743.93212 price
```

The average revenue percent per trade is still quite low (0.2%), with a high MDD (-69.62%) and a high number of trades, even though the overall net profit is enormous. 

We will now use the previously created code to construct a function named "RSI2_file" that will be used to prepare the data in the files for plotting using the "investment_price" and "yield_by_year" functions according to the RSI strategy that introduced before.

**RSI2 function**

```
RSI2_file<-function(file_name,Date,Open,Close)
{
RSI_file_name<-select(file_name,Date,Open,Close)
RSI_file_name$Date<-as.Date(RSI_file_name$Date)

RSI_file_name$change<-NA
for(i in 2:length(RSI_file_name$Close))
{RSI_file_name$change[i]<-RSI_file_name$Close[i]-RSI_file_name$Close[i-1]}

RSI_file_name$change_percent<-NA
for(i in 2:length(RSI_file_name$Close))
{RSI_file_name$change_percent[i]<-RSI_file_name$change[i]/RSI_file_name$Close[i-1]*100}

RSI_file_name$Loss<-RSI_file_name$change_percent
RSI_file_name$Loss[RSI_file_name$Loss>0]<-0

RSI_file_name$Gain<-RSI_file_name$change_percent
RSI_file_name$Gain[RSI_file_name$Gain<0]<-0

RSI_file_name$Gains_Ave<-NA
RSI_file_name$Losses_Ave<-NA

RSI_file_name$Gains_Ave[16]<-mean(RSI_file_name$Gain[2:15])
RSI_file_name$Losses_Ave[16]<--mean(RSI_file_name$Loss[2:15])

for(i in 17:length(RSI_file_name$Close))
{
  RSI_file_name$Gains_Ave[i]<-(RSI_file_name$Gains_Ave[i-1]*13+RSI_file_name$Gain[i])/14
  RSI_file_name$Losses_Ave[i]<-(RSI_file_name$Losses_Ave[i-1]*13+(-RSI_file_name$Loss[i]))/14
}

RSI_file_name$RS<-RSI_file_name$Gains_Ave/RSI_file_name$Losses_Ave
RSI_file_name$RSI<-100-(100/(1+RSI_file_name$RS))

RSI_file_name$S<-NA
for(i in 18:length(RSI_file_name$Close))
{
  RSI_file_name$S[i]<-mean(RSI_file_name$RSI[(i-2):i])
}

RSI_file_name$Action<-0

for(i in 17:length(RSI_file_name$Close))
{
  RSI_file_name$Action[i]<-case_when(
    RSI_file_name$RSI[i-1]<30&RSI_file_name$RSI[i]>30~"Buy",
    RSI_file_name$RSI[i-1]>70&RSI_file_name$RSI[i]<70~"Sell")
}


RSI_file_name$Action[is.na(RSI_file_name$Action)]<-0

for(i in 19:length(RSI_file_name$Close))
{
  if (RSI_file_name$RSI[i]<70&RSI_file_name$RSI[i]>RSI_file_name$S[i]& RSI_file_name$RSI[i-1]<RSI_file_name$S[i-1])
  {
    RSI_file_name$Action[i]<-"Buy"
  }
}

for(i in 19:length(RSI_file_name$Close))
{
  if (RSI_file_name$RSI[i]>30&RSI_file_name$RSI[i]<RSI_file_name$S[i]& RSI_file_name$RSI[i-1]>RSI_file_name$S[i-1])
  {
    RSI_file_name$Action[i]<-"Sell"
  }
}


##Hold
RSI_file_name$Action[is.na(RSI_file_name$Action)]<-0
for(i in 16:length(RSI_file_name$Close))
{
  if (RSI_file_name$Action[i]=="Buy")
  {
    for (j in i+1:length(RSI_file_name$Close))
    {
      if (RSI_file_name$Action[j]!="Sell"&!is.na(RSI_file_name$Action[j]))
      {
        RSI_file_name$Action[j]<-"Hold"
      }
      else break
    }
  }
}
##Wait

for(i in 16:length(RSI_file_name$Close))
{
  if (RSI_file_name$Action[i]=="Sell")
  {
    for (j in i+1:length(RSI_file_name$Close))
    {
      if (RSI_file_name$Action[j]!="Buy"&!is.na(RSI_file_name$Action[j]))
      {
        RSI_file_name$Action[j]<-"Wait"
      }
      else break
    }
  }
}

##investment price
RSI_file_name$investment_price<-NA
RSI_file_name$investment_price[match("Buy",RSI_file_name$Action)]<-RSI_file_name$Close[match("Buy",RSI_file_name$Action)]

for (m in ((match("Buy",RSI_file_name$Action))+1):length(RSI_file_name$Close))
{
  RSI_file_name$investment_price[m]<-case_when(
    RSI_file_name$Action[m]=="Buy"~RSI_file_name$investment_price[m-1],
    RSI_file_name$Action[m]=="Wait"~RSI_file_name$investment_price[m-1],
    RSI_file_name$Action[m]=="Hold"~(RSI_file_name$investment_price[m-1]/RSI_file_name$Close[m-1])*RSI_file_name$Close[m],
    RSI_file_name$Action[m]=="Sell"~(RSI_file_name$investment_price[m-1]/RSI_file_name$Close[m-1])*RSI_file_name$Close[m]
  )
}
return(RSI_file_name)
}
```

**DOW Jones Weekly**

```
investment_price(RSI2_file(Dow_Jones_weekly)," RSI 2nd strategy","Dow_Jones","weekly")
```

![RSI2 DOW Jones Weekly](https://user-images.githubusercontent.com/41892582/202899251-bb4c6d75-eff8-4127-9f2e-f4946c531721.png)

```
yield_by_year(RSI2_file(Dow_Jones_weekly),"RSI_2nd_strategy","Dow_Jones","weekly")
```

![RSI2 DOW Jones Weekly2](https://user-images.githubusercontent.com/41892582/202899296-1383a602-b94d-45cc-8508-ac24fcd3cb65.png)

```
buy_performance(RSI2_file(Dow_Jones_weekly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 1114.17196 price
2                    Profit Factor           1.2389
3           Total Number of Trades              468
4      %Profitable(Winning Trades)            41.9%
5         Average Trade Net Profit    2.38071 price
6                 %Average Revenue            0.27%
7               Avg. Winning Trade   29.48221 price
8                Avg. Losing Trade   17.14831 price
9               Avg. Win: Avg Loss            1.719
10           Largest Winning Trade  147.68448 price
11            Largest Losing Trade  107.08638 Price
12                  Trading Period      52.88 years
13              Time in the Market           50.11%
14 %M D D(intraday peak to valley)          -44.38%
15        portfolio starting price      700.4 price
16           portfolio close price 2090.12602 price
17                         %change          198.42%
18                   highest price 2090.26042 price
19                    lowest price  512.13908 price
```

**DOW Jones monthly**

```
investment_price(RSI2_file(Dow_Jones_monthly)," RSI 2nd strategy","Dow_Jones","monthly")
```

![RSI2 DOW Jones monthly](https://user-images.githubusercontent.com/41892582/202899307-2e771206-7cd8-4196-a217-3aed6b902842.png)

```
yield_by_year(RSI2_file(Dow_Jones_monthly),"RSI_2nd_strategy","Dow_Jones","monthly")
```

![RSI2 DOW Jones monthly2](https://user-images.githubusercontent.com/41892582/202899333-bef3d0a8-f560-4b3b-9359-03d6750527a4.png)

```
buy_performance(RSI2_file(Dow_Jones_monthly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 3013.52034 price
2                    Profit Factor           2.2201
3           Total Number of Trades               94
4      %Profitable(Winning Trades)            47.9%
5         Average Trade Net Profit   32.05873 price
6                 %Average Revenue            1.92%
7               Avg. Winning Trade  121.85504 price
8                Avg. Losing Trade   50.40727 price
9               Avg. Win: Avg Loss            2.417
10           Largest Winning Trade  829.44655 price
11            Largest Losing Trade   211.9551 Price
12                  Trading Period      52.83 years
13              Time in the Market           46.93%
14 %M D D(intraday peak to valley)           -28.3%
15        portfolio starting price      898.1 price
16           portfolio close price 4074.75713 price
17                         %change          353.71%
18                   highest price  4577.0274 price
19                    lowest price  649.22754 price
```

Even though it achieves a higher Total Net Profit than the RSI First Strategy in the weekly time frame, it performs less profitably in terms of Profitable Percent, Average Revenue Per Trade, and Time Spent in Market.

**Gold**

**Gold daily**

```
investment_price(RSI2_file(Gold_daily)," RSI 2nd strategy","Gold","daily")
```

![RSI2 Gold daily](https://user-images.githubusercontent.com/41892582/202899353-0f9cb24b-5cfc-430b-8926-6360c5f03e6a.png)

```
yield_by_year(RSI2_file(Gold_daily),"RSI_2nd_strategy","Gold","daily")
```

![RSI2 Gold daily2](https://user-images.githubusercontent.com/41892582/202899366-98631487-a7f0-4c75-9eeb-03e3e972d71c.png)

```
buy_performance(RSI2_file(Gold_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit 31.78924 price
2                    Profit Factor          1.054
3           Total Number of Trades           2279
4      %Profitable(Winning Trades)          38.9%
5         Average Trade Net Profit  0.01395 price
6                 %Average Revenue          0.05%
7               Avg. Winning Trade  0.70011 price
8                Avg. Losing Trade  0.41699 price
9               Avg. Win: Avg Loss          1.679
10           Largest Winning Trade  8.11149 price
11            Largest Losing Trade  5.90583 Price
12                  Trading Period    52.87 years
13              Time in the Market         49.04%
14 %M D D(intraday peak to valley)        -67.29%
15        portfolio starting price     35.2 price
16           portfolio close price 66.98924 price
17                         %change         90.31%
18                   highest price 86.08213 price
19                    lowest price 22.75196 price
```

**Gold weekly**

```
investment_price(RSI2_file(Gold_weekly)," RSI 2nd strategy","Gold","weekly")
```

![RSI2 Gold weekly](https://user-images.githubusercontent.com/41892582/202899391-388f4f0e-d551-42f9-8c4f-e954af42eef1.png)

```
yield_by_year(RSI2_file(Gold_weekly),"RSI_2nd_strategy","Gold","weekly")
```

![RSI2 Gold weekly2](https://user-images.githubusercontent.com/41892582/202899409-68bb28b7-fd50-4ec7-9d63-a598b6fcfd46.png)

```
 buy_performance(RSI2_file(Gold_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit 367.52061 price
2                    Profit Factor            1.47
3           Total Number of Trades             455
4      %Profitable(Winning Trades)           41.5%
5         Average Trade Net Profit   0.80774 price
6                 %Average Revenue           0.62%
7               Avg. Winning Trade    6.0818 price
8                Avg. Losing Trade   2.93962 price
9               Avg. Win: Avg Loss           2.069
10           Largest Winning Trade  50.65915 price
11            Largest Losing Trade  19.01615 Price
12                  Trading Period     52.88 years
13              Time in the Market          46.57%
14 %M D D(intraday peak to valley)         -32.21%
15        portfolio starting price      35.8 price
16           portfolio close price 428.70028 price
17                         %change        1097.49%
18                   highest price 445.82343 price
19                    lowest price  35.40056 price
```
**Gold monthly**

```
investment_price(RSI2_file(Gold_monthly)," RSI 2nd strategy","Gold","monthly")
```

![RSI2 Gold monthly](https://user-images.githubusercontent.com/41892582/202899422-0313e5db-8016-4e16-b170-5c3d444a2798.png)

```
yield_by_year(RSI2_file(Gold_monthly),"RSI_2nd_strategy","Gold","monthly")
```

![RSI2 Gold monthly2](https://user-images.githubusercontent.com/41892582/202899429-1d6e06f3-7441-4ef0-a823-d8c843064c9c.png)

```
buy_performance(RSI2_file(Gold_monthly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit 320.28872 price
2                    Profit Factor          1.8024
3           Total Number of Trades              91
4      %Profitable(Winning Trades)           46.2%
5         Average Trade Net Profit   3.51966 price
6                 %Average Revenue           1.44%
7               Avg. Winning Trade  17.12941 price
8                Avg. Losing Trade   8.14585 price
9               Avg. Win: Avg Loss           2.103
10           Largest Winning Trade  65.53902 price
11            Largest Losing Trade   23.6574 Price
12                  Trading Period     52.83 years
13              Time in the Market          37.95%
14 %M D D(intraday peak to valley)         -50.98%
15        portfolio starting price     173.4 price
16           portfolio close price 493.68872 price
17                         %change         184.71%
18                   highest price 545.60561 price
19                    lowest price 140.60043 price
```
On a weekly and monthly basis, it appears to be better than the RSI first strategy, but it is still far from the asset's price. 

**EUR/USD Rate**

**EUR/USD daily**

```
investment_price(RSI2_file(EurUsd_daily)," RSI 2nd strategy","EurUsd","daily")
```

![RSI2 EURUSD daily](https://user-images.githubusercontent.com/41892582/202899456-3956f294-87a8-473d-bc59-4a0fb4afa3b0.png)

```
yield_by_year(RSI2_file(EurUsd_daily),"RSI_2nd_strategy","EurUsd","daily")
```

![RSI2 EURUSD daily2](https://user-images.githubusercontent.com/41892582/202899471-9e359e88-db23-468d-9c74-7528de9ee0c9.png)

```
buy_performance(RSI2_file(EurUsd_daily))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 0.10546 price
2                    Profit Factor        1.0166
3           Total Number of Trades          2214
4      %Profitable(Winning Trades)         37.8%
5         Average Trade Net Profit 0.00005 price
6                 %Average Revenue         0.01%
7               Avg. Winning Trade 0.00773 price
8                Avg. Losing Trade 0.00458 price
9               Avg. Win: Avg Loss         1.687
10           Largest Winning Trade 0.08317 price
11            Largest Losing Trade 0.11105 Price
12                  Trading Period   51.87 years
13              Time in the Market        48.32%
14 %M D D(intraday peak to valley)       -52.13%
15        portfolio starting price  0.5372 price
16           portfolio close price 0.64266 price
17                         %change        19.63%
18                   highest price 1.28223 price
19                    lowest price  0.5362 price
```

**EUR/USD weekly**

```
investment_price(RSI2_file(EurUsd_weekly)," RSI 2nd strategy","EurUsd","weekly")
```

![RSI2 EURUSD weekly](https://user-images.githubusercontent.com/41892582/202899494-449e85d2-9cc0-42b7-8c82-0e6800c41153.png)

```
yield_by_year(RSI2_file(EurUsd_weekly),"RSI_2nd_strategy","EurUsd","weekly")
```

![RSI2 EURUSD weekly2](https://user-images.githubusercontent.com/41892582/202899497-1ca7f457-feca-4779-9c1e-13bd2fa9a39d.png)

```
buy_performance(RSI2_file(EurUsd_weekly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 0.65526 price
2                    Profit Factor        1.1289
3           Total Number of Trades           423
4      %Profitable(Winning Trades)         36.4%
5         Average Trade Net Profit 0.00155 price
6                 %Average Revenue          0.2%
7               Avg. Winning Trade 0.03726 price
8                Avg. Losing Trade 0.01882 price
9               Avg. Win: Avg Loss         1.979
10           Largest Winning Trade 0.17065 price
11            Largest Losing Trade 0.24746 Price
12                  Trading Period   51.86 years
13              Time in the Market        47.03%
14 %M D D(intraday peak to valley)       -43.25%
15        portfolio starting price  0.6156 price
16           portfolio close price 1.30606 price
17                         %change       112.16%
18                   highest price 2.16054 price
19                    lowest price 0.61207 price
```

**EUR/USD monthly**

```
investment_price(RSI2_file(EurUsd_monthly)," RSI 2nd strategy","EurUsd","monthly")
```

![RSI2 EurUsd_monthly](https://user-images.githubusercontent.com/41892582/202899507-03cc9c64-6cbd-4662-aaa9-df307ef39a63.png)

```
yield_by_year(RSI2_file(EurUsd_monthly),"RSI_2nd_strategy","EurUsd","monthly")
```

![RSI2 EurUsd_monthly2](https://user-images.githubusercontent.com/41892582/202899516-02ebc12c-5d19-4c96-bd3b-faae9077cbd7.png) 

```
 buy_performance(RSI2_file(EurUsd_monthly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 2.49502 price
2                    Profit Factor        2.5325
3           Total Number of Trades            82
4      %Profitable(Winning Trades)           50%
5         Average Trade Net Profit 0.03043 price
6                 %Average Revenue         1.97%
7               Avg. Winning Trade 0.10056 price
8                Avg. Losing Trade 0.03876 price
9               Avg. Win: Avg Loss         2.594
10           Largest Winning Trade 0.34053 price
11            Largest Losing Trade 0.23091 Price
12                  Trading Period   51.83 years
13              Time in the Market        44.94%
14 %M D D(intraday peak to valley)       -23.54%
15        portfolio starting price  0.7292 price
16           portfolio close price 3.36101 price
17                         %change       360.92%
18                   highest price 3.40435 price
19                    lowest price  0.7292 price
```

The monthly signals appear to be worth considering. 

**10-Year U.S. Bond Yield (10USY.B)**

**10USY.B daily**

```
investment_price(RSI2_file(US_10YB_daily)," RSI 2nd strategy","US_10YB","daily")
```

![RSI2 US_10YB daily](https://user-images.githubusercontent.com/41892582/202899537-ae40fad1-f30b-4f5d-9e7f-8a539525307f.png)

```
yield_by_year(RSI2_file(US_10YB_daily),"RSI_2nd_strategy","US_10YB","daily")
```

![RSI2 US_10YB daily2](https://user-images.githubusercontent.com/41892582/202899563-5fd28533-af63-40ff-916a-b90cfd5e0160.png)

```
buy_performance(RSI2_file(US_10YB_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit 23.63222 price
2                    Profit Factor         1.0421
3           Total Number of Trades           2009
4      %Profitable(Winning Trades)          41.1%
5         Average Trade Net Profit  0.01176 price
6                 %Average Revenue          0.12%
7               Avg. Winning Trade  0.70759 price
8                Avg. Losing Trade  0.45783 price
9               Avg. Win: Avg Loss          1.546
10           Largest Winning Trade  7.71745 price
11            Largest Losing Trade 10.66903 Price
12                  Trading Period    52.88 years
13              Time in the Market         48.57%
14 %M D D(intraday peak to valley)           -81%
15        portfolio starting price     7.77 price
16           portfolio close price 31.90212 price
17                         %change        310.58%
18                   highest price 57.52699 price
19                    lowest price  7.21107 price
```

**10USY.B weekly**

```
investment_price(RSI2_file(US_10YB_weekly)," RSI 2nd strategy","US_10YB","weekly")
```

![RSI2 US_10YB weekly](https://user-images.githubusercontent.com/41892582/202899619-ea97a79f-4c4c-40e0-912d-f1da7f33a931.png)

```
yield_by_year(RSI2_file(US_10YB_weekly),"RSI_2nd_strategy","US_10YB","weekly")
```

![RSI2 US_10YB weekly2](https://user-images.githubusercontent.com/41892582/202899615-5c6e210e-9d77-4645-80b0-e65d8946a323.png)

```
buy_performance(RSI2_file(US_10YB_weekly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit  0.69973 price
2                    Profit Factor         1.0055
3           Total Number of Trades            435
4      %Profitable(Winning Trades)            37%
5         Average Trade Net Profit  0.00161 price
6                 %Average Revenue          0.22%
7               Avg. Winning Trade  0.79665 price
8                Avg. Losing Trade  0.46386 price
9               Avg. Win: Avg Loss          1.717
10           Largest Winning Trade  4.47997 price
11            Largest Losing Trade  3.75487 Price
12                  Trading Period    52.88 years
13              Time in the Market         48.41%
14 %M D D(intraday peak to valley)        -90.06%
15        portfolio starting price     7.98 price
16           portfolio close price  8.67973 price
17                         %change          8.77%
18                   highest price 31.19818 price
19                    lowest price  3.10195 price
```

**10USY.B monthly**

```
investment_price(RSI2_file(US_10YB_monthly)," RSI 2nd strategy","US_10YB","monthly")
```
![RSI2 US_10YB_monthly](https://user-images.githubusercontent.com/41892582/202899617-5265b90d-1374-4087-b436-121435e5d885.png)

```
yield_by_year(RSI2_file(US_10YB_monthly),"RSI_2nd_strategy","US_10YB","monthly")
```

![RSI2 US_10YB_monthly2](https://user-images.githubusercontent.com/41892582/202899618-b2493bfa-31db-465c-a9e3-2078eced5296.png)

```
buy_performance(RSI2_file(US_10YB_monthly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit 14.84642 price
2                    Profit Factor         1.5224
3           Total Number of Trades            104
4      %Profitable(Winning Trades)          38.5%
5         Average Trade Net Profit  0.14275 price
6                 %Average Revenue          2.19%
7               Avg. Winning Trade   1.0817 price
8                Avg. Losing Trade  0.44409 price
9               Avg. Win: Avg Loss          2.436
10           Largest Winning Trade  8.57993 price
11            Largest Losing Trade  2.38011 Price
12                  Trading Period    52.83 years
13              Time in the Market         47.72%
14 %M D D(intraday peak to valley)        -61.94%
15        portfolio starting price     6.38 price
16           portfolio close price 21.22642 price
17                         %change         232.7%
18                   highest price 24.07737 price
19                    lowest price  4.92791 price
```

as we witnessed the spectacular (especially in a downtrend) monthly performance trading the 10USY.B

**US Dollar/USDX - Index - Cash (DX-Y.NYB)**

**USDX Daily**

```
investment_price(RSI2_file(USDX_daily)," RSI 2nd strategy","USDX","daily")
```

![RSI2 USDX_Daily](https://user-images.githubusercontent.com/41892582/202899694-19a803bc-739a-41b3-8830-e5c60294309e.png)

```
yield_by_year(RSI2_file(USDX_daily),"RSI_2nd_strategy","USDX","daily")
```

![RSI2 USDX_Daily2](https://user-images.githubusercontent.com/41892582/202899696-7980baf7-b404-44cc-97a0-6219ed8fffed.png)

```
buy_performance(RSI2_file(USDX_daily))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  15.55925 price
2                    Profit Factor          1.0205
3           Total Number of Trades            2139
4      %Profitable(Winning Trades)           38.2%
5         Average Trade Net Profit   0.00727 price
6                 %Average Revenue           0.01%
7               Avg. Winning Trade   0.94791 price
8                Avg. Losing Trade   0.56931 price
9               Avg. Win: Avg Loss           1.665
10           Largest Winning Trade   6.82128 price
11            Largest Losing Trade   8.66438 Price
12                  Trading Period     51.87 years
13              Time in the Market          50.73%
14 %M D D(intraday peak to valley)          -36.9%
15        portfolio starting price    120.26 price
16           portfolio close price 136.12477 price
17                         %change          13.19%
18                   highest price 162.34206 price
19                    lowest price  92.22095 price
```

**USDX Weekly**

```
investment_price(RSI2_file(USDX_weekly)," RSI 2nd strategy","USDX","weekly")
```

![RSI2 USDX_Weekly](https://user-images.githubusercontent.com/41892582/202899692-596a0fc8-658d-456e-af53-256ea2909cd4.png)

```
yield_by_year(RSI2_file(USDX_weekly),"RSI_2nd_strategy","USDX","weekly")
```

![RSI2 USDX_Weekly2](https://user-images.githubusercontent.com/41892582/202899693-ddda4fdd-7731-4357-8aa8-aa7fb9d13f94.png)

```
buy_performance(RSI2_file(USDX_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  61.86229 price
2                    Profit Factor          1.1327
3           Total Number of Trades             419
4      %Profitable(Winning Trades)           41.3%
5         Average Trade Net Profit   0.14764 price
6                 %Average Revenue           0.12%
7               Avg. Winning Trade   3.05134 price
8                Avg. Losing Trade   1.89439 price
9               Avg. Win: Avg Loss           1.611
10           Largest Winning Trade    18.879 price
11            Largest Losing Trade  20.81111 Price
12                  Trading Period     51.86 years
13              Time in the Market          51.83%
14 %M D D(intraday peak to valley)         -35.02%
15        portfolio starting price    119.43 price
16           portfolio close price 181.29229 price
17                         %change           51.8%
18                   highest price  239.5239 price
19                    lowest price  99.16591 price
```

**USDX Monthly**

```
investment_price(RSI2_file(USDX_monthly)," RSI 2nd strategy","USDX","monthly")
```

![RSI2 USDX_monthly](https://user-images.githubusercontent.com/41892582/202899697-20941a72-8ed1-4880-a108-dabe986e07c4.png)

```
yield_by_year(RSI2_file(USDX_monthly),"RSI_2nd_strategy","USDX","monthly")
```

![RSI2 USDX_monthly2](https://user-images.githubusercontent.com/41892582/202899689-3de98e1e-71e4-4629-8571-dc0bb4a8a9a9.png)

```
buy_performance(RSI2_file(USDX_monthly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  95.86186 price
2                    Profit Factor          2.2358
3           Total Number of Trades              61
4      %Profitable(Winning Trades)             41%
5         Average Trade Net Profit   1.57151 price
6                 %Average Revenue           1.15%
7               Avg. Winning Trade    6.9372 price
8                Avg. Losing Trade   2.15467 price
9               Avg. Win: Avg Loss            3.22
10           Largest Winning Trade  24.24911 price
11            Largest Losing Trade      9.05 Price
12                  Trading Period     37.83 years
13              Time in the Market          50.77%
14 %M D D(intraday peak to valley)         -21.83%
15        portfolio starting price    106.74 price
16           portfolio close price 202.60186 price
17                         %change          89.81%
18                   highest price 205.01117 price
19                    lowest price     85.42 price
```

Surprisingly, the method performed better here and the 10USY.B had downtrends more frequently than the gold, which had sharp uptrends.


# Stochastic Oscillator

According to "Investopedia", A stochastic oscillator is a momentum indicator comparing a particular closing price of a security to a range of its prices over a certain period of time. The sensitivity of the oscillator to market movements is reducible by adjusting that time period or by taking a moving average of the result. It is used to generate overbought and oversold trading signals, utilizing a 0–100 bounded range of values.

![stoch](https://user-images.githubusercontent.com/41892582/202843550-5c9b3d20-6041-4a01-8abd-ba5f6c91fe29.png)

**Stochastic Formula**


$$K=(\frac{C-L}{H-L})*100$$

where

  C is the current closing price
      
  H is the highest high over the lookback period
              
  L is the lowest low over the lookback period

In addition to %K, %D is also plotted. The expression %D is a simple moving average of %K over a specified smoothing period. Standard lookback and smoothing periods are 14 days and 3 days, respectively.%K and %D are. Always between 0 and 100.

The stochastic indicator establishes a range with values indexed between 0 and 100. A reading of 80+ points to a security being overbought, and is a sell signal. Readings of 20 or lower are considered oversold.
There are two trading strategies being examined.
the first one, When the stochastic oscillator crosses the 20 line, a buy signal is generated. and a sell signal is produced. when the stochastic oscillator crosses below the 80 line.

The second is applied to the first, and if the%k line crosses the%D line, a buy signal is generated (unless the stochastic exceeds 80).).
If the %k line crosses below the %D line (unless the Stochastic is less than 20), it is a sell signal.

applying the first strategy to Dow Jones daily, we will choose 14 day as a time mode lookback.

```
##assign the Dow Jones daily data in a new data frame
Stochastic_Dow_daily<-select(Dow_Jones_daily,-Volume)

##adding two new variables, "Highest high" and "Lowest low"
Stochastic_Dow_daily$Highest_high<-NA
Stochastic_Dow_daily$Lowest_Low<-NA

##calculate the highest high and the lowest low for every 14 days
for(i in 14:length(Stochastic_Dow_daily$Close))
{
  Stochastic_Dow_daily$Highest_high[i]<-max(Stochastic_Dow_daily$High[(i-13):i])
  Stochastic_Dow_daily$Lowest_Low[i]<-min(Stochastic_Dow_daily$Low[(i-13):i])
}

Stochastic_Dow_daily$K<-((Stochastic_Dow_daily$Close-Stochastic_Dow_daily$Lowest_Low)/(Stochastic_Dow_daily$Highest_high-Stochastic_Dow_daily$Lowest_Low))*100
Stochastic_Dow_daily$D<-NA

for(i in 16:length(Stochastic_Dow_daily$Close))
{
  Stochastic_Dow_daily$D[i]<-mean(Stochastic_Dow_daily$K[(i-2):i])
}

##creating the action variable
Stochastic_Dow_daily$Action<-0
##determine where the action will be "buy" or "sell"
for(i in 14:length(Stochastic_Dow_daily$Close))
{
  Stochastic_Dow_daily$Action[i]<-case_when(
    Stochastic_Dow_daily$K[i-1]<20&Stochastic_Dow_daily$K[i]>20~"Buy",
    Stochastic_Dow_daily$K[i-1]>80&Stochastic_Dow_daily$K[i]<80~"Sell")
}

##determine the "Hold" period
Stochastic_Dow_daily$Action[is.na(Stochastic_Dow_daily$Action)]<-0
for(i in 14:length(Stochastic_Dow_daily$Close))
{
  if (Stochastic_Dow_daily$Action[i]=="Buy")
  {
    for (j in i+1:length(Stochastic_Dow_daily$Close))
    {
      if (Stochastic_Dow_daily$Action[j]!="Sell"&!is.na(Stochastic_Dow_daily$Action[j]))
      {
        Stochastic_Dow_daily$Action[j]<-"Hold"
      }
      else break
    }
  }
}

##determine the "Wait" period
for(i in 16:length(Stochastic_Dow_daily$Close))
{
  if (Stochastic_Dow_daily$Action[i]=="Sell")
  {
    for (j in i+1:length(Stochastic_Dow_daily$Close))
    {
      if (Stochastic_Dow_daily$Action[j]!="Buy"&!is.na(Stochastic_Dow_daily$Action[j]))
      {
        Stochastic_Dow_daily$Action[j]<-"Wait"
      }
      else break
    }
  }
}

##investment price
Stochastic_Dow_daily$investment_price<-NA
Stochastic_Dow_daily$investment_price[23]<-Stochastic_Dow_daily$Close[23]

for (m in 24:length(Stochastic_Dow_daily$Close))
{
  Stochastic_Dow_daily$investment_price[m]<-case_when(
    Stochastic_Dow_daily$Action[m]=="Buy"~Stochastic_Dow_daily$investment_price[m-1],
    Stochastic_Dow_daily$Action[m]=="Wait"~Stochastic_Dow_daily$investment_price[m-1],
    Stochastic_Dow_daily$Action[m]=="Hold"~(Stochastic_Dow_daily$investment_price[m-1]/Stochastic_Dow_daily$Close[m-1])*Stochastic_Dow_daily$Close[m],
    Stochastic_Dow_daily$Action[m]=="Sell"~(Stochastic_Dow_daily$investment_price[m-1]/Stochastic_Dow_daily$Close[m-1])*Stochastic_Dow_daily$Close[m]
  )
}
```

**DOW Jones daily**

```
investment_price(Stochastic_Dow_daily,"Stochastic","Dow_Jones","daily")
```

![stochastic Dow_Jones_daily](https://user-images.githubusercontent.com/41892582/202903971-584bf948-69b3-42ef-aa6b-fadcd884465b.png)

```
yield_by_year(Stochastic_Dow_daily,"Stochastic","Dow_Jones","daily")
```

![stochastic Dow_Jones_daily2](https://user-images.githubusercontent.com/41892582/202903990-13710750-e3fa-41e3-82ed-8478fe7590b9.png)

```
buy_performance(Stochastic_Dow_daily)
```

```
                        Statistics       Long Trades
1                 Total Net Profit  9442.42962 price
2                    Profit Factor            1.5994
3           Total Number of Trades               328
4      %Profitable(Winning Trades)             75.9%
5         Average Trade Net Profit     28.7879 price
6                 %Average Revenue             0.88%
7               Avg. Winning Trade   101.18289 price
8                Avg. Losing Trade    199.3938 price
9               Avg. Win: Avg Loss             0.507
10           Largest Winning Trade   697.91637 price
11            Largest Losing Trade  1307.56098 Price
12                  Trading Period       52.88 years
13              Time in the Market            42.45%
14 %M D D(intraday peak to valley)           -62.29%
15        portfolio starting price       757.5 price
16           portfolio close price 10199.92962 price
17                         %change          1246.53%
18                   highest price 10950.26501 price
19                    lowest price   623.61385 price
```

This stochastic strategy is better than the RSI first strategy and the MACD strategy on Dow Jones daily trading. but still underperforming the Dow Jones itself.

We will now use the previously created code to construct a function named "Stochastic_file" that will be used to prepare the data in the files for plotting using the "investment_price" and "yield_by_year" functions according to the Stochastic strategy that introduced before.

**Stochastic function**

```
##Stochastic file function
Stochastic_file<-function(file_name)
{
##assign the Dow Jones daily data in a new data frame
Stochastic_file_name<-select(file_name,Date,Open,High,Low,Close)

##adding two new variables, "Highest high" and "Lowest low"
Stochastic_file_name$Highest_high<-NA
Stochastic_file_name$Lowest_Low<-NA

##calculate the highest high and the lowest low for every 14 days
for(i in 14:length(Stochastic_file_name$Close))
{
  Stochastic_file_name$Highest_high[i]<-max(Stochastic_file_name$High[(i-13):i])
  Stochastic_file_name$Lowest_Low[i]<-min(Stochastic_file_name$Low[(i-13):i])
}

Stochastic_file_name$K<-((Stochastic_file_name$Close-Stochastic_file_name$Lowest_Low)/(Stochastic_file_name$Highest_high-Stochastic_file_name$Lowest_Low))*100
Stochastic_file_name$D<-NA

for(i in 16:length(Stochastic_file_name$Close))
{
  Stochastic_file_name$D[i]<-mean(Stochastic_file_name$K[(i-2):i])
}

##creating the action variable
Stochastic_file_name$Action<-0
##determine where the action will be "buy" or "sell"
for(i in 14:length(Stochastic_file_name$Close))
{
  Stochastic_file_name$Action[i]<-case_when(
    Stochastic_file_name$K[i-1]<20&Stochastic_file_name$K[i]>20~"Buy",
    Stochastic_file_name$K[i-1]>80&Stochastic_file_name$K[i]<80~"Sell")
}

##determine the "Hold" period
Stochastic_file_name$Action[is.na(Stochastic_file_name$Action)]<-0
for(i in 14:length(Stochastic_file_name$Close))
{
  if (Stochastic_file_name$Action[i]=="Buy")
  {
    for (j in i+1:length(Stochastic_file_name$Close))
    {
      if (Stochastic_file_name$Action[j]!="Sell"&!is.na(Stochastic_file_name$Action[j]))
      {
        Stochastic_file_name$Action[j]<-"Hold"
      }
      else break
    }
  }
}

##determine the "Wait" period
for(i in 16:length(Stochastic_file_name$Close))
{
  if (Stochastic_file_name$Action[i]=="Sell")
  {
    for (j in i+1:length(Stochastic_file_name$Close))
    {
      if (Stochastic_file_name$Action[j]!="Buy"&!is.na(Stochastic_file_name$Action[j]))
      {
        Stochastic_file_name$Action[j]<-"Wait"
      }
      else break
    }
  }
}

##investment price
Stochastic_file_name$investment_price<-NA
Stochastic_file_name$investment_price[match("Buy",Stochastic_file_name$Action)]<-Stochastic_file_name$Close[match("Buy",Stochastic_file_name$Action)]

for (m in ((match("Buy",Stochastic_file_name$Action))+1):length(Stochastic_file_name$Close))
{
  Stochastic_file_name$investment_price[m]<-case_when(
    Stochastic_file_name$Action[m]=="Buy"~Stochastic_file_name$investment_price[m-1],
    Stochastic_file_name$Action[m]=="Wait"~Stochastic_file_name$investment_price[m-1],
    Stochastic_file_name$Action[m]=="Hold"~(Stochastic_file_name$investment_price[m-1]/Stochastic_file_name$Close[m-1])*Stochastic_file_name$Close[m],
    Stochastic_file_name$Action[m]=="Sell"~(Stochastic_file_name$investment_price[m-1]/Stochastic_file_name$Close[m-1])*Stochastic_file_name$Close[m]
  )
}
return(Stochastic_file_name)

}
```

**DOW Jones weekly**

```
investment_price(Stochastic_file(Dow_Jones_weekly)," Stochastic","Dow_Jones","weekly")
```

![stochastic Dow_Jones_weekly](https://user-images.githubusercontent.com/41892582/202907596-d3388747-b8c0-45c6-ae9e-986a5c022bca.png)

```
yield_by_year(Stochastic_file(Dow_Jones_weekly),"Stochastic","Dow_Jones","weekly")
```

![stochastic Dow_Jones_weekly2](https://user-images.githubusercontent.com/41892582/202907598-68eb347b-b53b-4b18-90bb-5fb7632b89be.png)

```
buy_performance(Stochastic_file(Dow_Jones_weekly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 3215.73312 price
2                    Profit Factor           2.4747
3           Total Number of Trades               64
4      %Profitable(Winning Trades)            76.6%
5         Average Trade Net Profit   50.24583 price
6                 %Average Revenue               3%
7               Avg. Winning Trade  110.13033 price
8                Avg. Losing Trade  145.37687 price
9               Avg. Win: Avg Loss            0.758
10           Largest Winning Trade  365.67989 price
11            Largest Losing Trade  884.69624 Price
12                  Trading Period      52.88 years
13              Time in the Market           40.33%
14 %M D D(intraday peak to valley)          -47.28%
15        portfolio starting price      702.2 price
16           portfolio close price 4253.61863 price
17                         %change          505.76%
18                   highest price 4371.07585 price
19                    lowest price  554.35652 price
```

**DOW Jones monthly**

```
investment_price(Stochastic_file(Dow_Jones_monthly)," Stochastic","Dow_Jones","monthly")
```

![stochastic Dow_Jones_monthly](https://user-images.githubusercontent.com/41892582/202907593-f43d177b-49ce-4e2d-8818-99547d92ae69.png)

```
yield_by_year(Stochastic_file(Dow_Jones_monthly),"Stochastic","Dow_Jones","monthly")
```

![stochastic Dow_Jones_monthly2](https://user-images.githubusercontent.com/41892582/202907594-03a934e7-e675-48aa-8562-9632eb1ef8ec.png)

```
buy_performance(Stochastic_file(Dow_Jones_monthly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 1025.84918 price
2                    Profit Factor           3.5426
3           Total Number of Trades                8
4      %Profitable(Winning Trades)              75%
5         Average Trade Net Profit  128.23115 price
6                 %Average Revenue            11.1%
7               Avg. Winning Trade   238.2183 price
8                Avg. Losing Trade  201.73031 price
9               Avg. Win: Avg Loss            1.181
10           Largest Winning Trade  390.91266 price
11            Largest Losing Trade  270.96061 Price
12                  Trading Period      52.83 years
13              Time in the Market           24.72%
14 %M D D(intraday peak to valley)           -41.1%
15        portfolio starting price      926.4 price
16           portfolio close price 2026.72322 price
17                         %change          118.77%
18                   highest price 2309.48914 price
19                    lowest price      607.9 price
```
The duration in the market looks to be longer than it needs to be, especially given the performance of the Dow Jones, a high-profit factor strategy.

**Gold**

**Gold daily**

```
investment_price(Stochastic_file(Gold_daily)," Stochastic","Gold","daily")
```

![stochastic Gold_daily](https://user-images.githubusercontent.com/41892582/202907611-94f9b648-2888-4ff6-b29c-eeac988fe1b1.png)

```
yield_by_year(Stochastic_file(Gold_daily),"Stochastic","Gold","daily")
```

![stochastic Gold_daily2](https://user-images.githubusercontent.com/41892582/202907615-7eece7d4-919c-4676-b916-108f6c1f554c.png)

```
buy_performance(Stochastic_file(Gold_daily))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  75.33724 price
2                    Profit Factor          1.2854
3           Total Number of Trades             326
4      %Profitable(Winning Trades)           60.1%
5         Average Trade Net Profit    0.2311 price
6                 %Average Revenue           0.46%
7               Avg. Winning Trade   1.73105 price
8                Avg. Losing Trade   2.01487 price
9               Avg. Win: Avg Loss           0.859
10           Largest Winning Trade  11.42497 price
11            Largest Losing Trade  14.81044 Price
12                  Trading Period     52.87 years
13              Time in the Market           48.4%
14 %M D D(intraday peak to valley)          -56.4%
15        portfolio starting price      35.2 price
16           portfolio close price 110.53724 price
17                         %change         214.03%
18                   highest price 120.66295 price
19                    lowest price  29.80841 price
```
Similar to the RSI first strategy, the price of the investments created by this stochastic strategy was typically quite low compared to the value of the asset and the length of time it was stuck in the market. This might have happened because of how long the asset price (Dow Jones, Gold) has been above the oversold threshold.

**Gold weekly**

```
investment_price(Stochastic_file(Gold_weekly)," Stochastic","Gold","weekly")
```

![stochastic Gold_weekly](https://user-images.githubusercontent.com/41892582/202907621-0b545bd6-46a8-4463-916f-8f68748fec94.png)

```
yield_by_year(Stochastic_file(Gold_weekly),"Stochastic","Gold","weekly")
```

![stochastic Gold_weekly2](https://user-images.githubusercontent.com/41892582/202907622-8e8f4263-5593-4a2e-9859-24cd348bb22f.png)

```
buy_performance(Stochastic_file(Gold_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit 211.36184 price
2                    Profit Factor          2.1225
3           Total Number of Trades              72
4      %Profitable(Winning Trades)           73.6%
5         Average Trade Net Profit   2.93558 price
6                 %Average Revenue           3.38%
7               Avg. Winning Trade   7.54063 price
8                Avg. Losing Trade   9.91007 price
9               Avg. Win: Avg Loss           0.761
10           Largest Winning Trade  32.18504 price
11            Largest Losing Trade  41.06489 Price
12                  Trading Period     52.88 years
13              Time in the Market          46.72%
14 %M D D(intraday peak to valley)         -61.94%
15        portfolio starting price      35.6 price
16           portfolio close price 234.11911 price
17                         %change         557.64%
18                   highest price 271.51812 price
19                    lowest price      35.6 price
```

**Gold monthly**

```
investment_price(Stochastic_file(Gold_monthly)," Stochastic","Gold","monthly")
```

![stochastic Gold_monthly](https://user-images.githubusercontent.com/41892582/202907618-b13c5091-929b-48ce-9cec-dbe25a8d0db1.png)

```
yield_by_year(Stochastic_file(Gold_monthly),"Stochastic","Gold","monthly")
```

![stochastic Gold_monthly2](https://user-images.githubusercontent.com/41892582/202907619-df303d29-3f8f-44ff-8f83-75531a647179.png)


```
buy_performance(Stochastic_file(Gold_monthly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit -70.14113 price
2                    Profit Factor          0.4149
3           Total Number of Trades               8
4      %Profitable(Winning Trades)           37.5%
5         Average Trade Net Profit  -8.76764 price
6                 %Average Revenue          -6.22%
7               Avg. Winning Trade  16.58029 price
8                Avg. Losing Trade   23.9764 price
9               Avg. Win: Avg Loss           0.692
10           Largest Winning Trade      27.8 price
11            Largest Losing Trade  33.04868 Price
12                  Trading Period     52.83 years
13              Time in the Market           41.1%
14 %M D D(intraday peak to valley)         -72.02%
15        portfolio starting price     142.8 price
16           portfolio close price  72.65887 price
17                         %change         -49.12%
18                   highest price     187.1 price
19                    lowest price   52.3528 price
```

**EUR/USD Rate**

**EUR/USD daily**

```
investment_price(Stochastic_file(EurUsd_daily)," Stochastic","EurUsd","daily")
```

![stochastic EurUsd_daily](https://user-images.githubusercontent.com/41892582/202907599-2bb2b938-002b-4e7e-bc0f-2575b624209c.png)

```
yield_by_year(Stochastic_file(EurUsd_daily),"Stochastic","EurUsd","daily")
```

![stochastic EurUsd_daily2](https://user-images.githubusercontent.com/41892582/202907600-314def0f-5ebf-4c97-bc51-e5747d142537.png)

```
buy_performance(Stochastic_file(EurUsd_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -0.18406 price
2                    Profit Factor         0.8592
3           Total Number of Trades            327
4      %Profitable(Winning Trades)          59.3%
5         Average Trade Net Profit -0.00056 price
6                 %Average Revenue         -0.09%
7               Avg. Winning Trade  0.00579 price
8                Avg. Losing Trade  0.00975 price
9               Avg. Win: Avg Loss          0.593
10           Largest Winning Trade  0.04918 price
11            Largest Losing Trade  0.05173 Price
12                  Trading Period    51.87 years
13              Time in the Market         47.73%
14 %M D D(intraday peak to valley)        -52.65%
15        portfolio starting price   0.5372 price
16           portfolio close price  0.35314 price
17                         %change        -34.26%
18                   highest price   0.6643 price
19                    lowest price  0.31452 price
```

**EUR/USD weekly**

```
investment_price(Stochastic_file(EurUsd_weekly)," Stochastic","EurUsd","weekly")
```

![stochastic EurUsd_weekly](https://user-images.githubusercontent.com/41892582/202907607-12a1f8eb-fd02-4f29-ab27-2f39b16d8df3.png)

```
yield_by_year(Stochastic_file(EurUsd_weekly),"Stochastic","EurUsd","weekly")
```

![stochastic EurUsd_weekly2](https://user-images.githubusercontent.com/41892582/202907609-1633fbe1-ae7b-4a48-8293-f46560121675.png)

```
buy_performance(Stochastic_file(EurUsd_weekly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 0.09027 price
2                    Profit Factor         1.095
3           Total Number of Trades            65
4      %Profitable(Winning Trades)         61.5%
5         Average Trade Net Profit 0.00139 price
6                 %Average Revenue         0.41%
7               Avg. Winning Trade 0.02602 price
8                Avg. Losing Trade 0.03802 price
9               Avg. Win: Avg Loss         0.684
10           Largest Winning Trade 0.08043 price
11            Largest Losing Trade 0.20794 Price
12                  Trading Period   51.86 years
13              Time in the Market        44.85%
14 %M D D(intraday peak to valley)       -45.76%
15        portfolio starting price  0.6129 price
16           portfolio close price 0.61203 price
17                         %change        -0.14%
18                   highest price 0.93798 price
19                    lowest price 0.50878 price
```

**EUR/USD monthly**

```
investment_price(Stochastic_file(EurUsd_monthly)," Stochastic","EurUsd","monthly")
```

![stochastic EurUsd_monthly](https://user-images.githubusercontent.com/41892582/202907601-55b093a9-e80b-4fa0-a03f-ef18a516ba74.png)

```
yield_by_year(Stochastic_file(EurUsd_monthly),"Stochastic","EurUsd","monthly")
```

![stochastic EurUsd_monthly2](https://user-images.githubusercontent.com/41892582/202907604-4cdf2e62-0957-4ef3-87bd-468b1292c1c7.png)

```
buy_performance(Stochastic_file(EurUsd_monthly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 0.00099 price
2                    Profit Factor        1.0035
3           Total Number of Trades            13
4      %Profitable(Winning Trades)         69.2%
5         Average Trade Net Profit 0.00008 price
6                 %Average Revenue         0.38%
7               Avg. Winning Trade 0.03128 price
8                Avg. Losing Trade 0.07014 price
9               Avg. Win: Avg Loss         0.446
10           Largest Winning Trade 0.06605 price
11            Largest Losing Trade 0.18288 Price
12                  Trading Period   51.83 years
13              Time in the Market        49.28%
14 %M D D(intraday peak to valley)       -47.25%
15        portfolio starting price  0.7625 price
16           portfolio close price 0.76349 price
17                         %change         0.13%
18                   highest price 0.82662 price
19                    lowest price 0.43604 price
```

**10-Year U.S. Bond Yield (10USY.B)**

**10USY.B daily**

```
investment_price(Stochastic_file(US_10YB_daily)," Stochastic","US_10YB","daily")
```
![stochastic US_10YB_daily](https://user-images.githubusercontent.com/41892582/202907623-3660ddc9-a3cd-4a58-9211-0bd94e445cb4.png)

```
yield_by_year(Stochastic_file(US_10YB_daily),"Stochastic","US_10YB","daily")
```

![stochastic US_10YB_daily2](https://user-images.githubusercontent.com/41892582/202907624-ea9953ea-b8bc-4dc7-80e7-062d7890791a.png)

```
buy_performance(Stochastic_file(US_10YB_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit  -6.8397 price
2                    Profit Factor         0.8261
3           Total Number of Trades            333
4      %Profitable(Winning Trades)          57.7%
5         Average Trade Net Profit -0.02054 price
6                 %Average Revenue         -0.38%
7               Avg. Winning Trade  0.16919 price
8                Avg. Losing Trade  0.27499 price
9               Avg. Win: Avg Loss          0.615
10           Largest Winning Trade  1.80115 price
11            Largest Losing Trade  2.17632 Price
12                  Trading Period    52.88 years
13              Time in the Market         50.06%
14 %M D D(intraday peak to valley)        -96.55%
15        portfolio starting price     7.77 price
16           portfolio close price   0.9303 price
17                         %change        -88.03%
18                   highest price 16.25581 price
19                    lowest price  0.56012 price
```

**10USY.B weekly**

```
investment_price(Stochastic_file(US_10YB_weekly)," Stochastic","US_10YB","weekly")
```

![stochastic US_10YB_weekly](https://user-images.githubusercontent.com/41892582/202907629-c89c589a-3bbd-4af6-bc0e-fb3662f2b909.png)

```
yield_by_year(Stochastic_file(US_10YB_weekly),"Stochastic","US_10YB","weekly")
```

![stochastic US_10YB_weekly2](https://user-images.githubusercontent.com/41892582/202907631-08068cf4-0a70-4131-baad-6e91785e027a.png)

```
buy_performance(Stochastic_file(US_10YB_weekly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -4.26037 price
2                    Profit Factor         0.7874
3           Total Number of Trades             69
4      %Profitable(Winning Trades)          65.2%
5         Average Trade Net Profit -0.06174 price
6                 %Average Revenue         -0.24%
7               Avg. Winning Trade  0.35069 price
8                Avg. Losing Trade  0.83505 price
9               Avg. Win: Avg Loss           0.42
10           Largest Winning Trade  1.40814 price
11            Largest Losing Trade  3.36881 Price
12                  Trading Period    52.88 years
13              Time in the Market         47.79%
14 %M D D(intraday peak to valley)        -88.16%
15        portfolio starting price     7.67 price
16           portfolio close price  3.40963 price
17                         %change        -55.55%
18                   highest price 11.40025 price
19                    lowest price   1.3502 price
```

**10USY.B monthly**

```
investment_price(Stochastic_file(US_10YB_monthly)," Stochastic","US_10YB","monthly")
```

![stochastic US_10YB_monthly](https://user-images.githubusercontent.com/41892582/202907626-d1f3aa7a-e4fa-41cc-a9e2-188597bbc92b.png)

```
yield_by_year(Stochastic_file(US_10YB_monthly),"Stochastic","US_10YB","monthly")
```

![stochastic US_10YB_monthly2](https://user-images.githubusercontent.com/41892582/202907627-bd459a16-0d21-4fc0-a167-ae686fd6483d.png)

```
buy_performance(Stochastic_file(US_10YB_monthly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -3.26124 price
2                    Profit Factor         0.4468
3           Total Number of Trades             15
4      %Profitable(Winning Trades)            40%
5         Average Trade Net Profit -0.21742 price
6                 %Average Revenue         -4.13%
7               Avg. Winning Trade  0.43898 price
8                Avg. Losing Trade  0.65501 price
9               Avg. Win: Avg Loss           0.67
10           Largest Winning Trade     1.17 price
11            Largest Losing Trade  2.17753 Price
12                  Trading Period    52.83 years
13              Time in the Market         54.49%
14 %M D D(intraday peak to valley)        -86.98%
15        portfolio starting price     6.08 price
16           portfolio close price  2.81876 price
17                         %change        -53.64%
18                   highest price  7.85113 price
19                    lowest price  1.02204 price
```

**US Dollar/USDX - Index - Cash (DX-Y.NYB)**

**USDX Daily**

```
investment_price(Stochastic_file(USDX_daily)," Stochastic","USDX","daily")
```

![stochastic USDX_daily](https://user-images.githubusercontent.com/41892582/202907581-a2aca759-8b34-4fa4-b6c1-9e61120ef80c.png)

```
yield_by_year(Stochastic_file(USDX_daily),"Stochastic","USDX","daily")
```

![stochastic USDX_daily2](https://user-images.githubusercontent.com/41892582/202907585-40488ab6-89ab-439f-9f4a-e9a692accd6f.png)

```
buy_performance(Stochastic_file(USDX_daily))
```

```
Statistics     Long Trades
1                 Total Net Profit -61.10737 price
2                    Profit Factor          0.7118
3           Total Number of Trades             314
4      %Profitable(Winning Trades)           58.6%
5         Average Trade Net Profit  -0.19461 price
6                 %Average Revenue           -0.2%
7               Avg. Winning Trade   0.82006 price
8                Avg. Losing Trade    1.6183 price
9               Avg. Win: Avg Loss           0.507
10           Largest Winning Trade   6.38233 price
11            Largest Losing Trade   7.16507 Price
12                  Trading Period     51.87 years
13              Time in the Market          50.02%
14 %M D D(intraday peak to valley)         -62.86%
15        portfolio starting price    120.28 price
16           portfolio close price  56.96704 price
17                         %change         -52.64%
18                   highest price 120.45088 price
19                    lowest price  44.74119 price
```

**USDX weekly**

```
investment_price(Stochastic_file(USDX_weekly)," Stochastic","USDX","weekly")
```

![stochastic USDX_weekly](https://user-images.githubusercontent.com/41892582/202907589-f6629443-6dc4-424f-aa8a-f12c553de9fa.png)

```
yield_by_year(Stochastic_file(USDX_weekly),"Stochastic","USDX","weekly")
```

![stochastic USDX_weekly2](https://user-images.githubusercontent.com/41892582/202907591-4c8d7b0e-5422-4f41-95fd-a9f174fb8162.png)

```
buy_performance(Stochastic_file(USDX_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit -52.02131 price
2                    Profit Factor          0.6094
3           Total Number of Trades              61
4      %Profitable(Winning Trades)           60.7%
5         Average Trade Net Profit  -0.85281 price
6                 %Average Revenue          -0.76%
7               Avg. Winning Trade   2.19359 price
8                Avg. Losing Trade   5.54933 price
9               Avg. Win: Avg Loss           0.395
10           Largest Winning Trade   6.63746 price
11            Largest Losing Trade  39.36602 Price
12                  Trading Period     51.86 years
13              Time in the Market          49.54%
14 %M D D(intraday peak to valley)         -53.97%
15        portfolio starting price    119.43 price
16           portfolio close price  67.40869 price
17                         %change         -43.56%
18                   highest price 133.97297 price
19                    lowest price  61.67299 price
```

**USDX monthly**

```
investment_price(Stochastic_file(USDX_monthly)," Stochastic","USDX","monthly")
```

![stochastic USDX_monthly](https://user-images.githubusercontent.com/41892582/202907586-6e00f370-bc2d-468d-9afb-e10c197093ed.png)

```
yield_by_year(Stochastic_file(USDX_monthly),"Stochastic","USDX","monthly")
```

![stochastic USDX_monthly2](https://user-images.githubusercontent.com/41892582/202907588-52cae3b6-a9b0-4a47-a53a-904af754d1ba.png)


```
buy_performance(Stochastic_file(USDX_monthly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  15.98792 price
2                    Profit Factor          1.4681
3           Total Number of Trades              13
4      %Profitable(Winning Trades)           69.2%
5         Average Trade Net Profit   1.22984 price
6                 %Average Revenue            1.5%
7               Avg. Winning Trade   5.57162 price
8                Avg. Losing Trade   8.53916 price
9               Avg. Win: Avg Loss           0.652
10           Largest Winning Trade  14.37289 price
11            Largest Losing Trade  19.84683 Price
12                  Trading Period     37.83 years
13              Time in the Market          44.18%
14 %M D D(intraday peak to valley)         -34.09%
15        portfolio starting price     99.79 price
16           portfolio close price 115.77793 price
17                         %change          16.02%
18                   highest price 117.15474 price
19                    lowest price  73.20672 price
```

It is obvious that this is not the best way to use Stochastic. 

As with the RSI, our way to improve the Stochastic strategy will be by using the 3-period moving average of the K
our sell signal still the same when the K cross below the 80 line plus when the K croses down it's moving average (D) at any area over the over sell area (above the 20 line), the same will happened with buy signal, when K cross over the 20 line or when K cross over D at any area under the over bought area (above 80 line)

**Stochastic second strategy**

The stochastic second approach will be used to analyze the Dow Jones Daily first.

```
##assign the Dow Jones daily data in a new data frame
Stochastic_Dow_daily2<-select(Dow_Jones_daily,-Volume)

##adding two new variables, "Highest high" and "Lowest low"
Stochastic_Dow_daily2$Highest_high<-NA
Stochastic_Dow_daily2$Lowest_Low<-NA

##calculate the highest high and the lowest low for every 14 days
for(i in 14:length(Stochastic_Dow_daily2$Close))
{
  Stochastic_Dow_daily2$Highest_high[i]<-max(Stochastic_Dow_daily2$High[(i-13):i])
  Stochastic_Dow_daily2$Lowest_Low[i]<-min(Stochastic_Dow_daily2$Low[(i-13):i])
}

Stochastic_Dow_daily2$K<-((Stochastic_Dow_daily2$Close-Stochastic_Dow_daily2$Lowest_Low)/(Stochastic_Dow_daily2$Highest_high-Stochastic_Dow_daily2$Lowest_Low))*100
Stochastic_Dow_daily2$D<-NA

for(i in 16:length(Stochastic_Dow_daily2$Close))
{
  Stochastic_Dow_daily2$D[i]<-mean(Stochastic_Dow_daily2$K[(i-2):i])
}
Stochastic_Dow_daily2$Action<-0
Stochastic_Dow_daily2$investment_price<-NA

for(i in 14:length(Stochastic_Dow_daily2$Close))
{
  Stochastic_Dow_daily2$Action[i]<-case_when(
    Stochastic_Dow_daily2$K[i-1]<20&Stochastic_Dow_daily2$K[i]>20~"Buy",
    Stochastic_Dow_daily2$K[i-1]>80&Stochastic_Dow_daily2$K[i]<80~"Sell")
}

Stochastic_Dow_daily2$Action[is.na(Stochastic_Dow_daily2$Action)]<-0

for(i in 16:length(Stochastic_Dow_daily2$Close))
{
  if (Stochastic_Dow_daily2$K[i]<80&Stochastic_Dow_daily2$K[i]>Stochastic_Dow_daily2$D[i]& Stochastic_Dow_daily2$K[i-1]<Stochastic_Dow_daily2$D[i-1])
  {
    Stochastic_Dow_daily2$Action[i]<-"Buy"
  }
}

for(i in 16:length(Stochastic_Dow_daily2$Close))
{
  if (Stochastic_Dow_daily2$K[i]>20&Stochastic_Dow_daily2$K[i]<Stochastic_Dow_daily2$D[i]& Stochastic_Dow_daily2$K[i-1]>Stochastic_Dow_daily2$D[i-1])
  {
    Stochastic_Dow_daily2$Action[i]<-"Sell"
  }
} 

#Hold

for(i in 16:length(Stochastic_Dow_daily2$Close))
{
  if (Stochastic_Dow_daily2$Action[i]=="Buy")
  {
    for (j in i+1:length(Stochastic_Dow_daily2$Close))
    {
      if (Stochastic_Dow_daily2$Action[j]!="Sell"&!is.na(Stochastic_Dow_daily2$Action[j]))
      {
        Stochastic_Dow_daily2$Action[j]<-"Hold"
      }
      else break
    }
  }
}
##Wait

for(i in 16:length(Stochastic_Dow_daily2$Close))
{
  if (Stochastic_Dow_daily2$Action[i]=="Sell")
  {
    for (j in i+1:length(Stochastic_Dow_daily2$Close))
    {
      if (Stochastic_Dow_daily2$Action[j]!="Buy"&!is.na(Stochastic_Dow_daily2$Action[j]))
      {
        Stochastic_Dow_daily2$Action[j]<-"Wait"
      }
      else break
    }
  }
}

##investment price
Stochastic_Dow_daily2$investment_price<-NA
Stochastic_Dow_daily2$investment_price[match("Buy",Stochastic_Dow_daily2$Action)]<-Stochastic_Dow_daily2$Close[match("Buy",Stochastic_Dow_daily2$Action)]

for (m in (match("Buy",Stochastic_Dow_daily2$Action)+1):length(Stochastic_Dow_daily2$Close))
{
  Stochastic_Dow_daily2$investment_price[m]<-case_when(
    Stochastic_Dow_daily2$Action[m]=="Buy"~Stochastic_Dow_daily2$investment_price[m-1],
    Stochastic_Dow_daily2$Action[m]=="Wait"~Stochastic_Dow_daily2$investment_price[m-1],
    Stochastic_Dow_daily2$Action[m]=="Hold"~(Stochastic_Dow_daily2$investment_price[m-1]/Stochastic_Dow_daily2$Close[m-1])*Stochastic_Dow_daily2$Close[m],
    Stochastic_Dow_daily2$Action[m]=="Sell"~(Stochastic_Dow_daily2$investment_price[m-1]/Stochastic_Dow_daily2$Close[m-1])*Stochastic_Dow_daily2$Close[m]
  )
}
```

**DOW Jones daily**

```
investment_price(Stochastic_Dow_daily2,"Stochastic 2nd strategy","Dow_Jones","daily")
```

![stochastic2 Dow_Jones_daily](https://user-images.githubusercontent.com/41892582/202904890-f5651a5e-9c32-4a45-bd78-fa199f45a437.png)

```
yield_by_year(Stochastic_Dow_daily2,"Stochastic_2nd_strategy","Dow_Jones","daily")
```

![stochastic2 Dow_Jones_daily2](https://user-images.githubusercontent.com/41892582/202907410-39a37973-9055-432e-bd5c-cedb6fb0354a.png)

```
buy_performance(Stochastic_Dow_daily2)
```

```
                        Statistics       Long Trades
1                 Total Net Profit 35377.52444 price
2                    Profit Factor            1.2609
3           Total Number of Trades              1292
4      %Profitable(Winning Trades)             52.2%
5         Average Trade Net Profit    27.38198 price
6                 %Average Revenue             0.33%
7               Avg. Winning Trade   253.69979 price
8                Avg. Losing Trade   218.03237 price
9               Avg. Win: Avg Loss             1.164
10           Largest Winning Trade  3061.25344 price
11            Largest Losing Trade  5447.56493 Price
12                  Trading Period       52.88 years
13              Time in the Market            43.17%
14 %M D D(intraday peak to valley)           -59.69%
15        portfolio starting price       744.1 price
16           portfolio close price 36121.62444 price
17                         %change           4754.4%
18                   highest price 41030.66503 price
19                    lowest price   663.48104 price
```
A high net profit with a small average percent revenue per trade and a high M D D, as with RSI's second strategy. 

We will now use the previously created code to construct a function named "Stochastic2_file" that will be used to prepare the data in the files for plotting using the "investment_price" and "yield_by_year" functions according to the Stochastic second strategy that introduced before.

**Stochastic second strategyfunction**

```
Stochastic2_file<-function(file_name)
{
  ##assign the Dow Jones daily data in a new data frame
  Stochastic_file_name<-select(file_name,Date,Open,High,Low,Close)
  
  ##adding two new variables, "Highest high" and "Lowest low"
  Stochastic_file_name$Highest_high<-NA
  Stochastic_file_name$Lowest_Low<-NA
  
  ##calculate the highest high and the lowest low for every 14 days
  for(i in 14:length(Stochastic_file_name$Close))
  {
    Stochastic_file_name$Highest_high[i]<-max(Stochastic_file_name$High[(i-13):i])
    Stochastic_file_name$Lowest_Low[i]<-min(Stochastic_file_name$Low[(i-13):i])
  }
  
  Stochastic_file_name$K<-((Stochastic_file_name$Close-Stochastic_file_name$Lowest_Low)/(Stochastic_file_name$Highest_high-Stochastic_file_name$Lowest_Low))*100
  Stochastic_file_name$D<-NA
  
  for(i in 16:length(Stochastic_file_name$Close))
  {
    Stochastic_file_name$D[i]<-mean(Stochastic_file_name$K[(i-2):i])
  }
  
  ##creating the action variable
  Stochastic_file_name$Action<-0
  
  for(i in 17:length(Stochastic_file_name$Close))
  {
    Stochastic_file_name$Action[i]<-case_when(
      Stochastic_file_name$K[i-1]<20&Stochastic_file_name$K[i]>20~"Buy",
      Stochastic_file_name$K[i-1]>80&Stochastic_file_name$K[i]<80~"Sell")
  }
  
  Stochastic_file_name$Action[is.na(Stochastic_file_name$Action)]<-0
  
  for(i in 17:length(Stochastic_file_name$Close))
  {
    if (Stochastic_file_name$K[i]<80&Stochastic_file_name$K[i]>Stochastic_file_name$D[i]& Stochastic_file_name$K[i-1]<Stochastic_file_name$D[i-1]&
        !is.na(Stochastic_file_name$K[i])&!is.na(Stochastic_file_name$D[i])&!is.na(Stochastic_file_name$K[i-1])&!is.na(Stochastic_file_name$D[i-1]))
    {
      Stochastic_file_name$Action[i]<-"Buy"
    }
  }
  
  for(i in 17:length(Stochastic_file_name$Close))
  {
    if (Stochastic_file_name$K[i]>20&Stochastic_file_name$K[i]<Stochastic_file_name$D[i]& Stochastic_file_name$K[i-1]>Stochastic_file_name$D[i-1]&
        !is.na(Stochastic_file_name$K[i])&!is.na(Stochastic_file_name$D[i])&!is.na(Stochastic_file_name$K[i-1])&!is.na(Stochastic_file_name$D[i-1]))
    {
      Stochastic_file_name$Action[i]<-"Sell"
    }
  } 
  
  #Hold
  
  for(i in 17:length(Stochastic_file_name$Close))
  {
    if (Stochastic_file_name$Action[i]=="Buy")
    {
      for (j in i+1:length(Stochastic_file_name$Close))
      {
        if (Stochastic_file_name$Action[j]!="Sell"&!is.na(Stochastic_file_name$Action[j]))
        {
          Stochastic_file_name$Action[j]<-"Hold"
        }
        else break
      }
    }
  }
  ##Wait
  
  for(i in 17:length(Stochastic_file_name$Close))
  {
    if (Stochastic_file_name$Action[i]=="Sell")
    {
      for (j in i+1:length(Stochastic_file_name$Close))
      {
        if (Stochastic_file_name$Action[j]!="Buy"&!is.na(Stochastic_file_name$Action[j]))
        {
          Stochastic_file_name$Action[j]<-"Wait"
        }
        else break
      }
    }
  }
  
  ##investment price
  Stochastic_file_name$investment_price<-NA
  Stochastic_file_name$investment_price[match("Buy",Stochastic_file_name$Action)]<-Stochastic_file_name$Close[match("Buy",Stochastic_file_name$Action)]
  
  for (m in ((match("Buy",Stochastic_file_name$Action))+1):length(Stochastic_file_name$Close))
  {
    Stochastic_file_name$investment_price[m]<-case_when(
      Stochastic_file_name$Action[m]=="Buy"~Stochastic_file_name$investment_price[m-1],
      Stochastic_file_name$Action[m]=="Wait"~Stochastic_file_name$investment_price[m-1],
      Stochastic_file_name$Action[m]=="Hold"~(Stochastic_file_name$investment_price[m-1]/Stochastic_file_name$Close[m-1])*Stochastic_file_name$Close[m],
      Stochastic_file_name$Action[m]=="Sell"~(Stochastic_file_name$investment_price[m-1]/Stochastic_file_name$Close[m-1])*Stochastic_file_name$Close[m]
    )
  }
  return(Stochastic_file_name)
}
```

**DOW Jones weekly**

```
investment_price(Stochastic2_file(Dow_Jones_weekly),"Stochastic 2nd strategy","Dow_Jones","weekly")
```

![stochastic2 Dow_Jones_weekly](https://user-images.githubusercontent.com/41892582/202988122-7e179008-5b9a-4fbd-91fd-b5e6864b7616.png)

```
yield_by_year(Stochastic2_file(Dow_Jones_weekly),"Stochastic_2nd_strategy","Dow_Jones","weekly")
```

![stochastic2 Dow_Jones_weekly2](https://user-images.githubusercontent.com/41892582/202988125-3941215c-6119-4d1b-952b-7e22e03948b0.png)

```
buy_performance(Stochastic2_file(Dow_Jones_weekly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 2734.70851 price
2                    Profit Factor           1.5199
3           Total Number of Trades              260
4      %Profitable(Winning Trades)            51.5%
5         Average Trade Net Profit   10.51811 price
6                 %Average Revenue            0.72%
7               Avg. Winning Trade   59.66088 price
8                Avg. Losing Trade   41.74484 price
9               Avg. Win: Avg Loss            1.429
10           Largest Winning Trade  406.79283 price
11            Largest Losing Trade  615.99961 Price
12                  Trading Period      52.88 years
13              Time in the Market           37.32%
14 %M D D(intraday peak to valley)          -38.16%
15        portfolio starting price      702.2 price
16           portfolio close price 3958.82447 price
17                         %change          463.77%
18                   highest price 3959.07904 price
19                    lowest price   522.9478 price
```

**DOW Jones monthly**

```
investment_price(Stochastic2_file(Dow_Jones_monthly),"Stochastic 2nd strategy","Dow_Jones","monthly")
```

![stochastic2 Dow_Jones_monthly](https://user-images.githubusercontent.com/41892582/202988118-816eb979-193c-4308-a154-ec969a1775e8.png)

```
yield_by_year(Stochastic2_file(Dow_Jones_monthly),"Stochastic_2nd_strategy","Dow_Jones","monthly")
```

![stochastic2 Dow_Jones_monthly2](https://user-images.githubusercontent.com/41892582/202988120-533ef7b7-2f47-4f76-ab87-a4d3f5657144.png)

```
buy_performance(Stochastic2_file(Dow_Jones_monthly))
```

```
                        Statistics      Long Trades
1                 Total Net Profit 1367.63487 price
2                    Profit Factor           1.6339
3           Total Number of Trades               51
4      %Profitable(Winning Trades)              51%
5         Average Trade Net Profit   26.81637 price
6                 %Average Revenue            2.37%
7               Avg. Winning Trade  135.57869 price
8                Avg. Losing Trade   86.29644 price
9               Avg. Win: Avg Loss            1.571
10           Largest Winning Trade  552.80881 price
11            Largest Losing Trade  788.33907 Price
12                  Trading Period      52.83 years
13              Time in the Market           31.18%
14 %M D D(intraday peak to valley)          -35.86%
15        portfolio starting price      898.1 price
16           portfolio close price 2360.22889 price
17                         %change           162.8%
18                   highest price 2667.64209 price
19                    lowest price  778.34269 price
```

**Gold**

**Gold daily**

```
investment_price(Stochastic2_file(Gold_daily),"Stochastic 2nd strategy","Gold","daily")
```

![stochastic2 Gold_daily](https://user-images.githubusercontent.com/41892582/202988151-10ee1968-f3fe-4227-bdb4-4942ecb44abc.png)

```
yield_by_year(Stochastic2_file(Gold_daily),"Stochastic_2nd_strategy","Gold","daily")
```

![stochastic2 Gold_daily2](https://user-images.githubusercontent.com/41892582/202988152-d89eeb86-ddb1-4d2d-90a1-cba9edc32e65.png)

```
buy_performance(Stochastic2_file(Gold_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit 23.68771 price
2                    Profit Factor         1.0776
3           Total Number of Trades           1385
4      %Profitable(Winning Trades)          46.1%
5         Average Trade Net Profit   0.0171 price
6                 %Average Revenue          0.07%
7               Avg. Winning Trade  0.51454 price
8                Avg. Losing Trade  0.40358 price
9               Avg. Win: Avg Loss          1.275
10           Largest Winning Trade  6.83092 price
11            Largest Losing Trade  7.69348 Price
12                  Trading Period    52.87 years
13              Time in the Market         48.06%
14 %M D D(intraday peak to valley)        -79.94%
15        portfolio starting price     35.2 price
16           portfolio close price 58.88771 price
17                         %change         67.29%
18                   highest price 65.91176 price
19                    lowest price 10.94457 price
```

**Gold weekly**

```
investment_price(Stochastic2_file(Gold_weekly),"Stochastic 2nd strategy","Gold","weekly")
```

![stochastic2 Gold_weekly](https://user-images.githubusercontent.com/41892582/202988161-3551f4bf-2be3-466f-a9f1-b69f436ef4ea.png)

```
yield_by_year(Stochastic2_file(Gold_weekly),"Stochastic_2nd_strategy","Gold","weekly")
```

![stochastic2 Gold_weekly2](https://user-images.githubusercontent.com/41892582/202988163-873c7978-ff40-4c72-901a-14c4e9b7728a.png)

```
buy_performance(Stochastic2_file(Gold_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  260.7001 price
2                    Profit Factor          1.5998
3           Total Number of Trades             306
4      %Profitable(Winning Trades)           47.7%
5         Average Trade Net Profit   0.85196 price
6                 %Average Revenue            0.8%
7               Avg. Winning Trade   4.76247 price
8                Avg. Losing Trade    2.6995 price
9               Avg. Win: Avg Loss           1.764
10           Largest Winning Trade  32.84715 price
11            Largest Losing Trade  27.67645 Price
12                  Trading Period     52.88 years
13              Time in the Market          47.26%
14 %M D D(intraday peak to valley)         -36.14%
15        portfolio starting price      35.8 price
16           portfolio close price 312.01545 price
17                         %change         771.55%
18                   highest price 316.10465 price
19                    lowest price  27.71226 price
```

**Gold monthly**

```
investment_price(Stochastic2_file(Gold_monthly),"Stochastic 2nd strategy","Gold","monthly")
```

![stochastic2 Gold_monthly](https://user-images.githubusercontent.com/41892582/202988155-1883e5bd-94e9-4060-aee8-466e28188411.png)

```
yield_by_year(Stochastic2_file(Gold_monthly),"Stochastic_2nd_strategy","Gold","monthly")
```

![stochastic2 Gold_monthly2](https://user-images.githubusercontent.com/41892582/202988160-b0f7123a-c4b4-42c8-9120-91bc003b0173.png)
 
```
buy_performance(Stochastic2_file(Gold_monthly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit -17.94353 price
2                    Profit Factor          0.9299
3           Total Number of Trades              72
4      %Profitable(Winning Trades)           45.8%
5         Average Trade Net Profit  -0.24922 price
6                 %Average Revenue           0.25%
7               Avg. Winning Trade   7.21788 price
8                Avg. Losing Trade   6.56752 price
9               Avg. Win: Avg Loss           1.099
10           Largest Winning Trade      68.3 price
11            Largest Losing Trade  36.36263 Price
12                  Trading Period     52.83 years
13              Time in the Market          45.98%
14 %M D D(intraday peak to valley)         -69.18%
15        portfolio starting price     101.2 price
16           portfolio close price  83.25647 price
17                         %change         -17.73%
18                   highest price 201.30251 price
19                    lowest price  62.03655 price
```

**EUR/USD Rate**

**EUR/USD daily**

```
investment_price(Stochastic2_file(EurUsd_daily),"Stochastic 2nd strategy","EurUsd","daily")
```

![stochastic2 EurUsd_daily](https://user-images.githubusercontent.com/41892582/202988129-fa256968-b917-4ef0-9757-c87da8c86296.png)

```
yield_by_year(Stochastic2_file(EurUsd_daily),"Stochastic_2nd_strategy","EurUsd","daily")
```

![stochastic2 EurUsd_daily2](https://user-images.githubusercontent.com/41892582/202988133-5c9599b6-70f3-48b3-9d26-ca4e94a3281f.png)

```
buy_performance(Stochastic2_file(EurUsd_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -0.37417 price
2                    Profit Factor         0.7704
3           Total Number of Trades           1111
4      %Profitable(Winning Trades)          44.7%
5         Average Trade Net Profit -0.00034 price
6                 %Average Revenue          -0.1%
7               Avg. Winning Trade  0.00253 price
8                Avg. Losing Trade  0.00263 price
9               Avg. Win: Avg Loss          0.961
10           Largest Winning Trade   0.0313 price
11            Largest Losing Trade  0.04549 Price
12                  Trading Period    51.87 years
13              Time in the Market         47.79%
14 %M D D(intraday peak to valley)        -77.08%
15        portfolio starting price   0.5372 price
16           portfolio close price  0.16303 price
17                         %change        -69.65%
18                   highest price  0.62513 price
19                    lowest price  0.14327 price
```

**EUR/USD weekly**

```
investment_price(Stochastic2_file(EurUsd_weekly),"Stochastic 2nd strategy","EurUsd","weekly")
```

![stochastic2 EurUsd_weekly](https://user-images.githubusercontent.com/41892582/202988143-50bba5dd-f2e1-4553-b959-bd7e1edb02da.png)

```
yield_by_year(Stochastic2_file(EurUsd_weekly),"Stochastic_2nd_strategy","EurUsd","weekly")
```

![stochastic2 EurUsd_weekly2](https://user-images.githubusercontent.com/41892582/202988146-c8a9b654-2a0e-433a-948f-6b8f4ca88b9c.png)

```
buy_performance(Stochastic2_file(EurUsd_weekly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit  -0.0407 price
2                    Profit Factor          0.978
3           Total Number of Trades            232
4      %Profitable(Winning Trades)          46.6%
5         Average Trade Net Profit -0.00018 price
6                 %Average Revenue          0.03%
7               Avg. Winning Trade  0.01673 price
8                Avg. Losing Trade   0.0149 price
9               Avg. Win: Avg Loss          1.123
10           Largest Winning Trade  0.05945 price
11            Largest Losing Trade  0.15282 Price
12                  Trading Period    51.86 years
13              Time in the Market          50.2%
14 %M D D(intraday peak to valley)        -44.05%
15        portfolio starting price   0.5369 price
16           portfolio close price  0.51946 price
17                         %change         -3.25%
18                   highest price  0.88363 price
19                    lowest price  0.49442 price
```

**EUR/USD monthly**

```
investment_price(Stochastic2_file(EurUsd_monthly),"Stochastic 2nd strategy","EurUsd","monthly")
```

![stochastic2 EurUsd_monthly](https://user-images.githubusercontent.com/41892582/202988134-c9b661e1-a9b7-451e-8dec-80366b95f9d5.png)

```
yield_by_year(Stochastic2_file(EurUsd_monthly),"Stochastic_2nd_strategy","EurUsd","monthly")
```

![stochastic2 EurUsd_monthly2](https://user-images.githubusercontent.com/41892582/202988138-88717c13-d37b-4d8a-bad6-eb1eb09c51f1.png)

```
buy_performance(Stochastic2_file(EurUsd_monthly))
```

```
                        Statistics   Long Trades
1                 Total Net Profit 1.42115 price
2                    Profit Factor        2.5752
3           Total Number of Trades            49
4      %Profitable(Winning Trades)         59.2%
5         Average Trade Net Profit   0.029 price
6                 %Average Revenue         2.42%
7               Avg. Winning Trade 0.08012 price
8                Avg. Losing Trade 0.04296 price
9               Avg. Win: Avg Loss         1.865
10           Largest Winning Trade  0.2144 price
11            Largest Losing Trade 0.24992 Price
12                  Trading Period   51.83 years
13              Time in the Market        47.51%
14 %M D D(intraday peak to valley)       -33.74%
15        portfolio starting price  0.7292 price
16           portfolio close price 1.95419 price
17                         %change       167.99%
18                   highest price 2.22306 price
19                    lowest price 0.72382 price
```

**10-Year U.S. Bond Yield (10USY.B)**

**10USY.B daily**

```
investment_price(Stochastic2_file(US_10YB_daily),"Stochastic 2nd strategy","US_10YB","daily")
```

![stochastic2 US_10YB_daily](https://user-images.githubusercontent.com/41892582/202988166-ca0c2e96-0e83-48a8-b5cb-0bbdfab55f94.png)

```
yield_by_year(Stochastic2_file(US_10YB_daily),"Stochastic_2nd_strategy","US_10YB","daily")
```

![stochastic2 US_10YB_daily2](https://user-images.githubusercontent.com/41892582/202988170-6da4c791-6da5-4f67-8c40-eb5e7f81889c.png)

```
buy_performance(Stochastic2_file(US_10YB_daily))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -2.89853 price
2                    Profit Factor         0.9566
3           Total Number of Trades           1021
4      %Profitable(Winning Trades)          48.1%
5         Average Trade Net Profit -0.00284 price
6                 %Average Revenue          0.05%
7               Avg. Winning Trade  0.13024 price
8                Avg. Losing Trade  0.12198 price
9               Avg. Win: Avg Loss          1.068
10           Largest Winning Trade  0.94669 price
11            Largest Losing Trade  1.66835 Price
12                  Trading Period    52.88 years
13              Time in the Market         50.12%
14 %M D D(intraday peak to valley)        -88.26%
15        portfolio starting price     7.77 price
16           portfolio close price  4.94902 price
17                         %change        -36.31%
18                   highest price 12.14516 price
19                    lowest price  1.42609 price
```

**10USY.B weekly**

```
investment_price(Stochastic2_file(US_10YB_weekly),"Stochastic 2nd strategy","US_10YB","weekly")
```

![stochastic2 US_10YB_weekly](https://user-images.githubusercontent.com/41892582/202988181-9ca57684-ff92-4817-aa9b-ebd03d1059cd.png)

```
yield_by_year(Stochastic2_file(US_10YB_weekly),"Stochastic_2nd_strategy","US_10YB","weekly")
```

![stochastic2 US_10YB_weekly2](https://user-images.githubusercontent.com/41892582/202988187-673fcb9d-608c-478a-b4a9-fbd49da9bca2.png)

```
buy_performance(Stochastic2_file(US_10YB_weekly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -7.00011 price
2                    Profit Factor         0.7348
3           Total Number of Trades            240
4      %Profitable(Winning Trades)            40%
5         Average Trade Net Profit -0.02917 price
6                 %Average Revenue         -0.78%
7               Avg. Winning Trade  0.20205 price
8                Avg. Losing Trade  0.18205 price
9               Avg. Win: Avg Loss           1.11
10           Largest Winning Trade  1.03132 price
11            Largest Losing Trade  2.72228 Price
12                  Trading Period    52.88 years
13              Time in the Market         49.89%
14 %M D D(intraday peak to valley)        -98.12%
15        portfolio starting price     7.45 price
16           portfolio close price  0.44989 price
17                         %change        -93.96%
18                   highest price 13.49435 price
19                    lowest price  0.25412 price
```

**10USY.B monthly**

```
investment_price(Stochastic2_file(US_10YB_monthly),"Stochastic 2nd strategy","US_10YB","monthly")
```

![stochastic2 US_10YB_monthly](https://user-images.githubusercontent.com/41892582/202988174-55365aef-467f-455c-9c6a-d51612d2aa80.png)

```
yield_by_year(Stochastic2_file(US_10YB_monthly),"Stochastic_2nd_strategy","US_10YB","monthly")
```

![stochastic2 US_10YB_monthly2](https://user-images.githubusercontent.com/41892582/202988178-73bcd769-afa7-4a0b-9d94-213a652ff3bb.png)

```
buy_performance(Stochastic2_file(US_10YB_monthly))
```

```
                        Statistics    Long Trades
1                 Total Net Profit -3.46102 price
2                    Profit Factor         0.7508
3           Total Number of Trades             55
4      %Profitable(Winning Trades)            40%
5         Average Trade Net Profit -0.06293 price
6                 %Average Revenue         -0.87%
7               Avg. Winning Trade  0.47398 price
8                Avg. Losing Trade  0.42086 price
9               Avg. Win: Avg Loss          1.126
10           Largest Winning Trade  1.31575 price
11            Largest Losing Trade  2.88598 Price
12                  Trading Period    52.83 years
13              Time in the Market         48.82%
14 %M D D(intraday peak to valley)        -91.93%
15        portfolio starting price     5.93 price
16           portfolio close price  2.46898 price
17                         %change        -58.36%
18                   highest price 10.60323 price
19                    lowest price  0.85593 price
```

**US Dollar/USDX - Index - Cash (DX-Y.NYB)**

**USDX Daily**

```
investment_price(Stochastic2_file(USDX_daily),"Stochastic 2nd strategy","USDX","daily")
```

![stochastic2 USDX_daily](https://user-images.githubusercontent.com/41892582/202988192-2211a521-0c3c-469c-ad57-b1882f734c87.png)

```
yield_by_year(Stochastic2_file(USDX_daily),"Stochastic_2nd_strategy","USDX","daily")
```

![stochastic2 USDX_daily2](https://user-images.githubusercontent.com/41892582/202988195-a852426e-6fc9-47c9-9854-bf5699655f45.png)

```
buy_performance(Stochastic2_file(USDX_daily))
```

```
                        Statistics     Long Trades
1                 Total Net Profit -75.14269 price
2                    Profit Factor          0.7333
3           Total Number of Trades            1116
4      %Profitable(Winning Trades)           45.6%
5         Average Trade Net Profit  -0.06733 price
6                 %Average Revenue          -0.08%
7               Avg. Winning Trade   0.40599 price
8                Avg. Losing Trade   0.45969 price
9               Avg. Win: Avg Loss           0.883
10           Largest Winning Trade   4.66596 price
11            Largest Losing Trade   9.48544 Price
12                  Trading Period     51.87 years
13              Time in the Market          50.85%
14 %M D D(intraday peak to valley)         -69.95%
15        portfolio starting price    120.28 price
16           portfolio close price  43.65927 price
17                         %change          -63.7%
18                   highest price 120.40012 price
19                    lowest price  36.17919 price
```

**USDX weekly**

```
investment_price(Stochastic2_file(USDX_weekly),"Stochastic 2nd strategy","USDX","weekly")
```

![stochastic2 USDX_weekly](https://user-images.githubusercontent.com/41892582/202988111-9e42b373-31c9-47ea-8ec9-89dd17916cd9.png)

```
yield_by_year(Stochastic2_file(USDX_weekly),"Stochastic_2nd_strategy","USDX","weekly")
```

![stochastic2 USDX_weekly2](https://user-images.githubusercontent.com/41892582/202988116-fce608c4-c96b-4139-ae29-87a87d293369.png)

```
buy_performance(Stochastic2_file(USDX_weekly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  -34.8068 price
2                    Profit Factor          0.8474
3           Total Number of Trades             231
4      %Profitable(Winning Trades)           45.5%
5         Average Trade Net Profit  -0.15068 price
6                 %Average Revenue          -0.12%
7               Avg. Winning Trade   1.84137 price
8                Avg. Losing Trade   1.79646 price
9               Avg. Win: Avg Loss           1.025
10           Largest Winning Trade   9.83877 price
11            Largest Losing Trade  15.67886 Price
12                  Trading Period     51.86 years
13              Time in the Market          48.36%
14 %M D D(intraday peak to valley)         -49.68%
15        portfolio starting price    119.34 price
16           portfolio close price   84.5332 price
17                         %change         -29.17%
18                   highest price 160.79154 price
19                    lowest price  80.91794 price
```

**USDX monthly**

```
investment_price(Stochastic2_file(USDX_monthly),"Stochastic 2nd strategy","USDX","monthly")
```

![stochastic2 USDX_monthly](https://user-images.githubusercontent.com/41892582/202988200-1e2066a7-cddb-49ab-8286-12d3064e2012.png)

```
yield_by_year(Stochastic2_file(USDX_monthly),"Stochastic_2nd_strategy","USDX","monthly")
```

![stochastic2 USDX_monthly2](https://user-images.githubusercontent.com/41892582/202988205-c47ddc7e-112c-48a9-ac57-641649861df4.png)

```
buy_performance(Stochastic2_file(USDX_monthly))
```

```
                        Statistics     Long Trades
1                 Total Net Profit  32.00601 price
2                    Profit Factor          1.4491
3           Total Number of Trades              33
4      %Profitable(Winning Trades)           60.6%
5         Average Trade Net Profit   0.96988 price
6                 %Average Revenue           0.95%
7               Avg. Winning Trade   5.16364 price
8                Avg. Losing Trade   5.48206 price
9               Avg. Win: Avg Loss           0.942
10           Largest Winning Trade  21.13925 price
11            Largest Losing Trade  23.70638 Price
12                  Trading Period     37.83 years
13              Time in the Market          49.45%
14 %M D D(intraday peak to valley)         -29.32%
15        portfolio starting price    117.51 price
16           portfolio close price 149.51602 price
17                         %change          27.24%
18                   highest price 149.51602 price
19                    lowest price  84.45171 price
```

# Initial conclusions

First, I must state that there is no guarantee that what happened in the market in the past will happen again, even if the conditions are 100% the same. What we are looking for is: what if the indicator analysis can catch a good opportunity to benefit as much as possible from the market, and correct its signals and follow up the market, or not? 

What we have saw thus far regarding the indicators we examined

+ A moving average greater than (12, 26, and 9) may be required to slow it down because the average percent revenue of daily MACD is quite low; this is what we will try to do in phase two. Alternatively, it may be necessary to trade with high leverage.
Weekly MACD delivers higher average revenue percent, higher winning trade percentage, and both with a profitable factor more than 1. monthly MACD gives higher average revenue percent. It seems to be the superior technique to trade in an uptrend with an irresistible average revenue per trade (we will try to define the uptrend numerically and test some hypotheses about it in phase two).

the initial conjecture about the MACD
The more sharp the uptrend, the more you need to use a higher time frame with trades and use the daily only with high leverage trades due to its low average percent revenue per trade.

+ When trading an asset that was uptrending, the RSI daily had a high average revenue percentage, but its signal in a downtrend or horizontal trend does not generate a favorable result.
The same could be said for the weekly and monthly RSI, but when we tried to trade gold (which was experiencing a sharp uptrend), we noticed a negative impact. Phase three will be used to determine whether there is a difference between the Dow Jones uptrend and the gold uptrend or if there is another factor at play.

+ Even if the net profit was enormous, we tended to stick with a low average revenue percentage when using the RSI second strategy. If we change it once more, it appears to be tradeable. Perhaps we might use a higher RSI moving average to slow it down.

+ The first stochastic strategy is nearly identical to the first RSI strategy in that it loses in a down or horizontal trend but has good results in most of its signals in an uptrend, particularly with higher time frames. It appears that the second stochastic strategy is also only uptrend tradeable.


# Further phases

+ First, there is part two of this phase, which is about moving averages (simple and exponential) and some of their strategies.

+ In order to improve each indicator's performance, we will try to change some of its parameters in phase two. For example, we'll experiment with using different values in place of the 12 and 26 moving averages, trying to buy and sell in various RSI zones in place of the 30 and 70 zones, and so on.

+ In phase three, we will use statistical techniques like the T-test and look for P values to test our conclusions from previous steps.

+ A trading strategy using technical indicators will be proposed in phase four, based on what we have learned from all the stages, and it will be tested using some historical random data.

# Notes

- The investment price was calculated without taking into account the buy and sell commissions or any trading costs, which would rise as their number increased.

- The closing price on November 20 is used as the closing price for the entire month in this project's monthly time frame.

- This project should not be considered financial advice. Its sole purpose is to demonstrate how R can be used as a tool for technical analysis.


