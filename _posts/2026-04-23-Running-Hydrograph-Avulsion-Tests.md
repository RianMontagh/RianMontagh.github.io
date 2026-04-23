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
| developed, low intensity | Shrub/Scrub | 
| developed, medium intensity| Grassland/Herbaceous | 
| developed, high intensity | Pasture/Hay |
| Barren land (rock/sand/clay) | Cultivated Crops | 
| Deciduous Forest | Emergent Herbaceous Wetlands |
| Evergreen Forest
| Mixed Forest
| Woody Wetlands

**Question: Why are we using 1m thickness on the floodplain and 10m in the channel?** 
**Question: How did we determine the nonerodible types for the 2021 flood and what does that mean for our avulsion testing that forests cannot erode?**

<img width="1396" alt="image" src="https://github.com/user-attachments/assets/d5a026d4-5528-4294-9750-47eb66f93a37" />

*Figure 1. Spatially varying sediment fractions for fine sand, coarse sand, gravel sand, fine gravel, coarse gravel, and cobble.*

In Figure 1, we can see that the sediment fractions are uniform other than the distinction between erodible and nonerodible areas. The large blob of dark blue to the east of the Everson corridor represents the developed area of the Everson downtown, and the line that extends southward from it is Emerson Road. It makes sense that these should not erode as they are paved surfaces. If anything they would fail suddenly rather than slowly erode. We can also see that the floodplain contains much finer sediment than the sediment in the channel, which makes sense because typically rivers convey coarse sediment in the channel and deposit finer material on the floodplain. 

## New Hydrographs

I made two simple updates to the synthetic hydrographs &emdash; I lowered the peak values to match a report that Paula had sent about the 100-year hydrograph at Deming, which should be about the same as the value at North Cedarville. I previously had a value of about 85,000 cfs from StreamStats, but I knew that this value could have some error in it, especially due to the large size of the watershed that StreamStats was using for its statistical calculations. Paula's Deming value was around 75,000 cfs, so I subtracted 10,000 cfs from all of my StreamStats peak values. 

**Question (maybe for Jessica): Does the 10,000 cfs difference in the 100-year peak flow apply linearly to other return periods?**

I also added a 3-day post-flood baseflow extension to all of the synthetic hydrographs. This allows us to see how long an avulsion in the model would last past the flood peak. See Figure 2 below. 

<img width="927" alt="image" src="https://github.com/user-attachments/assets/1f28c42f-da39-465a-80c5-2e3b0c3cf4ec" />

*Figure 2. Synthetic Hydrographs with updated peaks and post-flood duration.*


## Personal Developement

I have also seen two opportunities that sound interesting to me but I am not sure if the time sink to apply and odds of getting accepted make sense to apply. 

1. [UW Hebold Fellowship](https://www.engr.washington.edu/current/deans_scholarships)
- not sure if I have enough Data Science emphasis, and might need to consider taking more classes in it next year. 
2. [NextProf Pathfinding](https://nextprof.engin.umich.edu/nextprof-pathfinder/)
- would love to learn more about how to get set up for a faculty job! 






