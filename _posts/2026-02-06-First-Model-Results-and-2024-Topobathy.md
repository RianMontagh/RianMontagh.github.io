## First Model Run Results and 2024 Topobathy

Last week, I was able to run the 2025 flood model successfully, but wasn't able to fully view the results. This week, I dusted off my MATLAB skills and plotted up results! I want to make sure these results make sense on their own and when compared to high water marks and gage data. 

### Preliminary Results

These results are for the 2025 Flood, which peaked on December 11 at 2:30 am. They are using the 2022 topobathy, which is not accurate for the 2025 flood, but should be fine for making sure the model is reasonable.

#### Water Level

The water level spatial placement seems reasonable; the snapshot below is a few hours after the discharge peak, and shows water overtopping the banks and flowing north to the Overflow boundary. This matches generally with what occurred during the 2025 flood. One thing that is strange to me is the dry cells that are surrounded by wet cells&mdash;this seems unlikely to me unless there is a large, local increase in elevation at these dry cells.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b3eae236-4dd9-418b-afbb-06e3c32a03b1" />


In addition, there were sections of the model where the river channel dries up &mdash; this is definitely not correct. It turns out that this result was from some of the partitioned results missing, and when I redownloaded the data from Hyak, the water in the channel became continuous. It is good to know what it looks like if some of the partitions are missing. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/66d2cd06-d1f0-4f3f-ab2a-3986daae55ba" />

### Processing the 2024 Topobathy

Whatcom County and Northwest Hydraulics Consultants (NHC) have provided us with the 2024 topobathy of the Nooksack Basin, which will be essential to modeling events in 2025. The results in the above section are from a model using the 2025 North Cedarville Hydrograph and the old 2022 topobathy, which has likely changed due to the river eroding and aggrading its channel. To use the elevation raster, I needed to process the raster by resizing it to the size of the model grid, downsampling so that the resolution decreased from about 1x1m to 5x5m, projecting the raster to NAD83 UTM Zone 10N, converting from feet to meters, and converting from a TIF file to an XYZ file that Delft3D can use. This required a combination of tools in ArcGIS Pro and QGIS. Now, I have a _net.nc file (NetCDF grid file with bed level) for the 2022 and 2024 topobathy. Below is the new bed level in the Delft3D GUI.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/e476b52b-1619-413d-b981-b92bdfb3fd24" />

#### Lessons Learned

- File organization is key
  - The project directory should contain a subfolder with the project files and the two code files for running with Hyak.
  - There is only one .mdu file (main project file with general settings)
  - The project subfolder contains multiple input files that the .mdu file can direct to.
  - I started a OneNote to keep track of what each version of the model output means
- The GUI (graphical user interface) is not reliable
  - the project files should be generated and edited as much as possible outside of the GUI due to bugs
  - when saving a project, it is better to export individual files rather than save the whole project&ndash; otherwise the GUI will overwrite your naming conventions.

#### Next Steps with the 2025 Nooksack Model

Boundary Conditions: Consider adding the tributary boundaries and differentiating the two tidal boundaries. They are the same right now.  
Grid: Need to add the new 2024 topobathy. Waiting to receive it, currently using 2022 topobathy.  
Roughness: Using Wuming's  
Settings: Done - mimicked the basic settings of Wuming's but did not turn the special knobs he has turned.  
Initial conditions: initial water level from Wuming.  
Morphology: Need to add in sediment layers and gradations  
Visualizing Results: Need to download Open Earth Tools from Wuming's gscratch once he uploads it. 








