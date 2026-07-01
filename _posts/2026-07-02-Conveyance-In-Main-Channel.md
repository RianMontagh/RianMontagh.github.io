# Analyzing Conveyance in the Main Channel

## Definition of Channel Conveyance

I reviewed the definition of channel conveyance in preparation for creating my own analysis for the Nooksack. According to the Texas DOT Hydraulic Design Manaul, where K is conveyance:

<img width="200" alt="image" src="https://github.com/user-attachments/assets/60ce48e0-d276-49c7-9f87-f15b7b424b07" />

and

<img width="200" alt="image" src="https://github.com/user-attachments/assets/2d6ef3a4-c30f-4ade-9dfd-0d6f6d949d74" />

This was different than the definition in my head, where conveyance has units of duscharge. Instead, conveyance is the geometry and roughness of the channel - as these values allow for more discharge, the conveyance increases. It does not take into account the water surface slope, but instead says for a given surface slope, the channel can convey x amount. If flow is uniform, then S = bed slope instead of the energy grade line slope. 

## Reviewing Shelby's Dissertation

Shelby's method to calculate bed change and conveyance change for the Nooksack River is described in section 4.3.2. It seems like conveyance in this context is the discharge in the main channel. 

To calculate cross-sectionally averaged longitudinal profile for my model based on the 2025 flood:

1. Use active channel centerline from Anderson et al. (2019) and Boyd et al. (2019)
    - I have one from Wuming
2. Calculate average parameters at 2km transects, sapced 15 m in the streamwise direction
3. Define the main channel as the cells that have at least 1.5 m depth at bankfull flow (700 m^3/s).
    - upstream of Everson Overflow, where channel is braided: average the wetted channed for all occurrances of bankfull flow in the 2025 flood
    - downstream of the Everson Overflow, use the first occurrence of bankfull flow to prevent effects of ponded water on floodplain
  
**My Implementation**

I decided to run a bankfull, steady hydrograph model to get the limits of the main bankfull channel. Because I want to use this analysis for the entire lower Nooksack, I want flow to be at 600 cms at all times so that the flow is consistent throughout our reach. To my understanding, Shelby used times when the input hydrograph was at bankfull to get the main channel limits, but to me it seems like this would not take into account the lag time from North Cedarville to the rest of the model domain. 

Below are plots of discharge and water depth for the model run with a constant bankfull depth for three days. I used 600 cms because that is what we used for the bankfull discharge for our Manning n iterations. 





