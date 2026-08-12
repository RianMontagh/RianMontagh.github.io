# Learning about MOR File and Creating REMs

## Additional Test with the Current Model

I had an interesting development in the modeling occur this week! I discovered that the settings in the MOR file seem to be controlling the floodplain incision. See Figure 1, where the old MOR file causes the current model to carve channels into the floodplain. The prime suspect was the setting SedThr, which defines the depth of water required for the model to calculate sediment transport. In the old model, this value is only 0.1 m, while in the current model it is 2 m. It seems to me that sediment transport occurs in cells with less than 2 m of water, so it doesn't seems like a value of 0.1 m for the threshold for computing transport is unrealistic. I tried lowering SedThr in the current model, as shown in Figure 1, but did not see the floodplain activate as much as the model with all the old MOR settings. My next thought was that I wanted to go through the other MOR settings to see what else has been changed. I also wanted to do another run with just the old MOR file so that I could isolate the old MOR effects separate from any effects of the old SED file and sediment fractions. 

<img width="1620" alt="image" src="https://github.com/user-attachments/assets/78646005-75e1-4e63-8863-18fa2748bc0e" />

*Figure 1. The current model alongside the current model with the old MOR file and the current model with SedThr = 0.1 m.*

## Additional Test with the Old Model 

For the old model, I wanted to confirm that my current hydrograph has the ability to form the same incision on the floodplain as the 2021 flood used in the old model. I confirmed this is the case, as shown in Figure 2. Because my current hydrograph is a worse flood than the 2021 flood, this run showed additional channels in the distal boundaries of the grid.

<img width="1612" alt="image" src="https://github.com/user-attachments/assets/bf8aef4d-00f9-47e8-b85c-dc475359178b" />

*Figure 2. The old model tests with the fourth test of the current synthetic hydrograph added*

## Examining the MOR File

I started combing through the MOR file to see what changes have been made between the old and current model. I made a table for each different section of settings, see below. 

| MOR Morphology Setting | Old Model | Current Model | Units/class | Default | Description |
| ---------------------- | --------- | ------------- | ----- | ------- | ----------- |
| SedThr | 0.1 | 2 | [m] | 0.5 | Threshold depth for computing sediment transport. Also defines the threshold for wet and dry cells for erosion of dry points. The advection diffusion equation may bring suspended sediment into dry cells but it will not interact with the bed; the only way for this sediment to leave the water column is by an increase in water depth (such that interaction is allowed) or by advective/diffusive transport out of the shallow cell.| 
| DensIn | 0 | 1 | logical | 1 (true) | Include effect of sediment on density gradient and thereby its influence on the turbulence. Remark: a secondary effect of including sediment in the density calculations is a reduction of the flow velocity in the lower computational layers (when compared with a standard logarithmic velocity profile) and a consequent reduction in the computed bed shear stress. | 
| NeglectEntrainment | -- | false | logical | false | compute bed level changes from suspended sediment transport rates instead of from the entrainment and deposition terms; neglect the entrainment of suspended sediment in the mass balance |
| NeuBcSand | -- | true | logical | false | Zero-gradient Neumann boundary condition for noncohesive suspended sediment concentrations at inflow boundaries. Obsolete in Delft3D FM, only applies for Delft3D FLOW. |
| ISlope | 2 | 3 | integer | 2 | bed slope formulation. 1. no bed slope 2. Bagnold formulation 3. Koch & Flokstra formulation 4. Parker & Andrews formulation |
| IHidExp | 1 | 3 | integer | 1 | Hiding and exposure formulation number. 1. no hiding and exposure 2. Egiazaroff formulation 3. Ashida & Michiue formulation 4. Parker et al. formulation 5. Wu, Wang & Jia formulation | 
| SusW | 1 | 0 | [-] | 1.0 | wave-related suspended sediment transport factor | 
| BedW | 1 | 0 | [-] | 1.0 | wave-related bedload sediment transport factor | 
| ThetSD | 0 | 0.5 | [-] | 0.0 | Global / maximum dry cell erosion factor. In other words, the fraction of the erosion to assign (evenly) to the adjacent cells. If ThetSD equals zero the standard scheme is used, i.e. all erosion occurs at the wet cell. If ThetSD equals 1 all erosion that would occur in the wet cell is assigned to the adjacent dry cells and no erosion occurs in the wet cell. |
| HMaxTH | 1.5 | 1 | [m] | 1.0 | Maximum depth for variable THETSD. This is the most confusing variable for me so far. My understanding is that when a cell is barely wet (depth is slightly higher than SedThr) then there is not much transport happening there and you might not want the activity in this cell to affect the dry cells at the full ThetSD. In this case, you set your HMaxThr to be greater than SedThr. Then, instead of the model applying the full ThetSD to redistribute bed change to the dry cells, the model uses the theta defined by this equation. <img width="324" alt="Screenshot 2026-08-11 at 3 30 36 PM" src="https://github.com/user-attachments/assets/ad9608d4-4ad6-48cf-83dc-003feabdb3a9" /> The higher you make the HMaxThr, the smaller the new theta is. ThetSD is the maximum that theta can be and is only possible when h > HMaxTH. In other words, HMaxTH is the depth value at which the full ThetSD will be applied to the dry cells.
| UpdInf | -- | false | logical | false | Update bed levels at inflow boundaries |
| DzMax | -- | 0.05 | [m] | 0.05 | Maximum bed level change per time step expressed as percentage of water depth |

After examining the settings that are different for the morphology section of the MOR file, I identified potential culprits of reducing sediment transport. 
1. Turning DensIn on lowers the velocity near the bed, which reduces shear stress and therefore sediment transport.
2. ThetSD was zero in the old model, which means that no bed change is transferred from wet to dry cells in a time step. This could magnify the amount of erosion in the wet cells and explain why there is more channelization in the old model, because the wet cells are eroding much quicker than their surroundings.
3. The effects of HMaxTH are turned on in the old model and off in the new model. This is because HMaxTH > SedThr in the old model and HMaxTH < SedThr in the current model. I would expect this to make the current model have wet cells that affect the dry cells more than the old model.

Questions:  
- Why are we applying the A&M hiding and exposure formula? The manual says that no external formula should be applied to W&C because it has its own hiding function.
- I need to study the bed slope formulations to understand what is changing there.
- Are SusW and BedW both moot because W&C does not include waves?

| MOR Underlayer Setting | Old Model | Current Model | Units | Default | Description |
| ---------------------- | --------- | ------------- | ----- | ------- | ----------- |
| MxNULyr | 50 | 100 | integer | 1 | Maximum number of underlayers (excluding transport and base layers) in case IUnderLyr=2. |
| TTLForm | 1 | 2 | integer | 1 | Transport layer thickness formulation in case IUnderLyr=2. 1. constant user-defined. 2. proportional to water depth. 3. proportional to bedform height. |
| ThTrLyr | 0.2 | 0.05 | uniform value or file name | not listed | thickness of transport layer [m] in case of TTLForm=1 |
| TTLAlpha | -- | 0.05 | -- | 0.1 | proportionality constant in case of TTLForm=2 or 3 |
| TTLMin | -- | 0 | [m] | 0 | minimum thickness in case of TTLForm=2 or 3 | 
| ThUnLyr | 0.256 | 0.2 | [m] | not listed | characteristic maximum thickness [m] of stratigraphy layers in case IUnderLyr=2 | 

The difference in this section is that the old model uses one uniform active layer while the old model using multiple layers to keep track of the sediment that is deposited or eroded. 

Question:
- What effect does adding the multi-layer functionality have? It does seem more realistic.

| MOR Numerics Setting | Old Model | Current Model | Units | Default | Description |
| -------------------- | --------- | ------------- | ----- | ------- | ----------- |
| UpwindBedload | -- | true | logical | true | use upwind bedload (true, default) or central bedload (false). The central approach is more accurate but less stable (less damping). | 
| LaterallyAveragedBedload | -- | false | logical | false | smoothed bedload transport rates |
| MaximumWaterdepth | -- | true | logical | false | use locally maximum water depth to compute characteristic velocity for sediment transport at cell centre | 
| MaximumWaterdepthFraction | -- | 1 | real number 0-1 | 1 | fraction of the flow depth at links used in finding the maximum water depth. Only used if MaximumWaterdepth=true.

Only real difference in the Numerics section is the MaximumWaterdepth. There is very limited description of this variable, but it seems like turning this on causes the velocity used for sediment transport to be calculated where the water depth (which is at the nodes) is the highest. Is this a stability parameter?

> The Output settings section also had several differences originally, but in the runs I am analyzing they are the same in order to make the output compatible with the merge.slurm script. This script merges the map partitions into one NetCDF file on Hyak. To my understanding, these settings only affect what is being saved, so they shouldn't change how the model is working. 




