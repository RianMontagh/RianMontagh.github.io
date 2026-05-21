# Deming Return Period Flows and Verifying Model Results

## Deming Return Period Flows

I decided to spend some time with a report titled "FLOOD FREQUENCY ANALYSIS AT DEMING, FERNDALE, AND EVERSON" written by Delbert D. Franz of Linsley, Kraeger Associates, Limited. This report goes into detail on the methods used to reconstruct the return periods for flooding at Ferndale, Deming, and the overflow gage at Main St. The usual methods don't work here because 1) the gage at Deming is unreliable and 2) Some of the Ferndale annual peak flows are affected by the overflow at Everson (mixed population). Delbert used the general following methodology to get his peak flows at Ferndale and Deming (ignoring overflow frequency analysis since I care more about Ferndale and Deming. 

Steps for Deming Return Periods 

1. Run unsteady flow models of historic floods and extract 15 historic peak flows to get paired Deming and Ferndale peak flows.
2. Get a statistical relationship that allows you to go from Ferndale flows to Deming flows.
3. Apply this relationship to the full period of record of Ferndale (stable) (n=57) to get 57 Deming peak flow points.
4. Run a Log-Pearson Type III analysis on the 57 constructed Deming flows to get the return periods. 

<img width="300" alt="image" src="https://github.com/user-attachments/assets/96a9f347-0e62-4c66-9a0c-4e75e4a17f61" />

*Figure 1. Flood Return Periods at Deming*

Steps for Ferndale and Everson Overflow Flood Frequency Analysis

1. Run scaled up, larger flows to supplement the historic events and get Deming flows.
2. Get a statistical relationships that allow you to go from Deming flows to Ferndale flows and Deming flows to Everson overflow flows.
3. Apply these relationships to the flood frequency results for Deming to get flood frequency results for Ferndale and Everson overflow. 

<img width="300" alt="image" src="https://github.com/user-attachments/assets/55a794aa-0df5-4c6f-b675-dbe94aacee90" />

*Figure 2. Flood Return Periods at Everson Overflow (Main St) and Ferndale*

Takeaway: Uncertainty is high for the larger return periods. Should round to at most two significant figures. 

## Comparing Results of the New Model with 2025 Flood Hydrograph

After making so many changes to my model, I realized that I needed to make sure that the 2025 hydrograph was looking okay in comparison to the USGS observations to gain some confidence in the results from my synthetic hydrograph runs. I used a few runs that I had done in earlier model versions and some new model runs with the current configuration. 

Starting with water surface elevation at Everson, Ferndale, and the Main St. gage:

Everson WSE Comparison          |  Ferndale WSE Comparison
:-------------------------:|:-------------------------:
![](https://github.com/user-attachments/assets/7c03bac6-8b8a-42cd-ab8e-1e0be7161f1a)  |  ![](https://github.com/user-attachments/assets/309ca5fa-c70c-4e97-85dd-775ddc5e326f)

Overflow WSE Comparison          |
:-------------------------:|
<img width="500" alt="MainSt" src="https://github.com/user-attachments/assets/dc5a9eed-9ddb-408b-a42a-b6034ece56f7" /> |

*Figure 3. Comparisons of the WSE at Everson, Ferndale, and the Overflow gage to USGS measurements for a variety of model runs and configurations.*

## Along Channel, Cross-Section Averaged Bed and Water Surface Elevations

<img width="1373" alt="BedandWaterElevation" src="https://github.com/user-attachments/assets/3e145fc1-e189-4e14-a4a2-4228048d70bc" />

*Figure 4. Along Channel, cross-section averaged bed level and water surface elevations for 2025 Flood with Non-Erodible Areas. Sediment fractions and mannings n are current from the 2024 mannings iterations.*



