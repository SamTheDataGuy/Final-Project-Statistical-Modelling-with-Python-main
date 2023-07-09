# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
My goals for this project were to:
- Connect and pull bike station data from the CityBike API for my chosen city of Phoenix, AZ.  Specifically the latitude, longitude, and number of bikes.
- Parse the CityBike JSON object into a Pandas dataframe. 
- Connect to and pull Point of Interest (POI) data from the Foursquare, and Yelp API's
- Parse POI JSON objects to dataframes
- Clean CityBike, Foursquare, and Yelp dataframes
- Perform EDA on CityBike, Foursquare, and Yelp dataframes 
- Merge CityBike, Foursquare, and Yelp dataframes
- Analyze the effect the count, rating, cost, and popularity of POI's has on the number of bikes available and being used
- Build a model to predict the number of bikes available and being used.  I had to change this goal to predicting the number of bikes assigned to a station due to issues with the data.

## Process
### Step 1
Connected to CityBike API an query all data for bike stations in Phoenix, AZ.  I chose this city to honour my grandmother that passed away last week at the age of 96, as it was her favourite vacation destination.

### Step 2
Parsed CityBike JSON to Dataframe and perform EDA.  

I found that the data showed that every single bike was rented from every station, which suggested that there were issues with the quality of the data.  This prevented me from analyzing the ratio of used to free bikes and instead I chose to use the total number of bikes that there could be at each station as my dependant variable.

I also found that one bike station had almost 10,000 bikes assigned to it.  This was a significant outlier as the next highest bikes assigned to a single station was less than 130.  I chose to remove this station from the dataset as it would have significantly skewed the results.

### Step 3
Connect to Foursquare API.  I saved my private API key in a environment variable locally, and used the dotenv library to retrieve it in the notebook.

To pull the data for all POI's within 1000m of each bike station, I built a For Loop that queried the Foursquare API based on the bike station's coordinates (lat and long).  I ensured to only run the first 3 bike stations through the For Loop while writing and testing my code to make sure I didn't run out of API calls.

I then parsed the CityBike JSON to a Dataframe and performed EDA.

I did the same steps for the Yelp API.

### Step 4
Compared Foursquare and Yelp data, and Foursquare's was more consistent with less null and empty values.

### Step 5
Merged CityBike with Foursquare, and CityBike with Yelp data into 2 separate dataframes.  I then created a pivot tables with each bike station as the index and aggreated the count of POI's and the means of 'popularity','fsq_rating', and 'price'.  And finally I concatenated the 2 dataframes into 1, which shows the mean Foursquare and Yelp metrics for the POI's within 1000m of each bike station.

### Step 6
Performed EDA on the concatenated dataframe by visualizing the replationships between the Foursquare and Yelp metrics (rating, proce, etc.) with the total_bikes at each station.

### Step 7
Created a SQLite database and wrote the concatenated dataframe to it.

### Step 8
Created a multivariate linear regression model to predict the number of bikes at a given bike station based on POI metrics from Foursquare and Yelp.  

## Results
All P values were above 0.40, and the highest Adjusted R Squared value I could achieve by eliminating columns was -0.011.  This shows that there is no relation between the chosen metrics, and the model fails to reject the null hypothesis.


## Challenges 
The CityBike data for Phoenix, AZ 2 major issues.  One bike station called "Secret Warehouse" had almost 10,000 bikes assigned to it which could have thrown off results, and the 'free_bikes' field only contained zeros.  I assumed these zeros as missing data (since I don't think every bike in the city was being rented at the same time) and I ignored this column.

## Future Goals
Pick a different city and attempt a build a model that predicts the percentage of bikes rented from a given station.
