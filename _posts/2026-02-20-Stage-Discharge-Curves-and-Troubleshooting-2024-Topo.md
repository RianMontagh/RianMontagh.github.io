## Stage-Discharge Curves and Troubleshooting 2024 Topo

This week, I wrote code to plot the USGS rating curves alongside the water surface elevation and discharge data from the model to see how well they compare. I also started thinking about the next steps with roughness, which include processing the 2024 NLCD (land cover data) and iteratively running the model to get a converging $\tau$, $D_{50}$, and Mannings $n$. 

### Stage-Discharge Curves

I wrote a new function to scrape the USGS rating curve data and a second function to plot it alongside stage and discharge results from the model. There are start date and end date inputs that are currently unused in the function, which I was hoping to use for specifying a range for accessing USGS field measurements of stage and discharge. However, it seems that the only way to access the field measurements are through the USGS APIs (Application Programming Interface) that would allow my machine to interface with the USGS. The easier option would be to download the measurement data to an Excel file or similar and read the data from that &mdash; however, I am really interesting in learning how to use an API, so am excited to figure that out next week if I have time. 

Question: Will the field measurements from the USGS tell us different validation information that the regression rating curve?

```matlab
function [Qh] = import_USGS_ratingcurve(gageno, startdate, enddate, offset)                     
    %USGS gage height data URL and column names for data link
    url = 'https://waterdata.usgs.gov/nwisweb/get_ratings?file_type=exsa&site_no='+string(gageno);
        
    %Meaning of column headers
        % 'INDEP',... %gage heigh (ft)
        % 'SHIFT',...%not sure what this means yet, but has to do with the
        % shift in rating curve as the channel changes
        % 'DEP',...%Discharge (cfs)
        % 'STOR',...%not sure what this means 
    
    %Import site data
    Qh = readtable(url, 'FileType', 'text', 'Delimiter', '\t', 'CommentStyle', '#', 'VariableNamingRule', 'preserve');

    %rename columns with TS_ID to a consistent name between gages
    Qh = renamevars(Qh, [1, 3], {'gage_height', 'discharge'});

    %Account for the offset 
    Qh.gage_height = Qh.gage_height + offset;

    %convert to meters and m^3/s
    Qh.gage_height = Qh.gage_height * 0.3048;
    Qh.discharge = Qh.discharge * 0.02832;

end
```

I plotted the model results as circles with a color bar filling them in with the color changing based on the time and date of the result. The USGS rating curve is plotted as a line. This is the same plot design as Wuming's plots &mdash; a new feature I want to add are the USGS field measurements as different colored markers. 

```matlab
function plot_Qh_modelvsUSGS(filepath, station_name, xsec_name, gageno, startdate, enddate, offset)

    Qh_USGS = import_USGS_ratingcurve(gageno, startdate, enddate, offset);

    Q_USGS = Qh_USGS.discharge;
    h_USGS = Qh_USGS.gage_height;

    [data, im] = readhist(filepath);
    
    station_idx = find(data.stations == station_name);
    xsec_idx = find(data.xsec == xsec_name);


    Q_model = data.xsec_discharge(xsec_idx,:); %extracts the discharge at the station we want
    h_model = data.waterlevel(station_idx,:); %extracts the WSE at the station we want
    
    c = data.time;

    figure
    scatter(Q_model, h_model, 100, c, 'filled', 'MarkerEdgeColor', 'k');
    hold on;
    plot(Q_USGS, h_USGS, 'LineWidth',1.5);
    xlabel('Discharge (m^3/s)')
    ylabel('Water level (m)')
    title(["Stage-Discharge Curve at " station_name], 'Interpreter', 'none')
    cbar = colorbar;
    %xlim([0,400])
    tickvalues = linspace(min(data.time), max(data.time), 10);
    cbar.Ticks = tickvalues;
    tick_dates = datetime(2001,1,1) + seconds(tickvalues);
    tick_dates.Format = 'MM-dd-yyyy HH:mm';
    cbar.TickLabels = string(tick_dates);

    legend('model', 'USGS')
    
    movegui('center');

end
```

#### The Qh Plots

<img width="1092" alt="image" src="https://github.com/user-attachments/assets/c514ce32-4a54-4967-b5bf-064ae3a6a06f" />
*Figure 1. Stage-discharge from the Model and from USGS (regression rating curve) at **Everson**.*

<img width="1108" alt="image" src="https://github.com/user-attachments/assets/7090fe44-782d-4c73-b718-969786c717c4" />
*Figure 2. Stage-discharge from the Model and from USGS (regression rating curve) at **Ferndale**.*








