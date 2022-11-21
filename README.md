# Salt Lake County Real Estate Analysis

## Project Overview

We will be analyzing real estate data from different cities in the Salt Lake County. In this project, we will be scraping [Zillow](https://www.zillow.com), an American tech real-estate marketplace company. Zillow is one of the most popular real estate sites because offers the most robust suite of tools for home professionals and it sources postings from both the MLS (Multiple Listing Services) and non-MLS sources. Non-MLS sources include for sale by owner, non-MLS foreclosures, and auctions. Additionally, Zillow has the largest database of over 135 million properties. 

After gathering the data, we will create different visualizations with geospatial analysis and charts to uncover preliminary trends between different variables. We will also be using linear regressions and different unsupervised machine learning algorithms such as Principle Component Analysis and K-Means Clustering to gain meaningful insights in the data we collected.

## Resources

+ Analysis Software: `Jupyter Notebook 6.4.12`, `Python 3.10`
+ Data Source: `ZillowData.csv` - [Zillow.com](https://www.zillow.com)

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

Finally, we replaced all null values in `beds` and `baths` with 0 and removed all listings with erroneous `longitude` and `latitude` missing values.

#### Removing Outliers

![BC Home Price Scatter](https://user-images.githubusercontent.com/29410712/203016524-d055c0f4-d538-4c81-babd-31a9c4213548.png)

From the scatterplot above, we can see that the cities: Holladay, Salt Lake City, Sandy, South Jordan, West Jordan, and Draper have problematic outliers. To remove these outliers, we calculated the interquartile range to decipher the outlier cutoff. 

$$
  IQR = Q_3 - Q_1
$$

The outlier cutoff is the interquartile range times by 1.5 and subtracted from the lower and upper bounds. The data points that are above the upper bound and below the lower bound this cutoff will be removed to get a more precise analysis without outliers. The second scatterplot illustrates the data without the outliers:

![AC Home Price Scatter](https://user-images.githubusercontent.com/29410712/203016301-d128efa8-8d7d-40f1-9b49-e41136a83e95.png)

## Preliminary Analysis

![Average Home Price](https://user-images.githubusercontent.com/29410712/203016761-f21ed8fa-2d45-402b-b25f-14834407a9ac.png)

From the chart above, the cities with the highest average home prices are Draper and Holladay. The cities with the lowest average home prices are Kearns and Millcreek. Additionally, the chart remains relatively the same with the average home sqft. The cities with the highest average home sqft are Draper and Herriman and the cities with the lowest average home sqft are still Kearns and Millcreek. This could potentially be explained by the locations of the cities which is explained later in the analysis.

![Average Home Sqft](https://user-images.githubusercontent.com/29410712/203016713-d3fc1c61-6ce1-45fb-88be-6dac9c979050.png)

However, the order of the cities change dramatically based on the average cost per sqft. From the chart below, we can see that the cities with the highest average cost per sqft are Millcreek and Holladay and the cities with the lowest average cost per sqft are West Valley City and Bluffdale.

![Average Cost per Sqft](https://user-images.githubusercontent.com/29410712/203016843-5ead4c2b-7bcf-40b7-b8fb-bc468aec9ee3.png)

We also looked into the correlation between the list price of the homes and the sqft. Based on the scatterplot generated, we can visualize the positive correlation between price and sqft. This means that as the sqft of the homes increase, the prices also increase.

![Price vs Sqft Scatterplot](https://user-images.githubusercontent.com/29410712/203019394-f2f98c0b-1106-4eb8-9ff6-4a7f75567acb.png)

## GeoSpatial Analysis

