# Smoothing Bed Change and Recapping Old and Current Model Comparison

## Smoothing Bed Change

This week I started changing my streamwise bed change analysis from a cross sectional average to an areal average over the area between consecutive cross sections. I am hoping that this will improve the readability and smoothness of my bed change plots. The reason for this is that by averaging on a cross section, I am essentially picking just one point every 30 m to plot. This can make the data noisy and hard to interpret. 

I started looking back at my cross sections so I could understand how this averaging would work. For reference, here are two snapshots of the bankfull cross sections. The cross sections in the confined reaches are a lot more simple than the ones in the braided reach. 

<img width="1462" alt="image" src="https://github.com/user-attachments/assets/8d63a9d5-7de4-4916-9d28-acb6b48dbb75" />

*Figure 1. Bankfull cross sections in the braided reach*

<img width="1449" alt="image" src="https://github.com/user-attachments/assets/f5daf7ae-d03b-4e84-949c-2bfc8d2eb1b9" />

*Figure 2. Bankfull cross sections in the confined reach*

As a reminder, I removed cross sections that intersected with other cross sections, which ends up generating gaps anywhere cross sections are longer and rotate more due to curves in the provided centerline. I could consider straightening the centerline especially in the braided region to make the gaps smaller. I removed intersecting cross sections because in my head it made sense for all of the discharge collected in a cross section to be collected downstream from the previous cross section. The same goes for bed level and other variables, because I want a streamwise dataset. However, is this actually true? Maybe it doesn't matter and as long as the midpoint of consecutive cross sections march downstream that is all that matters?

One other note&mdash;the area between my cross sections is not uniform. This shouldn't be a problem because dividing by the area for averaging, but in the larger gaps I might lose more bed change granularity.

### Verifying Cross Section Lengths with 2025 Flooding

When I made my cross sections, I used a constant 600 cms hydrograph, which very likely has different inundation extents than the 2025 flood at bankfull flow. I still think that the constant bankfull discharge is the correct way to define the active channel, but I was curious to see if taking the 2025 bankfull output times line up with my bankfull cross sections.




One other limitation I thought of is that the channel shifting right or left should have no net change only if the shift remains within the cross section. If the shift goes beyond the cross section length, then the shift would register as a net aggradation. 






## Proposal for Nooksack Project Blurb for the Website

Last week we did some work on the UW EFM website and I noticed that the Nooksack project doesn't have a webpage yet so I decided to draft one blurb for our project and one blurb for the . 

River Research Blurb

> We are particularly interested in the dynamics of the rivers from their interface with the ocean to the mountain front. Our areas of study span topics like transport dynamics in river plumes, compound flooding, sediment transport and channel change, hydro-morphodynamic feedback during flooding, and river avulsion.

Nooksack Research Blurb

> The Nooksack River in northwestern Washington is a dynamic river system defined by a post-glacial landscape, sediment loads from the volcano Mount Baker, intensifying atmospheric rivers, and human engineering for flood control. To add to the Nooksack's complexity, in the late Holocene, the river occupied a different flow path than it does currently. The process that allowed the river to switch to its present-day course is called avulsion, which occurs when a river rapidly changes course. In the present-day lower Nooksack, extreme flooding forces water out of the main channel and into surrounding floodplains, and some of this floodwater is routed through the old Nooksack valley, raising questions about the potential for another river avulsion. We are using two-dimensional hydromorphodynamic modeling with Delft3D FM to address two goals: 1) we want to accurately model this complex region and use our model to support engineering solutions for the massive flooding, and 2) we want to understand if an avulsion is possible on the decadal time scale and understand the processes that govern avulsion in the Nooksack Valley. This work aims to improve our understanding of river response to extreme flooding and support the local communities in the Nooksack River floodplain.


