# Process of massing

## Solar Simulation & Shadow Analysis
Based on the ladybug sunpath the solar and shadow envelope are calculated in one file, since they have a largely corresponding step. To do so a cast a ray from the centroid of the voxel towards all the points on the sunpath. If a ray is not intersected by the context, then this voxel receives sunlight from this point. If the ray is intersected by the context, then the voxel does not receive sunlight from this point. For all the voxel that have been hit, the rays that were shot towards the sun are reversed, to calculate the shadow. If this ray intersects the context, then the voxel casts a shadow. If the ray does not intersect the context, then the voxel does not cast a shadow. Both the sunlight and the shadow envelope are then interpolated to a highres value, being of our voxel size. 

<center> <img src="https://cdn.discordapp.com/attachments/775754717346791494/803300922296893470/sun_and_shadow_hoofdstuk_3_sun_and_shadow.jpg"></center>

<center> <img src="https://cdn.discordapp.com/attachments/775754717346791494/801507207664762950/sun.png"></center>
*Sun calculation*

<center> <img src="https://cdn.discordapp.com/attachments/775754717346791494/801507211003822190/shadow.png"></center>
*Shadow calculation*


### Lowres size decisions
In the first run of the interpolated shadow file,  a lowres envelope of 2 voxels high was used. This resulted in the shadow casting calculation becoming much to generalized. In this situation it would see the entire bottom half of the building as not casting shadow on the neighbouring buildings, thus not showing them in the shadow casting and only showing the top half of the building in the visualisation. To solve this problem a lowres envelope of 3 voxels high was used. This resulted in a visualisation of the entire envelope. 

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/midterm/lowreshighres.png?raw=true">
________________________________________________
## Skylight & Skylight blocking
This script is very similar to the Solar simulation, but instead of loading a sunpath, a sphere is created to represent points in the sky. For each voxel a ray is cast from the centroid of the voxel towards all the points in the sky.  If a ray is not intersected by the context, then this voxel receives skylight from this point. If the ray is intersected by the context, then the voxel does not receive skylight from this point. For all the voxel that have been hit, the rays that were shot towards the sky are reversed, to calculate the skylight blocking. If this ray intersects the context, then the voxel casts blocks skylight from the context. If the ray does not intersect the context, then the voxel does not block skylight from the context. Both the skylight and the skylight blocking envelope are then interpolated to a highres value, being of our voxel size.

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803300915749716088/skylight_and_skylight_blocking_hoofdstuk_3_skylight_and_skylight_blocking.jpg">

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/2.0_skyaccess.png?raw=true">

*skylight access*


<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/2.0_skyview.png?raw=true">

*skylight blocking*

### difference between shadow and solar
The result of the shadow envelope, solar envelope, skylight envelope and skylight blocking envelope  are processed in 2 ways. Fundamentally our building should be of least disturbance for the surrounding area, so it would not make sense to keep voxels that cast too much shadow or block too much skylight. Therefore, the voxels that cast too much shadow and block too much skylight from the context are removed. 
For the sun and skylight values a similar argument could be made. For sake of the building one could argue that only the voxels with the best sun and skylight access should be used. However, this would result in removing the voxels on the ground floor. Since building can’t float in the sky(yet), and for the accessibility of public functions its best to have them on the ground floor, the data of sunlight and skylight is stored inside the voxels for the growth model (link to growth model). 
 


### Removing voxels
To not cause too much shadow or block too much skylight from the context, the voxels that cast too much shadow or block too much skylight from the context, are removed from the envelope based on a threshold. This results in a new envelope without the “bad voxels” 


<iframe src="https://thumbs.gfycat.com/ValidImaginativeChihuahua-size_restricted.gif" style="width:150%; height:430px;" frameborder="0"></iframe>

*Threshold* 



<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/2.2_newfulllattice.png?raw=true" style="width:280px;">

*New envelope*

________________________________________________
##  Sun and Skylight improvements
Although these scripts are functional, there are still some improvements that could be made. 
    -	Factoring the influence of voxels inside the envelope on the sun/skylight and shadow/ skylight blocking. 
        o	For the initial stage of storing data and removing voxels, solely using the influence of the context on the voxels to be blocking sun or skylight is sufficient. But for later stages when the building is generated (link to growth model), it would be an improvement to take the influence of light and shadow of voxels on each other. This would make that script even heavier, because it will have to calculate a light and shadow value each time it adds voxels. Besides that, a distinction should made with the outer voxels and inner voxels, to have some depth in the building. This depth should also be specified. To solve this, the growth model could be normally ran at first, and then after it has finished going through an evaluation loop to check the shadow and skylight blocking on the context.
    -	Removing bad voxels based on the shadow the context receives instead of the percentage of the time voxels cause shadow.  
        o	As of now a threshold is specified to remove voxels from the envelope based on the percentage of time these voxels cast shadow and block skylight from the context. However, it would be an improvement if the voxels are removed based on the effect they have on the context. If a voxel casts a shadow on the context 50% of the time, this could mean that it causes it on a different building each time, meaning that although the voxel looks “bad”, the net result on the context is negligible. This calculation should also be taken into the growth model (link to growth model). This does mean this script will become even more heavy as it now must calculate the shadow and skylight blocking each time it grows as well. To solve this, the growth model could be normally ran at first, and then after it has finished going through an evaluation loop to check the shadow and skylight blocking on the context. 




________________________________________________
## Quietness
To calculate the quietness noise in the building in relation to its context a path with noise points on it is loaded inside a script. Each voxel then calculates the distance from the voxel centroid towards the noise point. These distances are added together and converted into a ratio. This script is actually more about business on street level than about quietness, since it imports noise points and not actual decibels from areas. This does not matter too much, since noise could be filtered from a building by adding more insulation and the relative quietness matters more than the actual numbers in decibels

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.2_quietness.png?raw=true" style="width:280px;">

*Noise highres(quietness)*

________________________________________________

## Entrances and distance lattices to these entrances
For this script, a design decision is needed in regards to the placement of the entrances. For this, the following site analysis has been made: 

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803248102412779550/entrances.jpg">

As is shown, the accessibility from the city centre is highest on the south and westside of the plot. The main connection with the city centre is the route from the south, via the Luchtsingel and the Hofbogen. To expand the atmosphere of this vivid area, the public functions should have short distance to this place. By placing them across the old metroline along the westside of the plot, the street ambiance will get a new boost, the old metroline can be made in good use and an extension of the vivid street scenery from the city centre is then generated. 
To make this work, a distance lattice is being made. the distance is calculated from all streetlevel voxels that are along this street. This would attract all functions that care about having a public entrance; fablabs, café pub restaurant, arcade, shop, co-working spaces and startup office. By not giving them a determined entrance, there stays room for these functions to not only grow towards this set location but it gives them a range of places where they can grow with consideration of other values.  
As the accessibility from the city centre is also high for the eastside of the building, this could function as a more private, yet highly used entrance side. All communal functions could be located here, as well as the main housing entrances and the gym. For these functions, a few different lattices are generated, as the housing access needs a number of set location on different sides of the building, and the community centre entrance is set next to this housing entrance to have a entrance that brings people together.
For the parking entrance, the location that has been chosen is close to the housing entrance, yet it should minimize car traffic in the neighbourhood and is therefore determined on the very east side of the building, close to bigger streets. 
For each lattice, a distance graph is generated for all voxels within the envelope. To minimize these calculations it is done over a bigger voxelsize. After generating this graph, the specific entrance voxels are selected, and with these voxels of interest a distance lattice is generated. After normalizing, interpolation takes place to translate all values to the actual voxelsize that is being used. 


<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.3_lattice_public_entrances.png?raw=true">

*distance lattice public entrance*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.4_lattice_gym.png?raw=true">

*distance lattice gym*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.5_lattice_parking_entrance.png?raw=true">

*distance lattice parking entrance*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.6_lattice_housing.png?raw=true">

*distance lattice housing entrances*

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/3.7_lattice_comcen_entrance.png?raw=true">

*distance lattice community centre*

### Improvements 
To make an even more accurate and generative design, the initial locations of the entrances should not be chosen by hand but determined in a calculation of accessibility of the plot. This would be done by making an extensive model of all surrounding streets: their bustle, their usability for different types of traffic and their potential. This extensive model could not only be used for generating entrances, but also for a noise calculation to value all voxels based on the traffic. 
This more detailed way of locating an entrance should be implemented during the growth model, so closeness of other entrances would be taken into account for generating the wished for street ambiance

________________________________________________
## greenery
For the plot, 30% should be dedicated to becoming a greenspace. The location of this greenspace could be generated in three different ways: 

1.	By hardcoding where the park should be located, for example based on observations made in the surroundings. 
2.	By checking which ground level voxels would be most suitable for  greenery; checking for sun, noise and daylight, and also closeness to   certain functions.
3.	By checking which ground level voxels are least useful for generating the building, and removing those from the growth model.

The first one is a more classic architectural way of designing, and would therefore not fit in our ambitions to create a generative building. The second option would be very interesting. Only this calculation would be useless if the final form of the building would not be taken into account, so doing this the greenery should be part of the agent based model. Not only this, but the agent based model would need an iteration for the sun- and daylight blockage towards the greenery for every growth step, since the most hinder of sun- and daylight would be generated by buildings that are closest to the greenery, being the generative building itself. Next to that, also the voxel values that are above the selected greenery should be taken into account for each iteration. This because by placing a greenery voxel, all voxels above (or all voxels that do not leave a certain value of day-and sunlight available for the voxel) should be removed from the availability lattice. It would be a waste of good voxels if these voxels are very valuable, whilst the greenery voxel would be almost as satisfied with another voxel. There is need for a balance of value in voxels that are discarded because of the greenspace and voxels that are selected for greenspace. 
Because of the complexity of this system and the lack of worth if not done well, this is not the method that has been executed. For this design, the last method was chosen, finding the average worth of each voxel and removing all voxels that have a low value. This should be done in a two dimensional way, because the greenery will occupy the full height if it takes the ground level voxel. 
 
For each voxel, the multiplication of all values that are associated with that location are being taken. Then this value is being summed for all voxels in the z-direction, creating a two dimensional value for each location. After normalizing this value, it is copied to all z-voxels so if a value is lower than the threshold, all voxels in that z-direction are being removed. 

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/4.1_averagevoxelval.png?raw=true">

By then determining the minimum voxel value that is needed to clear 30% of the location, all values that are lower are being removed and all other voxels are being multiplied with the availability lattice that had been generated based on shadow. This results in the final availability lattice, and in the space dedicated for greenery. 

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/4.2_new_avail_lattice.png?raw=true">

### Evaluation of the location and the shape of the greenery 
Because of the many entrances that have been generated at the more accessible side of the plot, the distance values of these voxels are relatively impactful for the average value lattice, leaving a low-valued space at the north of the plot. This directly results in a greenspace that is not as open-oriented, but that is more encapsulated behind the building and oriented towards local users. This fits in with our concept of the greenery, that would be a place for retreat and also a place to have a food garden for the neighbourhood and the communal kitchen. 

### Improvements
As has been stated in the decision for this type of greenery selection, a much more complex way of determining the location of the greenery would be possible, but a self-evaluation loop for sun- and daylight would be needed, as well as the implementation of current voxel value evaluation with implementation of the greenery voxel values. This would be very useful, but was not manageable within the given timeframe. 
________________________________________________

## Shafts and Corridors
The shafts and corridors have been generated in a way that each floor has a network of corridors without any dead ends. 

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803363005420011520/depth_analysis_graphTekengebied_1.png">

<img src="https://media.discordapp.net/attachments/784009094474366977/803363007136137216/depth_analysis_legendTekengebied_1.png" style="width:400px;">

As is shown in the depth analysis (a translation of the topological map, with the focus on connections to the street) it can be seen that all public functions have their own entrance, all communal functions are connected to the community centre and all housing functions are connected to the housing entrance. For this corridor generation these two connection-groups are the connections that are being used. Other connections are considered secondary, and are therefore not taken into account. 
The division in functions (housing entrance and communal entrance) corresponds with the initial location of the functions, where all communal functions are located on the first two floors and the housing functions, along with the co-working spaces and the start-up offices, grow above this. This results in two types of corridor networks; one connecting all communal functions and all shafts to upper floors, and one connecting all functions that grow on the upper floors, along with the shafts.
For the growth of the communal corridors, the initial seeds of these functions are being placed. Because the location of the shafts is more related to the location of the entrances than it is to the seeds of the voxels, the shafts are located based on these entrance locations.

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/6.3_shafts.png?raw=true">

Instead of using the shafts as connections towards each seed, a list of each function seed is used. The shaft locations are then added to this list, and after generating a distance matrix the shortest path between each function in this list is then calculated. These shortest paths are calculated in a two-dimensional plane, and are then copied to all corresponding levels; voxels 1, 2 and 3 for the groundfloor level and voxels 4 and 5 for the second floor. 

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/5.1_corridors_groundfloor.png?raw=true">

The same steps are being taken for the higher level functions, but in addition the height of the corridors is limited by the length of the shaft that it is connected to. This because the length of the shaft determines whether or not the floor is accessible. 

<img src="https://github.com/EdaAkaltun/spatial_computing_project_template/blob/master/docs/img/finalscreenshots/6.4_shafts_and_corridors.png?raw=true">

### Corridor evaluation
Because the corridors are all connected, they limit the abm growth by enclosing voxel seeds and therefore we turned them off in this simulation. The location of the shafts led to the corridors growing along the border of the available voxels, occupying valuable space in the building.  The shafts itself are also along the border, but this is a design question that does not have a straightforward answer, as shaft locations differ in each building design. For this situation, a placement along the border had been chosen to minimize walking distance within the building. 

### Improvements
The concept of these corridors is useful, but should be generated after the growth of the functions. Then it can find paths along the borders of each function, making sure no function gets limited in their growth. For some functions it might not be a problem if they were to be split up by these corridors and this could be determined in the agent preferences (housing functions, offices). This method would also make sure the most valuable voxels would not be occupied by the corridors, because the voxels that would now be occupied are the voxels that are placed later in the growth model and are therefore less ideal. Right now the corridor grows towards the most valuable voxel, occupying both this voxel and (at least) two of its direct neighbours.
For the shafts, a stencil should be used to occupy all voxels that would be occupied when placing a shaft. 


________________________________________________


## **KPI's to agent criteria** 
explain what our criteria for the agents are and how they relate to the KPI's
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


<iframe src="https://thumbs.gfycat.com/LittleAdmiredBufeo-size_restricted.gif" style="width:150%; height:295px;" frameborder="0"></iframe> 

*Midterm ABM growth simulation*



### **Space heights to stencils **
In order to implement the height differences of spaces given from the Porgram of Requirements into the Apidae method, the initial given stencil for all agents (see left stencil in the picture below) has been expanded in the z axis. In the code, this has been done for 1, 2, 3, 4 and 5 voxels high. The highest stencil is 5 x 1.8m = 9 meters into the z axis and 1.8m on the x and y axis. The picture below shows, in the order of mention, the neighbors for a stencil that is 1.8m high, 3.6m high and 5.4m high.

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803249271100014612/unknown.png">

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

________________________________________________
________________________________________________
________________________________________________
________________________________________________
________________________________________________
________________________________________________

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
* Less timeframes (calculations behind)
It is better to find a way to do the frames code wise more efficiently since the current one has to be run the amount of voxels the biggest agent is. Which is kind of inefficient since maybe all others are already done halfway. 
* Squareness (conditional neighbour or different stencil) 
We weren’t able to implement squareness yet due to the time limit. This could have 2 ways: we could set conditions to the neighbours in order to do so or we could make bigger stencils in the x and y axes as cubes but that actually complicates things regarding neighbouring and occupying.
* Stencil assigning per floor 
Also we havent yet been able to implement specific stencils only for specific z coordinates, So the public spaces are now 5 z coordinate high stencils instead of one fo 3 that only grows 2 high stencils above itself.
* Shafts and corridors implementation
It is important to combine shafts and corridors, but when we combined those 2, it made the agents get stuck between corridors and not grow further (this happened because we dont want public spaces to grow above  a certain height). 
