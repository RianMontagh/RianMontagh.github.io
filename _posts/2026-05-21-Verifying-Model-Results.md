# Deming Return Period Flows and Verifying Model Results

## Deming Return Period Flows

I decided to spend some time with a report titled "FLOOD FREQUENCY ANALYSIS AT DEMING, FERNDALE, AND EVERSON" written by Delbert D. Franz of Linsley, Kraeger Associates, Limited. This report goes into detail on the methods used to reconstruct the return periods for flooding at Ferndale, Deming, and the overflow gage at Main St. The usual methods don't work here because 1) the gage at Deming is unreliable and 2) Some of the Ferndale annual peak flows are affected by the overflow at Everson (mixed population). Delbert used the general following methodology to get his peak flows at Ferndale and Deming (ignoring overflow frequency analysis since I care more about Ferndale and Deming. 

Steps for Deming Return Periods 

1. Run unsteady flow models of historic floods and extract 15 historic peak flows to get paired Deming and Ferndale peak flows.
2. Get a statistical relationship that allows you to go from Ferndale flows to Deming flows.
3. Apply this relationship to the full period of record of Ferndale (stable) (n=57) to get 57 Deming peak flow points.
4. Run a Log-Pearson Type III analysis on the 57 constructed Deming flows to get the return periods. 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/96a9f347-0e62-4c66-9a0c-4e75e4a17f61" />

*Figure 1. Flood Return Periods at Deming*

Steps for Ferndale and Everson Overflow Flood Frequency Analysis

1. Run scaled up, larger flows to supplement the historic events and get Deming flows.
2. Get a statistical relationships that allow you to go from Deming flows to Ferndale flows and Deming flows to Everson overflow flows.
3. Apply these relationships to the flood frequency results for Deming to get flood frequency results for Ferndale and Everson overflow. 

<img width="600" alt="image" src="https://github.com/user-attachments/assets/55a794aa-0df5-4c6f-b675-dbe94aacee90" />

*Figure 2. Flood Return Periods at Everson Overflow (Main St) and Ferndale*

Takeaway: Uncertainty is high for the larger return periods. Should round to at most two significant figures. 

## Comparing Results of the New Model with 2025 Flood Hydrograph

After making so many changes to my model, I realized that I needed to make sure that the 2025 hydrograph was looking okay in comparison to the USGS observations to gain some confidence in the results from my synthetic hydrograph runs. I used a few runs that I had done in earlier model versions and some new model runs with the current configuration. 

Starting with water surface elevation at Everson, Ferndale, and the Main St. gage:

Everson WSE Comparison          |  Ferndale WSE Comparison
:-------------------------:|:-------------------------:
![](https://github.com/user-attachments/assets/f7ce2eaa-7e20-4b71-8444-b3fed0be5237)  |  ![](https://github.com/user-attachments/assets/36ec5a6d-fb79-4fdf-bcde-33fc26d81969)

Overflow WSE Comparison          |
:-------------------------:|
<img width="800" alt="MainSt" src="https://github.com/user-attachments/assets/888a66d9-8875-4e1d-bb7c-3749eb77ddf5" /> |

*Figure 3. Comparisons of the WSE at Everson, Ferndale, and the Overflow gage to USGS measurements for a variety of model runs and configurations.*

From these figures, we can conclude that for the most part nothing super out of the ordinary is going on in the models, which makes me able to trust the results from the synthetic hydrograph runs a little more. 

Looking at the differences between the models is a bit more confusing. Some patterns I notice:

1. The two morpho on models generally have higher WSE.
2. Updating the Mannings n increases WSE for Everson and the Overflow but not Ferndale. 

## Along Channel, Cross-Section Averaged Bed and Water Surface Elevations

With Wuming's help, I was able to average variables across cross sections in the Everson reach and plot them vs. distance along the main channel. One question that I had when making this is about how we define the extents of the channel. For this analysis, we use a low-flow channel, but I think that updating this to be a bankfull channel makes more sense if we are using this as a proxy for conveyance change. For instance, if along the cross section the bed on average aggraded, we might assume that conveyance has been lost. We would want that cross section to cover the entire channel. 

<img width="700" alt="CrossSections_onAerial" src="https://github.com/user-attachments/assets/efe1fcf6-3250-4f88-b9a8-cbe06588d119" />

*Figure 4. Visualization of the cross sections used for the averaging. Notice how they don't cover many of the gravel bars.*

<img width="700" alt="BedandWaterElevation" src="https://github.com/user-attachments/assets/3e145fc1-e189-4e14-a4a2-4228048d70bc" />

*Figure 5. Along Channel, cross-section averaged bed level and water surface elevations for 2025 Flood with Non-Erodible Areas. Sediment fractions and mannings n are current from the 2024 mannings iterations.*

<img width="700" alt="NetBedChange" src="https://github.com/user-attachments/assets/12875ed3-6baf-4296-9087-5fd84aae766c" />

*Figure 6. Along Channel, cross-section averaged change in bed level 2025 Flood with Non-Erodible Areas. Sediment fractions and mannings n are current from the 2024 mannings iterations.*

Takeaway - most of the bed aggraded! Water surface elevation after the first peak is almost exactly the same for morpho-on and off, but after the big peak the water surfaces diverge. 

## Looking at the Spatial Distribution of Water for the Synthetic Hydrographs.

1-Day Duration              |  3-Day Duration          |  7-Day Duration
:-------------------------:|:-------------------------:|:-------------------------:
  |  ![](https://github.com/user-attachments/assets/77357114-d64e-4757-a7e2-78fb87025739)  |  ![](https://github.com/user-attachments/assets/5214735a-45e8-46ee-a08c-7c35b221c4be)

The distribution of water at the maximum water surface elevations are relatively similar between durations. The 7-day duration does have more inundation to river right but river left looks very similar to the 3-day duration. Is it possible that the longer duration just causes the water to spread out more but not necessarily push more through the overflow pathway?

Question: Does the length of our outflow boundary condition matter? Right now it does not span the entire width of the slug of water moving north.  
Question: Is there a way to plot arrows pointing which direction the velocity of the water in each cell is?







