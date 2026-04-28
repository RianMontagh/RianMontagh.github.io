# Creating More Quantifiable Analyses For Avulsion Tests

Last week, I generated different plots to try and get at the differences in bed change between the different avulsion sensitivity runs. This week my goal was to challenge myself to be more quantitative in my analysis. In addition to writing more scripts for model output analysis, I also ran the 100-year, 1-day synthetic hydrograph to complement my 3-day and 7-day duration runs. 

## Ratio of Q_channel to Q_overflow

One metric we can use to get a sense of how the system might be tending toward avulsion is the ratio of the discharge in the overflow channel to the discharge in the main channel. If we compare these ratios over time, we might see that one set of conditions leads to the ratio being elevated for a longer time period. Based on my plots of the overflow last week, I predicted that the runs without the erodible areas would have a higher $Q_{overflow}/Q_{main channel}$ ratio because they have more flow in the overflow corridor. See Figures 1 and 2 below for the location of the overflow and main channel cross sections and the discharges through the overflow cross section. I tested out using two different cross sections to define the overflow: one is the cross section at the boundary condition that is wider and defines the point where flow escapes to Sumas, the other is the cross section at Main Street overflow gage. The first option captures a larger fraction of the flow, but might double-count some if the flow in the main channel. 

<img width="660" height="598" alt="image" src="https://github.com/user-attachments/assets/72befa4a-5903-4e03-8c5f-9b62ca6d8de7" />

*Figure 1. Location of the Main Channel and Overflow Cross Sections used for this analysis.*

*Figure 2. Discharge through the Overflow Corridor Cross Section for different flood durations and erodibilities.*





