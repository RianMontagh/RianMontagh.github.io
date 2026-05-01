# Creating More Quantifiable Analyses For Avulsion Tests

Last week, I generated different plots to try and get at the differences in bed change between the different avulsion sensitivity runs. This week my goal was to challenge myself to be more quantitative in my analysis. In addition to writing more scripts for model output analysis, I also ran the 100-year, 1-day synthetic hydrograph to complement my 3-day and 7-day duration runs. 

## Looking Deeper at the Overflow Discharge

One metric we can use to get a sense of how the system might be tending toward avulsion is the ratio of the discharge in the overflow channel to the discharge in the main channel. If we compare these ratios over time, we might see that one set of conditions leads to the ratio being elevated for a longer time period. Based on my plots of the overflow last week, I predicted that the runs without the erodible areas would have a higher $Q_{overflow}/Q_{main channel}$ ratio because they have more flow in the overflow corridor. See Figures 1, 2,and 3 below for the location of the overflow and main channel cross sections and the discharges through the overflow cross sections. I tested out using two different cross sections to define the overflow: one is the cross section at the boundary condition that is wider and defines the point where flow escapes to Sumas, the other is the cross section at Main Street overflow gage. The first option captures a larger fraction of the flow, but might double-count some if the flow in the main channel. 

<img width="759" alt="image" src="https://github.com/user-attachments/assets/0c75880f-8f48-4b4b-ba0a-b9e7fd52a045" />

*Figure 1. Location of the Main Channel and Two Overflow Cross Sections used for this analysis (highlighted in orange).*

<img width="1024" alt="image" src="https://github.com/user-attachments/assets/80f90bf9-1691-4de5-8f50-a45707987a73" />

*Figure 2. Discharge through the Overflow to Sumas Cross Section for different flood durations and erodibilities.*

<img width="1017" alt="image" src="https://github.com/user-attachments/assets/4fb62376-b4f2-49bd-b3e2-4cfddd9d03ee" />

*Figure 3. Discharge through the Overflow at Main St. Cross Section for different flood durations and erodibilities.*

### There are several things to notice here:

1. The model runs without the nonerodible areas have consistently high discharge in the overflow corridor regardless of flood duration or cross section used.
2. The difference between the max peak flow of the "with/without erodible areas" runs increases as flood duration increases regardless of cross section used. Could this be related to the fact that discharge is increasing more slowly on the rising limb for the longer durations, giving more time for morphologic change?
3. The two overflow cross sections seem to have similar-looking results, but the Main St. cross section has less discharge (due to it being a shorter section) and the magnitude of changes from subplot to subplot is smaller.

I also created a table of the maximum discharge in the overflow channel for each run. 

*Table 1. Maximum Discharge in the Overflow to Sumas Cross Section* ($m^3/s$)

|                               | 1-day   | 3-day  | 7-day  |
| ----------------------------- | ------- | ------ | ------ |
| **Without Nonerodible Areas** | 1432.7  | 1510.6 | 1499.8 |
| **With Erodible Areas**       | 1393    | 1398.4 | 1359.2 |

*Table 2. Maximum Discharge in the Overflow at Main St. Cross Section* ($m^3/s$)

|                               | 1-day   | 3-day  | 7-day  |
| ----------------------------- | ------- | ------ | ------ |
| **Without Nonerodible Areas** |  893.09 | 934.38 | 936.18 |
| **With Erodible Areas**       |  877.85 | 887.34 | 871.05 |

The trends are a little easier to see in the table and the plot below &mdash; There is a more dramatic increase in overflow discharge from the 1-day to the 3-day duration, which decreases or stays about the same from the 3-day to the 7-day duration. 

<img width="915" alt="image" src="https://github.com/user-attachments/assets/27ab67aa-394a-4b3d-b19b-a4acf6543307" />

*Figure 4. Maximum Disharge at Overflow to Sumas. vs. Flood Duration.*

<img width="922" alt="image" src="https://github.com/user-attachments/assets/b126d74d-395a-40a7-ba71-c721b0b9db37" />

*Figure 5. Maximum Disharge at Main St. vs. Flood Duration.*

### Conclusions and Observations Based on Maximum Discharge Overflow

The plot of maximum values of discharge is surprising to me because the longest duration did not have the most discharge in the overflow channel. One explanation could be that even though the discharge is elevated for a longer period of time for the 7-day duration flood, it is not necesarily instantaneously higher. The only difference that duration does is scale the shape of the hydrograph so that the width of the hydrograph base matches the prescribed duration. However, I thought that longer time periods at an elevated discharge level might lead to more morphodynamic feedback looping between bed aggradation in the channel and potential incision in the overflow path.  

What this might mean is that at this event-scale, we are not seeing the feedback loop due to longer periods of elevated discharge at the upstream boundary condition. There might be a sweet spot between the duration of flow and the rapid increase of discharge on the rising limb of the North Cedarville hydrograph. The plots show that the 1-day flood duration had the least amount of discharge in the overflow channel, even though it has the most extreme rate of change in discharge vs. time. These results need to be complemented with bed change analysis to see why the 3-day discharge seems to be the more avulsive condition, based on the flow rate in the overflow corridor. 

## Analyzing the $Q_{overflow}/Q_{main channel}$ (or Q ratio)

So, based on the plots from the above section, we know that the model runs without the nonerodible areas have more discharge in the overflow corridor. This means that we would expect our $Q_{overflow}/Q_{main channel}$ to also be higher for the 'without nonerodible areas' models. This ends up being the case (see below). For the rest of the analysis, I decided to focus in on the overflow to Sumas, as this defines what flow is actually sent north in the potential avulsion pathway. I would expect the results to somewhat match the results from the overflow discharge analysis, because higher overflow discharge = higher Q ratio, but this requires that the flow in the main channel decreases or stays about the same when the amount of flow routed to the overflow increases. 

<img width="1021" alt="image" src="https://github.com/user-attachments/assets/be50d8f7-778e-4e85-891b-1639f308ebc5" />

*Figure 6. Q ratio at Everson for the Overflow to Sumas Cross Section.*

Again, here are the tables and plots for the maximum values of the Q ratio. 

*Table 3. Maximum Q Ratio in the Overflow to Sumas Cross Section* 

|                               | 1-day    | 3-day   | 7-day   |
| ----------------------------- | -------- | ------- | ------- |
| **Without Nonerodible Areas** |  0.84032 | 0.90272 | 0.94263 |
| **With Erodible Areas**       |  0.78276 | 0.79005 | 0.78278 |


<img width="911" alt="image" src="https://github.com/user-attachments/assets/1e0951b5-1316-47cd-b741-5dca57aae03a" />

*Figure 7. Maximum Q Ratio at Overflow to Sumas vs. Flood Duration.* 

### Conclusions and Observations based on Q Ratio

The Q_ration plots have similar trends to the overflow discharge, with some exceptions. The first one that jumped out to me is that instead of decreasing from the 3-day to the 7-day duration as I would have predicted based on the overflow discharges, we instead see an increase or very slight decrease in Q ratio. This made me wonder what exactly the discharge in the main channel is doing. I wondered if maybe there could be an effect such as the main channel cross section not capturing all of the flow in the 7-day model run. I plotted main channel discharge below: 


<img width="916" alt="image" src="https://github.com/user-attachments/assets/761c2701-d3a4-4358-afef-79f8f6c0b49c" />

*Figure 8. Maximum Main Channel Discharge at Everson*

For the model without nonerodible areas, the increase in Q ratio from the 3-day to 7-day duration is caused by the decrease in main channel flow rather than an increase in overflow. This made me wonder if not all of the flow is captured between the overflow and the main channel cross sections. In addition, I noticed that the timing of the peak in the main channel and peak in the overflow cross section occur at different times (7 hour difference in one case), pointing to the distance and therefore time lag between them. This means that comparing the discharges at the same time does not mean that the peak values are matched up. 

Solution 1: Calculate the overall volume that travels through the main channel versus the overflow channel by the end of the model run. This would get rid of the lag problem, but would eliminate the time-varying aspect of the Q ratio. 

Solution 2: Compare the discharge in the overflow right next to the channel to the total discharge just upstream of the flow split. This would conserve the time-dependency of the Q ratio and limit the lag between the discharge because the sections would be closer together. See below for the sections I have in mind. 


<img width="691" alt="image" src="https://github.com/user-attachments/assets/33be454f-dec1-4b8e-9c15-7288e15b226d" />

*Figure 9. Potential cross sections to use for a ratio of* $Q_{overflow}/Q_{total}$ *in purple.*

## Bed Change at Observation Points Along the Everson Corridor and Overflow Corridor

I generated a plot of the total bed change at different observation points that are approximately evenly spaced in the main channel, with point #1 being at the river outlet and #152 being upstream from the Twin View Levee. The number of the observation point is being used as a proxy for river station, which would be the ideal way to plot this data. The Everson bridge is near observation point #127, which is plotted for reference. 

<img width="1268" height="674" alt="image" src="https://github.com/user-attachments/assets/3fbec636-a5eb-40f2-b568-99345c0364ae" />

*Figure 10. Total bed change in the models WITHOUT the nonerodible areas at 152 observation points in the main channel. Plotted from downstream to upstream, left to right.*

<img width="1738" alt="image" src="https://github.com/user-attachments/assets/baad19fa-77e9-4e6f-ba6a-2f43093720d4" />

*Figure 11. Total bed change in the models WITH the nonerodible areas at 152 observation points in the main channel. Plotted from downstream to upstream, left to right.*

Notes:

1. Some observation points are missing data from the 3-day model runs for both with and without nonerodible areas - I will investigate this next week.
2. Adding the nonerodible areas seems to reduce channel bed change much more than I would have expected, especially because these areas are not in the main channel (except for at bridges). 
3. The nonerodible areas decrease the amount of aggradation that the Everson corridor experiences, and eliminates some unrealistic erosion that occurs further downstream at observation point #35. This makes sense after I found that this observation point was placed directly where the Main Street bridge crosses the Nooksack, so would have been made nonerodible. I am not sure why it would be experiencing such extreme erosion, however. This points to a step that should probably be taken next, which is to be more careful about what areas are made non-erodible. Bridges should still be allowed to 'erode' because the water flows underneath the bridge, so even though it is a developed land type, it should still be classified as open channel. This also has big implications for the Everson reach, which has nonerodible areas at the Everson bridge. To-do: Ask Wuming if he addressed this issue in his sediment thickness files. 
4. The different durations generally have the same trends in bed change. For example, if there is erosion in the 1-day duration, there is also erosion in the 7-day duration, just more of it. 

<img width="600" alt="image" src="https://github.com/user-attachments/assets/70a6e987-310b-4faa-8f66-f1bcdbd6be97" />

## Questions
1. I am currently developing back-to-back floods to test in our model, but I need to decide on the recovery time between the two floods. One starting point is the ArkStorm, which is a megastorm consisting of two consecutie atmospheric rivers with four days between them.
2. I want to figure out the best way to make a spatially-averaged bed change vs. time line plot. 






