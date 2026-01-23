## Delft3D Model Updates and Sediment Transport Concepts

### Modeling the 2025 Flood

As mentioned last week, part of my research goal this quarter is to put together a Delft3D model of the Lower Nookack during the 2025 atmospheric river. So far, I have added the grid and completed the boundary conditions. I have started creating the roughness file, which tells the model how the Mannings n values are spatially varied. 

#### Boundary Conditions
The upstream boundary condition at North Cedarville is complete; it consists of continuous discharge data from the USGS gage maintained there. This was a simple process of navigating the USGS's website and converting the units into metric (Delft3D only works in metric units). The more nuanced task was determining the appropriate range of time to include in the model. The highest peak discharge occurred on December 11, but several other flood peaks occurred during the storm. In the end, I decided on December 4th through the 14th. This allows some ramp up time and some time to see how the water levels decrease after the peak. 

<img width="749" alt="image" src="https://github.com/user-attachments/assets/6af1b9c1-50bc-458c-8b21-f7b1f153659a" />

The figure above shows the discharge at North Cedarville that will be used for the upstream boundary condition. The peak preceding the 12/11 peak is just above the bankfull discharge (about 700 m^3/s), and this is ideal because it will initiate sediment transport ahead of the major peak in discharge. 

Question: Why does a bankfull flow help with initializing the model?

At the downstream, the model has two tidal boundaries - one to Bellingham Bay and one to Lummi Bay. These boundaries are far from the section of river with the sediment transport dynamics we are most interested in (the Everson Corridor), so should have little impact on results. These are input as astronomical boundaries, with tidal components taken from an ocean model run by Wuming. 

<img width="735" height="296" alt="image" src="https://github.com/user-attachments/assets/05a4dd1d-7376-4997-b593-d36eeae046f2" />

The overflow boundary that describes the avulsive flow north to Sumas is set as a constant water level elevation. This is not realistic, because there should only be water at the boundary when flow is present there, but the way the boundary is set up is to have water present all the time. 
Question: how far did flow travel through the overflow corridor in the flood? 

<img width="750" height="306" alt="image" src="https://github.com/user-attachments/assets/426967c1-ed5c-471b-aa3b-6ba61f188bc3" />

Wuming set up a secondary Neumann condition, setting the gradient in water level across the boundary to zero. This prevents strange jumps/discontinuities at the boundary. 

#### Roughness 

<img width="1044" height="776" alt="image" src="https://github.com/user-attachments/assets/b63aee4b-1970-4726-9d32-026916a6b590" />






