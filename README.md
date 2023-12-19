### **Eucalyptus And Fire Behavior**
*GEOG-313 Advanced Geoanalytics with Python at Clark University MSGIS*   
*Jason Andrews & Clio Bate*

### About this Project
This notebook was developed in Microsofts Planetary Computer Hub and uses its API to collect relevent Landsat imagery.
We used a time series analysis to investigate how Eucalyptus, a large tree native to Australia behaves during and after fire events in California. Research suggests that eucalyptus, a large tree native to Australia, benefits from frequent, intense fires. Fires burn at a higher temperature when they reach eucalyptus stands because the oils in the wood and leaves lead to crown ignition. Moreover, stands of eucalyptus accumulate higher and denser levels of combustible fuels than surrounding vegetation, particularly in coastal California.
Being a fire adapted tree, research suggests that eucalyptus will resprout more quickly than other vegetation after high intensity burns, giving it an advantage in post fire succession. 

Research Questions:
Does Eucalyptus recover and resprout faster from fires than the surrounding vegetation?
Can we show these phenomena with real world data analysis?


### Methodology  - verrry rough- just copied and pasted - will fix later
1. The first will produce an NBR comparison plot for each fire
2. second takes an index input and produces a plot for the specified fire
3. The third produces an NBR average for the combined area of all fires and all veg
from 3 months before alarm date until 24 months after


### How to use this notebook
This notebook was developed in Microsofts Planetary Computer Hub and uses its API to collect relevent Landsat imagery.
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
Example:  
![Fire_Area_GIF(index))]() 

#### Data Sources
[California Fire Permimeters - California Department of Forestry and Fire Protection hoested CAL FIRE AGOL](https://gis.data.ca.gov/maps/e3802d2abf8741a187e73a9db49d68fe)  
> CalFire published a vector feature class of fire perimeters, dating back to the early 20th century. For this study we have extracted every fire that burned eucalyptus since 2002.
  
[Landcover and Fine Scale Veg](https://pacificvegmap.org/data-downloads/)  
> CalFire contracted with a GIS consultant to create detailed vegetation maps from eight counties in the Bay Area. Eucalyptus is one of the vegetation categories. We downloaded data for each of the 8 counties in CA and then filtered to include only vegetation from 2002 onwards.


#### Furtherwork/Limitations


##### Special thanks to our Professor Hamed Alemohammed[^1] and our TA, Aiyin Zhang[^2]!
  
[^1]: [Director, Center for Geospatial Analyitics Associate Prof., Graduate School of Geography; Clark University](https://github.com/HamedAlemo)
[^2]: [PhD Cantidate, Graduate School of Geography](https://github.com/zay1996)