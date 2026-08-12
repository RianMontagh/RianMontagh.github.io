# Learning about MOR File and Creating REMs

## Additional Test with the Current Model

<img width="1620" alt="image" src="https://github.com/user-attachments/assets/78646005-75e1-4e63-8863-18fa2748bc0e" />

*Figure 1. The current model alongside the current model with the old MOR file and the current model with SedThr = 0.1 m.*

## Additional Test with the Old Model 

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


| MOR Underlayer Setting | Old Model | Current Model | Units | Default | Description |
| ---------------------- | --------- | ------------- | ----- | ------- | ----------- |
| MxNULyr | 50 | 100 | integer 1-3 | 1 | Maximum number of underlayers (excluding transport and base layers) in case IUnderLyr=2. |
| TTLForm | 1 | 2 | integer 1-3 | 1 | Transport layer thickness formulation in case IUnderLyr=2. 1. constant user-defined. 2. proportional to water depth. 3. proportional to bedform height. |
| ThTrLyr | 0.2 | 0.05 | uniform value or file name | not listed | thickness of transport layer [m] in case of TTLForm=1 |
| TTLAlpha | -- | 0.05 | -- | 0.1 | proportionality constant in case of TTLForm=2 or 3 |
| TTLMin | -- | 0 | [m] | 0 | minimum thickness in case of TTLForm=2 or 3 | 
| ThUnLyr | 0.256 | 0.2 | [m] | not listed | characteristic maximum thickness [m] of stratigraphy layers in case IUnderLyr=2 | 
 
I didn't know that we were using the multiple layer option for sediment transport. 

| MOR Numerics Setting | Old Model | Current Model | Units | Default | Description |
| -------------------- | --------- | ------------- | ----- | ------- | ----------- |
| UpwindBedload | -- | true | logical | true | use upwind bedload (true, default) or central bedload (false). The central approach is more accurate but less stable (less damping). | 
| LaterallyAveragedBedload | -- | false | logical | false | smoothed bedload transport rates |
| MaximumWaterdepth | -- | true | logical | false | use locally maximum water depth to compute characteristic velocity for sediment transport at cell centre | 
| MaximumWaterdepthFraction | -- | 1 | real number 0-1 | 1 | fraction of the flow depth at links used in finding the maximum water depth. Only used if MaximumWaterdepth=true.

> The Output settings section also had several differences originally, but in the runs I am analyzing they are the same in order to make the output compatible with the merge.slurm script. This script merges the map partitions into one NetCDF file on Hyak. To my understanding, these settings only affect what is being saved, so they shouldn't change how the model is working. 




