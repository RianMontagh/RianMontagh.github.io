# Learning about MOR File and Creating REMs

## Additional Test with the Current Model

<img width="1612" alt="image" src="https://github.com/user-attachments/assets/09b80b77-6bd3-4dbe-be12-77407b4d76dd" />

<img width="1620" alt="image" src="https://github.com/user-attachments/assets/78646005-75e1-4e63-8863-18fa2748bc0e" />

## Additional Test with the Old Model 

<img width="1612" alt="image" src="https://github.com/user-attachments/assets/bf8aef4d-00f9-47e8-b85c-dc475359178b" />

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
| ThetSD | 0 | 0.5 | [-] | 0.0 | Global / maximum dry cell erosion factor. In other words, the fraction of the erosion to assign (evenly) to the adjacent cells. If ThetSD equals zero the standard scheme is used, i.e. all erosion occurs at the wet cell. If ThetSD equals 1 all erosion that would occur in the wet cell is assigned to the adjacent dry cells. |
| HMaxTH | 1.5 | 1 | [m] | 1.0 | Maximum depth for variable THETSD. 
| UpdInf | -- | false |
| DzMax | -- | 0.05 | 

| MOR Underlayer Setting | Old Model | Current Model | Units  | Description |
| ----------- | --------- | ------------- | ----------- |
| MxNULyr | 50 | 100 |
| TTLForm | 1 | 2 |
| ThTrLyr | 0.2 | 0.05 | 
| TTLAlpha | -- | 0.05 |
| TTLMin | -- | 0 |
| ThUnLyr | 0.256 | 0.2 | 

| MOR Numerics Setting | Old Model | Current Model | Description |
| ----------- | --------- | ------------- | ----------- |
| UpwindBedload | -- | true | 
| LaterallyAveragedBedload | -- | false | 
| MaximumWaterdepth | -- | true | 
| MaximumWaterdepthFraction | -- | 1 | 

> The Output settings section also had several differences originally, but in the runs I am analyzing they are the same in order to make the output compatible with the merge.slurm script. This script merges the map partitions into one NetCDF file on Hyak. To my understanding, these settings only affect what is being saved, so they shouldn't change how the model is working. 




