# Data
The dataset consists of three parts: **census data, geometric data (neighborhood json files), and venue data from Foursqure.com**. In the following I will explain the data acquiring and data cleaning processes for these three types of data: 

## 1. Json files:

Zillow provides neighborhood json files for every cities in the U.S. They are available at “opendatasoft”  (https://data.opendatasoft.com/). They provide the boundary of each neighborhood and also the geometric center of the neighborhood. Please note that the neighborhood number and names may not consistent with the census data described below. I utilized information from Wikipedia and other online information to match the neighborhood in each data. So the final number of neighborhoods and their names in my data may not be the same as those shown in the Zillow json files.

## 2.	Census data:
U.S. census data can be acquired from the United States Census Bureau website : www.census.gov. However, we are interested in census data presented by city neighborhood, which is people usually refer to when they search for certain venues. Here we search online to find the most recent  available Pittsburgh & Cleveland neighborhood census data. Since they may not be in csv form and may not be all up-to-date (we choose year 2018 to be the data collection time), we also explain how we apply data mining and cleaning procedures.
**My goal is to acquire the following census data for each neighborhood at year 2018:**
- Total Population
-	Income (median household income)	
-	White alone  (in percentage)
-	Black alone (in percentage)
-	Asian alone (in percentage)
-	Other races (in percentage)
-	Under age 18	(in percentage)
-	Age 18-64 (in percentage) Age 65 and over (in percentage)
-	With a High School diploma or less (in percentage)
-	With a Bachelor’s degree or higher (in percentage)
### 1)	Pittsburgh data:
The main content of Pittsburgh neighborhood census data at year 2018 is from University of Pittsburgh Center for Social & Urban Research [website](https://ucsur.pitt.edu/files/census/ACS_Pgh_Profile_of_Change_2009-2013_v_2014-2018_Tables.pdf), where they provide census data from 2018-2014:

The data is stored in pdf form. In order to scratch the table from the pdf, we first convert the pdf file into excel using Adobe Acrobat pro and then read in the excel file using Pandas in Python. The median household income information (2016)
comes from the [www.city-data.com](http://www.city-data.com/nbmaps/neigh-Pittsburgh-Pennsylvania.html):
I used the read_html function from Pandas to read in the income table, then a cumulative inflation rate of 4.62% from year 2016 to 2018 to correct income values.
### 2)	Cleveland data:
The main content of Cleveland neighborhood census data at year 2014 comes from [The Center for Community Solutions website](https://www.communitysolutions.com/resources/community-fact-sheets/cleveland-neighborhoods-and-wards/).

The data is again stored in pdf form. I first convert the pdf file into excel using Adobe Acrobat pro and then read in the excel file using Pandas in Python.
To extrapolate the data to year 2018, I used the total population change in Cleveland from 2014 to 2018 acquired from the 
[World Population Review](https://worldpopulationreview.com/us-cities/cleveland-population/).
to correct the total population in each neighborhood. Here I assumed that the population ratio in each neighborhood doesn’t change
from year 2014 to 2018, so are the percentage values in different race, age, and education groups. 
The median household income values are corrected using a cumulative inflation rate of 6.07% from year 2014 to 2018. 

The final census data used in my analysis consist of 88 neighborhoods in Pittsburgh and 36 neighborhoods in Pittsburgh, for which I had went through the process of matching the information in the Zillow json files and those census data. In some cases there are “”missing data,” and they are replaced by the average value of each columns in each city.

In figure 1. I show the distribution of median household income in each neighborhood in Pittsburgh (right panel) and Cleveland (left panel). Please note that both “total population” and “income” columns have been normalized using the “min-max normalization” across these two cities. It is interesting to note that Pittsburgh has much higher average income than Cleveland. 
| ![Figure 1](/images/Pitt_Clev_income_map.png) |
|:--:| 
| **Figure 1** : median household income in each neighborhood in Pittsburgh (right panel) and Cleveland (left panel). Please note that both “total population” and “income” columns have been normalized using the “min-max normalization” across these two cities. |

## 3.	Venue data:
The venue data consists of two parts:
-	Venues in each neighborhood.
-	Information of the target restaurant chains in this study : 
Shake Shack, BIBIBOP Asian Grill, Potbelly Sandwich Shop, and Krispy Krunchy Chicken

The methods and resources to acquire both data are similar: using [Foursquare](foursquare.com) data  to link the geometric information to each venue data. However, the detailed procedure to acquire those data are slightly different. Here I elaborate on the details of those processes:
#### 1)	Venues in each neighborhood:
Since I had the boundaries of each neighborhood provided by the json files, I searched for all the venues lying within each neighborhood.
Please note that although the foursquare sandbox account usage limits the max venue number of each query to be 100, by scanning small overlapping regions within an neighborhood and removing duplicated data, 
I can make sure to acquire a complete list of all venues within a neighborhood. 
To make sure a certain venue is located within a neighborhood, I used the [shapely](pypi.org/project/Shapely/) modules
to do so. In figure 2 I show the spatial distribution of all the venues in red points queried from Foursqure.com for Pittsburgh. 
| ![Figure 2](/images/Pittsburgh_venues.png) |
|:--:| 
| **Figure 2** : Spatial distribution of all the venues in red points queried from Foursqure.com for Pittsburgh. |

#### 2)	Information of the target restaurant chains:
To acquire the location of those restaurants, I searched the their names and find out which Cleveland neighborhood 
they are in based on their locations. Again, I used the “shapely” modules to do so.
In the left panel of figure 3, I show the data of those restaurants (name, coordinate, neighborhood).
In the right panel, their locations are marked by red (Shake Shack), pink (BIBIBOP Asian Grill), blue (Potbelly), and green (Krispy Krunchy Chicken) markers. 
| ![Figure 3](/images/Cleveland_restaurants.png) |
|:--:| 
| **Figure 3**: Left panel: data of restaurants (name, coordinate, neighborhood) in this study. Right panel: restaurant locations marked by red (Shake Shack), pink (BIBIBOP Asian Grill), blue (Potbelly), and green (Krispy Krunchy Chicken) markers. |
