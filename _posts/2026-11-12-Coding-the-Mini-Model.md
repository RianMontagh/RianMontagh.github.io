# Coding the Mini Model

This week I started coding up the mini model which I have spent the last few weeks conceptualizing. I started by defining the constants in the model to reflect the real-world Nooksack as closely as possible and then put the equations into use. 

## Defining the constants

I went through the constants by the governing equation they are associated with.

1. Continuity

2. We have one constant for continuity, which is $Q_{in}$. I used the overtopping threshold with the expectation that I might have to increase this to get a non-negligible head above the weir crest. 

```matlab
% continuity - Qin = Qover + Qout
% we will use a constant Qin
Qin = 850; % [cms] overflow threshold is about 30k cfs according to Scott Anderson
```
2. Mannings

For Mannings, we need the channel width, which we are assuming to stay constant. I took the average length of the bankfull cross sections from the Twin View levee to the Everson Bridge. For Mannings, I did a less precise inspection of the roughness XYZ file in the Delft3D GUI and found that values in the Everson reach are generally around 0.031. The last constant, bed slope, was the most fiddly. I tried a couple of different methods in ArcGIS Pro, starting with the "Add Surface Information" geoprocessing tool which can calculate the average slope for a polyline on a DEM. However, this method gave unrealistically high values of 0.023 for the low flow channel and 0.016 for the bankfull centerline. My theory is that this tool takes the average slope *magnitude* so that positive and negative slopes both contribute to a higher slope value instead of balancing each other out. Instead, I made elevation profiles in ArcGIS using the Exploratory 3D Analysis elevation profile tool. I made one for the low flow channel centerline and another for the bankfull channel centerline, plotted them, and performed a linear regression. See below:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/9af99258-0600-4fd8-9461-b70c89e8f574" />

*Figure 1. Comparing bed slope values for different channel centerlines*

The slope for the low flow channel is closer to the value that NHC got for the low flow water surface slope in their "Lower Nooksack Geomorphic Assessment Final Report" (2019). The bankfull channel slope is higher than any of the water surface slopes reported by NHC. My plan is to try both of the slopes in the model and see if they made a big difference. I will start off with the bankfull slope since that is more representative of the flow that is relevant to the model. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/d226c11b-b13f-4091-9b57-d34f6b99d711" />

*Figure 2. NHC's analysis of low flow water surface slope for their four reaches*

<img width="700" alt="image" src="https://github.com/user-attachments/assets/d021a572-6318-4b2e-8f67-41bf674f2123" />

*Figure 3. Table of NHC's analysis of low flow water surface slope for their four reaches broken up into upper and lower sections*

```matlab
% Mannings - assume uniform flow
% for channel width, take the average bankfull cross section width from
% Twin View to Everson Bridge
s_km = [CS.s_km]';
TV_dist = 8.22; % [km]
EB_dist = 10.02; % [km]
TV_idx = find(abs(s_km-TV_dist)==min(abs(s_km-TV_dist)));
EB_idx = find(abs(s_km-EB_dist)==min(abs(s_km-EB_dist)));
xs_length = zeros(EB_idx-TV_idx,1);
for i = TV_idx:EB_idx
    dx = CS(i).x_poly(1) - CS(i-1).x_poly(end);
    dy = CS(i).y_poly(1) - CS(i-1).y_poly(end);
    xs_length(i-(TV_idx-1)) = hypot(dx, dy);
end
b = mean(xs_length); % [m] channel width
n = 0.031; % [-] Mannings n roughness, taken from inspection of Mannings n from iterative framework
S_0 = 0.0026; % [-] bed slope assuming normal flow, where friction slope = bed slope
```

3. Bed Shear Stress

Here I define the acceleration due to gravity, density of water, and the specific weight of water.

```matlab
rho_water = 1000; % [kg/m^3] density of water
g = 9.81; % [m/s^2] gravitational acceleration
gamma_water = rho_water * g; % [N/m^3] specific weight of water
```

4. Meyer-Peter-Muller

The density of the sediment is the density for granite/quartz, which is used in the model, and the median grain size is taken from the following figure which I believe is an NHC figure.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/9350e1ae-b047-4e20-8249-61b5cd9ef860" />



