## Plots for 2024 Topobathy Model and Avulsion Sensitivity Research Plan

Last week, I ran the 2025 model with the 2024 topobathy and did not have a chance to plot the results. This week, I plotted these up and compared them to observational data at USGS gages. I also worked on a plan for the avulsion sensitivity model runs. This means mapping out the parameter space and doing a literature review. 

### Map File Plots 

Of the variables available to plot from the map files (results that spatially vary over the grid as opposed to results at a point or cross-section) I decided velocity magnitude, water depth, and total bed shear stress magnitude were the most interesting to visualize. The peak in the hydrograph occurs on Decemeber 11, 2025 at 3:00 a.m.

<img width="1322" alt="image" src="https://github.com/user-attachments/assets/77600a9d-739c-4a24-bba0-a37c8684f444" />

*Figure 1. Water surface elevation in NAVD88 (m) at 8:00 a.m. on December 11.*

Figure 1 helped me get a sense of how water interacts with roads an levees during a flood. For example, the large dry rectangle south of the most downstream section of the river shown seems to line up with Abbott Road and Polinder Road, which are represented as levees in the model. I wonder how realistic this is and how the lateral extent of flooding is effected by this representation. 

<img width="1288" alt="image" src="https://github.com/user-attachments/assets/b80f0150-f2cb-4aa0-ac0e-045d61ccb593" />

*Figure 2. Velocity magnitude (m/s) at 8:00 a.m. on December 11.*

A few hours after the flood peak, the velocity in the channel is about 2-4 m/s. This seems very realistic for not-steep river/stream. 

<img width="1337" alt="image" src="https://github.com/user-attachments/assets/97389089-7dd0-421f-99e3-3d111d362922" />

*Figure 3. Total Bed Shear Stress Magnitude (N/m^2) at 8:00 a.m. on December 11.*

Bed stress ($\tau_b$) can tell us information on sediment transport rates. Wherever $\tau_b$ is high, it means the flow of water is exerting a friction stress on the bed, and the bed is exerting a friction force on the flow &mdash; more $\tau_b$ means more possibility for sediment transport. We do see $\tau_b > 100 N/m^2$ especially at the constriction at the Everson Road bridge. There are also pockets of high $\tau_b$ in the overflow section, but not at the very upstream part of it. I would have expected lower sediment transport and lower $\tau_b$ at the overflow section because we predict that deposition occur here, not scour and transport.  
Question: Did any gravel bars form during this flood and can we compare it to $\tau_b$? (Would expect low $\tau_b$ where the bar formed)




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
Topo: Need to check for artifacts of bridges in the channel?








