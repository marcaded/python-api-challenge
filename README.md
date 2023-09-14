# python-api-challenge


Had to use this website to learn how to use hvplot

https://hvplot.holoviz.org/user_guide/Introduction.html


city_plot = city_data_df.hvplot.points(
"Lng",
"Lat", 
geo = True,
titles = "OSM",
size = "Humidity",
color = "City",
)


For the code below, I needed to go back a few lectures ago to use some old code for iterating through a df and checking a seperate API for the values in the df

radius = 10000
params = {
    'categories': 'accomodation.hotel',
    'apiKey' : geoapify_key,
    'limit': 20
}

# Print a message to follow up the hotel search
print("Starting hotel search")

# Iterate through the hotel_df DataFrame
for index, row in hotel_df.iterrows():
    # get latitude, longitude from the DataFrame
    latitude = row['Lat']
    longitude = row['Lng']
    
    # Add filter and bias parameters with the current city's latitude and longitude to the params dictionary
    params["filter"] = f"circle:{longitude}, {latitude}, {radius}"
    params["bias"] = f"proximity:{longitude}, {latitude}"
    
    # Set base URL
    base_url = "https://api.geoapify.com/v2/places"


    # Make and API request using the params dictionaty
    name_address = requests.get(base_url, params=params)
    
    # Convert the API response to JSON format
    name_address = name_address.json()
    
    # Grab the first hotel from the results and store the name in the hotel_df DataFrame
    try:
        hotel_df.loc[index, "Hotel Name"] = name_address["features"][0]["properties"]["name"]
    except (KeyError, IndexError):
        # If no hotel is found, set the hotel name as "No hotel found".
        hotel_df.loc[index, "Hotel Name"] = "No hotel found"
        
    # Log the search results
    print(f"{hotel_df.loc[index, 'City']} - nearest hotel: {hotel_df.loc[index, 'Hotel Name']}")

# Display sample data
hotel_df



Had to google and go back to the old notes for the code below as well
city_url = f"{url}appid={weather_api_key}&q={city.replace(' ', '%20')}"

Couldn't remember how to build a URL properly and couldnt find the quick method for building one
