# Why don't large floods drive avulsion of the Nooksack?

## Updates from last week`

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
- 

<img width="1369" alt="image" src="https://github.com/user-attachments/assets/35f44858-69bf-4296-8d2d-9d56637452d1" />

*Figure 2. REM from beginning to end for the current model.*


## To-do - read paper and Shelby's feedback hypothesis section again

