## Plots for 2024 Topobathy Model and Avulsion Sensitivity Research Plan

Last week, I ran the 2025 model with the 2024 topobathy and did not have a chance to plot the results. This week, I plotted these up and compared them to observational data at USGS gages. I also worked on a plan for the avulsion sensitivity model runs. This means mapping out the parameter space and doing a literature review. 

### Map File Plots 

Of the variables available to plot from the map files (results that spatially vary over the grid as opposed to results at a point or cross-section) I decided velocity magnitude, water depth, and total bed shear stress magnitude were the most interesting to visualize. The peak in the hydrograph occurs on Decemeber 11, 2025 at 3:00 a.m.

<img width="1322" alt="image" src="https://github.com/user-attachments/assets/77600a9d-739c-4a24-bba0-a37c8684f444" />

*Figure 1. Water surface elevation in NAVD88 (m) at 8:00 a.m. on December 11.*

<img width="1288" alt="image" src="https://github.com/user-attachments/assets/b80f0150-f2cb-4aa0-ac0e-045d61ccb593" />

*Figure 2. Velocity magnitude (m/s) at 8:00 a.m. on December 11.*

<img width="1337" alt="image" src="https://github.com/user-attachments/assets/97389089-7dd0-421f-99e3-3d111d362922" />

*Figure 3. Total Bed Shear Stress Magnitude (N/m^2) at 8:00 a.m. on December 11.*



#### Water Level

The water level spatial placement seems reasonable; the snapshot below is a few hours after the discharge peak, and shows water overtopping the banks and flowing north to the Overflow boundary. This matches generally with what occurred during the 2025 flood. One thing that is strange to me is the dry cells that are surrounded by wet cells&mdash;this seems unlikely to me unless there is a large, local increase in elevation at these dry cells. However, Wuming assured me that these are normal. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b3eae236-4dd9-418b-afbb-06e3c32a03b1" />


In addition, there were sections of the model where the river channel dries up &mdash; this is definitely not correct. It turns out that this result was from some of the partitioned results missing, and when I redownloaded the data from Hyak, the water in the channel became continuous. It is good to know what it looks like if some of the partitions are missing. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/66d2cd06-d1f0-4f3f-ab2a-3986daae55ba" />

### Processing the 2024 Topobathy

Whatcom County and Northwest Hydraulics Consultants (NHC) have provided us with the 2024 topobathy of the Nooksack Basin, which will be essential to modeling events in 2025. The results in the above section are from a model using the 2025 North Cedarville Hydrograph and the old 2022 topobathy, which has likely changed due to the river eroding and aggrading its channel. To use the elevation raster, I needed to process the raster by resizing it to the size of the model grid, downsampling so that the resolution decreased from about 1x1m to 5x5m, projecting the raster to NAD83 UTM Zone 10N, converting from feet to meters, and converting from a TIF file to an XYZ file that Delft3D can use. This required a combination of tools in ArcGIS Pro and QGIS. Now, I have a _net.nc file (NetCDF grid file with bed level) for the 2022 and 2024 topobathy. Below is the new bed level in the Delft3D GUI.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/e476b52b-1619-413d-b981-b92bdfb3fd24" />

### 2025 Flood with 2025 Topo

Just before publishing this blog, I was able to check out the output for my 2024 topo run. For some reason, there are no results in the output folder, even though Hyak said that the model completed (as opposed to failed). I will have to figure this issue out today to get results for next week. 

#### Lessons Learned

- Make sure that all 62 partitions of the model are downloaded from Hyak
  - Otherwise, there will be 'holes' in the model
- 1-2 GB is about the limit for the file size that is will work in Delft3D
  - Processing of the 2024 topo reduced the file size from a 19 GB TIF to a 1.6 GB XYZ, which was slow but usable in Delft3D. 

#### Next Steps with the 2025 Nooksack Model

Boundary Conditions: Consider adding the tributary boundaries and differentiating the two tidal boundaries. They are the same right now.  
Roughness: Have it update during model run. 
Morphology: Need to add in sediment layers and gradations, and turn on  morpho.
Avulsion research: Obtain future projected hydrographs
Validation: Validate the 2025 model








