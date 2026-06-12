# Small Peak Magnitude Correction and Floodplain Erodibilty

## Reflection on Avulsion Sensitivity Progress Presentation

Last week, I presented my first research update on the avulsion sensitivity analysis runs! The main takaways were that the morphodynamics so far are not showing a positive runaway feedback loop where larger or longer storms send lots more flow to Sumas. There were some small differences in the amount of overflow to Sumas. One change I made was to increase the length of the overflow to Sumas boundary, which was not capturing all of the flow. I thought this would make the overflows between the morpho-on and morpho-off models more pronounced, but the change also increased the amount flowing north in the morpho-off model. 

Some feedback and ideas I got from the presentation:

1. Paula may have other, larger values for the increase in the flood peaks with climate change. These might be more accurate because they could apply to instantaneous peaks rather than the 3-day average.
2. Shelby had an idea about a new overflow metric that looks at the overflow that leaves the channel just at the right bank. However, she also mentioned that some of the overbank flow returns to the main channel instead of heading north. I hadn't seen or noticed that pattern in my velocity quiver plots. She said this could be a healing mechanism.
3. Shelby has another code for calculating cross-section averaged bed level and water surface elevation. One concern I had with our code is that the cross sections are based on the low flow channel rather than the bankfull channel.
4. Shelby gave me the idea of making my multidimensional conceptual figures into actual plots. I could plot one of the overflow metrics in a two dimensional plot or in trivariate space using different colors or sizes of markers to distinguish maagnitude of the avulsion metric.
5. Paula is interested in me looking at wood racking up on the Everson Bridge.
6. Paula is interested in me looking at the profile of bed change and water surface elevation over longer distances to see if there are patterns of aggradation where there is overtopping.
7. Shelby noticed steepening in my profile plots and mentioned Lane's Balance, but I wasn't clear on what she was getting at here.
8. Consider shear stress on the floodplain as another avulsion metric. I love this idea because it can tell us about incision potential.

## Correction to the Climate-Scaled Hydrographs

In the beginning of this week, I made a small change to my climate-scaled synthetic hydrographs. Last week when I was preparing for my presentation and creating the unscaled 100-year hydrographs, I had forgotten that my method involved shifting the hydrograph up by the baseflow and then appending a warm up and a tail of baseflow to the hydrograph. This seemed reasonable at the time because I did not have confidence in my peak values. However, when I created the unscaled hydrographs I realized that the peak values should be consistent with the Whatcom County values and not shifted up. I used this equation to get the hydrograph. 

```matlab
    q10_base_unscaled{i} = q_ratio * (qp_10_unscaled(i) - baseflow) + baseflow;
    q50_base_unscaled{i} = q_ratio * (qp_50_unscaled(i) - baseflow) + baseflow;
    q100_base_unscaled{i} = q_ratio * (qp_100_unscaled(i) - baseflow) + baseflow;
```

In this method, the Qp that the nondimensional SCS hydrograph is redimensionalized by is the "direct runoff" which is the flow caused directly by the storm, i.e. not by baseflow. After multiplying by the maximum direct runoff, the hydrograph is shifted upwards by the baseflow. This week, I applied this method to the climate-scaled hydrographs as well. This is a small change of about 56 cms. 

## Trouble-Shooting the 1.5 Fining Factor Model Run 

Last week, I wanted to show the group results of the using a fining factor on the floodplain. However, my model would crash in the first minute with the error message that I had a negative sediment thickness of -0.00 somewhere in my floodplain, but I could not find it anywhere. This week I had more time to figure out the error. 

Attempts:
1. Find any negative sediment fractions. This showed that all fractions were equal to or greater than zero.
2. I found out that a lot of Matlab functions treat -0 the same as 0 (like `-0==0` would be true and `-0<0` would be false). To differentiate between them, I instead tested if `1/frac`, where frac is the sediment fractions, returned an `-Inf` values.

    $\frac{1}{-0}=-Inf$ while $\frac{1}{0}=Inf$.

   However, I did not find any $-Inf$ values.

3. Next I decided to look through all the .XYZ files to locate the exact coordinates specified in the error message and see what value was in my file.

        ** ERROR  : Negative sediment thickness for fraction  4:           -0.00 in file sediment thickness/sediment_thickness_nonerodible_nobridges_v2_f0.75/sand_gravel_IniSedThick.xyz at nm=683 (x, y =       530777.51     5413392.12)

   All of the thickness values in the files at the location were 0.0000.
4. Next, I plotted exactly where the error was occuring.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/4aaaa7d9-9314-481a-9c0e-bf1f0d1dab7d" />

Interestingly, the error is at a nonerodible cell on the edge of the domain. 
5. I found a cryptic message on an old Delft3D [forum] (https://oss.deltares.nl/web/riverlab-models/forum/-/message_boards/message/5065111#_com_liferay_message_boards_web_portlet_MBPortlet_message_5079141} that said the following:
>It is unfortunate that you encoutner the problem of negative thickness. The problem may be with interpolation of the data. The data provided at a point x-y is interpolated to the cell nodes and it may be that it is negative. That is something to try to solve, indeed. Setting it to a very small value seems a good workaround that I would have also used.

This fix worked! I replaced all zero sediment thickness with 1e^-6 and the models ran without crashing. I think it must be an error of interpolation by the model, perhaps exacerbated by being on the edge of the domain. 

Question: Will replacing our nonerodible areas with very small values have any weird effects on bed change?

## New Plots with Fining Factors 

I ran models with my fining factors with the corrected hydrograph peak value for the 50-year, 3-day floods. The plots below show the overflow values and bed change difference between the different fining factors, with the exception of f = 0.50 and f = 1.50, which is currently being reran with the correct peak. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/8467fbba-2d7a-410f-a2bf-d02f1e435e4c" />

*Figure 2: Overflow to Sumas Discharge for the 50-year, 3-day Flood*

Surprisingly, the difference between the amount of overflow is nearly nothing. Zooming in on the peak overflow, we get Figure 3 below. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/cd35d5ce-3b9b-48e6-bee2-86d103766701" />

*Figure 3: Zoomed to Peak: Overflow to Sumas Discharge for the 50-year, 3-day Flood*

There is minor difference shown here, and we do see a pattern where finer floodplains have more discharge to Sumas. Peak values are:

| f = 0.75 | f = 1.00 | f = 1.25 |
| -------- | -------- | -------- |
| 1124.47 m^3/s |  1124.07 m^3/s  |  1121.78 m^3/s |

Next, I wanted to look at the floodplain erosion and deposition patterns to get a sense of increased incision with finer material. 


















