# Creating Compatible Scripts For 2026 Version of Delft3D and Visualizing Bed Change for Finer Floodplains

This week with Hyak under maintenance for two days, I focused on working on my scripts that were giving me errors with the new version of Delft3D. This is because the number of variables in the mapfiles decreased, and I haven't looked into which ones these are, because all the variables I have been using are still available. Before, my `readmap` function had been hardcoded to the specific amount of variables in the old map files. I recoded this function to be more general, and in the process figured out that the `readDFMPartition` function from Open Earth Tools only works on variables that are defined on the grid faces. I also noticed that some variables have a third dimension other than grid face and time, which is related to the six sediment fractions. 

## Plotting Bed Level for the Varying Floodplain Coarseness 

The figure below shows bed change for three model runs with different fining factors. I updated my bed change plots with satellite imagery so that we can now see where the erosion and deposition is occuring. 

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/421b71fe-ebe8-47bc-9bf5-a611965c65f6" />

*Figure 1. Plots of the bed level change for f = 0.75, 1.00, and 1.25.*

These plots are almost identical, which makes sense with the almost identical amounts of overflow to Sumas I saw last week. I also am curious what my results look like for f = 0.50 and 1.50. I also want to be able to visualize the change between the two plots, so I made comparison plots between the finer and coarser floodplains compared to the f = 1.00 floodplain. 

## Side Quest to Understand Terminology from Open Earth Tools Scripts

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ea21d9bf-901a-444e-ad05-b98a7bf23f32" />

Sidenote that faces are the pressure points and 2D net links are edges. A face is more like a cell center instead of a geometric face of a shape. 

This [site](https://ugrid-conventions.github.io/ugrid-conventions/#2d-triangular-mesh-topology) was really helpful and I wish I had found this sooner. It describes the netCDF conventions for the variables stored that define the meshgrid. These are a little different than what the manual explains. For example, faces and edges are not explicitly defined in the manual but are used in the Open Earth Tool scripts. See below. 

<img width="229" alt="image" src="https://github.com/user-attachments/assets/7c1e1e96-7dbe-413e-af55-6e030a764a1a" />

Wumings `cleanGridData.m` function removes duplicate cells created by the overlapping ghost nodes. It is interesting to me that the Open Earth Tools functions for processing partitioned data don't already do this. I made an edit to the `cleanGridData.m` function so that is also outputs a `clean_NetElemNode` along with the `clean_X` and `clean_Y`, which are the x and y coordinates of the cell faces. The NetElemNode is the array of the variable `mesh2d_face_nodes`, which stores all the nodes (cell corners) that correspond with each cell face (cell center). I feel a lot better now that I understand this better. This means that I can now plot the clean data with duplicate ghost cells removed using the `patch()` function, which needs the NetElemNode information. My understanding of the patch function is that it takes the vertices of your grid, your information at the cell face, and then fills in the cell based on the information at the face and the nodes that go with that face. 

