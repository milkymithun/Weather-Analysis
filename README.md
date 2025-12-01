🌦 Weather API – Power BI ETL Process
Below is the complete process from Extracting → Transforming → Loading multiple city weather datasets using WeatherAPI and Power BI Power Query.

1️⃣ Extraction (Getting Data from Weather API)

Create an account on WeatherAPI.com and generate your API key.

Go to API Explorer → Forecast section.

Choose:
  
    City (e.g., Hyderabad, Chennai, Noida…)
    
    days = 7 (or any required number)
    
    aqi = yes
    
    alerts = no

The explorer automatically generates a full forecast JSON URL.

Copy this URL (it contains your API key + city name + parameters).

Open Power BI → Get Data → Web, paste the URL, and load the JSON response into Power Query.

2️⃣ Transformation (Power Query Processing)
A. Convert JSON to Table

After loading the JSON, you will see location, current, and forecast records.

Convert the JSON record → table.

Expand each field progressively:

    location
    
    current
    
    current.condition
    
    current.air_quality
    
    forecast
    
    forecast.forecastday
    
    forecast.forecastday.hour

Continue expanding as required.

B. Duplicate the First Query for Each City

  1. Right-click the first city query → Duplicate.
  
  2. For each duplicate, go to:
  Advanced Editor → Update only the city name in the Source URL.
  
  3. Repeat this for all regions you want (Hyderabad, Chennai, Shimla, Noida, etc.).

Each query now loads weather data for one city.

3️⃣ Combining All City Data
A. Append All City Queries

  1. Go to Home → Append Queries → Append Queries as New.
  
  2. Select Two or More Tables.
  
  3. Append all city tables together.
  
  4. Name the appended output table as Master.

This creates a single combined dataset with weather information for all cities.

4️⃣ Creating Clean Analytical Tables
A. Create "Current" Table

  1. Right-click Master → Reference (not duplicate).
  
  2. Remove all forecast-related columns (forecast., forecastday., hour.).
  
  3. Keep only location + current fields.
  
  4. Rename this table to Current.

B. Create "ForecastDay" Table

Again Reference the Master table.

Remove:

    Current columns
    
    Hourly forecast columns (forecast.forecastday.hour)
    
    Keep only daily-level forecast data.

Rename to ForecastDay.

5️⃣ Loading (Final Step)
A. Disable Loading for City Queries

    For all individual city tables:
    
    Right-click each city query → Enable Load (Turn Off)
    
    This prevents unnecessary raw tables from loading to the model.

B. Enable Load Only For:

    Master (combined raw dataset)
    
    Current (clean current weather dataset)
    
    ForecastDay (clean forecast dataset)

6️⃣ Close & Apply

Finally:

Click Close & Apply in Power Query

The cleaned and structured data loads into Power BI Model

Now you can build dashboards such as:
✔ Air quality index analysis
✔ Temperature trends
✔ City-wise weather comparison
✔ Forecast visualizations
