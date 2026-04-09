# New 2024 Sediment Fractions and Updated Synthetic Hydrographs

This week, I made sediment fractions that work with the 2024 NLCD data and 2024 calibrated Mannings n in the channel. Wuming sent me his code to do this, and this week I went through it line-by-line to ensure that I understand the concepts. I also added comments in my own words, translating much of them from Chinese. The final step in this process was writing my own code for adding 20% sand content (5% fine sand, 10% coarse sand, and 5% sand-gravel) to each cell's grain size distribution. For my own reference, I am going to write out a pseudo code here that I can refer back to if I need to review this process. 

## Sediment Fraction Calculation Pseudo Code

    D50 = Shields criterion with shear stress from last mannings iteration. set values less than 0.008 m to 0. This is to eliminate overbank cells that got a D50 assigned to them, because these do not represent the shear stress that would results from a bankfull flow. 
    mask = all non-zero D50

    D50_valid = D50(mask)*1000 %apply the mask
    n_valid = sum of true values in the mask (non-zero D50)
    z16 = standard normal z-scores for 16th percentile
    z84 = standard normal z-scores for 84th percentile

    Dmin = minimum grain size to include in the GSD (we use the upper size limit of silt)
    Dmax = grain size to include in the GSD (we use the upper size limit of cobble, any larger is boulder)
    n_bin = number of bins, one bin every 2 psi (whole number)
    edges = row vector of the edge values of the bins (mm) 
    bin_frac_valid = row of n_bin zeros %initialize where we will store our fractions for each bin
    D_center = row vector of the geometric center between each bin edge 

    for i = 1:n_valid %loop over each cell with non-zero D50
        D50 = D50_valid(i);
        D16 = apply power law for D16 from Jacob's work to the D50 
        D84 = apply power law for D84 from Jacob's work to the D50 

        if (D16, D50, or D84 is negative or if (D16 < D50 < D84) does not hold)
            bin_frac_valid(i, :) = zeros(1, n_bin);
            continue
        end

        sigma = (log10(D84) - log10(D16)) / (z84 - z16); %the spread of the distribution over the spread in standard normal distribution - see equation for sigma using two known percentile points 
        mu = log10(D50) %for a lognormal distribution, the mean mu (center of distribution) is the log of the median 

    
