**IMPORTANT UPDATE**: This repo and is no longer maintained. While this GitHub repo will be still accessible, the HTTP API (http://data.pystock.com) will only continue to work until **Aug 9, 2019** and the latest data will stay at **Mar 31, 2017** forever.

# US Stock Data

Historical data of US stocks, updated daily.

## How Data are Collected

The data are collected using [pystock-crawler](https://github.com/eliangcs/pystock-crawler)
and [pystock-github](https://github.com/eliangcs/pystock-github). Every day
before the US stock exchanges open at 9:30 EST/EDT, the crawler collects the
stock prices and financial reports, and pushes the data to this repository.
Thus, the latest data is of yesterday. The data are day-based, meaning that
you won't find hourly or minute-level data here.

`pystock-crawler` crawls data from the following sources:

* [NASDAQ.com](http://www.nasdaq.com) for company ticker symbols
* [Yahoo Finance](http://finance.yahoo.com) for stock prices
* [SEC EDGAR](http://www.sec.gov/edgar/searchedgar/companysearch.html) for
  financial reports

## Directory Layout

All data are stored in CSV and TXT files, archived with gzip. The files are
categorized and named by their created dates. For example, a file collected on
2015-03-23 is named `20150323.tar.gz` placed under `2015` directory.

Initial data are the first batch of collected data, whose date range spans from
2009-01-01 to 2015-03-20. They are split into three files: `0001_initial.tar.gz`
to `0003_initial.tar.gz`, under `2015` directory.

Every gzip archive file **may or may not** contain the following files:

* `symbols.txt`
* `prices.csv`
* `reports.csv`

`symbols.txt` is a list of companies, line by line. For example:

```
AAPL	Apple Inc.
FB	Facebook
GOOGL	Google Inc.
```

`prices.csv` contains daily prices in CSV format. Normally, in a
daily-generated file, two trading days of prices are included. This is because
when a company splits its shares, you will need to compare the close price
(`close`) and the adjusted close price (`adj_close`) of the previous trading
day to detect the split. See [Yahoo's explaination](https://help.yahoo.com/kb/finance/historical-prices-sln2311.html)
for more details.

Here's a sample of `prices.csv`:

```
symbol,date,open,high,low,close,volume,adj_close
AAPL,2015-03-23,127.12,127.85,126.52,127.21,36761000,127.21
AAPL,2015-03-20,128.25,128.40,125.16,125.90,67941100,125.90
```

`reports.csv` contains several financial metrics extracted from 10-Q or 10-K
reports. 10-Q is a quarterly report. 10-K is an annual report. The CSV file
looks like:

```
symbol,end_date,amend,period_focus,fiscal_year,doc_type,revenues,op_income,net_income,eps_basic,eps_diluted,dividend,assets,cur_assets,cur_liab,cash,equity,cash_flow_op,cash_flow_inv,cash_flow_fin
GOOG,2009-06-30,False,Q2,2009,10-Q,5522897000.0,1873894000.0,1484545000.0,4.7,4.66,0.0,35158760000.0,23834853000.0,2000962000.0,11911351000.0,31594856000.0,3858684000.0,-635974000.0,46354000.0
GOOG,2009-09-30,False,Q3,2009,10-Q,5944851000.0,2073718000.0,1638975000.0,5.18,5.13,0.0,37702845000.0,26353544000.0,2321774000.0,12087115000.0,33721753000.0,6584667000.0,-3245963000.0,74851000.0
GOOG,2009-12-31,False,FY,2009,10-K,23650563000.0,8312186000.0,6520448000.0,20.62,20.41,0.0,40496778000.0,29166958000.0,2747467000.0,10197588000.0,36004224000.0,9316198000.0,-8019205000.0,233412000.0
```

The financial metrics included are:

* `end_date`: the end date of the period of the filing report
* `amend`: is the filing an amendment?
* `period_focus`: Q1, Q2, Q3 for quarterly reports, or FY for annual reports
* `fiscal_year`: fiscal year of the company
* `doc_type`: 10-Q or 10-K

**Income statement**

* `revenues`: revenues or sales
* `op_income`: operating income
* `net_income`: net income or net earnings
* `eps_basic`: basic earnings per share
* `eps_diluted`: diluted earnings per share
* `dividend`

**Balance sheet**

* `assets`: total assets
* `cur_assets`: current assets
* `cur_liab`: current liabilities
* `cash`: cash and cash equivalents
* `equity`: total equity

**Cash flow**

* `cash_flow_op`: cash from operating activities
* `cash_flow_inv`: cash from investing activities
* `cash_flow_fin`: cash from financing activities

## HTTP API

Besides the Github repo (https://github.com/eliangcs/pystock-data), you can
also use the HTTP API provided by http://data.pystock.com.

This "website" is built with [Github Pages](https://pages.github.com/) and is
a mirror of the git repo. If you're building an application on pystock-data,
http://data.pystock.com is a better choice over the git repo, since not only is
HTTP API easier to use, but you will also benefit from the fast CDN provided by
Github Pages.

To navigate the filesystem on the HTTP API, you just visit index file:

http://data.pystock.com/index.txt

where you can find the index file for each year. For instance:

http://data.pystock.com/2016/index.txt

From there, you will find all the .tar.gz files for that year. For instance:

http://data.pystock.com/2016/20160104.tar.gz
