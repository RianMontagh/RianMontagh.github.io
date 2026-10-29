# Understanding Aggradation during the 2025 Flood

So far, I have created analyses for bed change during the 2025 flood by averaging the bed level along bankfull cross sections and plotting these cross sectionally averaged values from upstream to downstream. I have several versions of these plots &mdash; including raw elevation values at bankfull and peak discharge and differenced values. I want to find ways to make these plots more quantitative and easy to get coherent patterns from. 

## Visualizing the Bed Difference

### Adding Color!

I started by improving the way that I show the bed difference in the Everson Overflow. Two of my ideas were to 1. color in the area under the line to be red or blue for deposition and erosion, respectively and 2. plot the Bed difference using the initial bed as the subtrahend (vocab!). 

First, I tried adding color to my bed differene plots. I also cut off the first 4 km of the model domain to see patterns in the Everson Overflow area more clearly. 

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/07546546-f67e-44f9-ba2b-eddc41b7f84c" />

*Figure 1. Bed differenced between consecutive output times with blue for erosion and red for deposition.*

From this plot, I see that there appears to be net aggradation near the Twin View levee and net erosion closer to the Everson Bridge. I also see a continuing theme of alternating "bumps" of erosion and aggradation. I wonder if this has to do with Wuming's finding that sediment eroded from one bar contributes to the next bar. This plot also shows that the falling limb is important in driving bed change.

The output times are shown in the next plot:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/1eaea22d-c98f-4853-813f-5721d50cc22e" />

*Figure 2. Everson hydrograph with output times numbered to match with the titles of the tile plots in Figure 1.*

I think that the fact that time 2 is pretty far away from bankfull means that I should probably start averaging outputs before and after bankfull. 

### Total or Cumulative Bed Change

Next, I wanted to see if plotting the total bed change might expose more patterns in the bed change. It is hard to tell visually from the previous bed change plot if the bed during one time interval is negated by the next interval. However, from Figure 3, it appears that the locations of aggradation and erosion are generally consistent from bankfull to peak discharge. 

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/4132f224-a066-4b04-88ff-f293decbee33" />

*Figure 3. Total bed change as opposed to bed change during the intervals between output time*

I see similar patterns of alternating positive and negative change, as well as more aggradation near Twin View Levee and erosion near the bridge.

### Visualizing in Plan View

I was also inspired by one of Shelby's figures in her thesis (Fig. 4.8 b) that showed colored markers plotted on top of water depth data in map/plan view. I decided to try this out with my bankfull cross sections by having Codex create polygons extending 15 m upstream and downstream of each bankfull cross section. The total distance between each cross section is 30 m. I did this for the consecutive and the total bed change. I also added markers for distance from the upstream boundary so that we can match areas on the profile plot with the plan view plot. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/16bbc176-ccb3-486e-8a7b-9f29bd20a3c0" />

*Figure 4. Plan View of Consecutive Bed Change*

<img width="700" alt="image" src="https://github.com/user-attachments/assets/68b22c66-50e4-4c98-848a-86b4cc461c0d" />

*Figure 5. Plan View of Cumulative Bed Change*

These plots help give a sense of location and a sense of what I mean when I say cross-section averaged. I like being able to visualize how the width of the river changes from upstream to downstream. It also makes it clear how complicated the patterns of bed change are in the Everson Corridor from Twin View to the bridge. From this plot is it hard to say if flow conveyance is decreasing overall. 

### Zoomed in Plots of Bed Change to Compare with the Plan View Plots

<img width="900" alt="image" src="https://github.com/user-attachments/assets/d3114351-9c5d-484d-859b-76bc71930677" />

*Figure 6. Streamwise consecutive bed change in Everson reach*

<img width="900" alt="image" src="https://github.com/user-attachments/assets/aef6dca5-2d47-4734-a456-f36856025233" />

*Figure 7. Streamwise cumulative bed change in Everson reach*

## Developing Hypotheses for my Mini Model

1. How do 


















