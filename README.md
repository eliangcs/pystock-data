# US Stock Data

Historical data of US stocks, updated daily.

## How, When, and Where

The data are collected using [pystock-crawler](https://github.com/eliangcs/pystock-crawler)
and [pystock-github](https://github.com/eliangcs/pystock-github). Every day before the US stock exchanges
open (EST/EDT 9:00), the crawler collects the stock prices and financial reports, and pushes the data to
this repository. Thus, the latest data is of yesterday. The data are daily-based, meaning that you won't
find hourly or minute-level data here.

`pystock-crawler` crawls data from the following sources:

* [NASDAQ.com](http://www.nasdaq.com) for company ticker symbols
* [Yahoo Finance](http://finance.yahoo.com) for stock prices
* [SEC EDGAR](http://www.sec.gov/edgar/searchedgar/companysearch.html) for financial reports

## Directory Layout

All data are stored in CSV and TXT files, archived with gzip. The files are categorized and named by their
created dates. For example, a file collected on 2015-03-23 is placed to 2015/20150323.tar.gz. 

Initial data are the first batch of collected data, whose date range spans from 2009-01-01 to 2015-03-20. They are named as 0001_initial.tar.gz - 0003_initial.tar.gz under 2015/ directory.

Every gzip archive file *may* contain the following files:

* `symbols.txt`
* `prices.csv`
* `reports.csv`

`symbols.txt` is a list of companies, line by line. For example:

```
AAPL    Apple Inc.
AMZN    Amazon.com
FB  Facebook
GOOGL   Google Inc.
LNKD    LinkedIn Corporation
```

`prices.csv` contains daily prices in CSV format. Normally, in a daily-collected file, two trading days
of prices are included. This is because a company may split its stock. You will need to compare the close price (close) and the adjusted close price (adj_close) of the previous day to detect a split. Here's a sample of the file content:

```
symbol,date,open,high,low,close,volume,adj_close
AAPL,2015-03-23,127.12,127.85,126.52,127.21,36761000,127.21
AAPL,2015-03-20,128.25,128.40,125.16,125.90,67941100,125.90
```
