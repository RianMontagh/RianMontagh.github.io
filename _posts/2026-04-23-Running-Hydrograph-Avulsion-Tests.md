# Running Hydrograph Avulsion Tests

I am finally running avulsion sensitivity tests in my Nooksack River model! This means that I am trying out my proposed perturbations to the system, starting with large floods of different duration and magnitude. In addition, I have the chance to look at some floodplain erodibility effects because of Wuming's 2021 model updates &emdash; namely, he made progress in model calibration by making the forested bars in the Everson corridor nonerodible, as well as deveoped landtypes on the floodplain. 

This week I:

1. Created floodplain sediment fraction XYZ files with nonerodible areas based on the 2024 NLCD landcover data and the four vegetated bars identified by Wuming.
2. Created new test hydrographs with a lower peak based on the Whatcom County 100 year flood being 10,000 cfs smalled than the value from StreamStats. I also added a 3-day period of baseflow after the flood hydrograph to look for extensive flow in the channel.
3. Ran the 100-year peak flood at a 3-day and 7-day durations with the original sediment fractions and the sediment fractions with nonerodible areas.

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

In Figure 1, we can see that the sediment fractions are uniform other than the distinction between erodible and nonerodible areas. The large blob of dark blue to the east of the Everson corridor represents the developed area of the Everson downtown, and the line that extends southward from it is Emerson Road. It makes sense that these should not erode as they are paved surfaces. If anything they would fail suddenly rather than slowly erode. We can also see that the floodplain contains much finer sediment than the sediment in the channel, which makes sense because typically rivers convey coarse sediment in the channel and deposit finer material on the floodplain. See Figure 2 below for a comparison to the previous sediment fraction that did not have nonerodible areas. 

<img width="1397" alt="image" src="https://github.com/user-attachments/assets/b0c2ccc4-c5ca-437d-b159-edf28c1311df" />

*Figure 2. Sediment fractions before adding nonerodible areas.*

## New Hydrographs

I made two simple updates to the synthetic hydrographs &emdash; I lowered the peak values to match a report that Paula had sent about the 100-year hydrograph at Deming, which should be about the same as the value at North Cedarville. I previously had a value of about 85,000 cfs from StreamStats, but I knew that this value could have some error in it, especially due to the large size of the watershed that StreamStats was using for its statistical calculations. Paula's Deming value was around 75,000 cfs, so I subtracted 10,000 cfs from all of my StreamStats peak values. 

**Question (maybe for Jessica): Does the 10,000 cfs difference in the 100-year peak flow apply linearly to other return periods?**

I also added a 3-day post-flood baseflow extension to all of the synthetic hydrographs. This allows us to see how long an avulsion in the model would last past the flood peak. See Figure 3 below. 

<img width="927" alt="image" src="https://github.com/user-attachments/assets/1f28c42f-da39-465a-80c5-2e3b0c3cf4ec" />

*Figure 3. Synthetic Hydrographs with updated peaks and post-flood duration.*

## Results from 100-year Hydrograph runs: 3-day and 7-day Duration, Erodible and Non-Erodible

I ran four models this week:

1. 100-year, 3-day flood with uniform floodplain grain size distribution
2. 100-year, 3-day flood with uniform floodplain grain size distribution other than the nonerodible land cover
3. 100-year, 7-day flood with uniform floodplain grain size distribution
4. 100-year, 7-day flood with uniform floodplain grain size distribution other than the nonerodible land cover

I was specifically interested in the presence of flow and erosion patterns in the Everson Overflow. Prolonged flow or incision in the overflow zone indicate avulsive processes. The following four figures show bed level change from the start to the end of the model run time. 

<img width="1262" alt="image" src="https://github.com/user-attachments/assets/c311ad8a-786c-4673-b591-f184141b6f15" />

*Figure 4. Bed level change in overflow after 100 year, 3-day flood with nonerodible areas.*

<img width="1235" alt="image" src="https://github.com/user-attachments/assets/a74a21b9-7d2e-4ba5-ab4a-c45c2c3d8bff" />

*Figure 5. Bed level change in overflow after 100 year, 3-day flood without nonerodible areas.*

### The 7-Day Duration

Figure 6 below shows the bed change after the 100-year, 7-day duration flood, including the addition 3 days of winter low flow. Figure 7 shows the same result but for the model without the nonerodible areas added. 

<img width="1253" alt="image" src="https://github.com/user-attachments/assets/f90c1542-0d84-465c-9f6e-72964ea2e171" />

*Figure 6. Bed level change in overflow after 100 year, 7-day flood with nonerodible areas.*

<img width="1230" alt="image" src="https://github.com/user-attachments/assets/df23269c-0b12-4e05-89d9-f9b07e1479ee" />

*Figure 7. Bed level change in overflow after 100 year, 7-day flood without nonerodible areas.*

One interesting result is shown in the following figure: the incision and erosion caused by the large, long flood is not attenuated by the following three days of lower flow. Basically, the bed change right after the maximum discharge is relatively unchanged by the end of the model run. Comparre Figure 8 with Figure 7 above, and they are almost identical. 

<img width="1216" alt="image" src="https://github.com/user-attachments/assets/fad003de-bc20-48ea-88da-6bcca041705d" />

*Figure 8. Bed level change in overflow close to the time of the 100 year, 7-day flood peak without nonerodible areas.*

<img width="738" alt="image" src="https://github.com/user-attachments/assets/85fe8ca2-5c58-4ce4-b61f-fd86bb664214" />

*Figure 9. 100-year, 7-day Hydrograph with modeled dates.*

Let's take a look at the flow rates in the overflow cross section for each of the model runs. 

<img width="951" height="618" alt="image" src="https://github.com/user-attachments/assets/db07c043-6ff0-497b-8576-58f58e167388" />

*Figure 10. Discharge in the Overflow cross section for the 3-day duration, 100-year flood.*

<img width="960" alt="image" src="https://github.com/user-attachments/assets/c5f94e28-0831-41a0-9694-a41dc470ea79" />

*Figure 10. Discharge in the Overflow cross section for the 7-day duration, 100-year flood.*

## Conclusions

1. Areas of erosion and incision cluster around one of the levees (?) that is not modeled as a fixed weir.
2. There is one persistent area of bed change in all model runs the right overbank where sediment was observed in the farmer's field.
3. Adding the non-erodible areas decreases but does not eliminate the bed change in the right overbank. It also causes an increase in erosion downstream from the levee.
4. Adding the non-erodible areas decreases the peak amount and duration of flow throught the Overflow cross section.
5. Increasing the duration of the flood from 3 days to 7 days causes slightly more intense erosion and deposition in the similar spatial patterns. 

## Personal Developement

I have also seen two opportunities that sound interesting to me but I am not sure if the time sink to apply and odds of getting accepted make sense to apply. 

1. [UW Herbold Fellowship](https://www.engr.washington.edu/current/deans_scholarships)
- not sure if I have enough Data Science emphasis, and might need to consider taking more classes in it next year. 
2. [NextProf Pathfinding](https://nextprof.engin.umich.edu/nextprof-pathfinder/)
- would love to learn more about how to get set up for a faculty job!

## Thoughts on Backing Up Research Files

This week I also started experimenting with OneDrive as a way to backup my files to a cloud environment. I have been noticing some slower processing times in Matlab and slower download speeds from Hyak to my , but need to do a systematic check to make sure that's the case. Wuming and Shelby use hard drives, but I wonder if that is as safe as using a cloud environment or similar. Ryan uses Christie's Kopah storage which is a paid service.  






