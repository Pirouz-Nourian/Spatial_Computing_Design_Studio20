
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

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803382410254614538/met_stencils_Pagina_1.jpg">
<img src="https://cdn.discordapp.com/attachments/784009094474366977/803382435085287454/met_stencils_Pagina_2.jpg">

The concept of these stencil-based growth is visualised in the picture above. The timeframe of this course limited the elaboration on this complex growth model, but thoughts have been put into it. 

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803384607834898432/plattegrondenTekengebied_4.png">
<img src="https://cdn.discordapp.com/attachments/784009094474366977/803384610809184296/plattegrondenTekengebied_7.png">
<img src="https://cdn.discordapp.com/attachments/784009094474366977/803384611396780062/plattegrondenTekengebied_10.png">

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


# **Polygonization**

## **Perfect shape grasshopper script**
Everything thus far in this process has been modular and efficient. When it came to polygonization, which consists of a lot of manual labour, it did not feel like it was conformed to the ideology of this course. That’s why we decided to make this process more modular and efficient. This originated the beginning of the ‘’perfect shape’’ grasshopper script. The script takes a perfect shape (cube) and cuts it into 152 pieces. It then sorts these pieces based on a distance from a point to the centroid of a piece. With the usage of slider,  a selection can be made for a specific piece and a specific name. After this a directory is selected to save all the files to, by pressing a button. There is also the option to bake every object to their correct layer by pressing a button, as a backup check,  as well as the option and delete all objects.  
For altering the building, the desired geometry should be added onto the “perfect shape”, creating a “new geometry”. This shaped will then also be cut into 152 pieces, and sort based on a distance from the same point to the centroid of the pieces. The “perfect shape” pieces will form a cutting box by which the “new geometry” pieces are cut up. In theory the “new geometry” pieces should be in the same position as the “perfect shape” bounding box for a specific number. For this “new geometry” the only change that must be made to the rest of the script,  is to select a new directory to save the pieces to. This makes the process of generating multiple tilesets extremely efficient. 
This is the theory behind the script. Unfortunately, in practice, there are some challenges to work around. Firstly the “new geometry” should be somewhat cube like. This means that although the added geometry can be asymmetrical, adding/extruding the geometry on an edge can only be done in a symmetrical way, otherwise the box will be cut up into more than 152 pieces. 
The way the 152 pieces are sorted is done by a distance from the piece towards a point. By altering the geometry, these distances do not always stay completely the same though. In 5 “new geometries” tests, in 2 to 5 pieces out of 152 pieces, the order had changed.  This can be solved by manually changing the slider to the correct position. In the 5 test runs the change of the slider was a maximum of 5 up or down.

## **Pseudocode Perfect shape grasshopper script**
<iframe frameborder="0" style="width:100%;height:193px;" src="https://viewer.diagrams.net/?highlight=0000ff&edit=_blank&layers=1&nav=1&title=Grasshopperscript.drawio#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1CGnwSmRQPMmgTsY0JmWY-r1alWDR8-cj%26export%3Ddownload"></iframe>

## symmetry = 152 pieces
<img src="https://cdn.discordapp.com/attachments/775754717346791494/803293065761783848/152_pieces.jpg" style="width:280px;">

## assymetry = more pieces 
<img src="https://cdn.discordapp.com/attachments/775754717346791494/803293112804835328/160_pieces.jpg" style="width:280px;">



## **Polygonization script**
Out of the possible 24 subtiles, it is only necessary to use 10 of them, the rest could be used for exceptions or when shapes are more complicated. In the 10 subtiles, there are 2 subtiles missing. There is no distinction between floor and roof and no distinction between roof corner and floor corner. As of now if when adding a railing to the roof of the building, this railing will also appear on the bottom of the building. 


<img src="https://cdn.discordapp.com/attachments/775754717346791494/803358655361187871/different_facade_pieces.jpg" style="width:280px;">
*different facade types*


<img src="https://cdn.discordapp.com/attachments/775754717346791494/803363901935845486/polygonisatie_model.JPG" style="width:280px;">
*polygonization model*



In the polygonization script itself, there are also still some challenges. Loading in textures and giving it to objects without making a mesh out of it works fine. But when the object file is made a mesh, it loses its texture coordinates. Our group managed to extract texture coordinates from an OBJ and give them to the object that is made a mesh in the jupyter notebook, but the link between textures + texture coordinates and the generated tiles from the subset is missing when exporting the final results or running everything through the poligonization script. Perhaps in the future another group can solve and implement this. 
To give the building a varying appearance, instead of having the same look everywhere throughout the building, stencils were implemented. Due to our floors being 2 voxels high, this brought a challenge. To vary a floor from down to top, it was necessary to design a tileset for the top as well as the bottom of the floor. While selecting these singular voxels as a stencil, another problem appeared. When separating 1 voxel from a bigger number of voxels, it will change from being a wall, to being a corner. The design of the singular voxel will have to be placed on the corner now. Since there is no distinction in the subtiles between a top or a bottom corner, this singular voxel will be symmetrical in both directions. 

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803300933651791902/all_tiles_hoofdstuk_4_polygonisatie.jpg" style="width:280px;">

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803300928626753566/used_tiles_hoofdstuk_4_polygonisatie.jpg" style="width:280px;">

<img src="https://cdn.diiscordapp.com/attachments/775754717346791494/803300920514314250/tiles_changing_hoofdstuk_4_polygonisatie.jpg" style="width:280px;">

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803358655361187871/different_facade_pieces.jpg" style="width:280px;">






This process is yet to be perfect, but the solutions seems to be not to far away. If the linkage between the textures+ texture coordinates and the generated tiles, the 2 extra subtiles are added, the usage of stencils is improved, this should be a good way to design the exterior of the building in a quick and modular way. 


