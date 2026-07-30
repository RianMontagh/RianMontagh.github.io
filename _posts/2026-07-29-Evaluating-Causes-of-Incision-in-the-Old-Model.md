# Evaluating Causes of Incision on the Floodplain in the Old Model

I looked more into my own models' bed elevation after the flood and found that my models do not form the same incisional channels as the old model. 

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/7fa81bc3-c94d-468b-9d2f-35332e618bad" />

*Old Model bed elevation before and after the 2021 flood*

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/d6b2e165-9a7a-404d-ad45-130e71a25be7" />

*Current Model bed elevation before and after 100-year climate-adjusted, 7-day duration flood.*

## Differences Between Shelby's Model and the Current Model

I wanted to document the differences between the old model and the current model to identify things that could be causing the incisional channel formation in the old model but not in the new model. 

1. Sediment names/sizes
- the old model uses coarse and fine boulder in addition to the six sediment categories used by the current model for a total of eight categories and XYZ files.
2. Sediment thickness
- the old model uses a sediment thickness of 10 m everywhere, while our model has a thickness of 1 m on the floodplain.
3. Nonerodible areas
- The old model just has thin areas of about a one cell width defining roads and maybe levees. This looks more precisely done than the current model which uses the NLCD land use types instead of hand-tracing the roads.
4. Acal
- Old model uses Acal = 4, current model uses Acal = 1
5. SED file Setting - this setting is missing in the new model

   ```text
   [SedimentOverall]

   Cref                  = 1600                   [kg/m3]   Reference density for hindered settling calculations
   ```
6. Dry bed density
- The dry bed density `CDryB` is 1600 kg/m3 for all the sizes in the old model, whereas in the current model the `CDryB` is 1600, 1800, or 2000 kg/m3 depending on grain size.
 

A clear next step based on these differences was to try two things
- Run the current model with a 10 m thickness on the floodplain. This would preserve the channel and floodplain sediment fractions and the nonerodible areas. 
- Run the current model with the sediment fractions from the old model. This would change the nonerodible areas and the sediment in the channel.

## Visualizing the Sediment Differences

<img width="2025" alt="image" src="https://github.com/user-attachments/assets/f1f8f42c-547c-4d72-a8c6-f19195d7ed3a" />

*Sediment fractions for the current model. Darkest blue is the nonerodible areas (thickness = 0)*

<img width="2159" alt="image" src="https://github.com/user-attachments/assets/6ac2eebe-a9f1-4ede-84eb-b9773ac58919" />

*Sediment fractions for the old model. Darkest blue is the nonerodible areas (thickness = 0)*

Current Model Total Thickness    |  Old Model Total Thickness
:-------------------------:|:-------------------------:
![](https://github.com/user-attachments/assets/a9e0a6a8-3b21-4bfe-aee5-53a75cae2514)  |  ![](https://github.com/user-attachments/assets/b4e94048-a019-4db9-acf0-b9c6490c38b9)

## Trials with Parts of the Old Model

<img width="1775" alt="image" src="https://github.com/user-attachments/assets/618abf74-85b5-4347-89df-8cddd3493299" />

*Results from changing the sediment fractions to the old model and using the old model SED file settings.*

Changing the sediment fractions to the old model sediment fractions did not make a difference, which is really strange to me! I was expecting the nonerodible areas to be the thing limiting incision. I need to do a triple check that the sediment fractions are actually different. If I did everything correctly, then I am guessing there is a setting in the MOR file that is causing increased morphologic change. 

<img width="2445" alt="image" src="https://github.com/user-attachments/assets/f0be0ba1-9244-4365-bd81-14f8436f3bb7" />

*Results from changing the floodplain of the current model to have a total thickness of 10 m instead of 1 m*

This result for the current model run with the current sediment fractions and the current SED file settings is more expected to me. The only change here is that there is potential for up to 10 m in erosion instead of just 1 m. However, in the 1 m floodplain thickness version of the model, incisional channels are non existent, so it makes sense that they are also nonexistent in a thicker floodplain. 
















