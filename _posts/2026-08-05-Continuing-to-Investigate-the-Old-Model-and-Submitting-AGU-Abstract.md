# Continuing to Investigate the Old Model and Submitting AGU Abstract

## Tests with the Old Model

After being confused by my tests last week on figuring out why the old model has incision on the floodplain while mine doesn't, I tried a third test, which was to run the old model with the sediment (SED) files from the current model. I was expecting the model to not have incision on the floodplain, and this actually worked! Then I isolated the Acal factor to see if that was the most important factor, and as expected, the incision dramatically reduced on the floodplain when I turned down the Acal in the old model to 1. 

<img width="1620" alt="image" src="https://github.com/user-attachments/assets/624d856c-399c-4b25-9353-f40a2c66982a" />

*Figure 1. Old model with different changes to be like the new model*

## Tests with the Current Model

I tried the opposite experiments in the current model, and got interesting results. Even when running the model with the old sediment settings and fractions, channel formation did not occur. It also did not occur when just increasing the Acal from 1 to 4. That means there must be another factor in the new model that is suppressing the floodplain incision. Next I want to run models with the old 2015 terrain and the .MOR file. 

<img width="2909" alt="image" src="https://github.com/user-attachments/assets/e8bc4b08-0744-496f-85f9-3802cb5e2482" />

*Figure 2. Current model with different changes to be like the old model*

## Comparing Old and Current Model Upstream Forcing and Overflow to Sumas

I also plotted my hydrographs alongside the 2021 flood to make sure that my understanding of the floods was correct. My 3-day duration floods look similar to the 2021 peak. The non-climate scaled hydrograph matches the 2021 peak well. Since I am running the climate scaled hydrograph for my "current model" runs, I expected to see more incision than with the non-climate scaled 100-year hydrograph. 

<img width="800" alt="image" src="https://github.com/user-attachments/assets/1d51588b-827d-4a45-94fb-cb1b1f21484c" />

*Figure 3. Visualization of 2021 flood and my 100-year Hydrographs*

Lastly, I compared the normalized overflow to Sumas for the old model and the current model (this one has 10 m of sediment thickness on the floodplain). Surprisingly, they are almost exactly the same. The peak values are 0.45359 and 0.46039 for the old and current models, respectively. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/d5f005e0-fff8-4222-b13f-b52fbecbf797" />

*Figure 4. Overflow to Sumas, not normalized.*

<img width="700" alt="image" src="https://github.com/user-attachments/assets/1f7861dc-350e-4007-b5a7-a2fbb6bb6fea" />

*Figure 5. Overflow to Sumas, normalized.*

## Submitting AGU Abstract

I submitted my AGU abstract, which I am copying here for future reference and documentation. 

> **Evaluating the potential for event-driven avulsion in the Nooksack River (Washington, USA) using morphodynamic modeling**
> 
> River avulsion is the natural process by which a river suddenly changes course. This can occur locally at the meander scale or regionally at the basin scale; the latter typically results in a substantial reorganization of the downstream river system. There is evidence that within the late Holocene, the Nooksack River in northwest Washington, USA experienced a regional avulsion from its previous northward course toward Canada into its present westward course. During large present-day floods, floodwaters reoccupy the topographic low of the hypothesized pre-avulsion path. This occurred during the floods of 2021 and 2025, when a portion of the river’s flow rerouted through this low-lying corridor, flooding communities in Washington and British Columbia and causing extensive economic damage. Past work on the Nooksack River suggests that during large floods, sediment transport capacity and flow conveyance in the main channel decrease, creating a positive feedback loop that forces more water overbank in the direction of the hypothesized historical river path. These findings suggest that the Nooksack River may be trending toward avulsion, especially as climate change intensifies atmospheric rivers in the region. 

> Here, we investigate event-scale avulsion triggering on the Nooksack River using a depth-averaged, two-dimensional morphodynamic Delft3D-FM model forced with future climate-projected flood hydrographs. Specifically, we assess whether extreme floods can initiate an avulsion and identify mechanisms controlling avulsion initiation. We hypothesize that the ability of the rerouted flow to incise into the river’s alluvial ridges exerts a strong control on avulsion initiation. To test this, we vary morphodynamic parameters such as floodplain roughness, sediment coarseness, flood peak magnitude, flood duration, and number of flood events. Preliminary results indicate that larger floods and a more erodible floodplain increase flow in the potential avulsion pathway. This study improves understanding of event-scale avulsion triggering, which is understudied relative to longer-term avulsion setup, and will inform floodplain management strategies aimed at reducing avulsion risk in the Nooksack River.  

## Animation of the Old Model Bed Elevation

<video controls width="800">
  <source src="https://github.com/user-attachments/assets/335858d8-a7ab-4aa7-8f27-c3664c2ac9e9" type="video/mp4">
  Your browser does not support the video tag.
</video>














