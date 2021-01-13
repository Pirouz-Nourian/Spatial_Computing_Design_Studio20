# Process of massing

## Solar Simulation & Shadow Analysis
Based on the ladybug sunpath we first calculated the solar and shadow envelope in one file, since they have a largely corresponding steps. Once that worked we splitted the interpolation, because that calculation takes more time, so to protect our modest hardware, we thought it would be better to split them up. 

### Lowres size decisions
when first running our interpolated shadow file, we used a low res envelope which was only 2 voxels high. This resulted in the shadow casting calculation becoming much to generalized. In this situation it would calculate the entire bottom half of the building as not casting shadow on the neighbouring buildings, thus not showing them in the shadow casting and only showing the top half of the building in the visualisation. To solve this problem we changed the low res envelope from being 2 voxels, to being 3 voxels high. This resulted in a visualisation of the entire ennvelope. 

<img src="../img/midterm/lowreshighres.png" style="width:280px;">

### difference between shadow and solar
The result of our interpolated shadow envelope and the interpolated solar envelope, are both processed in a different way. Fundamentaly we want our building to be of least disturbance for the surrounding area, so it wouldn't make sense to keep voxles which cast too much shadow. This is why we remove the voxels which cast too much shadow on the surrounding areas. The solar value of each voxel however, you could argue that for the sake of the building, it would be best too only keep the voxels which have optimal sun access. However, this would result in removing the voxels on the ground level. We want to have functions on the lower level, and pathwise it wouldn't make sense as well to remove (almost) all voxels on the ground level. Besides that, in the growth script for our different rooms, the room types that want to have a lot of sun access, will automatically grow towards the voxels which have optimal sun access. 

### Threshold 
For our treshold value we chose 0.4. Every voxel that in more than 40% of the time it recieves sun, casts a shadow on the surrounding area, is removed. We chose this number based on an instinct, and because it gave us an interesting shape. We would have preferred to make another analysis of which of the context facades are receiving shadow, and make our selection based on a maximum percentage of the time that the context facades to recieve shadow, our teachers recommended to not do this because it would be to much computing for our hardware, and too big of a challenge for the limited time we have in this course. 

### Removing voxels
eindresultaat voxels removen

<iframe src="https://thumbs.gfycat.com/ValidImaginativeChihuahua-size_restricted.gif" style="width:150%; height:430px;" frameborder="0"></iframe>

<img src="../img/midterm/lattice.png" style="width:280px;">

## skyview

## noise

## greenery
<img src="../img/midterm/greenery.png" style="width:280px;">

## Shafts
<img src="../img/midterm/shafts.png" style="width:280px;">

## Corridors

## entrances
<img src="../img/midterm/entrances.png" style="width:280px;">


## Shortest path
Not sure if needed, but perhaps explain the approach? (for example taking an entire line instead of only one voxel as an entrance)

## KPI's to agent criteria 
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


## Configuring spaces to workable values
In order to implement the design criteria as mentioned before, those had to been converted to workable values. The values have been written for each space versus the criteria varying from 0 to 1. 0 indicates no connection, 1 indicates a strong connection. This has been applied for the following criteria: matrix based relations between spaces, sun access, entrance distance for public, housing, gym, parking and communal spaces, skyview, greenery and noise. 

For the space heights and space areas a different approach had to be made since those are hardcoded criteria coming from the Program of Requirements. Hence, those explained in the next paragraph. 

### Space heights to stencils Tim __> explain different stencil
explain why we are using different stencils 

### Space areas to voxel amounts
Based on the program of requirements, the required space sizes have been coverted to amount of necessary voxels to meet the area requirement and therefore fulfil it. This has been implemented in the script to maintain the desirded area per spaces, and has been used to limit the growth of the agents. 

Explain how script works.

### Definitive program table result (input for generative relations simulation)
The following table has been made based on the agent criterias, this has been used in the definitive script for generating the agent based design. 

<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRBfjaAFNv4mAaDcMxI9AJf91QjGnhEDCYvvPLZC6GWHoceZO_pG81HI14bg5hD9g/pubhtml?gid=1256579589&amp;single=true&amp;widget=true&amp;headers=false"style="width:150%; height:600px;"></iframe>

### The simulation

<iframe src="https://thumbs.gfycat.com/LittleAdmiredBufeo-size_restricted.gif" style="width:150%; height:400px;" frameborder="0"></iframe>