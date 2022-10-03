
# PyViz- Assignment-6

# Import the required libraries and dependencies
```
import pandas as pd

import hvplot.pandas

from pathlib import Path
```
# Import the data
### Using the read_csv function and Path module, create a DataFrame by importing the sfo_neighborhoods_census_data.csv file from the Resources folder
```
sfo_data_df = pd.read_csv(Path("./Resources/sfo_neighborhoods_census_data.csv"))
sfo_data_df.head()
```
```
   year  	neighborhood	sale_price_sqr_foot	 housing_units	gross_rent

0	2010 	Alamo Square	    291.182945	      372560      	1239

1	2010 	Anza Vista	        267.932583        372560	    1239

2	2010	Bayview	            170.098665	      372560	    1239

3	2010	Buena Vista Park	347.394919	      372560     	1239

4	2010	Central Richmond	319.027623        372560    	1239
```
## Review the first and last five rows of the DataFrame
```
sliced_df= sfo_data_df.head(1)

sliced_df= sliced_df.append(sfo_data_df.tail())
sliced_df
```
```
   year	     neighborhood	   sale_price_sqr_foot	housing_units	gross_rent

0   2010	Alamo Square	         291.182945	     372560	        1239
   
392	2016	Telegraph Hill	         903.049771      384242      	4390

393	2016	Twin Peaks	             970.085470      384242     	4390

394	2016	Van Ness/ Civic Center   552.602567	     384242	        4390

395	2016	Visitacion Valley	     328.319007	     384242     	4390

396	2016	Westwood Park	         631.195426 	 384242	        4390
```
# Calculate and Plot the Housing Units per Year

## Step 1: Use the `groupby` function to group the data by year. Aggregate the results by the `mean` of the groups.


### Create a numerical aggregation that groups the data by the year and then averages the results.
```
housing_units_by_year = sfo_data_df.groupby(['year']).mean()

housing_units_by_year
```
```
sale_price_sqr_foot	housing_units	gross_rent
year			
2010	369.344353	372560.0	1239.0
2011	341.903429	374507.0	1530.0
2012	399.389968	376454.0	2324.0
2013	483.600304	378401.0	2971.0
2014	556.277273	380348.0	3528.0
2015	632.540352	382295.0	3739.0
2016	697.643709	384242.0	4390.0
```

### Step 2: Use the `hvplot` function to plot the `housing_units_by_year` DataFrame as a bar chart. Make the x-axis represent the `year` and the y-axis represent the `housing_units`.

### Step 3: Style and format the line plot to ensure a professionally styled visualization.

#### Create a visual aggregation explore the housing units by year
```
housing_plot= housing_units_by_year.hvplot.bar(
    label='Housing units in San Fransisco form 2010 to 2016', x="year", 
    y="housing_units",
    ylim=(365000,385000), 
    yformatter= '%.0f',
    xlabel="Year",                                           
    ylabel="Housing Units", 
    )
housing_plot
```

![visual aggregation explore the housing units by year](https://github.com/AbuzarF/PyViz--Assignment-6/blob/main/bokeh_plot%20(1).png)

### Step 5: Answer the following question:

**Question:** What is the overall trend in housing_units over the period being analyzed?

**Answer:** # The overall trend in the housing_units from 2010 to 2016 is in the upward direction.


# Calculate and Plot the Average Sale Prices per Square Foot

### Step 1: Group the data by year, and then average the results.


#### Create a numerical aggregation by grouping the data by year and averaging the results
```
prices_square_foot_by_year = sfo_data_df.groupby(['year']).mean()
```
#### Review the resulting DataFrame
```
prices_square_foot_by_year
```
```
sale_price_sqr_foot	housing_units	gross_rent
year			
2010	369.344353	372560.0	1239.0
2011	341.903429	374507.0	1530.0
2012	399.389968	376454.0	2324.0
2013	483.600304	378401.0	2971.0
2014	556.277273	380348.0	3528.0
2015	632.540352	382295.0	3739.0
2016	697.643709	384242.0	4390.0
```
**Question:** What is the lowest gross rent reported for the years included in the DataFrame?

**Answer:** # The lowest average gross rent reported in the year 2010 was $1239.0

### Step 2: Create a new DataFrame named `prices_square_foot_by_year` by filtering out the “housing_units” column. The new DataFrame should include the averages per year for only the sale price per square foot and the gross rent.

#### Filter out the housing_units column, creating a new DataFrame Keep only sale_price_sqr_foot and gross_rent averages per year
```
prices_square_foot_by_year = prices_square_foot_by_year.drop(columns=['housing_units'])
```
#### Review the DataFrame
```
prices_square_foot_by_year
sale_price_sqr_foot	gross_rent
```
```
sale_price_sqr_foot	gross_rent
year		
2010	369.344353	1239.0
2011	341.903429	1530.0
2012	399.389968	2324.0
2013	483.600304	2971.0
2014	556.277273	3528.0
2015	632.540352	3739.0
2016	697.643709	4390.0
```
### Step 3: Use hvPlot to plot the `prices_square_foot_by_year` DataFrame as a line plot.

> **Hint** This single plot will include lines for both `sale_price_sqr_foot` and `gross_rent`

### Step 4: Style and format the line plot to ensure a professionally styled visualization.

#### Plot prices_square_foot_by_year inclued labels for the x- and y-axes, and a title.
```
prices_plot= prices_square_foot_by_year.hvplot(
    label='Sale price per square foot and average Gross Rent-2010-2016-San Fransisco', x="year", 
    y=['sale_price_sqr_foot','gross_rent'], 
    xlabel="Year",                                           
    ylabel="Gross Rent/Sale price per square foot",
    )
prices_plot
```
![Sale price per sqft](https://github.com/AbuzarF/PyViz--Assignment-6/blob/main/sale%20price%20per%20sqft.png)

### Step 6: Use both the `prices_square_foot_by_year` DataFrame and interactive plots to answer the following questions:

**Question:** Did any year experience a drop in the average sale price per square foot compared to the previous year?

**Answer:** # Year 2011 experience a drop in the average sale price per square foot compared to 2010.

**Question:** If so, did the gross rent increase or decrease during that year?

**Answer:** # The gross rent increased during the year 2011 compared to 2010.

# Compare the Average Sale Prices by Neighborhood

### Step 1: Create a new DataFrame that groups the original DataFrame by year and neighborhood. Aggregate the results by the `mean` of the groups.


#### Group by year and neighborhood and then create a new dataframe of the mean values
```
prices_by_year_by_neighborhood = sfo_data_df.groupby(['year', 'neighborhood']).mean()
```

#### Review the DataFrame
```
prices_by_year_by_neighborhood.head()
```
```
                     sale_price_sqr_foot	housing_units	gross_rent
year	neighborhood			
2010	Alamo Square	291.182945	         372560.0	      1239.0
      Anza Vista	    267.932583	         372560.0      	  1239.0
      Bayview	        170.098665	         372560.0	      1239.0
      Buena Vista Park	347.394919	         372560.0	      1239.0
      Central Richmond	319.027623	         372560.0	      1239.0
```

### Step 2: Filter out the “housing_units” column to create a DataFrame that includes only the `sale_price_sqr_foot` and `gross_rent` averages per year.


#### Filter out the housing_units
```
prices_by_year_by_neighborhood = prices_by_year_by_neighborhood.drop(columns=['housing_units'])
prices_by_year_by_neighborhood.head()
```
```
                       sale_price_sqr_foot	  gross_rent
year	neighborhood		
2010	Alamo Square	   291.182945       	1239.0
        Anza Vista	       267.932583        	1239.0
        Bayview	           170.098665	        1239.0
        Buena Vista Park   347.394919	        1239.0
        Central Richmond   319.027623	        1239.0
```

#### Review the first and last five rows of the 
```
sliced_prices_df= prices_by_year_by_neighborhood.head(1)
sliced_prices_df= sliced_prices_df.append(prices_by_year_by_neighborhood.tail())
sliced_prices_df
```
```
                               sale_price_sqr_foot	   gross_rent
year	neighborhood		
2010	Alamo Square	        291.182945	            1239.0
2016	Telegraph Hill	        903.049771	            4390.0
        Twin Peaks	            970.085470	            4390.0
        Van Ness/ Civic Center	552.602567	            4390.0
        Visitacion Valley	    328.319007	            4390.0
        Westwood Park	        631.195426	            4390.0
```

### Step 3: Create an interactive line plot with hvPlot that visualizes both `sale_price_sqr_foot` and `gross_rent`. Set the x-axis parameter to the year (`x="year"`). Use the `groupby` parameter to create an interactive widget for `neighborhood`.

### Step 4: Style and format the line plot to ensure a professionally styled visualization.

#### Use hvplot to create an interactive line plot of the average price per square foot. The plot should have a dropdown selector for the neighborhood
```
neighborhood_plot= prices_by_year_by_neighborhood.hvplot.line(
    label='Sale price per square foot and average Gross Rent-2010-2016-By Neighborhood', x="year", 
    y=["sale_price_sqr_foot", "gross_rent"],
    xlabel="Year",                                           
    ylabel="Gross Rent/Sale price per square foot",
    width= 600,
    groupby= 'neighborhood',
    
    )
neighborhood_plot
```
![DataFrame by Year and Nieghborhood](https://github.com/AbuzarF/PyViz--Assignment-6/blob/main/DataFrame%20by%20year%20and%20neighborhood.png)



### Step 6: Use the interactive visualization to answer the following question:

**Question:** For the Anza Vista neighborhood, is the average sale price per square foot for 2016 more or less than the price that’s listed for 2012? 

**Answer:** # For the Anza Vista neighborhood, the average sale price per square foot for 2016 is less than the price that’s listed for 2012. 


#  Build an Interactive Neighborhood Map 

### Step 1: Read the `neighborhood_coordinates.csv` file from the `Resources` folder into the notebook, and create a DataFrame named `neighborhood_locations_df`. Be sure to set the `index_col` of the DataFrame as “Neighborhood”.

#### Load neighborhoods coordinates data
```
neighborhood_locations_df = neighborhood_locations_df = pd.read_csv(
    Path("./Resources/neighborhoods_coordinates.csv"))
```
#### Review the DataFrame
```
neighborhood_locations_df.head()
```
```
Neighborhood	     Lat	      Lon
0	Alamo Square	37.791012	-122.402100
1	Anza Vista	    37.779598	-122.443451
2	Bayview	        37.734670	-122.401060
3	Bayview Heights	37.728740	-122.410980
4	Bernal Heights	37.728630	-122.443050
```
```
neighborhood_locations_df.set_index(neighborhood_locations_df['Neighborhood'], inplace=True)
neighborhood_locations_df
```
```
                   Neighborhood	      Lat	      Lon
Neighborhood			
Alamo Square	   Alamo Square	    37.791012	-122.402100
Anza Vista	       Anza Vista	    37.779598	-122.443451
Bayview	           Bayview	        37.734670	-122.401060
Bayview Heights	   Bayview Heights	37.728740	-122.410980
Bernal Heights	   Bernal Heights	37.728630	-122.443050
```
```
neighborhood_locations_df=neighborhood_locations_df.drop(columns=['Neighborhood']) 

neighborhood_locations_df
```
```
                    Lat	      Lon
Neighborhood		
Alamo Square	  37.791012	 -122.402100
Anza Vista	      37.779598	 -122.443451
Bayview	          37.734670	 -122.401060
Bayview Heights	  37.728740	 -122.410980
Bernal Heights	  37.728630	 -122.443050
```
```
neighborhood_locations_df = neighborhood_locations_df.rename(columns={"Lat": "Latitude", "Lon": "Longitude"})
neighborhood_locations_df.head()
                     Latitude	Longitude
Neighborhood		
Alamo Square	    37.791012	-122.402100
Anza Vista	        37.779598	-122.443451
Bayview	            37.734670	-122.401060
Bayview Heights	    37.728740	-122.410980
Bernal Heights	    37.728630	-122.443050
```
### Step 2: Using the original `sfo_data_df` Dataframe, create a DataFrame named `all_neighborhood_info_df` that groups the data by neighborhood. Aggregate the results by the `mean` of the group.

#### Calculate the mean values for each neighborhood
```
all_neighborhood_info_df = sfo_data_df.groupby(['neighborhood']).mean()
```

#### Review the resulting DataFrame
```
all_neighborhood_info_df.head()

                         year	              sale_price_sqr_foot	housing_units	gross_rent
neighborhood				
Alamo Square	       2013.000000	           366.020712	         378401.0	     2817.285714
Anza Vista	           2013.333333             373.382198	         379050.0	     3031.833333
Bayview	               2012.000000	           204.588623	         376454.0	     2318.400000
Bayview Heights	       2015.000000	           590.792839	         382295.0	     3739.000000
Bernal Heights	       2013.500000	           576.746488	         379374.5	     3080.333333
```

### Step 3: Review the two code cells that concatenate the `neighborhood_locations_df` DataFrame with the `all_neighborhood_info_df` DataFrame.

#### Using the Pandas `concat` function, join the neighborhood_locations_df and the all_neighborhood_info_df DataFrame The axis of the concatenation is "columns".

```
all_neighborhoods_df = pd.concat([neighborhood_locations_df, all_neighborhood_info_df], axis="columns")
```

#### Review the resulting DataFrame
```
display(all_neighborhoods_df.head())
display(all_neighborhoods_df.tail())
```
```

                 Latitude	 Longitude	    year	     sale_price_sqr_foot   housing_units   gross_rent
Alamo Square	 37.791012   -122.402100	2013.000000	     366.020712	        378401.0	   2817.285714
Anza Vista	     37.779598	 -122.443451	2013.333333	     373.382198	        379050.0	   3031.833333
Bayview	         37.734670	 -122.401060	2012.000000	     204.588623	        376454.0	   2318.400000
Bayview Heights	 37.728740	 -122.410980	2015.000000	     590.792839	        382295.0	   3739.000000
Bernal Heights	 37.728630	 -122.443050	NaN	             NaN	            NaN	           NaN

                 Latitude	 Longitude	    year	      sale_price_sqr_foot	housing_units	gross_rent
Yerba Buena	     37.79298	-122.39636	    2012.5	         576.709848	        377427.5	    2555.166667
Bernal Heights	 NaN	     NaN	        2013.5	         576.746488	        379374.5	    3080.333333
Downtown	     NaN	     NaN	        2013.0	         391.434378	        378401.0	    2817.285714
Ingleside	     NaN	     NaN	        2012.5	         367.895144	        377427.5	    2509.000000
Outer Richmond	 NaN	     NaN	        2013.0	         473.900773	        378401.0	    2817.285714
```

#### Call the dropna function to remove any neighborhoods that do not have data
#### Rename the "index" column as "Neighborhood" for use in the Visualization
#### Review the resulting DataFram
```
all_neighborhoods_df = all_neighborhoods_df.reset_index().dropna()

all_neighborhoods_df = all_neighborhoods_df.rename(columns={"index": "Neighborhood"})

display(all_neighborhoods_df.head())
display(all_neighborhoods_df.tail())
```
```
    Neighborhood	 Latitude	 Longitude	 year	    sale_price_sqr_foot	  housing_units	gross_rent
0	Alamo Square	 37.791012	-122.402100	2013.000000	   366.020712	          378401.0	 2817.285714
1	Anza Vista	     37.779598	-122.443451	2013.333333	   373.382198	          379050.0	 3031.833333
2	Bayview	         37.734670	-122.401060	2012.000000	   204.588623	          376454.0	 2318.400000
3	Bayview Heights	 37.728740	-122.410980	2015.000000	   590.792839	          382295.0	 3739.000000
5	Buena Vista Park 37.768160	-122.439330	2012.833333	   452.680591	          378076.5	 2698.833333

    Neighborhood	 Latitude	Longitude	year	    sale_price_sqr_foot	   housing_units gross_rent
68	West Portal	     37.74026	-122.463880	2012.25	       498.488485	          376940.75	 2515.500000
69	Western Addition 37.79298	-122.435790	2012.50	       307.562201	          377427.50	 2555.166667
70	WestwoodHighlands37.73470	-122.456854	2012.00	       533.703935	          376454.00	 2250.500000
71	Westwood Park	 37.73415	-122.457000	2015.00	       687.087575	          382295.00	 3959.000000
72	Yerba Buena	     37.79298	-122.396360	2012.50	       576.709848	          377427.50	 2555.166667
```

### Step 4: Using hvPlot with GeoViews enabled, create a `points` plot for the `all_neighborhoods_df` DataFrame:


   #### Create a plot to analyze neighborhood info
```  
all_neighborhoods_df.hvplot.points(
   'Longitude', 
   'Latitude', 
   geo=True, 
   size='sale_price_sqr_foot',
   scale=.04,
   color='gross_rent',
   tiles='OSM',
   frame_width=700,
   frame_height=500,
   title= 'Nighborhood Interactive Plot')
```
![Neighborhood Interactive Plot](https://github.com/AbuzarF/PyViz--Assignment-6/blob/main/Neighborhood%20Interactive%20plot%20(1).png)



### Step 5: Use the interactive map to answer the following question:

Question: Which neighborhood has the highest gross rent, and which has the highest sale price per square foot?

Answer: # Telegrph Hill has the highest gross rent and the highest sale price per square foot.