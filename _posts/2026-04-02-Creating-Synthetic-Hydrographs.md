# Creating Synthetic Hydrographs for Avulsion Sensitivity Testing

I am coming back after spring break and am starting the spring quarter off by diving back into my avulsion sensitivity analysis. A few of the things that need to happen before I can do this are:

1. Construct synthetic hydrographs for each of the planned storm return periods and durations
2. Do a preconditioning run and generate a 'restart' file
3. Generate sediment fractions from the 2024 $D_{50}$ values

## Creating Synthetic Hydrographs from the Unit Hydrograph

Creating unit hydrographs was a good chance for me to review concepts from my hydrology class last year. A unit hydrograph is a hydrograph for one inch of direct runoff that occurs uniformly across the delineated basin. Direct runoff is the immediate runoff that occurs during a storm (rather than baseflow). 

I used the SCS method based on the National Engineering Handbook, Section 4 (Hydrology) - Chapter 16 guidelines to create my synthetic hydrographs. This method makes a lot of assumptions, but for our avulsion sensitivity testing, it should still be applicable because we are more interested in the affects of different types of floods rather than the real physics of rainfall and runoff. 

See my code, below:

    %% =========================
    % PLOTTING
    % =========================
    
    dur_labels = {'24 hr','72 hr','168 hr'};
    
    %% Plot 1: Separate panels by return period
    figure;
    
    % --- 10-year ---
    subplot(3,1,1); hold on; box on;
    for i = 1:length(tb)
        plot(t{i}, q10{i}, 'LineWidth', 2);
    end
    title('10-Year Synthetic Hydrographs');
    ylabel('Discharge (cfs)');
    legend(dur_labels, 'Location','northeast');
    
    % --- 50-year ---
    subplot(3,1,2); hold on; box on;
    for i = 1:length(tb)
        plot(t{i}, q50{i}, 'LineWidth', 2);
    end
    title('50-Year Synthetic Hydrographs');
    ylabel('Discharge (cfs)');
    legend(dur_labels, 'Location','northeast');
    
    % --- 100-year ---
    subplot(3,1,3); hold on; box on;
    for i = 1:length(tb)
        plot(t{i}, q100{i}, 'LineWidth', 2);
    end
    title('100-Year Synthetic Hydrographs');
    xlabel('Time (hours)');
    ylabel('Discharge (cfs)');
    legend(dur_labels, 'Location','northeast');
    
    
    %% Plot 2: Compare durations for one return period (example: 100-year)
    figure; hold on; box on;
    
    for i = 1:length(tb)
        plot(t{i}, q100{i}, 'LineWidth', 2);
    end
    
    title('100-Year Hydrographs: Duration Comparison');
    xlabel('Time (hours)');
    ylabel('Discharge (cfs)');
    legend(dur_labels, 'Location','northeast');
    
    
    %% Plot 3: All hydrographs together (optional)
    figure; hold on; box on;
    
    colors = lines(3);
    
    for i = 1:length(tb)
        plot(t{i}, q10{i}, '--', 'Color', colors(i,:), 'LineWidth', 1.5);
        plot(t{i}, q50{i}, '-',  'Color', colors(i,:), 'LineWidth', 1.5);
        plot(t{i}, q100{i}, ':', 'Color', colors(i,:), 'LineWidth', 2);
    end
    
    title('All Synthetic Hydrographs');
    xlabel('Time (hours)');
    ylabel('Discharge (cfs)');
    legend({'10yr-24hr','10yr-72hr','10yr-168hr',...
            '50yr-24hr','50yr-72hr','50yr-168hr',...
            '100yr-24hr','100yr-72hr','100yr-168hr'}, ...
            'Location','eastoutside');

<img width="1704" alt="image" src="https://github.com/user-attachments/assets/6c60e7b4-c062-4577-839a-ec5e5f04df52" />

*Figure 1. Synthetic hydrographs for 10-year, 50-year, and 100-year peak flows with percent increases applied as predicted for 2080 by Evan Paul. The hydrographs also vary in duration (24 hours, 72 hours, or 168 hour).*

### Applying a Winter Base Flow and Ramp Up Time

The hydrographs above only include the direct runoff, but in our model we will want to include the base flow. Basically, the upstream boundary condition should start with some average or base flow that is not influenced by precipitation. I apply a value based on the 2025 flood of 2000 cfs, which is the discharge measured before the North Cedarville gage began increasing due to the atmospheric river. I may want to change this to a winter low flow or average flow value in the future. I also added a period of three days where the model will experience just this 2000 cfs base flow. 

<img width="1704" alt="image" src="https://github.com/user-attachments/assets/ab4dc431-78a4-4ea8-997a-44d5557d75c2" />

## Assumptions Made

1. The AEPs (annual exceedance probability) from StreamStats, which are an instantaneous discharges, can be used compatibly with the percent increases in peak flow from Paul's thesis, which are derived from 1-day, 3-day, and 7-day annual mean peak discharge averages.
2. The flow durations from Paul's thesis can be applied as hydrograph duration, although the flow durations are lengths of averaging, not hydrograph duration.
3. The shape of the hydrograph is consistent with the assumed partitioning of volume from the NEH. That means that 37.5% of the flow volume occurs in the rising limb of the hydrograph, which is the assumption that dictates where the time-to-peak falls.





