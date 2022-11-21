# Salt Lake County Real Estate Analysis

## Project Overview

We will be analyzing real estate data from different cities in the Salt Lake County listed as of November, 2022. In this project, we will be scraping [Zillow](https://www.zillow.com), an American tech real-estate marketplace company. Zillow is one of the most popular real estate sites because offers the most robust suite of tools for home professionals and it sources postings from both the MLS (Multiple Listing Services) and non-MLS sources. Non-MLS sources include for sale by owner, non-MLS foreclosures, and auctions. Additionally, Zillow has the largest database of over 135 million properties. 

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

![saltlake](https://user-images.githubusercontent.com/29410712/203144090-4bd1d62c-f755-4d2d-8b90-1aa00e17cedc.png)

Geospatial data defines specific geographical locations, either in the form of latitude and longitude coordinates or text fields with names of geographical areas, such as countries or states. Geospatial charts combine geospatial data with other forms of data to create map-based charts. Our geodata contains (x, y) coordinates of geographical locations. The geometric shapes in a GeoSeries or GeoDataFrame object are simply a collection of coordinates in an arbitrary space. The Coordinate Reference System (CRS) tells Python how those coordinates relate to places on the Earth and is used to project the location coordinates onto a map for visualization. We will use the WGS84 latitude-longitude projection. 

We will use two of the variables, latitude and longitude, of each listing to visualize the data of Salt Lake County with GeoPandas, a high-level interface Python library for making maps. This can be referred by using the authority code `EPSG:4326`.

We then created heat maps showcasing the distribution of property prices and sqft:

![Price Heat Map](https://user-images.githubusercontent.com/29410712/203021376-8a353e56-80d3-4256-b7b1-59b43c6901c6.png)

From the price heat map of Salt Lake County, we can see that a majority of the higher priced real estate properties are located on the east side of the Salt Lake valley. This corresponds with our previous analysis since Draper and Holladay are located in this area. Additionally, lower priced homes are located on the west side of Salt Lake City.

![Sqft Heat Map](https://user-images.githubusercontent.com/29410712/203021397-f9a790c7-c4cd-484c-9062-64cfc847c19e.png)

In the sqft heat map of Salt Lake County, we can see that the real estate properties with more square feet are located on the east and south-west sides of the Salt Lake Valley which is understandable since the locations are further away from the city center.

## Linear Regression

#### Ordinary Least Squares Assumptions:

1. Standard Errors assume that the covariance matrix of the errors is correctly specified.
2. The condition number is large, 1.22e+03. This might indicate that there are strong multicollinearity or other numerical problems.
3. The linear regression model is “linear in parameters.”
4. There is a random sampling of observations.
5. There is homoscedasticity and no autocorrelation.

We now develop a multilinear regression model for real estate property prices in the Salt Lake County. We could use this to come up with a price for properties coming on the market. Our model is now in the form:

$$
  Y = β_0 + β_1x_1 + β_2x_2 + β_3x_3\ ... β_nx_n + ε
$$

where $x_n$ are predictive variables.

```
                            OLS Regression Results                            
==============================================================================
Dep. Variable:                  price   R-squared:                       0.640
Model:                            OLS   Adj. R-squared:                  0.639
Method:                 Least Squares   F-statistic:                     872.9
Date:                Mon, 21 Nov 2022   Prob (F-statistic):               0.00
Time:                        02:36:54   Log-Likelihood:                -26554.
No. Observations:                1972   AIC:                         5.312e+04
Df Residuals:                    1967   BIC:                         5.315e+04
Df Model:                           4                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
Intercept   1.024e+05   1.28e+04      7.974      0.000    7.72e+04    1.28e+05
sqft         180.8805      5.669     31.907      0.000     169.763     191.998
beds       -8116.8371   3965.622     -2.047      0.041   -1.59e+04    -339.575
baths       2.722e+04   5920.118      4.598      0.000    1.56e+04    3.88e+04
proptype    4.042e+04   9865.190      4.097      0.000    2.11e+04    5.98e+04
==============================================================================
Omnibus:                      882.045   Durbin-Watson:                   1.447
Prob(Omnibus):                  0.000   Jarque-Bera (JB):             7474.571
Skew:                           1.898   Prob(JB):                         0.00
Kurtosis:                      11.750   Cond. No.                     9.78e+03
==============================================================================
```

In the multiple linear regression model, the intercept of the regression is 102,400 and the R-squared is 0.640. The R-squared is the proportion of the variation in the dependent variable that is predictable from the independent variable. This means that around 64.0% of the variability observed in the target variable is explained by this regression model. Additionally, all the variables are statistically significant at an alpha of 5% and can be used to predict the listed price of the real estate property. With further clarification, as the sqft of the real estate property and the number of baths increase by one, the price increases by $180.89 and $27,220, respectively. Moreover, as the number of bedrooms increase by one, the price actually decreases by $8,116. We can also see that if the real estate property is a Single Family Home, it increases the price of the property. 

## Machine Learning Algorithms (Deeper Analysis)

For this analysis, we will be using Principle Component Analysis, K-Means Clustering, Hierarchical Clustering, and DBSCAN to understand how the prices, sqft, and cost per sqft differ between zipcodes.

### Principal Component Analysis (PCA)

Principal Component Analysis (PCA) is one of the most used unsupervised machine learning algorithms across a variety of applications: exploratory data analysis, dimensionality reduction, information compression, and data de-noising. PCA is a dimensionality reduction technique that transforms a set of features in a dataset into a smaller number of features called principal components while at the same time trying to retain as much information in the original dataset as possible. Since we have 3 different variables, we have a three-dimensional data set. PCA can take 4 or more variables and make a two-dimensional PCA plot. PCA can also tell us which variable is the most valuable for clustering the data. It also can tell us how accurate the two-dimensional graph is.

> Python Code:
```
from sklearn.decomposition import PCA

pca_model = PCA(n_components=3)
X_PCA = pca_model.fit_transform(X)
df_plot = pd.DataFrame(X_PCA, columns=['PC1', 'PC2', 'PC3'])
df_plot.head()
fig,ax1 = plt.subplots(figsize=(10, 6))

ax1.set_xlim(X_PCA[:,0].min()-1,X_PCA[:,0].max()+1)
ax1.set_ylim(X_PCA[:,1].min()-1,X_PCA[:,1].max()+1)

for i,name in enumerate(summary['zipcode'].values):
    ax1.annotate(name, (X_PCA[i,0], X_PCA[i,1]), ha='center',fontsize=10)
```

Principal Component Analysis calculates the average of each variable and using this average, finds the center of the data. It then shifts the data so that the center of the data is at the origin. From here, we input principal components. The principal components are vectors, but they are not chosen at random. The first principal component (PC1) is computed so that it explains the greatest amount of variance in the original features. Thus, it minimizes the distance between each data point on the graph (Sum of Squared) so PC1 is a linear combination of variables. 

It uses a scaled vector called the "Eigenvector" or "Singular Vector" for PC1. The sum of squared distances for the best fit line is the Eigenvalue for PC1. The second component (PC2) is orthogonal to the first, and it explains the greatest amount of variance left after the first principal component. Then we find PC3 which is perpendicular to PC1 and PC2 that goes through the origin. The number of PCs is either the number of variables or the number of samples, whichever is smaller.

![PCA](https://user-images.githubusercontent.com/29410712/203155568-68b4015c-9f68-4087-bbb6-0a44f0228fe0.png)

Once all the principal components are figured out, you can use the eigenvalues to determine the proportion of variation that each PC accounts for. Then you can create a scree plot which is a graphical representation of the percentages of variation that each PC accounts for.

![scree-plot](https://user-images.githubusercontent.com/29410712/203156920-bd6d95ed-678d-4a91-8358-fe9daf9a9cdf.png)

In this scree plot, we can see that PC1 and PC2 account for the vast majority of the variation. This means that a two-dimensional graph, using just PC1 and PC2 would be a good approximation of this three-dimensional graph since it would account for 99.22% of the variation in the data. Also, a one-dimensional graph would account for 59.98% of the variation in the data.

### K-Means Cluster Analysis

K-means cluster identifies initial clusters and calculates the variances between each cluster or the Euclidean distance. It clusters all the remaining points, calculates the mean of each cluster, and then reclusters based on the new means. It repeats until the clusters no longer change. It restarts the cluster until it finds the best overall cluster. It does as much reclustering as we tell it to do. It then comes back and returns to the optimal one.

![Different K-Means](https://user-images.githubusercontent.com/29410712/203158939-fde4b778-24a9-4949-b8a5-51a517d1fded.png)

First we need to determine the best K value. An easy method for determining the best number for K is the elbow curve. To create an elbow curve, we'll plot the clusters on the x-axis and the values of a selected objective function on the y-axis. The intra-cluster distance is one of the most common objective functions to use when creating an elbow curve. The intra-cluster distance objective function is measuring the amount of variation in the dataset. For our elbow curve, we will plot the number of clusters (also known as the values of K) on the x-axis and the total intra-cluster distance values on the y-axis.

![Intra-Cluster-Distance](https://user-images.githubusercontent.com/29410712/203158951-caf2a648-746a-4908-9cd1-a423b225d669.png)

Using the "elbow" or "knee of a curve" as a cutoff point is a common heuristic in mathematical optimization to choose a point where diminishing returns are no longer worth the additional cost. In clustering, this means one should choose a number of clusters so that adding another cluster doesn't give much better modeling of the data. The intuition is that increasing the number of clusters will naturally improve the fit (explain more of the variation), since there are more parameters (more clusters) to use, but that at some point this is over-fitting, and the elbow reflects this. We can see that the total intra-cluster distance is large for k = 1 and decreases as we increase k, until k = 6, after which it tapers off and gets only marginally smaller. The slope becomes constant after k = 6. This indicates that k = 6 is a good choice. Therefore, will now cluster the states into six clusters using K-means. 

![K-Mean](https://user-images.githubusercontent.com/29410712/203157630-3e2f35b9-9fe7-4717-bb06-4335d841c807.png)

| Clusters    | Zipcodes   |
| ----------- | -----------|
| 1           | 84020, 84092, 84121, 84093, 84095, 84065, 84096 |
| 2           | 84009, 84094, 84088, 84081, 84084, 84047, 84128, 84120, 84070, 84044, 84129 |
| 3           | 84106, 84102, 84115 |
| 4           | 84101, 84111 |
| 5           | 84105, 84108, 84117, 84109, 84124, 84103 |
| 6           | 84107, 84116, 84118, 84104, 84119, 84123 |

### K-Means Clustering with Principal Component Analysis

By applying the K-means to the Principal Component Analysis projection data, this produces an additional categorical constraint to validate the clustering algorithm. In other words, we can use dimensionality reduction as a feature extractor and reveal the different clusters. Based on the updated PCA plot with the clustering, it is consistent with the clustering with the points split into six sections:

![K-Mean-PCA](https://user-images.githubusercontent.com/29410712/203165056-6b1c828b-6dcc-492f-b01e-61d6f91e2077.png)

### Hierarchical Cluster Analysis

Similar to K-means clustering, hierarchical clustering, also known as agglomerative clustering, works with groups (clusters) of data points. The algorithm starts by declaring each point with its own cluster, then merges the two most similar clusters until a declared stopping point has been reached. Hierarchical clustering is often associated with heatmaps and organizes the rows and columns based on similarity. This makes it easy to see correlations in the data.

![dendogram](https://user-images.githubusercontent.com/29410712/203176302-a42e2a89-f66e-4ade-809a-0b36ba9f99f4.png)

Additionally, we created a dendogram to know how many clusters to make. A dendrogram is a graph that keeps the values of the points on the x-axis, then connects all the points as they are clustered. This is similar to the elbow curve, as it gives us a better idea of the ideal amount of clusters we want to use. Based on the dendogram above, we will now use hierarchical clustering with complete linkage and Euclidean distance to sort the zipcodes into six clusters. 

> Python Code:
```
from sklearn.cluster import AgglomerativeClustering

fig,ax1 = plt.subplots(figsize=(10, 6))
agg_cluster_model = AgglomerativeClustering(linkage="complete", affinity='euclidean', n_clusters=6)
y_pred = agg_cluster_model.fit_predict(X)

plt.scatter(X[:, 0], X[:, 1], c=y_pred,  marker="o")
for i,name in enumerate(summary['zipcode'].values):
    ax1.annotate(name, (X[i,0], X[i,1]), ha='center',fontsize=10)
```

![Hierarchical](https://user-images.githubusercontent.com/29410712/203176736-5c797f25-067d-4624-b98f-dbac6299a9b9.png)

| Clusters    | Zipcodes   |
| ----------- | -----------|
| 1           | 84020, 84092, 84121, 84093, 84095, 84065, 84109, 84096 |
| 2           | 84009, 84094, 84088, 84081, 84084, 84047, 84128, 84120, 84070, 84044, 84129 |
| 3           | 84106, 84102, 84115 |
| 4           | 84101, 84111 |
| 5           | 84105, 84108, 84117, 84124, 84103 |
| 6           | 84107, 84116, 84118, 84104, 84119, 84123 |

### Hierarchical Clustering with Principal Component Analysis

By applying the Hierarichical Clustering to the Principal Component Analysis projection data, this produces an additional categorical constraint to validate the clustering algorithm. In other words, we can use dimensionality reduction as a feature extractor and reveal the different clusters. Based on the updated PCA plot with the clustering, it is consistent with the clustering with the points split into six sections:

![Hierarchical-PCA](https://user-images.githubusercontent.com/29410712/203176836-ca08d232-3bc9-4626-bfae-3cbb8d0ee762.png)


