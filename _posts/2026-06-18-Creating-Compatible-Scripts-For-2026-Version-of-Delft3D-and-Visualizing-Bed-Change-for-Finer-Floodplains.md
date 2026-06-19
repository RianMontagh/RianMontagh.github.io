# Creating Compatible Scripts For 2026 Version of Delft3D and Visualizing Bed Change for Finer Floodplains

This week with Hyak under maintenance for two days, I focused on working on my scripts that were giving me errors with the new version of Delft3D. This is because the number of variables in the mapfiles decreased, and I haven't looked into which ones these are, because all the variables I have been using are still available. Before, my `readmap.m` function had been hardcoded to the specific amount of variables in the old map files. I recoded this function to be more general, and in the process figured out that the `readDFMPartition.m` function from Open Earth Tools only works on variables that are defined on the grid faces. I also noticed that some variables have a third dimension other than grid face and time, which is related to the six sediment fractions. 

I also updated plots for my floodplain erodibility runs and reached out to Shelby to discuss cross-section averaged profile plots and see how I can improve that analysis. 

Lastly, I started running requential flood runs.

## Plotting Bed Level for the Varying Floodplain Coarseness 

The figure below shows bed change for three model runs with different fining factors. I updated my bed change plots with satellite imagery so that we can now see where the erosion and deposition is occuring. 

<img width="3120" alt="image" src="https://github.com/user-attachments/assets/7c9684de-608d-4835-ab63-81fd37c55a27" />

*Figure 1. Plots of the bed level change for f = 0.50, 0.75, 1.00, 1.25, and 1.50.*

These plots are almost identical (and too small to see clearly), which makes sense with the almost identical amounts of overflow to Sumas I saw last week. 

Here is a full plot of just the f = 1.00 bed change with the aerial imagery. It is interesting to me that the hotspot of bed change on the floodplain is not actually on Main St like I thought but on a smaller farm road. 

<img width="800" alt="image" src="https://github.com/user-attachments/assets/d85a6f39-44f1-4bdf-805d-1566286dea8b" />

*Figure 2. Plot of the bed level change for f = 1.00.*

I also want to be able to visualize the change between the two plots, so I started making comparison plots between the finer and coarser floodplains compared to the f = 1.00 floodplain. Unfortunately I ran into another bug with different grid partitioning with the 2026 model runs, so only the f = 1.25 run is compatible with this differencing. I noticed that in some places this model, which has coarser floodplain sediment, actually eroded more than the f = 1.00 model. 

<img width="800" alt="image" src="https://github.com/user-attachments/assets/a5139d19-7209-455c-a046-0d14cc3f3213" />

Update 6/19:

I thought I was able to fix the issue with comparing the other model runs and got a confusing result.

<img width="2871" alt="image" src="https://github.com/user-attachments/assets/a35ff00b-b655-4a56-b7c1-f29adda28b0c" />

These differenced plots make sense for the f = 1.00, where we see zero difference, and f = 1.25, where we see some slight difference, but for the rest the plots look strangely similar. I checked my file paths and they are the same that I used for the overflow to Sumas amounts, which do make some sense to me. I was expecting the floodplain to be more blue for the f = 0.50 and 0.75 as these floodplains are more erodible, and the f = 1.25 and 1.50 to be more red as these are less erodible floodplains. I think this is also caused by the increased number of partitions from 62 to 64, which causes the cells to be sorted differently and not directly comparable when extracting them as arrays from the model results. 

## Side Quest to Understand Terminology from Open Earth Tools Scripts

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ea21d9bf-901a-444e-ad05-b98a7bf23f32" />

Sidenote that faces are the pressure points and 2D net links are edges. A face is more like a cell center instead of a geometric face of a shape. 

This [site](https://ugrid-conventions.github.io/ugrid-conventions/#2d-triangular-mesh-topology) was really helpful and I wish I had found this sooner. It describes the netCDF conventions for the variables stored that define the meshgrid. These are a little different than what the manual explains. For example, faces and edges are not explicitly defined in the manual but are used in the Open Earth Tool scripts. See below. 

<img width="229" alt="image" src="https://github.com/user-attachments/assets/7c1e1e96-7dbe-413e-af55-6e030a764a1a" />

Wumings `cleanGridData.m` function removes duplicate cells created by the overlapping ghost nodes. It is interesting to me that the Open Earth Tools functions for processing partitioned data don't already do this. I made an edit to the `cleanGridData.m` function so that is also outputs a `clean_NetElemNode` along with the `clean_X` and `clean_Y`, which are the x and y coordinates of the cell faces. The NetElemNode is the array of the variable `mesh2d_face_nodes`, which stores all the nodes (cell corners) that correspond with each cell face (cell center). I feel a lot better now that I understand this better. This means that I can now plot the clean data with duplicate ghost cells removed using the `patch()` function, which needs the NetElemNode information. The patch function is that it takes the vertices of your grid, your information at the cell face, and then fills in the cell based on the information at the face and the nodes that go with that face. 

When I checked the number of faces in my unpartitioned grid and the number of faces in my merged grid from `dflowfm_readNetPartitioned.m` they did not match, and the merged grid has more faces. After running my `cleanGridData` function they do match. Similarly, the dimensions of variables from `readDFMPArtitioned.m` are different and running the `cleanGridData.m` brings the number of faces down to match the number in the unpartitioned grid. 

## Updating Plots with f = 0.50 and 1.50

I added results to my not-so-interesting comparison of discharge to Sumas for the different floodplain erodibilities. These new results are for f = 0.50 and 1.50.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/d0579349-a400-4429-864c-98774f385509" />

*Figure 2. Plots of the overflow to Sumas for all five fining factors.*

Zooming in: 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/d23ad158-47a8-4bfa-953d-d6777ca470d4" />

*Figure 2. Zoomed in plots of the overflow to Sumas for all five fining factors.*

I was wondering if we might see some bigger differences with the f = 0.50 and 1.50 runs, but the same pattern holds where differences in the overflow are very small. The more erodible runs have more overflow than the less erodible runs. The only exception is that the overflow for the f = 1.50 run plots slightly above f = 1.25. 

*Table 1: Max Overflow to Sumas (cms)*

| f = 0.50      |      f = 0.75   |     f = 1.00 |   f = 1.25  |  f = 1.50 |
| ------------- | --------------- | ------------ | ----------- | --------- |
| 1125.36       |    1124.47      | 1124.07      | 1121.78     | 1121.82   |

## Sequential Flood Runs

I made new hydrographs to run at North Cedarville to test what a sequential flood would do in relation to the amount of flow sent north to Sumas. I decided to start with a 100-year, climate-scale, 3-day flood followed by a 10-year, 50-year, or 100-year climate-scaled, 3-day flood. These are in progress and hoping to run them tomorrow. 

