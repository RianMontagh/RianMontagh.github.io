# Why don't large floods drive avulsion of the Nooksack?

## Updates from last week

Two of my models from the blog last week, "the MOR Mystery" finished running. The first one was `Old SED file + old sed frac + SedThr 0.1 + ThetSD 0 + nohiding`, which I ran to check if the hiding function that was turned on in the new model was in effect or if the Wilcock and Crowe hiding and exposure formula was working as expected. Turns out that we were right and turning on a separate hiding and exposure formula does not do anything when using Wilcock and Crowe. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/df6153b5-a1e0-46db-bd5a-836e556856a9" />

*Figure 1. Comparison between overflow to Sumas for model with and without no hiding.*

The second model that finished running was `Old SED file + old sed frac + SedThr 0.1 + ThetSD 0 + bsBagnold`, which switches the bed slope formulation from the Koch and Flokstra (1980) formulation to the Bagnold (1966) formulation, which is what the old model uses. The Bagnold formulation is the default formula. 

<img width="1543" alt="image" src="https://github.com/user-attachments/assets/7b4f072f-723c-460f-83fd-71ddb993f09c" />

*Figure 2. REM: Effect of changing the bed slope formulation to the one used by the old model.*

<img width="1546" alt="image" src="https://github.com/user-attachments/assets/f09ced97-4df6-4914-8680-945e9dccbe73" />

*Figure 3. Bed change: Effect of changing the bed slope formulation to the one used by the old model.*

Is the bed slope formula the setting responsible for the difference in distinct braided channels forming? I had been curious what was causing the 

<img width="1381" alt="image" src="https://github.com/user-attachments/assets/0fc5372b-847f-43ca-b00f-637c1e6ccf4a" />

*Figure 4. Overflow to Sumas with the bed slope = Bagnold.*

<img width="1593" height="913" alt="image" src="https://github.com/user-attachments/assets/097556dc-6e70-4fbb-9949-2c1d2edea5ef" />

*Figure 5. Zoomed-in overflow to Sumas with the bed slope = Bagnold.*





## New direction! Why doesn't higher discharge induce an avulsion?

Now that I have a better idea of the settings that control sediment transport and bed change, I wanted to apply it to investigate my earlier finding that discharge alone does not increase the normalized overflow to Sumas in a meaningful way. 

First, I need to rerun simulations with the 10 m floodplain thickness that I decided should be my "new normal." I already have a run with this setup for the 100-year, 3-day long flood. 

One thing we know is that from beginning to end, the alluvial ridges and floodplain do not change enough to route more flow out of the main channel. This might be a function of the erodibility in the model/reality, or, more likely, it is a function of the amount of discharge able to overtop the alluvial ridges and levees. This leads me to my new questions 

- Why isn't Shelby's feedback mechanism working to push more overflow to Sumas during these huge floods?
- Is there increased aggradation/bed level and/or decreased conveyance in the main channel as flood magnitude increases?
- Does conveyance stay low after the flood dissipates?

<img width="1369" alt="image" src="https://github.com/user-attachments/assets/35f44858-69bf-4296-8d2d-9d56637452d1" />

*Figure 2. REM from beginning to end for the current model.*

### Going back to Streamwise Analyses

I reread chapter 4 of Shelby's thesis to get a deeper understanding of the previous findings that our original hypothesis is based on. I want to try and recreate the analyses to see if we can see a decrease in conveyance that corresponds with an increase in bed level. 

I previously plotted flow through the main channel during the 2025 flood and got the following plot. 

Compare this with Shelby's plots of main channel Q shown below. Plot c.2 is the plot at the time of the first flood peak. However, the main channel Q is not elevated at the upstream boundary as it is in my plot. 

<img width="762" alt="image" src="https://github.com/user-attachments/assets/1f0b55a1-8d1d-48df-a3ab-3a5efb615145" />






---

## Paper - "Crevasse Splays Versus Avulsions: A Recipe for Land Building With Levee Breaches" (Neinhuis, 2018)
### Abstract
**Motivation/background:** levee breaches form avulsions and crevasse splays, and crevasse splays are not well understood. The sedimentation from crevasse splays is important for coastal restoration for sea level rise  
**Question:** What is the influence of vegetation and soil consolidation on the evolution of a natural levee breach?  
**Methods:** Delft3D  
**Results:**  
1. crevasse splays heal due to sedimentation reducing water surface slope
2. erodible and unvegetated floodplains —> more avulsion
3. less erodible and more vegetated floodplains —> small, short-lived splays
4. splays that create the most ‘new land’ have a balance between water and sediment discharge, vegetation root strength, and soil consolidation
### Intro
- some rivers rarely breach alluvial ridges while others do so frequently
- paper motivated to understand the conditions when crevasse splays for so that we can engineer them
- previous channel bifurcation stability models do not take into account floodplain properties
    - study focuses on vegetation and soil consolidation
- hypothesize that  crevasse splay is between a breach healing and an avulsion
### Methods
#### Model
- Delft3D FLOW
- MATLAB routines that feed back into Delft3D for vegetation and consolidation
- initial breach in the levee varied between 1-3 m
- model domain starts at the crest of the levee with the breach and ends in the floodplain
    - main channel is not part of the domain to simplify
- boundary conditions are water levels at the levee boundary and floodplain boundary
- water level in the crevasse is allowed to vary due to drawdown - for example, if a lot of water is flowing through the breach, water level lowers
- sediment allowed onto the floodplain is clay and sand
- multi-year simulations with only one main channel discharge considered
#### MATLAB
- soil consolidation - lowers floodplain in response to deposition
- vegetation - adjusts the critical shear stress for erosion and roughness depending on local water depth
    - in delft you can change the roughness to match vegetation, but not the critical shear
    - critical depth of 1 m is the depth where plants establish vs. die
### Results
#### Breach Healing vs Avulsion
- balance between net deposition vs erosion
    - net deposition on the floodplain causes the water surface slope at the breach to decrease —> healing
    - took 5 years for healing in one example run
    - net erosion (model with more erodible floodplain)
- Deposition/Erosion Ration
    - low D/E —> avulsion
    - high D/E —> breaches heal quickly
    - medium D/E —> crevasse splay formation
- non of the models maintained crevasse splay indefinitely — they all eventually healed
#### Soil Consolidation and Vegetation
- more subsidence —> longer-lived crevasse splays but not more new land
- more veg —> shorter-lived crevasse splays
- medium veg —> more new land
#### Compare with Mississippi
- study results had higher aggradation rates that Mississippi splays likely because the study used constant flood stage, which in reality only happens a few times a year

### Discussion
- Interesting - before leveeing of the Mississippi, up to 17% of flood water went into breaches
    - breaches help reduce flooding?

