# Running Hydrograph Avulsion Tests

I am finally running avulsion sensitivity tests in my Nooksack River model! This means that I am trying out my proposed perturbations to the system, starting with large floods of different duration and magnitude. In addition, I have the chance to look at some floodplain erodibility effects because of Wuming's 2021 model updates &emdash; namely, he made progress in model calibration by making the forested bars in the Everson corridor nonerodible, as well as deveoped landtypes on the floodplain. 

This week I:

1. Created floodplain sediment fraction XYZ files with nonerodible areas based on the 2024 NLCD landcover data and the four vegetated bars identified by Wuming.
2. Created new test hydrographs with a lower peak based on the Whatcom County 100 year flood being 10,000 cfs smalled than the value from StreamStats. I also added a 3-day period of baseflow after the flood hydrograph to look for extensive flow in the channel.
3. Ran the 100-year peak flood at a 3-day duration with the original sediment fractions and the sediment fractions with nonerodible areas.

## New Sediment Fractions 

I did a similar process as last week, where I went through Wuming's code to make sure I was understanding it as well as adding my own comments. This was a bit quicker as many of the same methods were used for this analysis. The final sediment fractions are shown in Figure 1. 

The land types that were set to nonerodible are: 

| Nonerodible | Erodible |
| ------------ | --------|
| Developed, open space | Open Water | 
| developed, low intensity | Shrubland | 
| developed, medium intensity| Grassland | 
| developed, high intensity | Pasture
| Barren land (rock/sand/clay) | Crops | 
| Deciduous Forest | Herbaceous Wetlands |
| Evergreen Forest
| Mixed Forest
| Woody Wetlands

**Question: Why are we using 1m thickness on the floodplain and 10m in the channel?** 
**Question: What 

<img width="1396" alt="image" src="https://github.com/user-attachments/assets/d5a026d4-5528-4294-9750-47eb66f93a37" />

*Figure 1. Spatially varying sediment fractions for fine sand, coarse sand, gravel sand, fine gravel, coarse gravel, and cobble.*





