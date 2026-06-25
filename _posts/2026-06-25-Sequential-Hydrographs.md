# Sequential Hydrographs

This week I ran sequential hydrographs to see how overflow in the second flood changes compared to a non-sequential hydrograph. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/19c6ba88-45b7-492d-a0c7-8587e9618828" />

*Figure 1. Sequential Hydrographs at North Cedarville*

## Looking at Erodibility Patterns 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/619b9ed7-db9a-47b8-8548-904ca27875ce" />

*Figure 2. Bed Change for No Change in Floodplain Sediment Fraction/Erodibility (f = 1.00)*


<img width="2871" alt="image" src="https://github.com/user-attachments/assets/363f3f9c-8db1-490a-bbc7-59e84c029b0f" />

*Figure 3. Bed Change Differenced from f = 1.00*

## Learning How to Use the 3D Morphologic Variables in Delft3D

I learned how to analyze some of the 3D variables in my MATLAB scripts. I had to edit some of the Open Earth Tools to make this work, because the ordering of dimensions for a 2D (cell faces and time) and for a 3D variable (cell faces, time, and sediment classes) is different, and this affects how the variables are concatenated. I first tested this with the equilibrium concentration, which is the equilibrium transport rate where the same amount of suspended sediment is desposited as is uplifted. However, I quickly realized that all of our suspended transport is equal to zero because Wilcock and Crowe only takes bedload into account. In map variables, the only bedload variables are the x and y variables, and I tested out my new code on the x component of bedload transport and had sucess! However I am not showing the plots here because I think it is much easier to compare the bedload transport rates at specific cross sections. 

## Investigating Sediment Transport of Different Floodplain Grain Size Distributions


