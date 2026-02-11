## Plots for 2024 Topobathy Model and Avulsion Sensitivity Research Plan

Last week, I ran the 2025 model with the 2024 topobathy and did not have a chance to plot the results. This week, I plotted these up and compared them to observational data at USGS gages. I also worked on a plan for the avulsion sensitivity model runs. This means mapping out the parameter space and doing a literature review. 

### Map File Plots 

Of the variables available to plot from the map files (results that spatially vary over the grid as opposed to results at a point or cross-section) I decided velocity magnitude, water depth, and total bed shear stress magnitude were the most interesting to visualize. The peak in the hydrograph occurs on Decemeber 11, 2025 at 3:00 a.m.

<img width="1322" alt="image" src="https://github.com/user-attachments/assets/77600a9d-739c-4a24-bba0-a37c8684f444" />

*Figure 1. Water surface elevation in NAVD88 (m) at 8:00 a.m. on December 11.*

Figure 1 helped me get a sense of how water interacts with roads an levees during a flood. For example, the large dry rectangle south of the most downstream section of the river shown seems to line up with Abbott Road and Polinder Road, which are represented as levees in the model. I wonder how realistic this is and how the lateral extent of flooding is effected by this representation. 

<img width="1288" alt="image" src="https://github.com/user-attachments/assets/b80f0150-f2cb-4aa0-ac0e-045d61ccb593" />

*Figure 2. Velocity magnitude (m/s) at 8:00 a.m. on December 11.*

A few hours after the flood peak, the velocity in the channel is about 2-4 m/s. This seems very realistic for not-steep river/stream. 

<img width="1337" alt="image" src="https://github.com/user-attachments/assets/97389089-7dd0-421f-99e3-3d111d362922" />

*Figure 3. Total Bed Shear Stress Magnitude (N/m^2) at 8:00 a.m. on December 11.*

Bed stress ($\tau_b$) can tell us information on sediment transport rates. Wherever $\tau_b$ is high, it means the flow of water is exerting a friction stress on the bed, and the bed is exerting a friction force on the flow &mdash; more $\tau_b$ means more possibility for sediment transport. We do see $\tau_b > 100 \mathrm{N m^{-2}}$ especially at the constriction at the Everson Road bridge. There are also pockets of high $\tau_b$ in the overflow section, but not at the very upstream part of it. I would have expected lower sediment transport and lower $\tau_b$ at the overflow section because we predict that deposition occur here, not scour and transport.  
Question: Did any gravel bars form during this flood and can we compare it to $\tau_b$? (Would expect low $\tau_b$ where the bar formed)

### History File Plots

I plotted the water surface elevation (WSE) in NAVD88 for comparison with three USGS gages: Everson, Ferndale, and the Overflow at Main St. To do this, I wrote a script to process the _hist.nc file into a data structure with time, water level, and all the station names that exist in the model. Each observation point is a station in the _hist.nc file. I wrote three functions to do this. 

This function below reads the history file and puts it into a `struct` with time in seconds since the model reference time (2001/01/01 00:00), timestamp, and water level. It would be simple to add more results like discharge or velocity. The file path to the _hist.nc file is a n input variable. 

    % read hist result
    function [data, im] = readhist(filepath)
      hisfile = dir([filepath '/*_his.nc']);
      hispath = fullfile(hisfile.folder, hisfile.name);
      
      im = ncinfo(hispath);
      
      data = struct();
      data.time = ncread(hispath, 'time'); %seconds since reference data 2001/01/01 (set in model)
      data.timestamp = datetime(2001,01,01)+seconds(ncread(hispath,'time')); %convert to YYYY-MM-DD HH:mm:ss
      data.stations = ncread(hispath, 'station_name');
      data.stations = string(data.stations'); %convert from char array to string
      data.stations = strtrim(data.stations); %trims all the trailing spaces
      
      data.waterlevel = ncread(hispath, 'waterlevel'); %store water level - dimensions are (stations, time)
    end

This function scrapes the USGS gage web page by reading a text file into a table in MATLAB. The date range and gage number must be specified, as well as the gage datum, or offset from NAVD88 (in feet). The function stores the table and renames two of the columns that have unique ID numbers in them to make the table headers universal between gages. Then the offset is added to the gage height and the gage height is converted to meters. 

    % Scrape USGS streamgaging data from NWIS server - adapted from Shelby's Python (demo_scraping_NWIS_data.ipynb)
    function [measdata] = import_USGS_gageheight(gageno, startdate, enddate, offset)                     
        %USGS gage height data URL and column names for data link
        url = 'https://waterservices.usgs.gov/nwis/iv/?sites='+string(gageno)+'&agencyCd=USGS&startDT='+startdate+...
            '-08:00&endDT='+enddate+'-08:00&parameterCd=00065&format=rdb';
            
        %Meaning of column headers
            % 'agency_cd',... %USGS
            % 'site_no',...%gage number
            % 'datetime',...%date time in YYYY-MM-DD HH:MM
            % 'tz_cd',...%time zone (PST)
            % 'TS_ID_00065',...%gage height (ft)
        	% 'TS_ID_00065_cd'; %gage height data qualification code (P=provisional)
        
        %Import site data
        measdata = readtable(url, 'FileType', 'text', 'Delimiter', '\t', 'CommentStyle', '#', 'VariableNamingRule', 'preserve');
    
        %rename columns with TS_ID to a consistent name between gages
        measdata = renamevars(measdata, [5, 6], {'gage_height', 'gage_height_code'});
    
        %Account for the offset 
        measdata.gage_height = measdata.gage_height + offset;
    
        %convert to meters
        measdata.gage_height = measdata.gage_height * 0.3048;
    
    end

The last function used the first two functions to plot the data and create the im `struct`, which contains the _hist.nc file information. The function calls the `import_USGS_gageheight` function to get the USGS data, and the `readhist` function to get the model station data, and then plots these two timeseries on the same plot. 

    % Plot Water Surface Elevation from the model and USGS 
    function [im] = plot_WSE_modelvsUSGS(filepath, station_name, gageno, startdate, enddate, offset)
    
        measdata = import_USGS_gageheight(gageno, startdate, enddate, offset);
    
        USGS_WSE = measdata.gage_height;
        USGS_time = measdata.datetime;
    
        [data, im] = readhist(filepath);
        
        station_idx = find(data.stations == station_name);
        WSE_model = data.waterlevel(station_idx,:); %extracts the timeseries at the station we want
        
        figure
        plot(data.timestamp, WSE_model,'LineWidth',1.5);
        hold on;
        plot(USGS_time, USGS_WSE, 'LineWidth',1.5);
        xlabel('Time')
        ylabel('Water level (m)')
        title(["Water level at " station_name], 'Interpreter', 'none')
        legend('model', 'USGS')
        
        movegui('center');
    
    end

I decided to make these functions because I figured I would be using them repeatedly to plot different USGS gages and model stations together. For example, I ran the following lines:

    im = plot_WSE_modelvsUSGS(filepath, station_name_Everson, gageno_Everson, startdate, enddate, offset_Everson);
    im = plot_WSE_modelvsUSGS(filepath, station_name_Ferndale, gageno_Ferndale, startdate, enddate, offset_Ferndale);
    im = plot_WSE_modelvsUSGS(filepath, station_name_overflow, gageno_overflow, startdate, enddate, offset_overflow);
and got the following plots (Figures 4-6).

<img width="700" alt="image" src="https://github.com/user-attachments/assets/40c57207-4442-4f09-a0ce-273f2502bc03" />

*Figure 4. Model and USGS Gage WSE at Everson.*

<img width="700" alt="image" src="https://github.com/user-attachments/assets/5eb298bf-88d8-4db8-9035-5d45920c993f" />

*Figure 5. Model and USGS Gage WSE at Ferndale.*

<img width="700" height="617" alt="image" src="https://github.com/user-attachments/assets/5eb31efe-e314-404d-9d97-4ea77fdc2264" />

*Figure 6. Model and USGS Gage WSE at Overflow Gage.*

#### Thoughts on the Plots 

The Nooksack and Ferndale plots seem to line up well in terms of peak timing and the stage is relatively tracking well. There are some noticeable deviations between them, however. The first peak is slightly overestimated, while the third peak is underestimated by 0.5-1 meter. The recovery stage after the first and third peak is overestimated at both gages. The second peak is quite accurate at Everson but overestimated at Ferndale.  

At the overflow gage, the stage peak is matched nicely. The dry period is offset by about 0.2 meters, which makes me wonder if the offset from NAVD88 is accurate. 

Two of the first things that come to mind that might improve the match to the USGS gage are 1) turning on morphology and 2) adjusting the North Cedarvile input hydrograph based on channel change during the storm. These two adjustments have to do with a change in channel conveyance, which influences the stage that the same volume of flow would cause. For example, a wider channel would experience a lower stage for the same volume of flow. 

#### Lessons Learned

- Make sure that all 62 partitions of the model are downloaded from Hyak
  - Otherwise, there will be 'holes' in the model
- 1-2 GB is about the limit for the file size that is will work in Delft3D
  - Processing of the 2024 topo reduced the file size from a 19 GB TIF to a 1.6 GB XYZ, which was slow but usable in Delft3D. 

#### Next Steps with the 2025 Nooksack Model

Boundary Conditions: Consider adding the tributary boundaries and differentiating the two tidal boundaries. They are the same right now.  
Roughness: Have it update during model run. 
Morphology: Need to add in sediment layers and gradations, and turn on  morpho.
Avulsion research: Obtain future projected hydrographs
Validation: Validate the 2025 model  
Topo: Need to check for artifacts of bridges in the channel?








