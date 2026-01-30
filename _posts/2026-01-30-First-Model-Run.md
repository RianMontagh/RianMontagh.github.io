## Delft3D Model Updates - First Run 

Last week, I was working on adding the roughness and the boundary conditions. This week, in order to speed up the process, I used files from Wuming that contain elements such as the roughness, the levees, the observation points, observation cross sections, initial water level. These would have taken me a long time to develop on my own, and it seemed worth it to use them and get results sooner. I will naturally learn these skills later on once I am deeper into using Delft3D. 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/74d58770-5f7a-4763-87dc-6467d6890ea2" />  

The above shows all of the roads and levees in the model (roads are non-erodible right now). 

<img width="700" alt="image" src="https://github.com/user-attachments/assets/d5c83612-7f7e-41d1-ae2d-41e2f7556d03" />  

The image above shows the observations points and cross-sections, which tell the model the specific locations where we want it to save data. 

<img width="700" height="573" alt="image" src="https://github.com/user-attachments/assets/769090fd-3554-47db-b591-652fbfa57c34" />  

The image above shows the initial water elevation - the channel starts out wet, as well as the overflow boundary condition. 

### Hyak
Hyak is an ecosystem of high-performance compute clusters, currently deployed in its third generation, Klone (Hyak = "fast" and Klone = "three" in the Chinook language). This is what I will be running the model on, because my local laptop would not be powerful enough. The model is run in parallel, which means the grid is split up into different chunks that are run separately and pass information to each other, which allows for a quicker runtime. This means the output from the model run is split into these different chunks, which I then need to piece together before I can view them in Delft3D. Specifically, the parallel runs create ghost points between the sections of grid in order to pass information between them - these points need to be removed because they do not represent actual model results. The computer infrastructure looks like this:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/e3b363e1-9fe6-4515-a353-a1ff122a3344" />

### First Run

I successfully logged into Hyak and sent in my first batch job to Slurm (Simple Linux Utility for Resource Management) which is a job scheduler that Hyak uses. Two pieces of code are needed&mdash;one that tells Slurm the properties of the job, such as how many nodes to allocate to the job, the time limit of the job, etc. and one that specifies how the parallelization of the job should occur. For the Nooksack, there are 62 parts running in parallel, and two nodes are allocated to the job. Nodes are singular computers/servers within the overall cluster. 

#### Visualizing the Results

The goal of this first run is partly to learn how to put together the results so that they can be viewed in Delft3D. Wuming has written MATLAB code that does this for the Nooksack - it combiens the 62 model partitions back into one grid. Once doing this, it is possible to see if the model worked or not based on the model output. At this point, I need to wait for Wuming to provide me the Open Earth Tools code, which are used in his code to visualize the model results. However, I did see preliminary water level results during the flood on Wumng's computer, and it looked reasonable! So I am optimistic that this first run was sucessful. 

#### Lessons Learned

- File organization is key
  - The project directory should contain a subfolder with the project files and the two code files for running with Hyak.
  - There is only one .mdu file (main project file with general settings)
  - The project subfolder contains multiple input files that the .mdu file can direct to.
  - I started a OneNote to keep track of what each version of the model output means
- The GUI (graphical user interface) is not reliable
  - the project files should be generated and edited as much as possible outside of the GUI due to bugs
  - when saving a project, it is better to export individual files rather than save the whole project&ndash; otherwise the GUI will overwrite your naming conventions.

#### Next Steps with the 2025 Nooksack Model

Boundary Conditions: Consider adding the tributary boundaries and differentiating the two tidal boundaries. They are the same right now.  
Grid: Need to add the new 2024 topobathy. Waiting to receive it, currently using 2022 topobathy.  
Roughness: Using Wuming's  
Settings: Done - mimicked the basic settings of Wuming's but did not turn the special knobs he has turned.  
Initial conditions: initial water level from Wuming.  
Morphology: Need to add in sediment layers and gradations  
Visualizing Results: Need to download Open Earth Tools from Wuming's gscratch once he uploads it. 








