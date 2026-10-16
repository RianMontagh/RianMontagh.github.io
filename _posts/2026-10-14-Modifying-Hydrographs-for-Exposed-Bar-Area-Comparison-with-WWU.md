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

I am considering chopping the hydrograph at the point I have highlighted and ramping down to the low flow from there. After looking at Wuming's modeling, it seems like he usually doesn't run past the second peak. 

This is all I have this week since most of my time has been completing class work and prepping content for the EFM meetings. Tomorrow I hope to get the 2021 modified hydrograph done and do a spatially varying Froude number analysis. 

## Next Steps 

Extract more meaning from the bed level change along channel.
- plot along the river to make the locations of aggradation and degradation more clear
- see if there is net aggradation or degradation along channel lengths

Plot along channel Froude number and water surface elevation. Is there a curve that indicates a critical point?

Check water surface elevation on the second peak to see if it is higher for the same discharge, which indicates a loss in conveyance. 

Run a fixed discharge through saved terrain to see how water elevation increases or decreases. 

Simple conceptual model of Exner's linked with a decrease in discharge. 

Is there aggradation in the overflow reach? Is this always going to mean a decrease in flow conveyance?

Overlay velocity vectors or streamlines on the Fr map.

## Froude Plots

Deeper look at how my output times line up with the discharge through Everson cross section now that I am doing map variable analysis instead of his variables.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/f5b8c58e-3949-4d71-96f7-c721a1b681fe" />

Because my map output times are less frequent (every 2 hours instead of every 1 hour), the nearest output times for the map variables in some cases are more offset from the bankfull discharge. Luckily both of the peaks are pretty well captured. However, two of the bankfull discharges are missed by about 60 cms. It could be worth doing an average later on especially for the map variables. 

Here are my Fr plots for the map output times. Time 1 is the first blue square on the hydrograph, Time 2 is the next time of the hydrograph, etc. Time 1 is not shown because nothing is going on during the first time step of the model. 

<img width="1206" alt="image" src="https://github.com/user-attachments/assets/af43e896-6bcd-4dac-8eda-fbbcd47d9554" />

Fr values less than 0.001 are set to 0. 

Roads are darker blue, which lines up with our hypothesis that flows will be closer to critical as they pass over high points. However, they are not that close to 1. Below I filtered out cells with Fr < 0.5. I did a lazy trick where I set the max of my color bar to 0.4 so all cells not filtered show up as yellow.

<img width="1207" alt="image" src="https://github.com/user-attachments/assets/c2fac4f0-663c-487c-a039-2cb3f9b9409e" />

Not many Fr values are greater than 0.5. When I filtered for values greater than 0.8, even less cells remained, and almost zero cells have Fr greater than 1. This might actually make sense for the Nooksack, because during the flood, maximum depths at all timesteps vary from about 6 to 12 m. The Nooksack is unlikely to be supercritical at the deepest parts, because the balancing velocity in the numerator would need to be sufficiently high. For D = 10 m, V would need to be 10 m/s (22 mi/hr). Most of the high Fr values seem to be near the banks of the river where depths are shallower. Same for roads and high points - these are shallower locations. 



