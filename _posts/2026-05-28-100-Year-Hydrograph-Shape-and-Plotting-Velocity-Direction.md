# 100-Year Hydrograph Shape and Plotting Velocity Direction

This week I am proud to have finished up and submitted my application for the NextProf Pathfinder workshop! I will find out in July if I have been accepted or not. I also had time to read through the report that Paula sent me on the process of how the 100-year hydrograph was created. 

## Summary of how the 100-year Hydrograph was Created

The report "Deming Gage Analysis and Development of a 100-Year Design Hydrograph" is where I got my original 100-year peak flow from to synthesize my hydrographs for avulsion sensitivity analysis. This week I decided to read through the procedure thay use to determine the hydrograph shape to see if it could be applicable to our hydrographs. They followed a method from Balocki and Burges (ASCE, 1994) that is based on the idea of a nested and coincident flood, which occurs when, for example, the 100-year 1-,2-,3-..., n-day duration floods all occur during the same flood event. They tested if seven Pacific Northwest watersheds satisfied the nested condition (Nooksack was not one of them), and found that it is not necessarily true. They found that this assumption only held true for the largest few floods on record. If the floods are nested and coincident, then a design hydrograph can be made by extrapolating the cumulative distribution functions for each annual max n-day volume. 

When Whatcom County applied this method to the Nooksack River using the following Steps

First, make sure that the largest few floods are actually nested and coincident. This was done at Ferndale to make use of its stable gage readings.
1. Conduct running averages of the daily flow volumes at Ferndale to get n-day flow volumes (1-,2-,3-,4-,5-, and 10-day averages, or durations)
2. Get the annual maximum volumes at each duration and rank these from high to low volume separately by duration. So you would have one list for each duration.
3. Run a frequency analysis on the volumes to get return intervals.
4. Get the critical volumes for each duration by taking the bankfull flow and multiplying by n
5. Get critical durations by taking all the durations that have annual max volumes that exceed the critical volume (5- and 10-day durations were found to be critical).
6. Rank all the critical duration volumes separately by duration, do LP3 on them and get the 100-year 5-day and 10-day volumes.
7. Select the biggest ~5 floods
8. Check these biggest floods for nesting and coincidence.
    - for nesting, make sure that for the year in which the flood occured, the max n-day duration volumes occured during that flood
    - for coincidence, make sure these maximum duration volumes that are nested in the big flood are all the same return period as calculated in step 3

After the nesting and coincidence assumption was confirmed for two of the largest storm events at Ferndale, the authors were able to build the design hydrograph at Deming. The critical duration was found to be between 5 and 10 days.
1. Create a hydrograph with the 100-year peak flow rate obtained from frequency analysis described last week and a shape similar to observed hydrographs.
2. Run this hydrograph through the FEQ model and get the Ferndale hydrograph.
3. Compute n-day flow volumes from the simulated Ferndale hydrograph.
4. Compare the n-day flow volumes from the simulated Ferndale hydrograph to the 100-year n-day flow volumes for Ferndale from the Balocki and Burges method.
5. Iteratively adjust the Deming hydrograph until the Ferndale simulated and calculated 100-year n-day volumes match.

<img width="1040" alt="image" src="https://github.com/user-attachments/assets/f15579a2-1afa-4c6c-a42f-04baed9b855c" />

*Figure 1. Final 100-Year Design Hydrograph at Deming (red)*


