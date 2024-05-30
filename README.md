# SnP500-EDA
***
The work presents the analysis of S&P500 companies using common metrics used by analysts. This analysis aims to relate all numerical metrics to S&P500's ***Recommendation Score*** (i.e., whether to buy to sell) and/or to its ranking based on ***Market Capitalisation***, to better understand the performance of each ***GISC Sectors***.

The above was achieved as such:
- Acquiring the list of S&P500 companies from Wikipedia and then using the stock symbols to web scrap the numerical metrics from Yahoo Finance.
- Cleaning/preparing the data by making it consistent in terms of units and decimal places, and filling in any missing values by either calculating it from raw data or using the mean value.
- Analysing the data by using target questions produced from exploratory data analysis.
- Producing a conclusion of the insights gained and future work to expand on the work.

## Step 1: Crawling Real-World Datasets
***
The dataset that is extracted is about S&P500 stocks. S&P500 is a common equity index which includes 500 of the largest companies listed on stock exchanges in the United States. 

First, the table of S&P500 companies is scraped from Wikipedia's __[S&P500 Companies](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies)__ homepage. The columns of interest from this table are ***Symbol*** of the stock (e.g. AAPL for Apple Inc.), ***Security*** (i.e. the company name), ***Global Industry Classification Standard*** (GICS, i.e. field of work such as Tech, Finance, Healthcare etc) sector, and ***Headquarters Location***. 

Second, common key features used in analysing stocks are scraped from __[Yahoo Finance](https://finance.yahoo.com/)__. The key features of interest are ***Market Capitalisation ***, ***Revenue***, ***Profit Margin***, ***Earning per Share***, ***Profit to Earnings ratio*** and ***Payout Ratio***. These features are scraped by taking the symbols from the table acquired from Wikipedia and using them to create a URL to the respective stock's statistics page (e.g. __https://finance.yahoo.com/quote/AAPL/key-statistics?p=AAPL__ for Apple Inc.) Additionally, a ***Recommendation Score***, ranging from 1 (strong buy) to 5 (strong sell), where 3 is hold, from analysts is also extracted from Yahoo Finance; however, this is achieved by yfinance API library.

The two tables are then merged and saved as `SnP500_raw_data.csv` in the directory of this jupyter notebook

## Step 2: Data Preparation & Cleaning
***
The following is performed sequentially to prepare the data for analysis:
- Column headings are simplified where possible (e.g. ***Security*** is changed to ***Company***) to make it more understandable
- Column ***Headquarters Location*** is split into two columns, ***HQ Country*** and ***HQ State/City***. This is because the initial column contains a mix of 'City,State,Country', 'State,Country' and 'City,Country'
- Missing values are identified and quantified for any columns with numeric values to create a logical strategy to clean data
    1. For columns, ***Market Cap***, ***Revenue***, ***Profit Margin***, ***Earnings per Share***, and ***Payout ratio*** any units with the value is removed and then rounded to 2 decimal places
    2. The list of companies is then ordered from the largest to smallest ***Market Cap***
    3. Missing values in ***Price to Earnings ratio*** are filled by calculating from raw data using the formula:
        - `PE = current share price / (sum of EPS over the last 4 quarters)`
    4. Missing values in ***Recommendation score*** is filled with the mean value
    5. ***Price to Earnings ratio*** values have their units removed if any, values are rounded to 2 decimal places, and any incorrect values are corrected (i.e. where PE ratio doesn't share the same sign as EPS, as illustrated by the equation)

## Step 3: Exploratory Data Analysis and Step 4: Investigating Data-Set with questions
***
The process of EDA and investigating the data set with targeted questions is achieved as such:
- Categorical columns are explored first:
    - Columns ***Symbol*** & ***Company***, by analysing the distribution of the number of characters in symbols and company names. This is followed by looking out for any outliers in symbols or company name
    - **QUESTION 1: Is the length of an S&P500 company symbol and name correlated to the recommendation score of analysts?**
    - Columns ***GICS Sector***, ***HQ Country*** & ***HQ City/State***, by analysing the distribution of counts of S&P500 companies
    - **QUESTION 2: Which HQ countries of S&P500 dominate each of GICS Sector?**
- Numerical columns are explored next:
    - Columns ***Market Cap***, ***Revenue***, ***Profit Margin***, ***Earnings per Share***, ***Price to Earning ratio***, ***Payout ratio***, ***Recommendation Score***, by analysing a statistical summary of data distribution to highlight any columns with visible outliers
    - **QUESTION 3: Which companies are outliers in each of the 7 numerical features?**
    - This is followed by dropping any outliers and exploring the correlation coefficients between numerical values
    - **QUESTION 4: What is the relationship between Market Capitalisation and Revenue?**
- Mixing Categorical and Numerical columns
    - Exploring the following sub-questions:
        - Total Market Cap split across industries
        - Total and average Revenue split across industries
        - Average profit margins per industry
        - Average EPS per industry
        - Average PE ratio per industry
        - Average Payout ratio per industry
        - Average Recommendation score per industry