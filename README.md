# API

```python
#Dependencies
import datetime
import matplotlib.pyplot as plt
import pandas as pd
from matplotlib.dates import DateFormatter
import requests
import random
from citipy import citipy

url="https://api.openweathermap.org/data/2.5/weather"
api_key = "dfcce77b90caed738028f45ab2ea7099"
params = {"appid":api_key}

#Creating a data frame with the below coloumns
columns=["City", "Latitude", "Longitude", "Cloudiness", "Humidity", "Max Temp", "Wind Speed", "Country", "Date"]
city_weather_df = pd.DataFrame(columns=columns)

# Creating a counter (initializing with zero)which will break once we get 550cities
num_cities = 0
# get the list of cities
cities = list(citipy.WORLD_CITIES_DICT.keys())
#randomize the city list
random.shuffle(cities)
#Perform API Calls
for city in cities:
    params['lat'] = city[0]
    params['lon'] = city[1]
    req = requests.get(url, params=params)
    print("Requesting URL: " + req.url)
    location = req.json()
    try: 
        city_name = location["name"]
        Latitude=location["coord"]["lat"]
        Longitude=location["coord"]["lon"]
        Cloudiness=location["clouds"]["all"]
        Humidity=location["main"]["humidity"]
        MaxTemp=location["main"]["temp_max"]
        WindSpeed=location["wind"]["speed"]
        Country=location["sys"]["country"]
        Date=location["dt"]
        current_city_data= {
            "City":[city_name],
            "Latitude":[Latitude],
            "Longitude":[Longitude],
            "Cloudiness":[Cloudiness],
            "Humidity":[Humidity],
            "Max Temp":[MaxTemp],
            "Wind Speed":[WindSpeed],
            "Country":[Country],
            "Date":[Date]
        }
        city_weather_df = city_weather_df.append(pd.DataFrame.from_dict(current_city_data))
        num_cities += 1
        if num_cities == 550:
            break
    except KeyError:
        print("Error with city data. Skipping")
        continue
        
        # Display the city_weather Data Frame
city_weather_df.head()
#Scatter plot for Latitude vs Temperature 

plt.scatter(city_weather_df["Latitude"], 
            city_weather_df["Max Temp"],
            edgecolor="blue", linewidths=1, marker="o", 
            alpha=0.8)

# Incorporate the other graph properties
plt.title("City Latitude VS Max Temperature")
plt.ylabel("Max Temp")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim([-80, 100])
plt.ylim([100, 400])
plt.show()
#Scatter plot for Latitude vs. Humidity 
plt.scatter(city_weather_df["Latitude"], 
            city_weather_df["Humidity"],
            edgecolor="blue", linewidths=1, marker="o", 
            alpha=0.8)

# Incorporate the other graph properties
plt.title("City Latitude VS Humidity")
plt.ylabel("Humidity")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim([-80, 100])
plt.ylim([-20, 120])
plt.show()
#Scatter plot for Latitude vs. Cloudiness 

plt.scatter(city_weather_df["Latitude"], 
            city_weather_df["Cloudiness"],
            edgecolor="blue", linewidths=1, marker="o", 
            alpha=0.8)

# Incorporate the other graph properties
plt.title("City Latitude VS Cloudiness")
plt.ylabel("Cloudiness")
plt.xlabel("Latitude")
plt.grid(True)
plt.ylim([-20, 120])
plt.xlim([-80, 100])
plt.show()
#Scatter plot for Latitude vs. Wind Speed 
plt.scatter(city_weather_df["Latitude"], 
            city_weather_df["Wind Speed"],
            edgecolor="blue", linewidths=1, marker="o", 
            alpha=0.8)

# Incorporate the other graph properties
plt.title("City Latitude VS Wind Speed")
plt.ylabel("Wind Speed")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim([-80, 100])
plt.ylim([-5, 40])
plt.show()
