## Delft3D Model Updates and Sediment Transport Concepts

###Modeling the 2025 Flood

As mentioned last week, part of my research goal this quarter is to put together a Delft3D model of the Lower Nookack during the 2025 atmospheric river. So far, I have added the grid and completed the boundary conditions. I have started creating the roughness file, which tells the model how the Mannings n values are spatially varied. 

####Boundary Conditions
The upstream boundary condition at North Cedarville is complete; it consists of continuous discharge data from the USGS gage maintained there. This was a simple process of navigating the USGS's website and converting the units into metric (Delft3D only works in metric units). The more nuanced task was determining the appropriate range of time to include in the model. The highest peak discharge occurred on December 11, but several other flood peaks occurred during the storm. In the end, I decided on December 8th through the 14th. This allows some ramp up time and some time to see how the water levels decrease after the peak. 

<img width="749" alt="image" src="https://github.com/user-attachments/assets/6af1b9c1-50bc-458c-8b21-f7b1f153659a" />

At the downstream, the model has two tidal boundaries - one to Bellingham Bay and one to Lummi Bay. These boundaries are far from the section of river with the sediment transport dynamics we are most interested in (the Everson Corridor), so should have little impact on results. These are input as astronomical boundaries, with tidal components taken from an ocean model run by Wuming. 

The overflow boundary that describes the avulsive flow north to Sumas is set as a constant water level elevation. This is not realistic, because there should only be water at the boundary when flow is present there, but the way the boundary is set up is to have water present all the time. 
Question: how far did flow travel through the overflow corridor in the flood? 
Wuming set up a secondary Neumann condition, setting the gradient in water level across the boundary to zero. This prevents strange jumps/discontinuities at the boundary. 

####Roughness 







