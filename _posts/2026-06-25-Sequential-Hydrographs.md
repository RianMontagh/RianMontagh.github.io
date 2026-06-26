# Sequential Hydrographs

This week I ran sequential hydrographs to see how overflow in the second flood changes compared to a non-sequential hydrograph. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/19c6ba88-45b7-492d-a0c7-8587e9618828" />

*Figure 1. Sequential Hydrographs at North Cedarville*

First, here is the timeseries of discharge through the overflow to Sumas cross section. From the 100-year + 100-year flood run, there isn't an obvious difference in the discharge peaks. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/fe2c5c9f-a1db-4514-90e8-56e95a9f29d1" />

*Figure 2. Timeseries of Discharge through the Overflow to Sumas Cross Section*

I then plotted the maximum discharge results from the single 10-, 50-, and 100-year floods alongside the sequential floods to see how different they are. The non-sequential 10-year flood will be ready Friday morning!

<img width="700" alt="image" src="https://github.com/user-attachments/assets/91c1c133-f090-4427-921b-702e1a0a9aee" />

*Figure 3. Comparison of Maximum Overflow to Sumas Values for different Floods*

This figure confirms what we expected, where we suspect there is a mechanism that causes overflow to increase after a large flood. We think that this would be because of feedback looping from deposition and bank overtopping in the Everson Corridor. Another explanation could be that the floodplain is saturated after a flood, which would limit potential for infiltration and increase runoff. I am not sure if the distance from the river to the overflow cross section is large enough for that to be a factor. In the model, there is no groundwater or infiltration, so water tends to pond on the floodplain and that could add to the amount of overflow in a subsequent flood. 

We have also talked about capacity in the channel changing and affecting the threshold for overtopping the banks. However, this seems like a morphodynamic feature that would be interesting to know about, but might not tell us anything directly about avulsion hazard. On the other hand, it would be good to confirm what is causing the increase in overflow to Sumas so that we have an understanding of how channel changes impact flow to the north. 

## Looking at Erodibility Patterns 

I also completed my bed change differencing plots to see how erosion and deposition patterns change due to the floodplain fining factors. Below, for reference, is the bed change for a 50-year, 3-day flood with no change to the floodplain sediment fraction, which is what I compared my floodplain erodibiltiy runs to. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/619b9ed7-db9a-47b8-8548-904ca27875ce" />

*Figure 4. Bed Change for No Change in Floodplain Sediment Fraction/Erodibility (f = 1.00)*

Next, I plotted the bed change of each floodplain erodibility run differenced from the bed change shown in Figure 4. 

<img width="2871" alt="image" src="https://github.com/user-attachments/assets/363f3f9c-8db1-490a-bbc7-59e84c029b0f" />

*Figure 5. Bed Change Differenced from f = 1.00*

This figure is interesting to be because it shows that when the floodplain is finer, the existing pattern of erosion and deposition is amplified. For example, if in the f=1.00 run a cell experiences erosion, then making the floodplain finer increases the amount of erosion that occurs at that cell. The same goes for deposition. The opposite seems to be true when the floodplain gets coarser. To investigate this further, I started playing around with the 3D variables that give information about each sediment class in the model. I was interested to see if fine sediment was getting deposited from the floodplain into the channel. 

## Learning How to Use the 3D Morphologic Variables in Delft3D

I learned how to analyze some of the 3D variables in my MATLAB scripts. I had to edit some of the Open Earth Tools to make this work, because the ordering of dimensions for a 2D (cell faces and time) and for a 3D variable (cell faces, time, and sediment classes) is different, and this affects how the variables are concatenated. I first tested this with the equilibrium concentration, which is the equilibrium transport rate where the same amount of suspended sediment is desposited as is uplifted. However, I quickly realized that all of our suspended transport is equal to zero because Wilcock and Crowe only takes bedload into account. In map variables, the only bedload variables are the x and y variables, and I tested out my new code on the x component of bedload transport and had sucess! However I am not showing the plots here because I think it is much easier to compare the bedload transport rates at specific cross sections. 

## Investigating Sediment Transport of Different Floodplain Grain Size Distributions

Hoping to have this section tomorrow! 

## Goals for Next Week

1. Perturb the forested bar that might be a control on the ability of a flood to form an avulsive channel on the floodplain to river right.
2. Reach out to Shelby again about along-channel profile plots.
3. Create transects of bed change on the floodplain.
4. Make a more comprehensive summer goals plan to keep myself on track. 


