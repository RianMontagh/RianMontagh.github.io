## Comparing 2022 and 2024 Topo Results 

This week, I compared my 2022 and 2024 topo model results. 

### Stage-Discharge Curves

Comparing the state-discharge curves between two model results with different topography is analogous to running a morpho-on model, because the bed level is different in each model. I compared the stage-discharge curve to get a sense of how channel conveyance might have changed between the two runs. 


<img width="1000" alt="image" src="https://github.com/user-attachments/assets/a2873e3e-338c-49be-98cb-fddbafa41b4a" />

*Figure 1. 2022 topo (left) and 2024 topo (right) modeled Qh curve for the 2025 flood at Everson.* 

At the 200 to 400 m^3/s, the 2024 topo Qh curve plots higher above the USGS rating curve than the 2022 topo Qh curve. This means that between 2022 and 2024, the bed terrain changed such that at the same channel discharge, the water surface elevation increased. We can infer that this means the channel conveyance reduced at this discharge. 

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/ee51afce-7f62-4ccf-9f51-08f4301be873" />

*Figure 1. 2022 topo (left) and 2024 topo (right) modeled Qh curve for the 2025 flood at Ferndale.* 

The Ferndale Qh plots looks approximately the same, indicating that less channel change ocurred here. This makes sense because we know that Ferndale is generally more stable than Everson. 


### Lessons Learned

- make sure to check that any files exported from the GUI are as expected &mdash; this could be more involved than visually inspecting them.

### Next Steps with the 2025 Nooksack Model

Boundary Conditions: Consider adding the tributary boundaries and differentiating the two tidal boundaries. They are the same right now.  
Roughness: Have it update during model run.   
Morphology: Need to add in sediment layers and gradations, and turn on morpho.  
Avulsion research: Obtain future projected hydrographs. Quantify the knobs we want to turn.  
Validation: Potentially adjust incoming hydrograph to improve calibration.  
Topo: Need to check for artifacts of bridges in the channel?  














