Examining the Visual Representation of Relationships in Routing for Uber using Python, Pandas + Matplotlib.

Three Trends: 
    1)Uber Trips per city have a much higher ratio (x4) of Urban trips in comparison to Rural Trips.
    2)Uber Trips in Rural locations are more costly but less frequent in comparison to the trips in more populated and urban areas.
    3)Due to popularity and trends 1 and 2, there will be less drivers per city type in a Rural setting rather than Suburban and Urban (pie chart 3). 


```python
#Dependencies
import pandas as pd
import matplotlib.pyplot as plt 
import numpy as np
```


```python
#Import CSV Data
#Location Path
file1='../raw_data/ride_data.csv'
file2='../raw_data/city_data.csv'

#Reading CSV as DF
ride_df=pd.read_csv(file1)
city_df=pd.read_csv(file2)

#Showing 'Ride_Data.Csv'
ride_df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lake Jonathanshire</td>
      <td>2018-01-14 10:14:22</td>
      <td>13.83</td>
      <td>5739410935873</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Michelleport</td>
      <td>2018-03-04 18:24:09</td>
      <td>30.24</td>
      <td>2343912425577</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Samanthamouth</td>
      <td>2018-02-24 04:29:00</td>
      <td>33.44</td>
      <td>2005065760003</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rodneyfort</td>
      <td>2018-02-10 23:22:03</td>
      <td>23.44</td>
      <td>5149245426178</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South Jack</td>
      <td>2018-03-06 04:28:35</td>
      <td>34.58</td>
      <td>3908451377344</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Showing 'City_Data.Csv'
city_df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Richardfort</td>
      <td>38</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Williamsstad</td>
      <td>59</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Angela</td>
      <td>67</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rodneyfort</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>West Robert</td>
      <td>39</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Merging CSVs, DFs into One and Sorting by City
merged_df = pd.merge(city_df,ride_df,on="city",how="outer")
merged_df = merged_df.sort_values("city")
merged_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1523</th>
      <td>Amandaburgh</td>
      <td>12</td>
      <td>Urban</td>
      <td>2018-01-11 02:22:07</td>
      <td>29.24</td>
      <td>7279902884763</td>
    </tr>
    <tr>
      <th>1522</th>
      <td>Amandaburgh</td>
      <td>12</td>
      <td>Urban</td>
      <td>2018-02-10 20:42:46</td>
      <td>36.17</td>
      <td>6455620849753</td>
    </tr>
    <tr>
      <th>1529</th>
      <td>Amandaburgh</td>
      <td>12</td>
      <td>Urban</td>
      <td>2018-03-13 12:52:31</td>
      <td>13.88</td>
      <td>6222134922674</td>
    </tr>
    <tr>
      <th>1524</th>
      <td>Amandaburgh</td>
      <td>12</td>
      <td>Urban</td>
      <td>2018-01-21 04:12:54</td>
      <td>9.26</td>
      <td>5528427024492</td>
    </tr>
    <tr>
      <th>1525</th>
      <td>Amandaburgh</td>
      <td>12</td>
      <td>Urban</td>
      <td>2018-04-19 16:30:12</td>
      <td>6.27</td>
      <td>4400632718421</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Separating CITY_DF Data, by City Type Only , URBAN
urban_city_df = city_df.loc[city_df["type"]=="Urban"]
urban_city_df.sort_values("city")
#Calculating Driver Count Per City, URBAN
urban_drivers = urban_city_df["driver_count"].tolist()
urban_drivers = [each*5 for each in urban_drivers]
urban_city_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Richardfort</td>
      <td>38</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Williamsstad</td>
      <td>59</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Angela</td>
      <td>67</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rodneyfort</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>West Robert</td>
      <td>39</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Separating CITY_DF Data, by City Type Only , URBAN
rural_city_df = city_df.loc[city_df["type"]=="Rural"]
rural_city_df.sort_values("city")
#Calculating Driver Count Per City, RURAL
rural_drivers = rural_city_df["driver_count"].tolist()
rural_drivers = [each*5 for each in rural_drivers]
rural_drivers
rural_city_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>102</th>
      <td>South Jennifer</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>103</th>
      <td>West Heather</td>
      <td>4</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Newtonview</td>
      <td>1</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>105</th>
      <td>North Holly</td>
      <td>8</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Michaelberg</td>
      <td>6</td>
      <td>Rural</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Separating CITY_DF Data, by City Type Only , SUBURBAN
suburban_city_df = city_df.loc[city_df["type"]=="Suburban"]
suburban_city_df.sort_values("city")
#Calculating Driver Count Per City, SUBURBAN
suburban_drivers= suburban_city_df["driver_count"].tolist()
suburban_drivers = [each*5 for each in suburban_drivers]
suburban_city_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>66</th>
      <td>Port Shane</td>
      <td>7</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Lake Ann</td>
      <td>3</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Lake Scott</td>
      <td>23</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Colemanland</td>
      <td>23</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>70</th>
      <td>New Raymond</td>
      <td>17</td>
      <td>Suburban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Summarizing MERGED_DF by CITY TYPE, URBAN
urban_df = merged_df.loc[merged_df["type"]=="Urban"]
urban_df.sort_values("city")
#Calculating number of Rides/City and Average Fare/City, URBAN
#Grouping Data by City
urban_group = urban_df.groupby("city")

urban_rides_df = urban_group["type"].count()
urban_rides_df

urban_average_fare_df = round(urban_group["fare"].mean(),2)
urban_average_fare_df
```




    city
    Amandaburgh             24.64
    Barajasview             25.33
    Carriemouth             28.31
    Christopherfurt         24.50
    Deanville               25.84
    East Kaylahaven         23.76
    Erikaland               24.91
    Grahamburgh             25.22
    Huntermouth             28.99
    Hurleymouth             25.89
    Jerryton                25.65
    Johnton                 26.79
    Joneschester            22.29
    Justinberg              23.69
    Karenberg               26.34
    Karenside               27.45
    Lake Danielberg         24.84
    Lake Jonathanshire      23.43
    Lake Scottton           23.81
    Leahton                 21.24
    Liumouth                26.15
    Loganberg               25.29
    Martinezhaven           22.65
    New Jacobville          26.77
    New Kimberlyborough     22.59
    New Paulton             27.82
    New Paulville           21.68
    North Barbara           23.49
    North Jasmine           25.21
    North Jason             22.74
                            ...  
    Port Johnbury           23.01
    Port Samanthamouth      25.64
    Raymondhaven            21.48
    Reynoldsfurt            21.92
    Richardfort             22.37
    Roberthaven             23.73
    Robertport              23.06
    Rodneyfort              28.62
    Rogerston               22.10
    Royland                 20.57
    Simpsonburgh            23.36
    South Evanton           26.73
    South Jack              22.97
    South Karenland         26.54
    South Latoya            20.09
    South Michelleport      24.45
    South Phillip           28.57
    Valentineton            24.64
    West Angela             25.99
    West Anthony            24.74
    West Christopherberg    24.42
    West Ericstad           22.35
    West Gabriel            20.35
    West Heidi              23.13
    West Josephberg         21.72
    West Patrickchester     28.23
    West Robert             25.12
    West Samuelburgh        21.77
    Williamsstad            24.36
    Williamsview            26.60
    Name: fare, Length: 66, dtype: float64




```python
#Summarizing MERGED_DF by CITY TYPE,RURAL
rural_df = merged_df.loc[merged_df["type"]=="Rural"]
rural_df.sort_values("city")

#Calculating number of Rides/City and Average Fare/City, RURAL
#Grouping Data by City
rural_group=rural_df.groupby("city")
rural_rides_df = rural_group["type"].count()
rural_rides_df

rural_average_fare_df = round(rural_group["fare"].mean(),2)
rural_average_fare_df
```




    city
    Bradshawfurt         40.06
    Garzaport            24.12
    Harringtonfort       33.47
    Jessicaport          36.01
    Lake Jamie           34.36
    Lake Latoyabury      26.06
    Michaelberg          35.00
    New Ryantown         43.28
    Newtonview           36.75
    North Holly          29.13
    North Jaime          30.80
    Penaborough          35.25
    Randallchester       29.74
    South Jennifer       35.26
    South Marychester    41.87
    South Saramouth      36.16
    Taylorhaven          42.26
    West Heather         33.89
    Name: fare, dtype: float64




```python
#Summarizing MERGED_DF by CITY TYPE, SUBURBAN
suburban_df = merged_df.loc[merged_df["type"]=="Suburban"]
suburban_df.sort_values("city")
#Calculating number of Rides/City and Average Fare/City, SUBURBAN
#Grouping Data by City
suburban_group = suburban_df.groupby("city")

suburban_rides_df = suburban_group["type"].count()
suburban_rides_df

suburban_average_fare_df = round(suburban_group["fare"].mean(),2)
suburban_average_fare_df
```




    city
    Barronchester         36.42
    Bethanyland           32.96
    Brandonfort           35.44
    Colemanland           30.89
    Davidfurt             32.00
    East Aaronbury        25.66
    East Danielview       31.56
    East Kentstad         29.82
    East Marymouth        30.84
    Grayville             27.76
    Josephside            32.86
    Lake Ann              30.89
    Lake Omar             28.07
    Lake Robertside       31.26
    Lake Scott            31.89
    Lewishaven            25.24
    Lewisland             34.61
    Mezachester           30.76
    Myersshire            30.20
    New Olivia            34.05
    New Raymond           27.96
    New Shannonberg       28.38
    Nicolechester         30.91
    North Jeffrey         29.24
    North Richardhaven    24.70
    North Timothy         31.26
    Port Shane            31.08
    Rodriguezview         30.75
    Sotoville             31.98
    South Brenda          33.96
    South Teresa          31.22
    Veronicaberg          32.83
    Victoriaport          27.78
    West Hannah           29.55
    West Kimmouth         29.87
    Williamsonville       31.88
    Name: fare, dtype: float64




```python
#Place relevant data into scatterplot to access trend
#Grouped by City Type 
#URBAN
fig = plt.figure(figsize=(10,8))
plt.grid(True)
sct_urban = plt.scatter(x=urban_rides_df,y=urban_average_fare_df,marker="o",
                        color="lightcoral",s=10*urban_drivers,edgecolor='black', linewidths=1,alpha=0.8,label="Urban")
#RURAL
sct_rural = plt.scatter(x=rural_rides_df,y=rural_average_fare_df,marker="o",
                        color="gold",s=10*rural_drivers,edgecolor='black',linewidths=1,alpha=0.8,label="Rural")
#SUBURBAN
sct_sub = plt.scatter(x=suburban_rides_df,y=suburban_average_fare_df,marker="o",
                      color="lightskyblue",s=10*suburban_drivers,edgecolor='black',linewidths=1,alpha=0.8,label="Suburban")

#LEGENDS, SET SIZE FOR LEGEND +HANDLES
lgnd = plt.legend(handles=[sct_urban,sct_rural,sct_sub],loc="best")
lgnd.legendHandles[0]._sizes = [70]
lgnd.legendHandles[1]._sizes = [70]
lgnd.legendHandles[2]._sizes = [70]

#ADD IN TEXT FOR EXPLANATION OF BUBBLE SIZE
plt.text(36,37,'Note:\nCircle size correlates with driver count per city.',fontsize=10)

#LIMITS, X AND Y
plt.xlim(0,35)
plt.ylim(0,50)

#LABEL AND TITLE
plt.title("Pyber Ride Sharing Data (2016)")
plt.xlabel("Total Number Of Rides(Per City)")
plt.ylabel("Total Average of Fare($)")

plt.show()
```


![png](output_11_0.png)



```python
#Pie Chart, 1,Total Fares by City Type
#Calculating TOTAL FARE for ALL CITIES
fare_total = round(ride_df["fare"].sum(),2)

#Calculating Total Fare from Merged_df, Grouping By Type to find Fares Percentage Per Type
city_grouped_type = merged_df.groupby("type")
fare_total_type = city_grouped_type["fare"].sum()

#Calculating Percentage of Total Fare for Each TYPE
percent_fare_type = [(x/fare_total)*100 for x in fare_total_type]
percent_fare_type
labels = ["Urban", "Suburban", "Rural"]
percent_fare_type
colors = ["Gold", "lightskyBlue", "lightCoral"]
explode = [0, 0, 0.5]
plt.pie(percent_fare_type, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=340)
plt.title("Percentage Of Total Fares by City Type")
plt.axis("equal")
plt.show()
```


![png](output_12_0.png)



```python
#Pie Chart, 2, Total Rides by City Type

#Calculating Total RIDES for All Cities
ride_total= ride_df["ride_id"].count()

#Calculating Total Fare for RIDES PER CITY TYPE
ride_total_type = city_grouped_type["ride_id"].count()
ride_total_type

#Calculating Percentage of Total Fare for Each TYPE
percent_ride_type = [(x/ride_total)*100 for x in ride_total_type]
percent_ride_type
labels = ["Urban", "Suburban", "Rural"]
percent_ride_type
colors = ["Gold", "lightskyBlue", "lightCoral"]
explode = [0, 0, 0.25]
plt.pie(percent_ride_type, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=340)
plt.title("Percentage Of Total Rides by City Type")
plt.axis("equal")
plt.show()

```


![png](output_13_0.png)



```python
#PIE chart, 3, Total Drivers by City Type

driver_total = city_df["driver_count"].sum()

# total drivers for urban
# groupby city type and city
city_grouped_type_city = city_df.groupby("type")
driver_total_type = city_grouped_type_city["driver_count"].sum()

#Calculating Percentage of DRIVERS PER CITY TYPE
percent_drivers_type = [(x/driver_total)*100 for x in driver_total_type]
percent_drivers_type
labels = ["Urban", "Suburban", "Rural"]
percent_drivers_type
colors = ["Gold", "lightskyBlue", "lightCoral"]
explode = [0, 0, 0.25]
plt.pie(percent_drivers_type, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=340)
plt.title("Percentage Of Total Drivers by City Type")
plt.axis("equal")
plt.show()
```


![png](output_14_0.png)

