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

The density of the sediment is the density for granite/quartz, which I set the same in the Delft3D model. The median grain size is taken from the following figure which an NHC figure from their 2025 "Lower Nooksack River 1D HEC-RAS Sediment Model Draft Report". The nondimensional critical shear is 0.047 as found by MPM. With these values I can calculate the submerged specific gravity, $R$. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/9350e1ae-b047-4e20-8249-61b5cd9ef860" />

5. Exner

The two missing parameters for the Exner equation are the bed porosity (volume of void space per volume of bed) and the along channel length of the control volume. In the Delft3D model, we define the dry bed density, which is the mass of sediment per volume of bed. We can calculate a bed porosity from this value. I defined the $dx$ term based on the distance from the Twin View levee to the Everson Bridge. 

```matlab
rho_drybed = 1800; % [kg/m^3] dry bed density, mass of sediment per bed volume
lambda_p = 1-rho_drybed/rho_s; % [-] porosity, percent of the bed volume that is void space
dx = 1818; % [m] from TV to EB
```

6. Weir Equation

Lastly, we need to define our initial bed elevation and the elevation of our "weir." I decided to make elevations relative to the initial bed elevation, which means the initial bed elevation is our datum. The height of the weir crest is therefore the difference of the elevation of the weir crest and the elevation of the initial bed elevation. I calculated the bed elevation for the lowflow and bankfull channels and decided to start with the bankfull bed elevation as the datum. For the weir elevation, I decided to go with the average elevation along Emerson Road as this is the closest nonerodible high point in the overflow pathway.

```matlab
eta_0_bankfull = 24.5; % [m] average elevation along the bankfull centerline from TV to EB
eta_0_lowflow = 22.9; % [m] average elevation along the lowflow centerline from TV to EB
eta_0 = 0; % [m] initial bed elevation, this is our datum
wc_Emerson = 28.2-eta_0_bankfull; % [m] weir crest elevation
```

## Evaluating the Governing Equations

The final step in this process is to solve the governing equations, especially the Exner ODE. First, I solved for all the upstream variables ($Q_{in}, y_{in}, \tau_{in}, q_{s,in}$) as these are constant. I set up a conditional if statement to ensure that the upstream flow depth is greater than the height of the weir crest so that there is overflow at the initial time step. I quickly found that my first try of $Q_{in}=850 cms$ was not enough flow to overtop the weir crest in my model setup. I increased $Q_{in}$ until overtopping was achieved, which ended up being at a value of 2,500 cms to generate a head of 0.2 m. I am guessing that the mini model needs a much higher $Q_{in}$ because there are no backwater effects to accelerate overtopping in the mini model. I set the condition of overtopping to be a prerequisite for the model to solve the Exner equation because otherwise the model might reflect other processes than the overtopping feedback mechanism.

I then wrote a function called `bed_evolution` for input into a MATLAB ODE solver. The `bed_evolution` function first computes the downstream variables ($Q_{out}, y_{out}, \tau_{out}, q_{s,out}$) based on the current $\eta$ and returns the computed $\frac{\partial \eta}{\partial t}$ and $Q_{over}$ as its output. 

I reviewed my notes from AMATH 581: Scientific Computing to remind myself of the different methods for solving ODEs. I ended up choosing the MATLAB function `ode45` which uses a Runge-Kutta (4,5) method. This method is explicit, so it only needs the value at the previous timestep to compute the next timestep. This is applicable to the Exner ODE because it can be arranged so that you only need the current $\eta$ to solve for the next $\eta$ value. The numbers 4,5 relate to the truncation error &mdash; the 4 means the global error is fourth order, while the local error is fifth order. That is, the local error at each time step is fifth order while the sum of these errors (global error) is fourth order. Higher order error is better because, for example, fifth order error means the truncated term is on the order of $\Delta t^5$. 

```matlab
[t,eta] = ode45(@(t, eta) bed_evolution(t, eta, params), [0 tf], eta_0);
```

## Initial Results

The initial results for the model setup described above are shown below. Specifically, I plot $\eta,\frac{d\eta}{dt},Q_{over}$. I modeled for a very long time to see the range of behavior of the variable. There is a slight nonlinearity but overall the trends in the following plots look close to linear.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/9f93a820-efe8-4f2b-85e1-96f82819891b" />

<img width="700" alt="image" src="https://github.com/user-attachments/assets/5e0fb469-0883-453e-af3e-4e6a21a51446" />

<img width="700" alt="image" src="https://github.com/user-attachments/assets/157f532a-b0b2-4cae-866d-4fe4d01db228" />

Zooming in to the event timescale, the trends are very linear. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/00132d0d-cbe8-479f-becf-2cae32301502" />

<img width="700" alt="image" src="https://github.com/user-attachments/assets/5e802e2c-907b-447d-87f2-a64e011cc9fb" />

<img width="700" alt="image" src="https://github.com/user-attachments/assets/0d095d9d-4019-4ee4-8cbb-e000a3df6163" />

There are a few things to note in these results. First, the magnitudes of all the values are very small. I realized this was because I had seen other weir equations where L is defined as the crest length parallel to flow (road width) instead of the weir length perpendicular to flow (overtopped length). After changing `Lw_Emerson` from 6 m to 150 m, I started getting somewhere more meaningful. I started getting errors because I hadn't added fail safes for when $Q_{over}>Q_{in}$ or for when $Q_{out}$ is so small that $\tau_{out}^\ast<\tau_c^\ast$. Both of these conditions are met when $Q_{out}$ gets very small, so I added a `Qout_min` variable set to 1% of $Q_{in}$. This updated model returned the following results. Note that for the same time range the model now stops itself at ~50 days. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b0954e6e-f420-48e9-ace9-ad983fa13eb3" />

<img width="700" alt="image" src="https://github.com/user-attachments/assets/63f010f4-1abe-4d5a-8864-f44613c6e041" />

<img width="700" alt="image" src="https://github.com/user-attachments/assets/60db6af4-3086-49df-8991-e213c4f67147" />

For event scales:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/07f66ea1-6823-4af1-b000-9498d961cd0d" />

<img width="700" alt="image" src="https://github.com/user-attachments/assets/0c7c630b-4383-4bdf-ad47-2474bf6ac8c3" />

<img width="700" alt="image" src="https://github.com/user-attachments/assets/658f5720-68b9-423a-b991-faf9713eb425" />

Next, I want to write out my equations with all the constants and revisit my analyses with the constants fully written out instead of represented as 'C's.' My preliminary interpretation of these plots is that they look like what a runaway feedback loop because of the exponential increase. I am realizing now that I had not done any pre-coding analysis of how variables should vary in time, so I don't have anything to compare these above results with. I do, however, have a prediction that $\frac{d\eta}{dt}$ should vary approximately linearly with $Q_{over}$ and like $H^\frac{3}{2}$ at small $Q_{over}$. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b1981aef-9a83-4441-a0b0-1ddff140dc6a" />

$\frac{d\eta}{dt}$ does look linear with $Q_{over}$! This seems to hold even when $Q_{over}$ is over 50% the magnitude of $Q_{in}$. The black line in this plot above is simply connecting the model data points. Next, I fit a linear trend line to the data, excluding the last point where $Q_{over}$ gets very close to $Q_{in}$, to get a more quantitative idea of the linearity and goodness of fit.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/03b0827e-fbb1-4d05-a4a9-65dbf088e0ab" />

The $R^2$ value is very high here. However, visually I do see a slight concave down coherent curve in the modeled data. I think that one way I could check my model further is to plot the line as predicted by the full ODE equation with constants included. I feel confident from this plot that my prediction that $d\eta /dt$ vs. $Q_{over}$ can be approximated with a linear relationship at the smaller range of $Q_{over}$ values, but definitely is not exactly linear. The normalized RMSE (NRMSE) value is small, which adds to my confidence that the linear approximation captures the first order behavior of the relationship. I normalized the RMSE by the range of values in the modeled output (excluding the last value). 

Next, let's see how $\frac{d\eta}{dt}$ varies with weir head H.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/abb1ac07-ba7d-4b03-9c2a-af38b036111e" />

First, I plotted the data with a fitted power law relationship of the form $ax^b$. The best fit in this case was an exponential function as shown above, with power of $b=1.29$. The fit looked okay, and definitely diverged at higher $H$. The $b$ value is close to my predicted value of $\frac{3}{2}$, which felt promising. Next, I held the exponent fixed at $\frac{3}{2}$ per my prediction and got the following result.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/cc988ab9-34fc-420c-9fdf-1188c10d8f8f" />

To me, this fit is not bad! However, I want to go back to my CEWA 565: Data Analysis in Water Science notes to refresh myself on the details of these curve fitting functions and how to interpret them.

## Closer look at $R^2$ and RMSE





















