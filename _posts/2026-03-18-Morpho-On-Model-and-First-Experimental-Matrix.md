# First Morpho On Model and First Experimental Matrix

This week I ran my first first model with morphology turned on (morpho on) and came up with a testing matrix for a first round of experiments on avulsion sensitivity in our model. We are starting with the duration and peak magnitide of the hydrograph at the upstream boundary, which is easy to manipulate and we have projections for what these hydrographs might look like in the future. 

## Hydrograph Tests Matrix

To create this matrix, I obtained projected changes in the peak magnitude of the 20-year, 50-year, and 100-year peak flows from Evan Paul's 2023 masters thesis, "Modeling 21st century peak flows in the Nooksack River Basin in northwestern Washington State using dynamically-downscaled global climate model projections" (Figure 1). I then used the 1%, 2%, and 10% Annual Exceedance Probabilities (AEPs) from the USGS Streamstats tool for a delineated basin with North Cedarville at the outlet (Figure 2). The 1% AEP corresponds to the 100-year flood, the 2% AEP to the 50-year flood, etc. Evan Paul's percent changes in peak magnitude can be applied to these values. 

<img width="588" alt="Screenshot 2026-03-19 at 12 28 22 AM" src="https://github.com/user-attachments/assets/dc7a5fe6-74d1-44d3-8fe9-00ed49eecf0c" />

*Figure 1. Table 11 from Evan Paul's masters thesis. These are the predicted change in peak flow magnitudes for 30 year normals centered on 2050 and 2080 at different locations on the Nooksack. The values we are interested in are the North Cedarville for the 10-year, 50-year, and 100-year flood. The 2050 and 2080 normals are differenced with the 1990 normal to get the percent change.*

As seen in Figure 1, the 500-year event was not analyzed, so I revised my original plan to have a 500-year flood and added on an event with four 20-year floods to the matrix. 

**Question**: Do we want a 500-year event in our model?

<img width="787" height="741" alt="Screenshot 2026-03-19 at 12 21 35 AM" src="https://github.com/user-attachments/assets/bdb5acb8-ef02-4282-8448-7809703f23fc" />

*Figure 2. Streamstats report for the basin above North Cedarville.*

<img width="687" alt="Screenshot 2026-03-18 at 10 51 46 PM" src="https://github.com/user-attachments/assets/97d36185-f51b-476b-8ae0-bf6d0a7cf421" />

*Figure 3. Updated hydrograph matrix with duration, peak magnitude, and number of peaks varied.*

## Morpho On Model

I ran my first model with sediment transport and bed level change turned on! 

I only had time to plot the bed level change at the last time step relative to the first time step.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/18436234-a9fb-4873-a634-1e98f58f0d5f" />

*Figure 4. Bed level change from the start of the model run to the end of the model run.*

I need more time to think about these results, but first reaction is that there is not nearly as much change as I am used to seeing in Wuming's plots. There is a lot of white space within the borders of the channel. 

