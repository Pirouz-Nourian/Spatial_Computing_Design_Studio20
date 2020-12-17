# Agent Based Modelling

### 1   Choosing the stencils

These stencils will describe how a function is going to grow in the 3 dimensional world and what shape it will end up in. We have defined two stencils. One for the z-plane and one for the xy-plane.

#### Code: Stencil z-plane
``` python
# creating neighborhood definition - center with 0 neighbours
s_z = tg.create_stencil("von_neumann", 0, 1)
# setting the center to zero
s_z.set_index([0, 0, 0], 0)
# setting z neighbours to 1
s_z.set_index([0, 0,-1], 1)
s_z.set_index([0, 0, 1], 1)

```

![title](../img/StencilZ.JPG)

#### Code: Stencil xy-plane

``` python
# creating neighborhood definition - center with 1x neighbours
s_xy = tg.create_stencil("von_neumann", 1, 1)
# setting the center to zero
s_xy.set_index([0, 0, 0], 0)
# setting z neighbours to 0
s_xy.set_index([0, 0, 1], 0)
s_xy.set_index([0, 0, -1], 0)

```
![title](../img/StencilXY.JPG)

#### To do
The xy-stencil could be adjusted so that it would grow more in a square like shape. The corners would be filled as well, instead of just these 4 neighbours, there would be 8. This is still a work in process and we are not sure if this should be solved with the stencil.

### 2   Agent & Enviroment Information 
Earlier we had defined the relations between functions and their Key Performance Indicators (KPI) in a matrix created in excel. This has been transformed into the program.csv where a couple criteria have been added and some are not yet  added. The ones that are added are the maximum voxel count (representing the maximum area/volume) and the preference of each function to expand in the z-direction. In the code certain variables refer to a column with these criteria for each function (located in a row). Besides the Agents criteria & relations we also need the environmental information. These pieces of information are important because they will define the placement of the functions and how they will grow and shape.

#### Code Loading Agents Information
``` python
# loading program (agents information) from CSV
prgm_path = os.path.relpath('../data/program.csv')
agn_info = np.genfromtxt(prgm_path, delimiter=',')[1:, 1:]
# extract agent ids
agn_ids = agn_info[:, 0]
# extract agent preferences
agn_prefs = agn_info[:, 1:]
# extract agent preference to expand in the z-direction
agn_expandz = agn_info[:, 3]
# extract maximum voxels of each agent agent. This represents the maximu8m area & volume
agn_vox_req = agn_info[:, 4]

```
![title](../img/MatrixVSCode.JPG)

This is a small part of the matrix used for the growing process. z_exp & voxel_count are new values.  

One of the environmental information csv’s is the one for entrances created in w+3 where the distance fields between the entrances are created. In this notebook we have selected 6 voxels as our entrances since our metromap is based on 6 entrances. The placement of these entrances is random thus far. This will have to change to make sense on the urban scale so our building connects good with the surrounding area and infrastructure. 

![title](../img/Enrances.JPG)

#### To Do
-	Add more criteria (green access, noise etc.)
-	Make sure this matrix and the one shown before are equal
-	Understand the sequence of rows and columns when new criteria are added
-	Change voxel_count into real necessary voxel count when using a higher resolution envelope
-	Add more csv’s (green access (causes problem), noise, groundfloor access, urban functions, shortest paths for pedestrians, cars, bikes, UHI-effect?)
-	Change the entrances. Perhaps these should be based on urban routes and all have separate csv’s and act as separate criteria’s? For example the most used path for cars should have quick access to an entrance where the function parking should be located.
-	Understand the order env_info picks data from the program matrix and match that with the desired data. And understand what should be added and what not. code: env_info = [ent_acc_lattice, sun_acc_lattice]

### 3   Initialize the agents
Here our functions are represented by a single voxel or agent as a starting point to grow later in the process. They are placed based on their Key Performance Indicators / criteria (sun access & entry access) and the environmental data (sun exposure, distance fields to entrances). 

![title](../img/SeedLocations.JPG)

There is a problem occurring here that needs to be solved which is the fact that a lot of functions are crammed into one corner leaving them no room to grow around their starting point. Shervin has provided us a starting point to solve this problem. The idea is that we create a new criteria for all functions to stay away from each other by computing the distance field of the already existing functions. 

**Code idea Shervin**
``` python
for a_id in agn_ids:    
        voxel_vals = []
        pot_voxels = []
        # retrieve agent preferences
        a_pref = agn_prefs[int(a_id)]
        # for all the voxels that are already occupied in the occ_lattice
            occ_vox_1d = np.ravel_indices(np.where(occ_lattice > -1), occ_lattice) 
            distance_to_occ = dist_mtrx[occ_vox_1d]
            # combine all of this distance lattices
            init_env_info = env_info + [init_dist_lattice]
            init_a_pref = a_pref + [1.0]
            # Voxel Evaluation Loop
            for pot_vox in avail_index:
                if avail_lattice[tuple(pot_vox)]:

```

#### To Do

-	Interpolate to create the seed allocation in the high resolution envelope
-	Solve Shervins Idea for the crammed corner problem
-	Add criteria to solve crammed corner problem
-	Alter criteria’s based on illogical placement of functions: reflection
-	Add multiple agents for housing units to optimize their connection with green areas

### 4   The growing process