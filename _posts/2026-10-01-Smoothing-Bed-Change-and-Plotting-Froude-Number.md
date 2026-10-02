# Verifying Conveyance Plots, Plotting Streamwise Froude Number, and Fixing Nonerodible Area

## Verifying Conveyance Plots

Now that I have plotted Q, V, and A through my bankfull cross sections, I wanted to do some more work to verify they are working as I am expecting them to. I want to gain more confidence in them before I start drawing conclusions from them. The first thing I did was see if Q (output variable cross_section_discharge) equals $V*A$, where V is the cross_section_velocity and A is the cross_section_area. After meeting with Alex and Wuming last week, I realized that the model could be outputting the average velocity including parts of the cross section that have zero velocity. For example, if a cross section spans a bar or a braided section, the average velocity in the cross section might be close to zero due to averaging zeros in with the actually velocity. However, the plot below verifies that cross_section_velocity is the same as $Q/A$ which is the velocity through the wetted areas only, and does not include dry areas like bars. 

<img width="1994" alt="image" src="https://github.com/user-attachments/assets/3602059f-94af-4681-9ecf-eb0437e9c4b7" />

*Figure 1. Showing that Q = V*A*

Next, I quickly verified that my output times made sense. For the history files, I verified that the output times were as close as possible to the Q = 600 m^3/s and the two peaks at Everson. This turned out to be correct and the red dots that are offset from exactly Q = 600 m^3/s is because the output times don't line up exactly with the time that Everson experiences 600 m^3/s.  

<img width="1740" alt="image" src="https://github.com/user-attachments/assets/ad4fbfec-e5fd-41c0-9b95-d6059a86f715" />

*Figure 2. Visualization of the data points spanning the Q = 600 m^3/s line.

Next, I need to verify the extents of flooding at bankfull make sense for my bankfull cross sections. 

## Streamwise Froude Number

I created a version of my along-channel profiles that plots the Froude number at each of my bankfull cross sections. I was hoping to see when and where the river transitions between different regimes, if at all, to understand if certain features act as control points on the river. 

To remind myself of the theory behind the Froude number, I reviewed my notes from Open Channel Flow.

The Froude number is $Fr = \frac{V}{\sqrt{gD}}$ which is a comparison of the flow velocity to the wave celerity or wave speed. When Fr > 1 (supercritical), the river flows faster than waves can propagate. When Fr < 1 (subcritical) waves are able to move upstream. Control points are features like constrictions, steps, bumps, etc in the channel that force flow through Fr = 1 (critical) so that a transition occurs between super and subcritical. 

In the equation, $D$ is the hydraulic depth, where $D=\frac{A}{B}$, where A is the cross sectional area and B is the top width. For purposes of my plots, I have cross sectional area as a model output, but I do not have the top width. However, since the output times I am currently using are all at bankfull discharge or above, I thought it was a good approximation to use the entire bankfull cross section length as the top width $B$. See below for my plot. 

<img width="1614" alt="image" src="https://github.com/user-attachments/assets/42615347-6dc3-426d-ae64-5f297d824bae" />

*Figure 3. Streamwise Froude Number*

Interestingly, this plot shows that Fr stays below 1 in all space and all six output times. I interpret this to mean that there are no control points that are causing a transition in flow regime that my analysis was able to detect. Since the flow is subcritical, more energy is stored in the denominator of the Froude number, which is governed by the hydraulic depth $D = \frac{A}{B}$. The highest Fr occurs about 1 km downstream from North Cedarville, and there are other consistently high values at the 10.5 km and 12 km distances. 

I reviewed the mechanisms that could cause a regime to transition from sub to supercritical:

1. large increase in velocity
2. force flow to get shallow
3. Constrict flow

These sound like processes that would be most likely to happen near a bridge or the onset of leveeing (constriction). However, at most of the output times the location of the Twin View levee is lower than other locations. Just upstream and downstream of Everson Bridge the Froude number is more elevated. 

## Debugging Sediment Fraction Code

In this section I wanted to document a bug I found in the code for creating the sediment fractions. After talking to Wuming, we don't expect it to affect our model results but decided it is still important to fix and test. The bug has to do with using two slightly different masks to define where the channel is; one mask we use to calculate sediment bins from D_50, and the other mask we use add sand and the floodplain. This causes some cells to have a zero sediment thickness (nonerodible) within the channel, because the second mask is slightly bigger than the first mask, which means it added cells on the bank that did not have sediment bins calculate for it. 

I discovered that in my process of defining the channel vs the floodplain, some cells in the channel end up having zero sediment thickness, making them nonerodible. This happens because after getting the D50, we set small values of D50 to zero, and then make a mask for all the nonzero D50 values.

```matlab
D503 = taus_data3(:,end)/rho_water/g/(s-1)/tau_star_r; %recalculate D503 
D503(D503<0.008)=0; %apply a lower condition for setting D50 to 0 (above it was set to <0.020, which is why we need to recalculate the D503)
mask = D503~=0; %Mask is true for every nonzero D50 value
```

Then, we use that mask to add the sediment bins calculated from the nonzero D50 to the entire domain.

```matlab
bin_frac_all(mask, :) = bin_frac_valid;
```

This is where the zeros get introduced: a second mask is created called `channel_idx` (and inverse mask `floodplain_idx`) is used for defining which cells should be modified to have 20% sand and which cells should be filled in with the floodplain sediment data. 

```matlab
channel_idx = u_data3(:,end)>1;
floodplain_idx = ~channel_idx;
```

`channel_idx` is used here: 

```matlab
bin_frac_all_addsand(channel_idx, sand_bins) = repmat(sand_target, nnz(channel_idx), 1);

% Scale the non-sand fractions proportionally to 0.8
ns = bin_frac_all(channel_idx, nonsand_bins);
ns_sum = sum(ns, 2);
bin_frac_all_addsand(channel_idx, nonsand_bins) = (1-sum(sand_target)) * ns ./ ns_sum;
```

And `floodplain_idx` is used here: 

```matlab
bin_frac_all_addsand(floodplain_idx, :) = repmat(bin_frac_floodplain/sum(bin_frac_floodplain), sum(floodplain_idx), 1); %repmat tiles the fixed distribution to fill all floodplain cells and normalize to get the fraction to add to 1
```

Having zeros in the channel sediment fractions causes NaNs to show up in the final sediment fractions as plotted below. I assume that NaN is the same as 0, which is nonerodible.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/82f9e887-90c1-4bc6-a52b-f3ac1df491d6" />

*Figure x. locations of NaN sediment thickness.*

### The Fix

The fix is to simply keep the channel mask consistent between adding in the channel D50, adding sand, and adding the floodplain. I decided to go with a combo of the `mask` that defines everywhere the D50 is non-zero and a velocity criteria. Adding `& mask` ensures that none of the channel_idx have zero sediment fractions. 

```matlab
channel_idx = u_data3(:,end)>1 & mask;
```

I replotted the NaN values in the new sediment fraction and none showed up on the plot. 

## Reviewing AGU Abstract

Let's take a look back at my abstract to see what I should focus on in the next couple of months. 

> River avulsion is the natural process by which a river suddenly changes course. This can occur locally at the meander scale or regionally at the basin scale; the latter typically results in a substantial reorganization of the downstream river system. There is evidence that within the late Holocene, the Nooksack River in northwest Washington, USA experienced a regional avulsion from its previous northward course toward Canada into its present westward course. During large present-day floods, floodwaters reoccupy the topographic low of the hypothesized pre-avulsion path. This occurred during the floods of 2021 and 2025, when a portion of the river’s flow rerouted through this low-lying corridor, flooding communities in Washington and British Columbia and causing extensive economic damage. Past work on the Nooksack River suggests that during large floods, sediment transport capacity and flow conveyance in the main channel decrease, creating a positive feedback loop that forces more water overbank in the direction of the hypothesized historical river path. These findings suggest that the Nooksack River may be trending toward avulsion, especially as climate change intensifies atmospheric rivers in the region.
>
> Here, we investigate event-scale avulsion triggering on the Nooksack River using a depth-averaged, two-dimensional morphodynamic Delft3D-FM model forced with future climate-projected flood hydrographs. Specifically, we assess whether extreme floods can initiate an avulsion and identify mechanisms controlling avulsion initiation. We hypothesize that the ability of the rerouted flow to incise into the river’s alluvial ridges exerts a strong control on avulsion initiation. To test this, we vary morphodynamic parameters such as floodplain roughness, sediment coarseness, flood peak magnitude, flood duration, and number of flood events. Preliminary results indicate that larger floods and a more erodible floodplain increase flow in the potential avulsion pathway. This study improves understanding of event-scale avulsion triggering, which is understudied relative to longer-term avulsion setup, and will inform floodplain management strategies aimed at reducing avulsion risk in the Nooksack River.

I think my abstract set me up to explore a wide range of factors and mechanisms. I like the track I am on now, which is more focused on in-channel processes, we decided that the floodplain erosion and deposition patterns are not making a big difference, and that bigger floods are also not making as big of a difference as we thought in overflow to Sumas discharge. I think that my hypothesis that the alluvial ridge needs to be breached fits in with the observation that changes on the floodplain aren't changing the threshold for overflow. There needs to be some process that makes it easier for water to leave the main channel, be that a crevasse in the alluvial ridge (or hole in a levee) or the bottom of the channel aggrading in elevation such that water overtops the banks. Also, in my mind, more overtopping --> more erosion and potential for breaching of the alluvial ridge. However, we have already tested that theory with the different magnitude floods. Maybe I really need to run like 10 to 2o of them?





