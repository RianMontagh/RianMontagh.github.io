# Evaluating Causes of Incision on the Floodplain in the Old Model

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/7fa81bc3-c94d-468b-9d2f-35332e618bad" />

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/d6b2e165-9a7a-404d-ad45-130e71a25be7" />

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
The dry bed density `CDryB` is 1600 kg/m3 for all the sizes in the old model, whereas in the current model the `CDryB` is 1600, 1800, or 2000 kg/m3 depending on grain size.
 

A clear next step based on these differences was to try two things
- Run the current model with a 10 m thickness on the floodplain. This would preserve the channel and floodplain sediment fractions and the nonerodible areas. 
- Run the current model with the sediment fractions from the current model. This would change the nonerodible areas and the sediment in the channel.

## Visualizing the Sediment Differences

<img width="2025" alt="image" src="https://github.com/user-attachments/assets/f1f8f42c-547c-4d72-a8c6-f19195d7ed3a" />

<img width="2159" alt="image" src="https://github.com/user-attachments/assets/6ac2eebe-a9f1-4ede-84eb-b9773ac58919" />

<img width="1207" alt="image" src="https://github.com/user-attachments/assets/a9e0a6a8-3b21-4bfe-aee5-53a75cae2514" />

<img width="1207" alt="image" src="https://github.com/user-attachments/assets/b4e94048-a019-4db9-acf0-b9c6490c38b9" />















