### **Eucalyptus And Fire Behavior**
*GEOG-313 Advanced Geoanalytics with Python at Clark University MSGIS*   
*Jason Andrews & Clio Bate*

### About this Project
Final deliverables:  
1. finalproj_part1_plottingfires
2. finalproj_part2_timeseriesGIFS
if youd like to run these notebooks yourself, we've included a data folder containing all the shapefiles in this repository. 
  
These notebooks were developed in Microsoft Planetary Computer Hub and queries an API hosted by Microsoft to collect Landsat imagery stored as a STAC (Spatial Temporal Asset Catalog).
We used a time series analysis to investigate how Eucalyptus, a large tree native to Australia behaves during and after fire events in California. Research suggests that eucalyptus, a large tree native to Australia, benefits from frequent, intense fires. Fires burn at a higher temperature when they reach eucalyptus stands because the oils in the wood and leaves lead to crown ignition. Moreover, stands of eucalyptus accumulate higher and denser levels of combustible fuels than surrounding vegetation, particularly in coastal California.
Being a fire adapted tree, research suggests that eucalyptus will resprout more quickly than other vegetation after high intensity burns, giving it an advantage in post fire succession. 

Research Questions:
Does Eucalyptus recover and resprout faster from fires than the surrounding vegetation?
Can we show these phenomena with real world data analysis?


### Methodology  
#### Data Preperation
1. The first step in this notebook is accessing the fire perimeter polygons and eucalyptus area polygons, which are read in with gpd.read_file as goepandas Geodataframes. These are then filtered to only include fires with greater than 5 acres of eucalyptus.
2. To make sure everything lines up correctly we set the crs for the fires and eucalyptus to 4326, convert each item in each geodataframe to a geojson dictionary, and map them together on a folium map to visualize the areas that burned, and the eucalyptus within these burn zones.
#### xarray Operations
3. Next is the creation of our xarray image stack. The first step here is to filter landsat images to the entire extent and temporal window of our study area: 8 counties in the California Bay Area, and 2002-2023. We use the intersect function with a dictionary representing the convex hull uniary union of the 8 counties in our catalog.search function. 
4. We then pull out the bands needed for our analysis-- NIR, SWIR, R, G,B-- and run them through a bitmask filter to keep ony 'good' pixels.
5. The xarray stack is then further divided into NBR and RGB, and then each of these is temporally compressed into monthly median images.
6. The last step before the data is ready for plotting is to convert the fire and eucalyptus areas go crs 32610, so they match with the xarray stacsk of Landsat imagery.
#### Plotting
7. We clip the NBR xarray to the bounds of each fire and the eucalyptus extent of that fire. Then we mask the part of each fire that is eucalyptus. Then we do a temporal clip to 3 months before the start of each fire, until 24 months afterwards. An average monthly NBR for each area is calculated over this time frame and these are plotted on the same graph. This function exists as a standalone where an individual fire can be called by index, or a loop, that produces a graph for each fire in the set.
#### Time Series
##### For ease of use the Time Series and GIF creation functions are in a diffferent .ipynb notebook
8. Time Series and GIFs require a bounding box for each fire. These are created using the .envelope geopandas function and added (as a list of coordinates) as a nex column in the geodataframe Fires32610.
9. The function Fire_Area_TS takes 3 arguments: index, data_type, and clean=True. Index refers to which fire out of the set you'd like to create a time series for. Data type is either "NBR" or "RGB", and clean defaults to True, if set to false, the script will skip the forward fill function that fills in missing pixels for each frame in the series by taking pixels from the previous image.
10. The Fire_Area_TS function converts the geometry of each item (fire perimeter) in Fires32610 into a geojson dictionary that it can then use to clip the monthly compisite xarray stack.
11. The xarray is also clipped temporaly to the 27 month window around each fire, which is derived by feeding the 'ALARM_DATE' for each fire through pd.DateOffset operators.
12. The result of the function is a frame by frame image set of a specified fire, which can help troubleshoot issues with NBR data or GIF creation.
13. #### GIFs
14. The GIF creation functions do much of the same things as the Time Series functions, clipping bboxes for each fire in the same way. They take two of the same 3 arguments -- index and data_type-- but the third is FPS, which is frames per second. GIFs will automatically be forward filled/cleaned.

### How to use this notebook
This notebook was developed in Microsofts Planetary Computer Hub and uses its API to collect relevent Landsat imagery.
You can clone this repository to your local machine by copying and pasting the following code into your terminal/command prompt  
```
git init
```
followed by:  
```
git clone https://github.com/cvbate/geog313_finalproject.git
```
#### Environment
shapely = v2.0.1    
datetime  
geopandas = v0.13.2  
stackstac = v0.5.0 
dask.array = v2023.5.0  
pystac_client = v0.7.1  
leafmap = v0.21.3  
dask = v2023.5.0  
planetary_computer = v0.5.1   
geogif = v0.1.4  
pandas = v2.0.2
geojson = v3.0.1  
json = v2.0.9  
numpy = v1.24.3  
xarray = v2023.5.0  
rioxarray = v0.14.1  
cartopy.crs  
pyproj = v3.6.0  
matplotlib.pyplot  

#### Functions
1.**plot_fire_by_index(index):** this function takes an arg "index", the index of the desired fire, and will return the monthly veg vs fire area NBR plot for the specified fire. Example: 
![plot_fire_by_index(12)](https://github.com/cvbate/geog313_finalproject/blob/25e5adec061983f7d14ca93135d9d6e404798973/example_images/plot_fire_by_index_example.png)  

2.**Fire_Area_TS(index, data_type, clean= True):**  this function takes three args, "index", data_type(RBG or NBR) and clean (if True, empty pixels will be forwardfilled from images from previous dates) anbd will return a frame by frame of monthly NBR and RGB for a fire bbox specified in the index. This is helpfulf for troubleshooting unexpected NBR mean values in graphs from plot_fire_by_index or for the creation of GIFs.  
Example:   
![Fire_Area_TS(7,'NBR',True)](https://github.com/cvbate/geog313_finalproject/blob/d32a3f0d578e5762b55de41c25ba5f79d234dac6/example_images/Fire_area_TS_example.png)  

3.**Fire_Area_GIF(index):** This function takes an argument "index" creates a GIF of each fire through time.  
Example: Tubbs fire 2 months before ignition   
![Fire_Area_GIF(12))](https://github.com/cvbate/geog313_finalproject/blob/3f09235d6136524a02e8c7348b597652a44c2ad5/example_images/Fire_Area_GIF_example_tubbs.jpeg) 

#### Data Sources
[California Fire Permimeters - California Department of Forestry and Fire Protection hoested CAL FIRE AGOL](https://gis.data.ca.gov/maps/e3802d2abf8741a187e73a9db49d68fe)  
> CalFire published a vector feature class of fire perimeters, dating back to the early 20th century. For this study we have extracted every fire that burned eucalyptus since 2002.
  
[Landcover and Fine Scale Veg](https://pacificvegmap.org/data-downloads/)  
> CalFire contracted with a GIS consultant to create detailed vegetation maps from eight counties in the Bay Area. Eucalyptus is one of the vegetation categories. We downloaded data for each of the 8 counties in CA and then filtered to include only eucalyptus within the perimeters of fires that occured from 2002 onwards.


#### Limitations/ Future Work
While the notebooks are functional, and create the desired outputs, there are several improvements that could be made, and a couple bugs to be resolved.
Limitations:
1. In functions that plot monthly average NBR, fire [9] will not plot. I believe that this is because the date of the fire was less than two years ago, so asking for Landsat imagery in that standard +24 month timeframe will thrown an error. However, an attempt to fix this by defining end_time to current_date has not yet worked.
   current_date = pd.to_datetime(datetime.datetime.now()) - pd.DateOffset(months=1)
   end_time = min(end_time, current_date)
2. When I run the Time Series and GIF creation functions in the same notebook as the monthly NBR plotting it throws off some variable, and I can not return to the the plotting functions without them throwing an error. This is why we've seperated the functions into two notebooks. This is somewhat annoying though as it requires running code to produce the xarray for each.
3. I'd like to add fire/euc outlines to the GIFs. I can not find any documentation that even suggests this is possible. I've done it in GEE. I've tried a couple different approaches to no success so far. 
Future Work:
1. Much of the preprocessing of our vector data was done in ArcGIS Pro-- filtering datasets by year, and spatial relations to eachother. I'd like to develope a pipeline to do the entire thing in Python.
2. Application with another dataset-- Try feeding different datasets into this pipeline. One idea would be to get data on hardwood vs conifer vs grassland vs shrub landcover for a set of fires, and compare NBR between these areas. 
3. Fix 3rd plot function that compages average NBR for all fire/Euc polygons. 

##### Special thanks to our Professor Hamed Alemohammed[^1] and our TA, Aiyin Zhang[^2]!
  
[^1]: [Director, Center for Geospatial Analytics Associate Prof., Graduate School of Geography; Clark University](https://github.com/HamedAlemo)
[^2]: [PhD Cantidate, Graduate School of Geography](https://github.com/zay1996)
