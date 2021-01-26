# Agent Based Model

## **KPI's to agent criteria** 
The following criteria have been implemented in the agent based simulation:

* Matrix based relations between spaces (Collectivity)
* Sun access (Home quality) + (Sustainability)
* Entrance distance for public, housing, gym, parking and communal spaces (Diversity in audience) 
* The heights of the spaces (Home quality)
* The desired area requirements per space (Program of requirements)
* Skyview (Home quality)
* Greenery (Sustainability)
* Noise (Home quality) + (Collectivity) + (Sustainability)

________________________________________________

## **Configuring spaces to workable values**
In order to implement the design criteria as mentioned before, those had to be converted to workable values. The values have been written for each space versus the criteria varying from 0 to 1. 0 indicates no connection, 1 indicates a strong connection (Referencing back to the matrix from A1_Configuring). This has been applied for the following criteria: matrix based relations between spaces, sun access, entrance distance for public, housing, gym, parking and communal spaces, skyview, greenery and noise. 

For the space heights and space areas a different approach had to be made since those are hardcoded criteria coming from the Program of Requirements. Hence, those explained in the next paragraph. 


<center><iframe src="https://thumbs.gfycat.com/LittleAdmiredBufeo-size_restricted.gif" style="width:150%; height:295px;" frameborder="0"></iframe> 

*Midterm ABM growth simulation*
</center>



### **Space heights to stencils **
In order to implement the height differences of spaces given from the Porgram of Requirements into the Apidae method, the initial given stencil for all agents (see left stencil in the picture below) has been expanded in the z axis. In the code, this has been done for 1, 2, 3, 4 and 5 voxels high. The highest stencil is 5 x 1.8m = 9 meters into the z axis and 1.8m on the x and y axis. The picture below shows, in the order of mention, the neighbors for a stencil that is 1.8m high, 3.6m high and 5.4m high.

<center><img src="https://cdn.discordapp.com/attachments/784009094474366977/803249271100014612/unknown.png">

*height stencils*
</center>

The creation of the new stencils can be created with the following piece of code:
In this piece of code new neighbours have been defined for the stencils that need to be higher than 1.8m. For example, s_2 has it's z axis neighbor now set at 2 high. Beware that later in the growth model, the occupation has to be adjusted accordingly.

```python
# creating neighborhood definition for stencil that is 1.8m high
s_1 = tg.create_stencil("von_neumann", 1, 1)

# setting the center to zero
s_1.set_index([0,0,0], 0)

#####################################################################
# skipping s_2, s_3, s_4 for this example because it's the same way
#####################################################################

# creating neighborhood definition for stencil that is 9m high
s_5 = tg.create_stencil("von_neumann", 1, 5)
# setting the center to zero
s_5.set_index([0, 0, 0], 0)
s_5.set_index([0, 0, 1], 0)
s_5.set_index([0, 0, 5], 1)
s_5.set_index([0, 0,-1], 0)
s_5.set_index([0, 0,-5], 1)

# setting the center to zero
s_5.set_index([0,0,0], 0)

# listing the stencils in order to make them correspond later with the spaces and their height requirement
stencils = [s_1, s_2, s_3, s_4, s_5]
```

### **Space areas to voxel amounts**
Based on the program of requirements, the required space sizes have been coverted to amount of necessary voxels to meet the area requirement and therefore fulfil it. This has been implemented in the script to maintain the desirded area per spaces, and has been used to limit the growth of the agents. 
From the Program of Requirements the room areas can be obtained, and by giving every space a stencil id according to the desired free height (as explained before how the desired height can be implemented) the total amount of voxel necessary for the space to grow towards can be determined. This can be done with the following piece of code:

```python
# max voxel count per space (here we do the stencil type + 1 
#times the amount of area needed in order to obtain the total amount of voxels that need to be occupied by the script)
# pick room area data
a_room_vox = agn_prefs["room_area"]
# pick stencil id's and do +1 because we start with 0 instead of 1 (id 2 would have stencil 3 high otherwise)
a_room_stencil = agn_prefs["stencil_id"] + 1
# obtain the max amount of voxels needing to be occupied by doing the room area times the height of the space (stencil)
a_room_voxels = a_room_stencil * a_room_vox
# print in order to check below if you obtain correct values
print(a_room_voxels)
``` 
Which gives the following table that can be used later in the growth model:

<center> <img src="https://cdn.discordapp.com/attachments/784009094474366977/803255219545964564/unknown.png"></center>
________________________________________________

## **Definitive program table result (input for generative relations simulation)**
The following table has been made based on the agent/space criterias, this has been used in the definitive script for generating the agent based design. 

4 kinds of values are included next to [white] the agent/space names and space id's:

1. [blue] Desire to closeness to given entrance (0 to 1)
2. [yellow] Desire for high values of the given analysis (0 to 1)
3. [green] Necessary data from the program of requirements (imput specific values)
4. [grey] Desire to be close to given agent/space (0 to 1)

The given entrances for blue and given analysis for yellow are loaded in as csv files. For the making of those csv files, address their corresponding script.

<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vR3BSCNWlacKeNAtlluEjTCw5SMh3Tet-m3ixMxbSwR_aIhWDu0YJLZGvVQdgqWNg/pubhtml?gid=1634426511&amp;single=true&amp;widget=true&amp;headers=false"style="width:150%; height:600px;"></iframe>

# **The Generative Relations Simulation**
After obtaining all the input necessary for the growth model (distance caltulations, solar simulations etc.) it is imporant to be able to make decisions based on that. For this, the script for the Multiple-criteria decision analysis for agents was used and further developed to our needs.  

With the method explained before, the stencils and area calculations were made. The final ABM (Agent Based Model) growth script can be found here 
(INSERT LINKKKKKKKKKKKKKKKKKKK)!!!!!!!!!!!!!!!!!!!!!!!!!!!!

## **How it works**
<center> <img src="https://cdn.discordapp.com/attachments/784009094474366977/803280396954370088/unknown.png"></center>
The ABM (Agent Based Model) growth script runs over a given timeframe. This timeframe is set for it to grow a voxel/stencil per frame. Due to the implementation of stencils with different z axis, the script becomes more efficient (since it covers more voxels per frame). 

After intiializing the script over a given timeframe, it evaluates for every agent the voxels and stores the values of the free voxels in a list. This list is later used for the evaluation of the voxels. 
After having made this list, another for loop is initialized. This is the main agent loop, where all free neighbours are retrieved that are accessible. In the meantime the agents are assessed regarding their maximum given z coordinate value and the max area they're allowed to occupy. 
After all the free neighbours are known, for every agent, if there is a free neighbour is found; it checks the preferences of the agent and evaluates this with the fuzzy framework. When this is done, there are 2 options for the method of occupation: 

1. The max area limit has not been reached yet so it picks the highest value neighbouring voxel. 
2. Else, it checks if there are better voxels to be occupied (if the given evaluation limit is higher than the agent satisfaction; it departs that bad voxel and occupies the new one.

After this is done for all time frames, the new lattice is constructed with the occupation lattice. This is done with stencils that are assigned per agent (for example the housing will use the stencil with an height of 3.6m, this would also occupy 2 (2x1.8)voxels for the agent per frame instead of 1 with the initial stencil). 

For this particular case the code has been ran through 2500 frames.

### **Occupying with different stencils**
For making sure the agents with different kinds of stencils corresponding to their height preferences occupy the necessary amount of voxel in the z axis, a variable has been made and used in the occupation function. 
```python
# making a variable that gives the height of the stencils in voxel (coincidentally +1 since all stencils grow with 1 voxel per id)
a_height = a_stencil_id + 1

#checking if there is enough space for it to see it as available
# Function for checking the availability (Since it is repeated several times in the main loop)
def check_avail(avail_lattice, ind, a_stencil_id):
    condition = 1
    ind_array = np.array(ind)
    for step in range(a_stencil_id + 1):
        new_ind_array = ind_array + np.array([0,0,step])
        condition *= avail_lattice[tuple(new_ind_array)]
    return condition

# if its available: 
# Function for the occupation (and departure but for this piece of demonstration that part isn't important)
def mult_occupation(selected_neigh_3d_address, a_id, a_height, agn_locs, agn_src_locs, occ_lattice, avail_lattice, departure=False):
    # Doing this for x times in the z axis with x coming from a_height
    for step in range(a_height):
        
        #giving a step to the regular occupation in order to run this for every step in the z aces
        new_address = selected_neigh_3d_address + np.array([0,0,step])
        # check if there's enough space in the z axis
        if new_address[2] < occ_lattice.shape[2]:
            # make tuple of the address
            selected_neigh_3d_id = tuple(new_address)
            # find the location of the newly selected neighbour
            selected_neigh_loc = np.array(selected_neigh_3d_id).flatten()

            if departure==False:
                # add the newly selected neighbour location to agent locations
                agn_locs[a_id].append(selected_neigh_loc)
                if step == 0:
                    agn_src_locs[a_id].append(selected_neigh_loc)
                # set the newly selected neighbour as UNavailable (0) in the availability lattice
                avail_lattice[selected_neigh_3d_id] = 0
                # set the newly selected neighbour as OCCUPIED by current agent 
                # (-1 means not-occupied so a_id)
                occ_lattice[selected_neigh_3d_id] = a_id
```

## Housing plan modularity with stencils

A highly modular building needs to be adaptable and reusable for different functions over time. The standardised voxels are very suitable for generating this, but more importantly, they give freedom to generate many different housing plans. For the agent based model, an extra layer of information could be added by growing stencil-based for all housing, co-working spaces and start-up offices. 
Different housing tiles are generated, that together carry all needed functions. For each housing type, a tile library should be made to generate rooms and spaces that fulfil all requirements. These tiles each have markings along the sides, where closed walls, windows, openings to public functions or openings to indoor functions are determined. This way, each house will form based on the markings and their personal library. By generating the housing units like this, all houses will automatically be correctly connected to a corridor, the depth of the building will be limited to the depth of a house and each house will have sufficient daylight and have all their functions available. An extra layer to this would then be the implementation of placement of open and closed facades for each house, coherent with the sun orientation of the building, but also the view. For this the demand of sky visibility and sunlight availability could be calculated for each potential window, which would then also limit the occurrence of two houses growing opposite of each other with minimal free space between them. 

<center><img src="https://cdn.discordapp.com/attachments/784009094474366977/803382410254614538/met_stencils_Pagina_1.jpg">

*function stencil concepts: student housing and corridor*
<img src="https://cdn.discordapp.com/attachments/784009094474366977/803382435085287454/met_stencils_Pagina_2.jpg">

*conceptual configuration of the growth of housing units*
</center>

The concept of these stencil-based growth is visualised in the picture above. The timeframe of this course limited the elaboration on this complex growth model, but thoughts have been put into it. 

<center><img src="https://cdn.discordapp.com/attachments/784009094474366977/803384607834898432/plattegrondenTekengebied_4.png">
<img src="https://cdn.discordapp.com/attachments/784009094474366977/803384610809184296/plattegrondenTekengebied_7.png">
<img src="https://cdn.discordapp.com/attachments/784009094474366977/803384611396780062/plattegrondenTekengebied_10.png">

*simplified housing plan stencils*
 </center>

Instead, simpler housing plans are generated. Still based on the voxel-sized plan, but with a standardisation for each housing unit, where there are just a few fixed stencils for each building. This is also not implemented in the growth model due to time limitations. 
 


### **Evaluation**
The current evaluation part of the code measures if an agent is satisfied with the given evaluation value through the program table. If the agent is not satisfied, it allows the agent to grow towards better voxels, where less valuable voxels are swapped for better ones. If the agent is satisfied, it stays the same and stops growing once it reaches it's max area value. 

The agent satisfaction has been tracked over the final result (2500 frames). With this data Panda tables have been made and visualized through graphs. This way, the agent satisfaction can be tracked without constantly having to look at the visualization and voxel growth manually. See graphs below for the results. The agent names correspond to their agent id in the program.

Evaluation over time for public spaces and entrances:
<center> <img src="https://cdn.discordapp.com/attachments/784009094474366977/803322582107029554/AySsljdNmbF9AAAAAElFTkSuQmCC.png"></center>

Evaluation over time for Assisted living (11), Student housing (17) and Start-up office (19)
<center> <img src="https://cdn.discordapp.com/attachments/784009094474366977/803322582107029554/AySsljdNmbF9AAAAAElFTkSuQmCC.png"></center>

Evaluation over time for starter housing (16):
<center> <img src="https://cdn.discordapp.com/attachments/784009094474366977/803322623199150130/B02eVgwU5jOiAAAAAElFTkSuQmCC.png"></center>

### **Final growth**

<iframe src="https://media3.giphy.com/media/WDhRzS8AIleGliAs9s/giphy.gif" style="width:150%; height:300px;" frameborder="0"></iframe>

### **Improvement points**
* Distance calculation and evaluation between spaces: 
We werent able to implement distance calculations between spaces, so spaces do not pull on each other, in fact they are next to each other only because they have the same preferences. 
* Further development of the evaluation
The current evaluation does not take into account whether a specific voxel would've been more valuable for another kind of agent, so it does not assess already occupied voxels by agents other than itself. It only assesses itself. This could be one of the further improvements on the evaluation.
* Less timeframes (calculations behind)
It is better to find a way to do the frames code wise more efficiently since the current one has to be run the amount of voxels the biggest agent is. Which is kind of inefficient since maybe all others are already done halfway. 
* Squareness (conditional neighbour or different stencil) 
We weren’t able to implement squareness yet due to the time limit. This could have 2 ways: we could set conditions to the neighbours in order to do so or we could make bigger stencils in the x and y axes as cubes but that actually complicates things regarding neighbouring and occupying.
* Stencil assigning per floor 
Also we havent yet been able to implement specific stencils only for specific z coordinates, So the public spaces are now 5 z coordinate high stencils instead of one fo 3 that only grows 2 high stencils above itself.
* Shafts and corridors implementation
It is important to combine shafts and corridors, but when we combined those 2, it made the agents get stuck between corridors and not grow further (this happened because we dont want public spaces to grow above  a certain height). 
