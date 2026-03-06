# Spatially Varying Manning's Roughness in the Channel

This week I worked on first obtaining the 2024 NLCD (National Landcover Database) raster for the 2025 model run, and I also implemented Wuming's iterative approach to get a converging solution to the spatially varying roughness on the bed of the Nooksack. This is important because the NLCD raster classifies the landcover type of the river as 'Open Water' which does not give us any information on the way the bed material changes in space. To get around this problem, Wuming found the following solution. 

1. Set a uniform value of n=0.23 everywhere in the model.
2. Run the model with a constant, approximately bankfull flow until steady state is reached.
3. Convert the spatially varying shear stress on the bed to a spatially varying $D_{50}$ (median grain size) with the Shields criterion. This uses the assumption that $D_{50}$ is mobilized as bed load at bankfull flow.
4. Convert spatially varying $D_{50}$ to a spatially varying Manning's roughness (n) with the Manning-Strickler equation.

This process is there iterated by running a new model with the new spatially varying Manning's roughness until the Manning's roughness at the end of an iteration converges (negligible change in roughness). 

<img width="800" height="616" alt="image" src="https://github.com/user-attachments/assets/d82d98df-1788-4ea4-a00d-34d914373e86" />

*Figure 1. Ensuring that steady state is reached in the first iteration.*

This figure shows that after about one day, a constant WSE is reached. This is true for the other two iterations as well. 

<img width="800" height="620" alt="image" src="https://github.com/user-attachments/assets/ef0387bd-2cf3-4ff0-9bf6-24fc276c5252" />

*Figure 2. Shear stress over the model domain after one run*

This figure shows how the river channel has spatially varying shear bed stress, and the floodplain has zero stress.

<img width="700" height="626" alt="image" src="https://github.com/user-attachments/assets/8a9de486-e4d7-49fd-b61c-2eba415000cf" />

*Figure 3. Manning's Roughness calculated from $\tau_b$ using the Shields criterion and Manning-Strickler equation (Iteration 1).*

The Manning's roughness data shown in Figure 3 was then used as the starting roughness value for iteration 2. 

<img width="800" height="615" alt="image" src="https://github.com/user-attachments/assets/3274b62d-feb3-4181-9dae-df8cd5700400" />

*Figure 4. Differencing Manning's roughness results from iteration 1 and 2.*

From iteration 1 to iteration 2, roughness mostly increased (red dots). This means that shear stress was higher overall at the end of second simulation than the first simulation. 

<img width="800" height="618" alt="image" src="https://github.com/user-attachments/assets/071cb4f2-e203-467a-abd2-215de3364053" />

*Figure 5. Differencing Manning's roughness results from iteration 3 to iteration 1 and iteration 2.*

Figure 5 shows that the Manning's roughness changes more from iteration 2 to 3 than from 1 to 2. This gives an indication that the solution is converging. 

We can also look at some statistics:

    mean difference (MN2-MN1) = 0.000026 
    RMSE between MN2 and MN1 = 0.000179
    mean difference (MN3-MN2) = -0.000003 
    RMSE between MN3 and MN2 = 0.000172

The mean difference has decreased by a factor of about 10 from iteration 2 to iteration 3 (MN2 and MN3). The RMSE has also decreased, but not by much. These statistics could be improved by only using the values in the channel, as the floodplain has zero change and is skewing these statistics. 

Question for Wuming: How did you know that three iterations was enough?

The last step in the analysis was to combine the spatially varying roughness in the channel with the NLCD roughness on the floodplain. 

<img width="900" height="612" alt="image" src="https://github.com/user-attachments/assets/6655ea2f-51b7-44d9-bbaf-37aab00d51d4" />

*Figure 6. Combined Roughness at Everson.*

