## Delft3D Model Updates - Boundary Conditions and Roughness

### Modeling the 2025 Flood in the Nooksack River

As mentioned last week, part of my research goal this quarter is to put together a Delft3D model of the Lower Nookack during the 2025 atmospheric river. So far, I have added the grid and completed the boundary conditions. I have started creating the roughness file, which tells the model how the Mannings n values are spatially varied. 

#### Boundary Conditions
The upstream boundary condition at North Cedarville is complete; it consists of continuous discharge data from the USGS gage maintained there. This was a simple process of navigating the USGS's website and converting the units into metric (Delft3D only works in metric units). The more nuanced task was determining the appropriate range of time to include in the model. The highest peak discharge occurred on December 11, but several other flood peaks occurred during the storm. In the end, I decided on December 4th through the 14th. This allows some ramp up time and some time to see how the water levels decrease after the peak. 

<img width="749" alt="image" src="https://github.com/user-attachments/assets/6af1b9c1-50bc-458c-8b21-f7b1f153659a" />

The figure above shows the discharge at North Cedarville that will be used for the upstream boundary condition. The peak preceding the 12/11 peak is just above the bankfull discharge (about 700 m^3/s), and this is ideal because it will initiate sediment transport ahead of the major peak in discharge. 

Question: Why does a bankfull flow help with initializing the model?

At the downstream, the model has two tidal boundaries - one to Bellingham Bay and one to Lummi Bay. These boundaries are far from the section of river with the sediment transport dynamics we are most interested in (the Everson Corridor), so should have little impact on results. These are input as astronomical boundaries, with tidal components taken from an ocean model run by Wuming. 

<img width="735" alt="image" src="https://github.com/user-attachments/assets/05a4dd1d-7376-4997-b593-d36eeae046f2" />

The overflow boundary that describes the avulsive flow north to Sumas is set as a constant water level elevation. This is not realistic, because there should only be water at the boundary when flow is present there. Wuming set up a secondary Neumann condition, setting the gradient in water level across the boundary to zero. This prevents strange jumps/discontinuities at the boundary, but Wuming still reported some difficulty at this boundary -- it will be interesting to see what this looks like in my model run.  

Question: How far did flow travel through the overflow corridor in the flood? 

<img width="750" alt="image" src="https://github.com/user-attachments/assets/426967c1-ed5c-471b-aa3b-6ba61f188bc3" />

#### Roughness 
A key part of any river model is the roughness parameter, in this case Mannings n. This parameter relates to the amount of friction flow feels as in moves over a surface. For example, if the banks of the channel are vegetated but the center of the channel is sandy, the river flow will feel a different force once it reaches overflow conditions. I downloaded landcover data from the [National Landcover Database] (https://www.mrlc.gov/viewer/) and am currently processing it to make sense for the model. For now, I am starting with changing the landcover type at areas with bridges from "developed" to "open water." This is because the river flows under the bridge, so is not affected by the roughness of the structures. Later down the line, I will also want to use the median grain size in the river to define the roughness, so that there is a spatially-varying roughness throughout the channel, instead of a blanket value applied to all of the "open water" area. 

<img width="1044" alt="image" src="https://github.com/user-attachments/assets/b63aee4b-1970-4726-9d32-026916a6b590" />

<img width="252" alt="image" src="https://github.com/user-attachments/assets/36b7a4f0-f1f8-43a0-831b-f0a3c2ce1bb0" />

#### Next Steps with the 2025 Nooksack Model

Boundary Conditions: Complete  
Grid: Need to add the new 2024 topobathy  
Roughness: In progress  
Settings: Need to copy from Wuming's model  
Initial conditions: Need to make the BCs initially wet to prevent crashing  
Morphology: add in sediment layers and gradations









