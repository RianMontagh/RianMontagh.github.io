# Modifying Hydrographs for Exposed Bar Area Comparison with WWU

This week I switched gears to prep for a meeting with Colin and Allison from WWU to compare our model output with their GIS analysis of the bars in the Everson corridor. They are interested in the 2021 and 2025 bar movement due to the large floods, and how well the model and the aerial imagery agree. To do this, I needed to modify the upstream hydrograph to match their low flow values before and after the flood. 

This was my first pass at modifying the 2025 hydrograph:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/af4af12a-87e8-4c2d-a0d6-cbdfa3d46a30" />

Then I decided that there was a possibility for the sudden drop in discharge to create weird hydrodynamics that could affect the shape of the bars instead of freezing the bars in place after the flood, and that it was probably safer to gradually decrease the flow to WWU value of 45 cms. My second pass is below:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/828f9097-bb1c-440f-95e4-30d21333e063" />

The 2025 run with the appended low flows, which I am calling `exposedbar`, is currently running. 

I am also editing the 2021 hydrograph, which might need a ramp up discharge at the beginning to get the initial low flow to be seamless. 

This is the current 2021 hydrograph I have:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b116563c-5c46-4359-bca1-f25fcf3a1224" />

