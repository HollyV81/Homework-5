

```python
import pandas as pd
import matplotlib.pyplot as plt
```


```python
city_data=pd.read_csv("city_data.csv")
ride_data=pd.read_csv("ride_data.csv")
```


```python
avg_fare=ride_data.groupby("city")["fare"].mean()
total_rides=ride_data.groupby("city")["city"].count()
total_fare=ride_data.groupby("city")["fare"].sum()
```


```python
new_ride= pd.DataFrame(total_rides)
new_ride=new_ride.rename(columns={"city":"Total_Rides"})
new_ride["Average_Fare"]=avg_fare
new_ride["Total_Fare"]=total_fare
new_ride.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total_Rides</th>
      <th>Average_Fare</th>
      <th>Total_Fare</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>31</td>
      <td>23.928710</td>
      <td>741.79</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>26</td>
      <td>20.609615</td>
      <td>535.85</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>9</td>
      <td>37.315556</td>
      <td>335.84</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>22</td>
      <td>23.625000</td>
      <td>519.75</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>19</td>
      <td>21.981579</td>
      <td>417.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_ride=new_ride.reset_index()
```


```python
merged_df = pd.merge(city_data, new_ride, on="city")
merged_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>Total_Rides</th>
      <th>Average_Fare</th>
      <th>Total_Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>28</td>
      <td>21.806429</td>
      <td>610.58</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
      <td>26</td>
      <td>25.899615</td>
      <td>673.39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
      <td>22</td>
      <td>26.169091</td>
      <td>575.72</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
      <td>29</td>
      <td>22.330345</td>
      <td>647.58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
      <td>23</td>
      <td>21.332609</td>
      <td>490.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
merged_df["driver_count"]=merged_df["driver_count"].astype(float)

merged_df["Total_Rides"]=merged_df["Total_Rides"].astype(float)
```


```python
merged_df.dtypes
```




    city             object
    driver_count    float64
    type             object
    Total_Rides     float64
    Average_Fare    float64
    Total_Fare      float64
    dtype: object




```python
urban_df= merged_df.loc[merged_df["type"]== "Urban",:]
suburban_df= merged_df.loc[merged_df["type"]== "Suburban",:]
rural_df= merged_df.loc[merged_df["type"]== "Rural",:]
```


```python
plt.scatter(urban_df.Total_Rides, urban_df.Average_Fare, s=urban_df.driver_count, marker="o", facecolors="lightcoral", edgecolors="black", label="Urban")
plt.scatter(suburban_df.Total_Rides, suburban_df.Average_Fare, s=suburban_df.driver_count, marker="o", facecolors="lightskyblue", edgecolors="black", label="Suburban")
plt.scatter(rural_df.Total_Rides, rural_df.Average_Fare, s=rural_df.driver_count, marker="o", facecolors="gold", edgecolors="black", label="Rural")

plt.title("Pyber Rideshare Date")
plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare ($)')
plt.grid(True)
plt.legend(loc="best")
```




    <matplotlib.legend.Legend at 0x28f232a26d8>




```python
plt.show()
```


![png](output_10_0.png)



```python
urban_fare=urban_df["Total_Fare"].sum()
suburban_fare=suburban_df["Total_Fare"].sum()
rural_fare=rural_df["Total_Fare"].sum()
```


```python
labels=["Urban","Suburban","Rural"]
data=[urban_fare, suburban_fare, rural_fare]
colors=['lightcoral','lightskyblue','gold']
explode= (0, 0.1, 0.1)
plt.pie(data, explode=explode, labels=labels, colors=colors, autopct='%1.1f%%', shadow=True, startangle=220 )
plt.title("% of Total Fares by City Type")
```




    <matplotlib.text.Text at 0x28f23e72f98>




```python
plt.axis('equal')
plt.show()
```


![png](output_13_0.png)



```python
urban_rides=urban_df["Total_Rides"].sum()
suburban_rides=suburban_df["Total_Rides"].sum()
rural_rides=rural_df["Total_Rides"].sum()
```


```python
labels=["Urban","Suburban","Rural"]
data=[urban_rides, suburban_rides, rural_rides]
colors=['lightcoral','lightskyblue','gold']
explode= (0, 0.1, 0.1)
plt.pie(data, explode=explode, labels=labels, colors=colors, autopct='%1.1f%%', shadow=True, startangle=220 )
plt.title("% of Total Rides by City Type")
```




    <matplotlib.text.Text at 0x28f23f0b160>




```python
plt.axis('equal')
plt.show()
```


![png](output_16_0.png)



```python
urban_drivers=urban_df["driver_count"].sum()
suburban_drivers=suburban_df["driver_count"].sum()
rural_drivers=rural_df["driver_count"].sum()
```


```python
labels=["Urban","Suburban","Rural"]
data=[urban_drivers, suburban_drivers, rural_drivers]
colors=['lightcoral','lightskyblue','gold']
explode= (0, 0.1, 0.1)
plt.pie(data, explode=explode, labels=labels, colors=colors, autopct='%1.1f%%', shadow=True, startangle=220 )
plt.title("% of Total Drivers by City Type")
```




    <matplotlib.text.Text at 0x28f250b3a20>




```python
plt.axis('equal')
plt.show()
```


![png](output_19_0.png)

