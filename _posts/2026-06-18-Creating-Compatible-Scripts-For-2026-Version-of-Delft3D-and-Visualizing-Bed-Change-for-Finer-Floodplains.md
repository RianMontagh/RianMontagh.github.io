# Creating Compatible Scripts For 2026 Version of Delft3D and Visualizing Bed Change for Finer Floodplains

This week with Hyak under maintenance for two days, I focused on working on my scripts that were giving me errors with the new version of Delft3D. This is because the number of variables in the mapfiles decreased, and I haven't looked into which ones these are, because all the variables I have been using are still available. Before, my `readmap` function had been hardcoded to the specific amount of variables in the old map files. I recoded this function to be more general, and in the process figured out that the `readDFMPartition` function from Open Earth Tools only works on variables that are defined on the grid faces. I also noticed that some variables have a third dimension other than grid face and time, which is related to the six sediment fractions. 

## Plotting Bed Level for the Varying Floodplain Coarseness 

The figure below shows bed change for three model runs with different fining factors. I updated my bed change plots with satellite imagery so that we can now see where the erosion and deposition is occuring. 

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/421b71fe-ebe8-47bc-9bf5-a611965c65f6" />

*Figure 1. Plots of the bed level change for f = 0.75, 1.00, and 1.25.*

These plots are almost identical, which makes sense with the almost identical amounts of overflow to Sumas I saw last week. I also am curious what my results look like for f = 0.50 and 1.50. I also want to be able to visualize the change between the two plots, so I made comparison plots between the finer and coarser floodplains compared to the f = 1.00 floodplain. 

## Side Quest to Understand Terminology from Open Earth Tools Scripts

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ea21d9bf-901a-444e-ad05-b98a7bf23f32" />

Side note that faces are the pressure points and 2D net links are edges. A face is more like a cell center instead of a geometric face of a shape. 

