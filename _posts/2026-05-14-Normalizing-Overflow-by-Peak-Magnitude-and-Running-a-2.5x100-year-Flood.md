# Normalizing Overflow by Peak Magnitude and Running a 2.5x100-year Flood

Last week, I realized that some of the results I was getting from running my different duration floods were misleading because my peak magnitudes for the different duration floods are specific to each duration. In order to compare the overflow of the different duration flood hydrographs, I need to normalize them by their unique peak magnitudes in the main channel. However, I learned that duration in the context of the masters thesis that I was getting the percent increases from are a different kind of duration than flood duration. Duration in the context of the masters thesis refers to the averaging length used to generate the annual max peak flows. For a 3-day duration, for example, the annual max peak flow for a single year is obtained by averaging hourly flows over 3-day intervals, and then finding the maximum of those averaged values. So, when we think about how we want to use the percent increases in our analysis by applying them to instantaneous peak discharges, it makes the most sense to use the 3-hr duration percent increase for all of our different length hydrographs. A 3-hr average most closely resembles an instantaneous flow peak. This is also the most conservative value to use, as the percent increases for the shorter duration peaks are higher than for other durations. See Figure 1 below. 

<img width="557" alt="image" src="https://github.com/user-attachments/assets/07264ac1-e931-4490-8787-f7f3de24aeb8" />

*Figure 1. Table 11 from Evan Paul's thesis, "Modeling 21st Century Peak Flows in the Nooksack River Basin in Northwestern Washington State Using Dynamically-Downscaled Global Climate Model Projections"*

However, I didn't have a chance to run models with new hydrographs this week, so I instead went the normalizing route. 

<img width="1378" alt="Max_normalized_overflow_alldurations" src="https://github.com/user-attachments/assets/10d122db-a555-4918-822e-0b1916e44eb9" />

*Figure 2. Max overflow disharge to Sumas divided by the peak discharge magnitude as the upstream boundary (different for each duration).*

Here we are not seeing any significant difference between the proportion of the peak discharge that makes it into the overflow. Also, Wuming was wondering if these values are accurate because 45% of the flow making it to the overflow is much higher than values he has gotten in the past. I have also changed a lot in the model since running the 2025 flood, so I think it would be a good idea to do that again to make sure that everything is working properly by rerunning the 2025 flood and checking the USGS observations against the model. 

# 2.5*100-year Flood

I also aplied a 2.5 multiplier to the 100-year flood and chose to run the 3-day duration. I plotted the spatially-varying bed change which is shown below alongside the 7-day duration of the 1.4*100-year flood. 

1.4*100-year             |  2.5*100-year
:-------------------------:|:-------------------------:
![](https://github.com/user-attachments/assets/e2eb97f4-89c3-479f-97dc-56cd59567ed9)  |  ![](https://github.com/user-attachments/assets/b146e2ab-6c0d-4cc8-9120-6112865c3c06)

The floodplain erosion seems to be controlled strongly by Main Street and the small creek that runs through the culvert under Main Street. The pattern of erosion that extends further to the south in the 2.5* flood is very interesting because I can imagine that erosion creeping up to the main channel and breaching the bank to river-right. That seems to be the scenario that would be required to allow flow to leave the channel once high stages attentuate. 

## Next Steps

1. Rerun all the floods with consistent percent increase.
2. Run other durations for the 2.5* flood.
3. Look at hydrograph shape from Whatcom County.
4. Look at Shelby's 'avulsion' model run.
5. Plot channel conveyance change.
6. Plot channel bed change vs. river station. 
