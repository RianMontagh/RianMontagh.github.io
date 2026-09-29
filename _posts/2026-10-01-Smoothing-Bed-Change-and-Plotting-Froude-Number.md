# Verifying Conveyance Plots, Smoothing Streamwise Bed Change, and Plotting Streamwise Froude Number

## Verifying Conveyance Plots

Now that I have plotted Q, V, and A through my bankfull cross sections, I wanted to do some more work to verify they are working as I am expecting them to. I want to gain more confidence in them before I start drawing conclusions from them. The first thing I did was see if Q (output variable cross_section_discharge) equals $V*A$, where V is the cross_section_velocity and A is the cross_section_area. After meeting with Alex and Wuming last week, I realized that the model could be outputting the average velocity including parts of the cross section that have zero velocity. For example, if a cross section spans a bar or a braided section, the average velocity in the cross section might be close to zero due to averaging zeros in with the actually velocity. However, the plot below verifies that cross_section_velocity is the same as $Q/A$ which is the velocity through the wetted areas only, and does not include dry areas like bars. 

<img width="1994" alt="image" src="https://github.com/user-attachments/assets/3602059f-94af-4681-9ecf-eb0437e9c4b7" />

*Figure 1. Showing that Q = V*A*

Next, I wanted to visualize the extent of flooding in comparison to my bankfull cross sections. Specifically, I wanted to see if the length of my cross sections make sense and if I can understand more about the bumps in my plots. 


## Area-Averaging Instead of Cross Section-Averaging

## Reviewing my AGU Abstract
