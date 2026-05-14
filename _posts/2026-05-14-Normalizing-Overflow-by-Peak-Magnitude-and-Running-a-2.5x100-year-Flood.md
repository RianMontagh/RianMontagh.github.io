# Normalizing Overflow by Peak Magnitude and Running a 2.5x100-year Flood

Last week, I realized that some of the results I was getting from running my different duration floods were misleading because my peak magnitudes for the different duration floods are specific to each duration. In order to compare the overflow of the different duration flood hydrographs, I need to normalize them by their unique peak magnitudes in the main channel. However, I learned that duration in the context of the masters thesis that I was getting the percent increases from are a different kind of duration than flood duration. Duration in the context of the masters thesis refers to the averaging length used to generate the annual max peak flows. For a 3-day duration, for example, the annual max peak flow for a single year is obtained by averaging hourly flows over 3-day intervals, and then finding the maximum of those averaged values. So, when we think about how we want to use the percent increases in our analysis by applying them to instantaneous peak discharges, it makes the most sense to use the 3-hr duration percent increase for all of our different length hydrographs. A 3-hr average most closely resembles an instantaneous flow peak. This is also the most conservative value to use, as the percent increases for the shorter duration peaks are higher than for other durations. See Figure 1 below. 

<img width="557" alt="image" src="https://github.com/user-attachments/assets/07264ac1-e931-4490-8787-f7f3de24aeb8" />

Figure 1. Table 11 from Evan Paul's thesis, "Modeling 21st Century Peak Flows in the Nooksack River Basin in Northwestern Washington State Using Dynamically-Downscaled Global Climate Model Projections"

However, I didn't have a chance to run models with new hydrographs this week, so I instead went the normalizing route. 


