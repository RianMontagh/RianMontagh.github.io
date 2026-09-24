# Improving and Exploring Channel Conveyance Analyses

## Lagging the Output Times to Match the Everson Hydrograph

One of the first improvements I made was account for lag time from the boudary of our model to the Everson gage. I have accounted for this lag before in other analyses, but I am surprised that I haven't tried to quantify it. I plotted the hydrographs side by side and I compared the output times for each. For example, the output times for the bankfull and peak discharges are different if I obtain them with the North Cedarville Hydrograph or with the Everson hydrograph.

<img width="1431" alt="image" src="https://github.com/user-attachments/assets/63314268-ec35-44b0-9615-97db4bc3c838" />

*Figure 1. Visualizing lag from North Cedarville to Everson (about 10 km).*

When I looked closer at this plot at the times at which the bankfull and peak discharges pass through North Cedarville and Everson, I see a lag time of about 2 hours. Intuitively, it seems like the bed of a river should be able to change significantly within 2 hours of a flood, so I want to take this into account in my plots. Since we are most interested in the Everson corridor, I will make my output times for my conveyance plots be the times that the Everson gage experiences bankfull and peak flows. 

## Calculating Along-Channel Distance

The second improvement I made in my plots was converting cross section index with along-channel distance on the x-axis. This was as simple as saving the points of each cross section that intersect with the centerline and calculating the distance between each of these centerline points. 

## Adding Velocity and Wetted Cross Sectional Area



