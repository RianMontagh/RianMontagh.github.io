# Continuing to Investigate the Old Model

After being confused by my tests last week on figuring out why the old model has incision on the floodplain while mine doesn't, I tried a third test, which was to run the old model with the sediment (SED) files from the current model. I was expecting the model to not have incision on the floodplain, and this actually worked! Then I isolated the Acal factor to see if that was the most important factor, and as expected, the incision dramatically reduced on the floodplain when I turned down the Acal in the old model to 1. 

<img width="1620" alt="image" src="https://github.com/user-attachments/assets/624d856c-399c-4b25-9353-f40a2c66982a" />

*Figure 1. Old model with different changes to be like the new model*

I tried the opposite experiments in the current model, and got interesting results. Even when running the model with the old sediment settings and fractions, channel formation did not occur. It also did not occur when just increasing the Acal from 1 to 4. That means there must be another factor in the new model that is suppressing the floodplain incision. Next I want to run models with the old 2015 terrain and the .MOR file. 

<img width="1620" alt="image" src="https://github.com/user-attachments/assets/7f5af3d1-814e-4de9-84aa-e6b2f98185ee" />

*Figure 2. Current model with different changes to be like the old model*

I also plotted my hydrographs alongside the 2021 flood to make sure that my understanding of the floods was correct. My 3-day duration floods look similar to the 2021 peak. The non-climate scaled hydrograph matches the 2021 peak well. Since I am running the climate scaled hydrograph for my "current model" runs, I expected to see more incision than with the non-climate scaled 100-year hydrograph. 

<img width="800" alt="image" src="https://github.com/user-attachments/assets/1d51588b-827d-4a45-94fb-cb1b1f21484c" />

*Figure 3. Visualization of 2021 flood and my 100-year Hydrographs*

Lastly, I compared the normalized overflow to Sumas for the old model and the current model (this one has 10 m of sediment thickness on the floodplain). Surprisingly, they are almost exactly the same. The peak values are 0.45359 and 0.46039 for the old and current models, respectively. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/d5f005e0-fff8-4222-b13f-b52fbecbf797" />

*Figure 4. Overflow to Sumas, not normalized.*

<img width="700" alt="image" src="https://github.com/user-attachments/assets/1f7861dc-350e-4007-b5a7-a2fbb6bb6fea" />

*Figure 4. Overflow to Sumas, normalized.*









