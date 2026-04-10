# New 2024 Sediment Fractions and Updated Synthetic Hydrographs

This week, I made sediment fractions that work with the 2024 NLCD data and 2024 calibrated Mannings n in the channel. Wuming sent me his code to do this, and this week I went through it line-by-line to ensure that I understand the concepts. I also added comments in my own words, translating much of them from Chinese. The final step in this process was writing my own code for adding 20% sand content (5% fine sand, 10% coarse sand, and 5% sand-gravel) to each cell's grain size distribution. For my own reference, I am going to write out a pseudo code here that I can refer back to if I need to review this process. 

## Sediment Fraction Calculation Pseudo Code
```matlab
D50 = %Shields criterion with shear stress from last mannings iteration. set values less than 0.008 m to 0. This is to eliminate overbank cells that got a D50 assigned to them, because these do not represent the shear stress that would results from a bankfull flow. 
mask = %all non-zero D50

D50_valid = D50(mask)*1000 %apply the mask
n_valid = %sum of true values in the mask (non-zero D50)
z16 = %standard normal z-scores for 16th percentile
z84 = %standard normal z-scores for 84th percentile

Dmin = %minimum grain size to include in the GSD (we use the upper size limit of silt)
Dmax = %grain size to include in the GSD (we use the upper size limit of cobble, any larger is boulder)
n_bin = %number of bins, one bin every 2 psi (whole number)
edges = %row vector of the edge values of the bins (mm) 
bin_frac_valid = %row of n_bin zeros %initialize where we will store our fractions for each bin
D_center = %row vector of the geometric center between each bin edge 

for i = 1:n_valid %loop over each cell with non-zero D50
    D50 = D50_valid(i);
    D16 = %apply power law for D16 from Jacob's work to the D50 
    D84 = %apply power law for D84 from Jacob's work to the D50 

    if (D16, D50, or D84 is negative or if (D16 < D50 < D84) does not hold)
        bin_frac_valid(i, :) = zeros(1, n_bin);
        continue
    end

    sigma = (log10(D84) - log10(D16)) / (z84 - z16); %see equation for sigma using two known percentile points - based on plugging in know values to z = (x-mu)/sigma and solving for sigma, mu cancels
    mu = log10(D50) %for a lognormal distribution, the mean mu (center of distribution) is the log of the median 

    for j = 1:n_bin
        lower edge for bin j 
        upper edge for bin j
        bin_frac_valid(i, j) = normcdf(upper, mu, sigma) - normcdf(lower, mu, sigma); %get percentile in each bin - use normal distribution stats bc lognormal distribution is normal when in log space 
    end

    %normalize bin fractions so they sum to 1
end 
```

## Adding 20% Sand Pseudo Code

This is the code I developed independently:

    SET fine_sand_fraction   = 0.05   (5%)
    SET coarse_sand_fraction = 0.10   (10%)
    SET sand_gravel_fraction = 0.05   (5%)
    SET total_sand_to_add    = fine_sand_fraction + coarse_sand_fraction + sand_gravel_fraction  (20%)
    
    SCALE DOWN all existing bin fractions by (1 - total_sand_to_add)
        → this makes room for the sand while keeping all fractions summing to 1
    
    ADD fine_sand_fraction   to bin 1 (fine sand)    for all channel cells
    ADD coarse_sand_fraction to bin 2 (coarse sand)  for all channel cells
    ADD sand_gravel_fraction to bin 3 (sand gravel)  for all channel cells
    
    PRINT the maximum total sand fraction across all channel cells
        → should be close to 0.20 (20%) for all cells

The printed line at the end of this code tells me that `maximum sand fraction sum in the channel = 0.2491`. So there was not much sand pre-existing the addition of the 20% sand. 

## Results from the Sedimemt Fraction Calculations 

The rest of the code involves building the floodplain grain size distributions from a fixed distribution, such that the entire floodplain has the same sediment fractions. The channel and floodplain distributions are then added into bin_frac_all. I then plotted the results. 

<img width="1393" alt="image" src="https://github.com/user-attachments/assets/46817164-8eab-4bfa-ba3c-46808e969720" />

*Figure 1. Spatial variation of the sediment fractions and their percent content at each cell in the overflow reach at Everson.*

This figure helps confirm that the sand content was added to the channel correctly. For fine sand, coarse sand, and sand gravel, there is a uniform value throughout the channel. The fine sand and sand gravel are both around 0.05 in the channel, although there is a bit more sand gravel because before I added 5% sand gravel, there was already some existing content in the channel. 

It is also clear from Figure 1 that the floodplain has a spatially-fixed grain size distribution. 

Finally, we can see some patterns in the gravel fraction spatial distributions. Fine gravel appears more abundantely on the edge of the channel. Coarse gravel is the most common sediment class to find in the middle of the channel. Cobble has less uniform patterns, and seems to dominate at the meander near Everson Bridge. 

**Next Step: Compare with Wuming's sediment fractions and add in non-erodible areas (set sediment fractions to 0) at forested bars and developed areas.**

## Hydrograph Report 

I looked through a 2008 report that Paula sent me about the trouble-shooting of the retired Deming gage and the development of the 100-year design hydrograph at Deming. This was helpful to get another number for the 100-year peak flow to compare my StreamStats number to. The Deming value is about 75,000 cfs for the 100-year peak, which is smaller than the North Cedarville StreamStats value of about 85,000 cfs. I am not sure if we would expect North Cedarville peak flow to be much larger than the Deming peak flow, because even though North Cedarville is at least 5 miles downstream from Deming, there is no extra flow entering the channel other than rainfall. 
    
