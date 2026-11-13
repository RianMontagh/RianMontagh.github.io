# Improving the Mini Model and Forming a Narrative for AGU

Last week, I generated initial results of my mini model using the default settings for the MATLAB `ode45` ODE solver. I want to play around with increasing the resolution in time to make the solutions smoother and see if I can decrease the error tolerance. 

## Improving Numerics of the ODE Solver

First, I investigated the default parameters of the MATLAB ODE solvers using the command `odeset` with no input parameters. 

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


## Forming a narrative for AGU

