# The MOR Mystery 

## Thoughts on Updated Overflow to Sumas Plot

I updated my overflow to Sumas plot from last week with a few runs that had to be redone. The `Old SED file + old sed frac + ThetSD 0` needed to be rerun with the correct hydrograph and the `SedThr 0.1 + Acal 4` and `Old SED file + old sed frac + SedThr 0.1` needed to be run with a longer runtime limit in the `run_202601.sh` file. Rerunning the `Old SED file + old sed frac + SedThr 0.1` fixed the low overflow values that were shown in the plot last week. I am not sure why that is, but it seems like there is some process that was cut off at the end of the run due to the 24 hour runtime limit that messed up the results of the model. I also reordered the plotting order to be from highest to lowest overflow discharge to make the plot more legible. The zoomed-in plot is still much easier to read. 

<img width="1410" alt="image" src="https://github.com/user-attachments/assets/319055cd-901c-4abc-baf8-1ba485818925" />

*Figure 1. Corrected Plot from last week*

<img width="1390" alt="image" src="https://github.com/user-attachments/assets/909a870f-6493-4514-bdee-3c5bec12747c" />

*Figure 2. Zoomed-in corrected Plot from last week*

There are a few surprising observations from these results which are in the following subsections.

### 1. MOR Mystery

As noted last week, there is still a large-ish gap between the `Old SED file + old sed frac + Old MOR` run and the `Old SED file + old sed frac + SedThr 0.1 + ThetSD 0` run. The only difference between these two models are a few other MOR settings, which is why I called this blog "The MOR Mystery". During my study of the MOR file, SedThr and ThetSD seemed to be the only settings capable of making a difference in the floodplain erodibility, but these results indicate there is more going on. The other settings I have not tested yet and that are different between the old and current model are in the tables below.

| MOR Morphology Setting | Old Model | Current Model | Units/class | Default | Description |
| ---------------------- | --------- | ------------- | ----- | ------- | ----------- |
| DensIn | 0 | 1 | logical | 1 (true) | Include effect of sediment on density gradient and thereby its influence on the turbulence. Remark: a secondary effect of including sediment in the density calculations is a reduction of the flow velocity in the lower computational layers (when compared with a standard logarithmic velocity profile) and a consequent reduction in the computed bed shear stress. | 
| NeuBcSand | -- | true | logical | false | Zero-gradient Neumann boundary condition for noncohesive suspended sediment concentrations at inflow boundaries. Obsolete in Delft3D FM, only applies for Delft3D FLOW. |
| ISlope | 2 | 3 | integer | 2 | bed slope formulation. 1. no bed slope 2. Bagnold formulation 3. Koch & Flokstra formulation 4. Parker & Andrews formulation |
| IHidExp | 1 | 3 | integer | 1 | Hiding and exposure formulation number. 1. no hiding and exposure 2. Egiazaroff formulation 3. Ashida & Michiue formulation 4. Parker et al. formulation 5. Wu, Wang & Jia formulation | 
| HMaxTH | 1.5 | 1 | [m] | 1.0 | Maximum depth for variable THETSD. This is the most confusing variable for me so far. My understanding is that when a cell is barely wet (depth is slightly higher than SedThr) then there is not much transport happening there and you might not want the activity in this cell to affect the dry cells at the full ThetSD. In this case, you set your HMaxThr to be greater than SedThr. Then, instead of the model applying the full ThetSD to redistribute bed change to the dry cells, the model uses the theta defined by this equation. <img width="324" alt="Screenshot 2026-08-11 at 3 30 36 PM" src="https://github.com/user-attachments/assets/ad9608d4-4ad6-48cf-83dc-003feabdb3a9" /> The higher you make the HMaxThr, the smaller the new theta is. ThetSD is the maximum that theta can be and is only possible when h > HMaxTH. In other words, HMaxTH is the depth value at which the full ThetSD will be applied to the dry cells.

I don't think that DensIn or NeuBcSand should be factors because they seem relevant to models with suspended sediment, which ours does not have. IHidExp should also have no affect on our model because Wilcock and Crowe has its own hiding and exposure formula. That leaves ISlope and HMaxTH as potential settings contributing to floodplain erodibility. 

| MOR Underlayer Setting | Old Model | Current Model | Units | Default | Description |
| ---------------------- | --------- | ------------- | ----- | ------- | ----------- |
| MxNULyr | 50 | 100 | integer | 1 | Maximum number of underlayers (excluding transport and base layers) in case IUnderLyr=2. |
| TTLForm | 1 | 2 | integer | 1 | Transport layer thickness formulation in case IUnderLyr=2. 1. constant user-defined. 2. proportional to water depth. 3. proportional to bedform height. |
| ThTrLyr | 0.2 | 0.05 | uniform value or file name | not listed | thickness of transport layer [m] in case of TTLForm=1 |
| TTLAlpha | -- | 0.05 | -- | 0.1 | proportionality constant in case of TTLForm=2 or 3 |
| TTLMin | -- | 0 | [m] | 0 | minimum thickness in case of TTLForm=2 or 3 | 
| ThUnLyr | 0.256 | 0.2 | [m] | not listed | characteristic maximum thickness [m] of stratigraphy layers in case IUnderLyr=2 | 

The difference in this section is that the old model uses less sediment bookkeeping layers (50 vs 100) and the thickness of the transport layer is different. In the old model, the transport layer thickness is 0.2 m always, while in the current model it is proportional to the water depth with a maximum thickness of 0.2 m. It's hard to say how this would affect erodibility. Technically, if we consider bed armoring, the old model should be slightly less erodible because its transport layer is thicker so the effect of bed armoring is mixed throughout the entire transport layer. 

| MOR Numerics Setting | Old Model | Current Model | Units | Default | Description |
| -------------------- | --------- | ------------- | ----- | ------- | ----------- |
| MaximumWaterdepth | -- | true | logical | false | use locally maximum water depth to compute characteristic velocity for sediment transport at cell centre | 
| MaximumWaterdepthFraction | -- | 1 | real number 0-1 | 1 | fraction of the flow depth at links used in finding the maximum water depth. Only used if MaximumWaterdepth=true.

Only real difference in the Numerics section is the MaximumWaterdepth. There is very limited description of this variable, but it seems like turning this on causes the velocity used for sediment transport to be calculated where the water depth (which is at the nodes) is the highest. Is this a stability parameter? I don't know how this would affect the erodibility exactly. If turning this setting on makes the water depth higher, then I would expect transport to increase. 

### 2. `SedThr 0.1 Acal 4` and `SED file + old sed frac + ThetSD 0` have Low Overflow to Sumas

Once I reran the `SedThr 0.1 Acal 4` model with a longer runtime limit, I was able to plot its overflow to Sumas against the other trials. Strangely, it had less overflow to Sumas than both `SedThr 0.1` and `Acal 4` alone. The only explanation I had for this was that the combination of the two more erodible settings causes sediment to move in a different way such that overflow routing decreases instead of increases.

The `SED file + old sed frac + ThetSD 0` model, now that it has the correct hydrograph, plots below the `SED file + old sed frac` model. I expected ThetSD = 0 to increase erosion of defined channels because it brings the transfer of bed change from wet cells to dry cells to zero. However, it seems like increasing this term actually increases erosion and channel formation.

I wanted to compare the bed change and bed relative elevation for both of these runs to get a better idea of what is happening in the model. 

<img width="1543" alt="image" src="https://github.com/user-attachments/assets/e80dae7f-b06b-4db3-b611-9507964696b3" />

*Figure 3. Relative bed elevation for SedThr 0.1 Acal 4*

<img width="1546" alt="image" src="https://github.com/user-attachments/assets/cfd1d572-4fcd-4194-8918-deb80721e95f" />

*Figure 4. Bed change for SedThr 0.1 Acal 4*

I noticed that the Masey Road area has become very elevated in this run. Perhaps it is limiting the amount of overflow and incision of continuous channels from the main channel to Main St. The elevated Masey Rd feature is not present in the `Old SED file + old sed frac + Old MOR`, which has the most overflow to Sumas of my runs so far. There is still increased erosion after Main St. compared to the current model with no changes. 

Why do some of my runs have higher bed levels at Masey Road? 

<img width="1543" alt="image" src="https://github.com/user-attachments/assets/2cfbc94f-4962-4a24-b05c-a7f5e288de30" />

*Figure 5. Relative bed elevation for old sediment settings and ThetSD 0*

<img width="1546" alt="image" src="https://github.com/user-attachments/assets/80257bf1-4f01-4442-9792-b776b9903679" />

*Figure 6. Bed change for old sediment settings and ThetSD 0*

This result is very baffling to me. How can there be so little floodplain change where the sediment settings and fractions are changed? The only difference is that the ThetSD setting is reduced to zero. I double-checked the hydrograph, MOR settings, and the sediment settings. To confirm that this result is true, I want to run a model that changes ThetSD to 0 only, without touching the sediment settings. If lowering ThetSD from 1.5 to 0 truly reduces erosion as significantly as seen here, then I would expect there to be less overflow in this `ThetSD 0` model than in the `No change` model. 

## Additional MOR Setting Tests

I ran three additional models this week:

- `OldsedfracXYZfiles_OldSEDfile_SedThr0.1_ThetSD0_bsBagnold`
- `OldsedfracXYZfiles_OldSEDfile_SedThr0.1_ThetSD0_HMaxTH1.5`
- `OldsedfracXYZfiles_OldSEDfile_SedThr0.1_ThetSD0_nohiding`

These models add on the three untested settings that I am hoping make up the gap between `OldsedfracXYZfiles_OldSEDfile_SedThr0.1_ThetSD0` and `Old SED file + old sed frac + Old MOR`. So far the `OldsedfracXYZfiles_OldSEDfile_SedThr0.1_ThetSD0_HMaxTH1.5` run has completed, and turns out to have exactly the same overflow to Sumas as `OldsedfracXYZfiles_OldSEDfile_SedThr0.1_ThetSD0`. The - `OldsedfracXYZfiles_OldSEDfile_SedThr0.1_ThetSD0` run has a HMaxTH = 1, which is the value of the current model, whereas HMaxTH = 1.5 is consistent with the old model. 

<img width="1265" alt="image" src="https://github.com/user-attachments/assets/60d0d09b-407f-4bd2-9af9-b8e4a2da3f40" />

*Figure 7. OVerflow to Sumas for model runs with and without HMaxTH1.5. 


## Note on transferring masters credits

I learned after talking to Anna Egeland that I can't transfer my masters credits from UC Berkeley to UW until I pass my general exam, and that I also need to send official transcripts over to verify my graduation. Once I do that, I get 30 credits in the dissertation units.

From the Graduate School Policy 1.1: Graduate Degree Requirements: 
>A master’s degree in a relevant field of study from an accredited institution, including UW, may substitute for up to 30 of the required 90 credits. No other transfer credits are allowed for doctoral programs. Transfer credits may not be applied towards the dissertation or culminating experience requirement.
