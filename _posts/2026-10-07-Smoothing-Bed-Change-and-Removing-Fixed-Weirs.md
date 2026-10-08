# Smoothing Bed Change and Removing Fixed Weirs

## Smoothing Bed Change

This week I wanted to change my streamwise bed change analysis from a cross sectional average to an areal average over the area between consecutive cross sections. I hoped that this will improve the readability and smoothness of my bed change plots, because by averaging on a cross section, I am essentially picking just one point every 30 m to plot. This can make the data noisy and hard to interpret. 

I started looking back at my cross sections so I could understand how this averaging would work. For reference, here are two snapshots of the bankfull cross sections. The cross sections in the confined reaches are a lot more simple than the ones in the braided reach. 

<img width="1462" alt="image" src="https://github.com/user-attachments/assets/8d63a9d5-7de4-4916-9d28-acb6b48dbb75" />

*Figure 1. Bankfull cross sections in the braided reach*

<img width="1449" alt="image" src="https://github.com/user-attachments/assets/f5daf7ae-d03b-4e84-949c-2bfc8d2eb1b9" />

*Figure 2. Bankfull cross sections in the confined reach*

As a reminder, I removed cross sections that intersected with other cross sections, which ends up generating gaps anywhere cross sections are longer and rotate more due to curves in the provided centerline. I could consider straightening the centerline especially in the braided region to make the gaps smaller. I removed intersecting cross sections because in my head it made sense for all of the discharge collected in a cross section to be collected downstream from the previous cross section. The same goes for bed level and other variables, because I want a streamwise-oriented dataset. However, is this actually true? Maybe it doesn't matter and as long as the midpoint of consecutive cross sections march downstream that is all that matters?

After thinking about this and looking back at my centerline, I decided to edit the centerline so that it follows the center of bankfull flow instead of the center of a low flow channel, which has more bends in it. This solved the problem of intersecting cross sections, at least in the braided reach and the Everson Overflow reach. 

One other note&mdash;the area between my cross sections is not uniform. This shouldn't be a problem because dividing by the area for averaging, but in the larger gaps I might lose more bed change granularity.

Next, I checked what my cross sections look like plotted on the unstructured grid and this is where I realized I might be stuck with the cross sectional averaging&mdash;my cross sections have the same spacing as the cells in the channel! See below.

<img width="424" alt="image" src="https://github.com/user-attachments/assets/3145b677-60b7-4dd2-af0c-a4b892f559fc" />

*Figure 3. Bankfull cross sections on the unstructured grid*

This means that the an areal average and a cross sectional average should be almost the same thing, because they average over about one row of cells. This leads me to wonder why my plots are so up and down if I am essentially sampling every cell in the channel. I redid my plots but also added a plot of the bed elevation alongside the bed change to see is my actual bed is very up and down. 


### Verifying Cross Section Lengths with 2025 Flooding

When I made my bankfull cross sections, I used a constant 600 cms hydrograph, which very likely has different inundation extents than the 2025 flood at bankfull flow. I still think that the constant bankfull discharge is the correct way to define the active channel, but I was curious to see if taking the 2025 bankfull output times line up with my bankfull cross sections. I plotted the bankfull cross sections alongside the depths during the 2025 flood at the output times (bankfull or peak discharge times), and applied a mask that I created using the same parameters that I used to generate the limits of my bankfull cross sections: cells with depth > 0.5 m and velocity > 0.5 m. 

<img width="1474"  alt="image" src="https://github.com/user-attachments/assets/e4ae9a4f-68f6-42b1-8116-ee0e0fab17ff" />

*Figure x. Inundated area of "active flow" during the first bankfull discharge at Everson during 2025 flood*

<img width="1474" alt="image" src="https://github.com/user-attachments/assets/82048b1d-3a43-4e87-b560-5359b5682ae7" />

*Figure x. Inundated area of "active flow" during the first peak discharge at Everson during 2025 flood*

<img width="1474" alt="image" src="https://github.com/user-attachments/assets/50220111-317a-4c25-a20d-26765d91bab8" />

*Figure x. Inundated area of "active flow" during the second peak discharge at Everson during 2025 flood*

The other bankfull output times not shown here look similar to the first output time at bankfull discharge.

Overall, these plots tell me that my cross sections should be capturing flow that is at least half a meter deep and moving at least half a meter per second. One [web source](https://bwi.earth/understanding-surface-water-speed-in-rivers-why-it-matters-and-how-its-measured/)  claims that 0.5 m/s is the upper limit for a calm river. 

One other limitation I thought of is that the channel shifting right or left should have no net change only if the shift remains within the cross section. If the shift goes beyond the cross section length, then the shift would register as a net aggradation. So far, that hasn't seemed like a problem for the Nooksack. I believe that any lateral shifts should be contained within my cross section length.

## Erodible Roads and Levees

After looking at my abstract, I realized that since my hypothesis is related to breaching of alluvial ridges, I might want to start focusing on the nonerodible weirs we have in our model. I started reviewing the Delft3D manual to learn about how the fixed weirs work, because so far I have not had to mess around with them at all yet. 

In the model, a fixed weir is "a fixed non-movable construction generating energy losses due to constriction of the flow. They are commonly used to model sudden changes in depth (roads, summer dikes) and groynes in numerical simulations of rivers." One of the benefits of using a fixed weir instead of modeling these objects as terrain features is that the grid might be too coarse to adequately represent all the geometries of sharp feature like a narrow road. Looking at our grid at Main St. for example, we see how this is the case since one cell is larger than the width of the road.

<img width="999" alt="image" src="https://github.com/user-attachments/assets/170cd124-c8a3-47fd-ac09-4129dc6ac284" />

*Figure x. Comparison of road feature to grid cell size*

So, when I remove the fixed weirs to make the roads act as erodible objects, one implication is that the exact terrain at the road might not be represented as well. I looked at our bed level to see what this looks like, shown below. The triangular cells are large in this area, and are slightly elevated at the road compared to the surrounding fields. 

<img width="1033" height="532" alt="image" src="https://github.com/user-attachments/assets/757a2a9a-fee9-4ac7-8977-1cb91dc8b05f" />

*Figure x. Model-interpolated bed level at Main St*

When I draw a profile line across Main St. using the Delft3D GUI, depending on where I draw the line I get different profiles due to the arrangement of the triangles. 

<img width="939" alt="image" src="https://github.com/user-attachments/assets/0452f4c8-c97e-44e9-a07f-81d85e8df38c" />

*Figure x. Profile through elevated cells in Main St*

<img width="1009" alt="image" src="https://github.com/user-attachments/assets/ccca1d7c-151f-44c6-94c3-b93a337b3fba" />

*Figure x. Profile through non-elevated cells in Main St*

At first, I thought that this would mean that once I remove the fixed weirs, the flow will be able to go through/over the road at Main St very easily through the vertices of the lower-lying cells. However, I reminded myself of the numerics of Delft3D, which say that flow information is passed from cell face to cell face and cannot pass through nodes/vertices.

With this information, I decided to remove the fixed weirs in the overflow path only. This includes the Masey Road, Main St, other downtown Everson roads, and the short levee just upstream of Everson Bridge. I am currently in the process of editing the fixed weir input file so I can run the model.

Question: Is it important for me to update the fixed weirs to make the 2024 topo?

Also, while reading the manual, I came across the dam break section, which piqued my interest because in Delft3D, a "dam break is a structure that models a growing breach after a dam failure or levee breach." Maybe this is something I could pursue, which would force a breach to occur at a location of my choosing? This is more inline with the papers I have read so far that model avulsion, where the bifurcation or levee breach exist at the beginning of the model run.

## Proposal for Nooksack Project Blurb for the Website

Last week we did some work on the UW EFM website and I noticed that the Nooksack project doesn't have a webpage yet so I decided to draft one blurb for our project and one blurb for the . 

River Research Blurb

> We are particularly interested in the dynamics of the rivers from their interface with the ocean to the mountain front. Our areas of study span topics like transport dynamics in river plumes, compound flooding, sediment transport and channel change, hydro-morphodynamic feedback during flooding, and river avulsion.

Nooksack Research Blurb

> The Nooksack River in northwestern Washington is a dynamic river system defined by a post-glacial landscape, sediment loads from the volcano Mount Baker, intensifying atmospheric rivers, and human engineering for flood control. To add to the Nooksack's complexity, in the late Holocene, the river occupied a different flow path than it does currently. The process that allowed the river to switch to its present-day course is called avulsion, which occurs when a river rapidly changes course. In the present-day lower Nooksack, extreme flooding forces water out of the main channel and into surrounding floodplains, and some of this floodwater is routed through the old Nooksack valley, raising questions about the potential for another river avulsion. We are using two-dimensional hydromorphodynamic modeling with Delft3D FM to address two goals: 1) we want to accurately model this complex region and use our model to support engineering solutions for the massive flooding, and 2) we want to understand if an avulsion is possible on the decadal time scale and understand the processes that govern avulsion in the Nooksack Valley. This work aims to improve our understanding of river response to extreme flooding and support the local communities in the Nooksack River floodplain.


