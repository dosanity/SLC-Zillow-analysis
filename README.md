# Salt Lake County Real Estate Analysis

## Project Overview

We will be analyzing real estate data from different cities in the Salt Lake County. In this project, we will be scraping [Zillow](https://www.zillow.com), an American tech real-estate marketplace company. Zillow is one of the most popular real estate sites because offers the most robust suite of tools for home professionals and it sources postings from both the MLS (Multiple Listing Services) and non-MLS sources. Non-MLS sources include for sale by owner, non-MLS foreclosures, and auctions. Additionally, Zillow has the largest database of over 135 million properties. 

After gathering the data, we will create different visualizations with geospatial analysis and charts to uncover preliminary trends between different variables. We will also be using linear regressions and different unsupervised machine learning algorithms such as Principle Component Analysis and K-Means Clustering to gain meaningful insights in the data we collected.

## Webscraping Process

#### Scrape Zillow using BeautifulSoup

BeautifulSoup is a Python package for parsing HTML and XML documents. It creates a parse tree for parsed pages that can be used to extract data from HTML, which is useful for web scraping. We will be scraping data from different cities in the Salt Lake County from Zillow. We avoided any problems with Zillow blocking us from downloading the data by saving all the HTML files in the data folder. The path to the data folder is stored in the `DATA_PATH` variable. Furthermore , the `ZillowData` folder contains the HTML source code for the different cities. The file name contains the city name with the corresponding page number from Zillow:

|                  |              |     |           |    
|------------------|--------------|-----|-----------|  
|`westjordan1.html`|`draper1.html`| ... |`slc1.html`|
|`westjordan2.html`|`draper2.html`| ... |`slc2.html`|
|`westjordan3.html`|`draper3.html`| ... |`slc3.html`|

#### The Data

We were able to scrape 14 different variables associated with each of the 2,107 properties in this dataset. The data selection, processing, and transformation Python code can be viewed in the `ZillowUT_Data.ipnb` file and the cleaned data can be viewed in the `ZillowData.csv` file.

We also checked the data types and converted any numbers that were read as strings to numerical values. In particular, we converted `type` into a piecewise function where `proptype` takes a binary form of 0 or 1. That is:

$$
  \text{proptype} =
  \begin{cases}
  0,  & \text{if i-th listing is a condo/townhouse/lot} \\
  1, & \text{if i-th listing is a single family house}
  \end{cases}
$$
