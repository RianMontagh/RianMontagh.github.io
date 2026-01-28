## Delft3D Model Updates - First Run 

Last week, I was working on adding the roughness and the boundary conditions. This week, in order to speed up the process, I used files from Wuming that contain elements such as the roughness, the levees, the observation points, observation cross sections, initial water level. These would have taken me a long time to develope on my own, and it seemed worth it to use them and get results sooner even though I didn't learn how to develope them myself. I will likely have to learn this during the research process naturally. 

### Hyak
Hyak is an ecosystem of high-performance compute clusters, currently deployed in its third generation, Klone (Hyak = "fast" and Klone = "three" in the Chinook language). This is what I will be running the model on, because my local laptop would not be powerful enough. The model is run in parallel, which means the grid is split up into different chunks that are run separately and pass information to each other, which allows for a quicker runtime. This means the output from the model run is split into these different chunks, which I then need to piece together before I can view them in Delft3D. Specifically, the parallel runs create ghost points between the sections of grid in order to pass information between them - these points need to be removed because they do not represent actual model results. The computer infrastructure looks like this:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/e3b363e1-9fe6-4515-a353-a1ff122a3344" />

### First Run

I successfully logged into Hyak and sent in my first batch job to Slurm (Simple Linux Utility for Resource Management) which is a job scheduler that Hyak uses. Two pieces of code are needed&mdash;one that tells Slurm the properties of the job, such as how many nodes to allocate to the job, the time limit of the job, etc. and one that specifies how the parallelization of the job should occur. For the Nooksack, there are 62 parts running in parallel, and two nodes are allocated to the job. Nodes are singular computers/servers within the overall cluster. 




#### Next Steps with the 2025 Nooksack Model

Boundary Conditions: Consider adding the tributary boundaries and differentiating the two tidal boundaries. They are the same right now. 
Grid: Need to add the new 2024 topobathy. Waiting to receive it, currently using 2022 topobathy.  
Roughness: Using Wuming's
Settings: Done - mimicked the basic settings of Wuming's but did not turn the special knobs he has turned. 
Initial conditions: initial water level from Wuming. 
Morphology: Need to add in sediment layers and gradations








