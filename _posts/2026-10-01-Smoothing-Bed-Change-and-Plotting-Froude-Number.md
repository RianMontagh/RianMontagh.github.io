# Verifying Conveyance Plots, Smoothing Streamwise Bed Change, and Plotting Streamwise Froude Number

## Verifying Conveyance Plots

Now that I have plotted Q, V, and A through my bankfull cross sections, I wanted to do some more work to verify they are working as I am expecting them to. I want to gain more confidence in them before I start drawing conclusions from them. The first thing I did was see if Q (output variable cross_section_discharge) equals $V*A$, where V is the cross_section_velocity and A is the cross_section_area. After meeting with Alex and Wuming last week, I realized that the model could be outputting the average velocity including parts of the cross section that have zero velocity. For example, if a cross section spans a bar or a braided section, the average velocity in the cross section might be close to zero due to averaging zeros in with the actually velocity. However, the plot below verifies that cross_section_velocity is the same as $Q/A$ which is the velocity through the wetted areas only, and does not include dry areas like bars. 

<img width="1994" alt="image" src="https://github.com/user-attachments/assets/3602059f-94af-4681-9ecf-eb0437e9c4b7" />

*Figure 1. Showing that Q = V*A*

Next, I quickly verified that my output times made sense. For the history files, I verified that the output times were as close as possible to the Q = 600 m^3/s and the two peaks at Everson. This turned out to be correct and the red dots that are offset from exactly Q = 600 m^3/s is because the output times don't line up exactly with the time that Everson experiences 600 m^3/s.  

<img width="1740" alt="image" src="https://github.com/user-attachments/assets/ad4fbfec-e5fd-41c0-9b95-d6059a86f715" />

*Figure 2. Visualization of the data points spanning the Q = 600 m^3/s line.*

Thirdly, I wanted to visualize the extent of flooding in comparison to my bankfull cross sections. Specifically, I wanted to see if the length of my cross sections make sense and if I can understand more about the bumps in my plots. 


## Area-Averaging Instead of Cross Section-Averaging

## Streamwise Froude Number

## Reviewing my AGU Abstract

## Debugging Sediment Fraction Code

In this section I wanted to document a bug I found in the code for creating the sediment fractions. After talking to Wuming, we don't expect it to affect our model results but decided it is still important to fix and test. The bug has to do with using two slightly different masks to define where the channel is; one mask we use to calculate sediment bins from D_50, and the other mask we use add sand and the floodplain. This causes some cells to have a zero sediment thickness (nonerodible) within the channel, because the second mask is slightly bigger than the first mask, which means it added cells on the bank that did not have sediment bins calculate for it. 

I discovered that in my process of defining the channel vs the floodplain, some cells in the channel end up having zero sediment thickness, making them nonerodible. This happens because after getting the D50, we set small values of D50 to zero, and then make a mask for all the nonzero D50 values.

```matlab
{
D503 = taus_data3(:,end)/rho_water/g/(s-1)/tau_star_r; %recalculate D503 
D503(D503<0.008)=0; %apply a lower condition for setting D50 to 0 (above it was set to <0.020, which is why we need to recalculate the D503)
mask = D503~=0; %Mask is true for every nonzero D50 value
}
```

Then, we use that mask to add the sediment bins calculated from the nonzero D50 to the entire domain.

```matlab
{
bin_frac_all(mask, :) = bin_frac_valid;
}
```

This is where the zeros get introduced: a second mask is created called `channel_idx` (and inverse mask `floodplain_idx`) is used for defining which cells should be modified to have 20% sand and which cells should be filled in with the floodplain sediment data. 

```matlab
{
channel_idx = u_data3(:,end)>1;
floodplain_idx = ~channel_idx;
}
```

`channel_idx` is used here: 

```matlab
{
bin_frac_all_addsand(channel_idx, sand_bins) = repmat(sand_target, nnz(channel_idx), 1);

% Scale the non-sand fractions proportionally to 0.8
ns = bin_frac_all(channel_idx, nonsand_bins);
ns_sum = sum(ns, 2);
bin_frac_all_addsand(channel_idx, nonsand_bins) = (1-sum(sand_target)) * ns ./ ns_sum;
}
```

And `floodplain_idx` is used here: 

```matlab
{
bin_frac_all_addsand(floodplain_idx, :) = repmat(bin_frac_floodplain/sum(bin_frac_floodplain), sum(floodplain_idx), 1); %repmat tiles the fixed distribution to fill all floodplain cells and normalize to get the fraction to add to 1
}
```

Having zeros in the channel sediment fractions causes NaNs to show up in the final sediment fractions as plotted below. I assume that NaN is the same as 0, which is nonerodible.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/82f9e887-90c1-4bc6-a52b-f3ac1df491d6" />

*Figure x. locations of NaN sediment thickness.*





