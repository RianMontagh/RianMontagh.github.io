# Improving the Mini Model and Forming a Narrative for AGU

Last week, I generated initial results of my mini model using the default settings for the MATLAB `ode45` ODE solver. I want to play around with increasing the resolution in time to make the solutions smoother and see if I can decrease the error tolerance. 

## Improving Numerics of the ODE Solver

I noticed in my plots of $\frac{d\eta}{dt}$ last week that the last point in time has a different pattern than the rest of the curve. I wanted to investigate if that behavior was real or a numerical problem. 

First, I investigated the default parameters of the MATLAB ODE solvers using the command `odeset` with no input parameters. The parameters that control the error are RelTol (relative tolerance), AbsTol (absolute tolerance), and NormControl (do/don't control the error relative to the norm of the solution rather than the absolute value).  In AMATH 581, we customized the absolute and relative tolerances rather than using the default values, which are listed below. 

```
>> odeset
          AbsTol: [ positive scalar or vector {1e-6} ]
          RelTol: [ positive scalar {1e-3} ]
     NormControl: [ on | {off} ]
     NonNegative: [ vector of integers ]
       OutputFcn: [ function_handle ]
       OutputSel: [ vector of integers ]
          Refine: [ positive integer ]
           Stats: [ on | {off} ]
     InitialStep: [ positive scalar ]
         MaxStep: [ positive scalar ]
         MinStep: [ positive scalar ]
             BDF: [ on | {off} ]
        MaxOrder: [ 1 | 2 | 3 | 4 | {5} ]
        Jacobian: [ matrix | function_handle ]
        JPattern: [ sparse matrix ]
      Vectorized: [ on | {off} ]
            Mass: [ matrix | function_handle ]
MStateDependence: [ none | {weak} | strong ]
       MvPattern: [ sparse matrix ]
    MassSingular: [ yes | no | {maybe} ]
    InitialSlope: [ vector ]
          Events: [ function_handle ]
```

The relative tolerance has a default value of $10^{-3}$ and the absolute tolerance has a default value of $10^{-6}$. These control the accuracy you want the solver to have at each timestep. The relative error tells you the allowable relative tolerance around the solution at a given point. For example, an relative tolerance of $10^{-3}$ means that if the true value is 100, then the modeled value has to be within $100*0.001=0.1$ of 100. The absolute tolerance tells you the an absolute amount the modeled value is allowed to be away from the true value. If the absolute error is $10^{-6}$ then the modeled value would have to be within $10^{-6}$ of 100. The relative error is more important when the true values get very small and the absolute tolerance's magnitude is large relative to the true values. 

We also know that the Runge-Kutta 4,5 scheme has local truncation error of $\mathcal{O}(n) \Delta t^6)$ and global error (sum of the local truncation error) of $\mathcal{O}(n) \Delta t^5)$. This confused me at first because in my notes I have the `ode45` method written down as the 4,5 indicating a local error of $\mathcal{O}(n) \Delta t^4)$ and global error of $\mathcal{O}(n) \Delta t^5)$. However, it seems like the 4,5 actually refer to the two schemes that the method uses, and the final solution is based on the 5th order solution which has a local error of 6th order and global error of 5th order. 

I started by testing the following three settings:

| Relative Tolerance | Absolute Tolerance |
| ------------------ | -------------------|
| $10^{-3}$ (default) | $10^{-6}$  (default) | 
| $10^{-6}$ | $10^{-8}$ |
| $10^{-9}$ | $10^{-11}$ |

After plotting and comparing the results from these three settings, I concluded that the smoothness improved a lot but the overall solution with the default tolerances was not far off. The kink at the end of the solution for $\frac{d\eta}{dt}$ remained. I then realized that my condition to stop the ODE solver was allowing the model to reach a point where the $\frac{d\eta}{dt}$ suddenly switches to a constant rate once there is zero downstream sediment transport. I think that this is an interesting condition to consider for avulsion, but I wanted to see what changing the stop condition would do to the result. Originally, I told the ODE solver to stop once the $Q_{out}$ reached a value of $0.1Q_{in}$. I updated this condition to be when the downstream shear stress is equal to the critical shear stress. This ends up being a more conservative condition than the $Q_{out}$ condition because the ODE solver has to stop earlier to meat the shear stress condition. The updated plot with the different tolerances is shown below. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/4ce1303d-51b2-4d22-abcf-f9709b3ffb72" />

From now on I decided to stick with the most stringent tolerances because it still runs instantaneously. To my knowledge there is no downside to decreasing the tolerances other than run time as long as the tolerance is not close to the rounding error. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/996a0d24-a936-41bb-9d8b-d78f84ba470e" />

## Defining a Threshold for the Onset of Feedback

Looking at the exponential plots of overflow discharge and bed elevation, there is a period of slow change before the onset of rapid change. It would be interesting and useful to define a threshold of when we think the feedback is ramping up in an impactful way. One variable I haven't looked at yet is the sediment flux, which might have a behavior that indicates a change in the regime of slow vs. fast change.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/f75ada15-bb5d-4029-9c19-28b5ab4c7504" />

 







## Forming a narrative for AGU

