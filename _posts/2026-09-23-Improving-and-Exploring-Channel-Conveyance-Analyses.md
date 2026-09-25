# Improving and Exploring Channel Conveyance Analyses

## Lagging the Output Times to Match the Everson Hydrograph

One of the first improvements I made was account for lag time from the boudary of our model to the Everson gage. I have accounted for this lag before in other analyses, but I am surprised that I haven't tried to quantify it. I plotted the hydrographs side by side and I compared the output times for each. For example, the output times for the bankfull and peak discharges are different if I obtain them with the North Cedarville Hydrograph or with the Everson hydrograph.

<img width="1431" alt="image" src="https://github.com/user-attachments/assets/63314268-ec35-44b0-9615-97db4bc3c838" />

*Figure 1. Visualizing lag from North Cedarville to Everson (about 10 km).*

When I looked closer at this plot at the times at which the bankfull and peak discharges pass through North Cedarville and Everson, I see a lag time of about 2 hours. Velocity and discharge can change dramatically in two hours. Intuitively, the bed of a river should also be able to change significantly within 2 hours of a flood, so I want to take this into account in my plots. Since we are most interested in the Everson corridor, I will make my output times for my conveyance plots be the times that the Everson gage experiences bankfull and peak flows. 

## Calculating Along-Channel Distance

The second improvement I made in my plots was converting cross section index with along-channel distance on the x-axis. This was as simple as saving the points of each cross section that intersect with the centerline and calculating the distance between each of these centerline points. I also added markers for different regions of the channel. The Twin View levee marker indicates the beginning of the confined reach, or the overflow reach. I think of the Everson Bridge as the end of the overflow reach, but wonder how Whatcom County defines the Overflow Reach. 

The updated lagged plots are below. 

<img width="1572" alt="image" src="https://github.com/user-attachments/assets/e7005160-1935-4c61-8bc7-84961f2305dc" />

*Figure 2. Updated Conveyance Plots*

<img width="1559" alt="image" src="https://github.com/user-attachments/assets/bae47f3b-9ea7-439c-80fd-66dfce49b8fe" />

*Figure 3. Updated Bed Change Plots*

Now the influence from the flood peaks entering the model domain are more diffuse in the Q plots. It is still visible, but it is more clear to me now what if an effect of the hydrograph timing versus a channel effect. In the second panel on the right, which should be when the biggest peak passes through the Everson gage, it is interesting that there is still more discharge in the upstream cross sections. To me this is indicative of the overtopping before and within the overflow reach. 

For the bed change plot, marking the start of the Twin View levee makes me wonder if the larger spikes of erosion and deposition are related to the narrowing of the channel at this point. This is an area where overtopping due to choking and erosion from flow speeding up due to constriction could be occurring. 

## Adding Velocity and Wetted Cross Sectional Area

To understand my conveyance plots more, I need the velocity and wetted area area to understand what is contributing to changes in discharge (because Q = VA!). I recently discovered that wetted area is an output variable that is saved at each output time for each cross section. 

The velocity variable I decided to use is the cross section velocity, which is described as the "space-averaged velocity through observation cross section." I am assuming this is the average velocity normal to the cross section, which is the velocity relevant to calculating discharge. 

<img width="1534" alt="image" src="https://github.com/user-attachments/assets/48bb3862-025c-4633-bb76-544cdd01334d" />

*Figure 4. Velocity Plots*

This plot surprised me by how static in time some of the velocity patterns are. It made me feel better when I referred back to Shelby's plot and saw that her along-channel velocity also had consistent patterns regardless of output time. In my plots, from 4 km to 8 km, the velocity is consistently lower with less fluctuations. I am guessing this has to do with this portion of the river being wider and braided. 

The portion from 0-4 km has large spikes, which I propose come from the input hydrograph needing some distance to regulate to a realistic velocity. The very upstream part of the model is confined from the North Cedarville bridge, but then quickly becomes braided. I wonder if the higher velocities here helped prevent the massive deposition that was occurring in the old model.

To plot the area, I used the variable cross_section_area, which is described as "wet area of observation cross section." This term could be used to assess how the cross sectional area has changed due to erosion and deposition &mdash; the only problem with it is that during overtopping, the slice of area that is above the banks will be accounted for. That increase in area does not correspond with erosion of the channel or an increase in the ability of the channel to convey water. However, I still wanted to visualize it because I was hoping to attribute increases or decreases in Q with increases or decreases in velocity and/or wetted area. 

<img width="1572" alt="image" src="https://github.com/user-attachments/assets/3123facc-9ded-4f1b-8244-111f71f38716" />

*Figure 5. Wetted Area Plots*

Similar to velocity, the wetted area has spatial trends that hold true at different times during the flood. Consistent with my predictions from the velocity plot, the wetted area at the upstream boundary is small throughout the flood. In addition, in the braided reach, wetted area increases, which makes sense for this less confined area that also experiences lower velocities. 

## Plotting Q, V, and A Together

I plotted combinations of Q, V, and A on the same plot to see if I could attribute changes in Q to changes in V or A. 

<img width="1606" alt="image" src="https://github.com/user-attachments/assets/4259aa42-d1cc-47c9-8c46-56bc234b15f5" />

*Figure 6. Q and V*

In this plot, we can see that V can spike and drop off without a significant effect on Q. For example, on the rising limb of the first peak, velocity spatially varies from less than 1 m/s to about 3 m/s, but discharge does not appear to change with these changes in velocity. I zoomed in on the y-axis of Q to make sure that the y limits of the left axis were squashing effects from velocity. See below. 

In the plot taken at the output time during the main flood peak, we can see that in the overflow corridor between the Twin View levee and the Everson Bridge, the pattern is different because that spikes in Q line up with spikes of V. I don't have a good explanation of why this might be happening during the flood in the overflow reach, and it is possible that the effect is random. Next week I am hoping to get a better understanding of why bumps are forming in the Q anad V plots. 

<img width="926" alt="image" src="https://github.com/user-attachments/assets/5dfe5286-612e-4407-96de-2a573aaa6f11" />

*Figure 7. Zoomed in Q and V on rising limb of first peak*

Even after looking closer at discharge in Figure 7, velocity does not seem to be controlling changes in Q. I expect that when V changes, A is changing in the opposite direction such that Q stays relatively constant. This is shown in Figure 9 and discussed further. 

<img width="1640" alt="image" src="https://github.com/user-attachments/assets/c38b569f-d1fe-4d1c-b972-62d591567bc9" />

*Figure 7. Q and A*

 

<img width="1602" alt="image" src="https://github.com/user-attachments/assets/797c810c-21b6-43e0-9b30-f4c9fcda1d18" />

*Figure 8. V and A*





