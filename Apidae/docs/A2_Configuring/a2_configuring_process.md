# Process of configuring
## Determining voxelsize
In order to determine the voxelsize that our growth simulation will work with, a case study has been done. The voxel size has been based on stair sizing because ----------------------.
### Starting points
*Stairs*
Free space residences +/- 270 cm 
270 cm= 1 height?

270/6= 45
270/9= 30
270/5= 54
270/3= 90
270/2= 135

Stair dimensions from ‘’menselijke maat’’ :
stair angle between 30 and 41 degrees
Stair width for moving in 2 directions, minimum 120 cm 
rise 14cm advised, minimum of 8cm 
tread with  minimum 25 cm 
38 degrees ideal size for stairs?

*Bouwbesluit* 
minimum tread with 22cm 
maximum rise 0,188cm 
[source](https://www.bouwbesluitonline.nl/docs/wet/bb2012/hfd2/afd2-5/par2-5-1/art2-33)

*Corridor*
Main hallways: at least 1,5 metre in width 
support hallways: at least 1,2 metre in width 
[source](https://rijksoverheid.bouwbesluit.com/Inhoud/docs/wet/bb2012/hfd4/afd4-4/art4-23)

#### Stair sizes 

Option | Rise | Tread width| Length stairs| Width staircase
---------|----------|---------|---------|---------
 Option 1| Rise= 18cm (3x6) 270/18 = 15 steps | 30cm (5x6)| 15x30 = 450cm| 120cm (20x6)
 Option 2 | 18cm (2x9) 270/18 = 15 steps | 27cm (3x9)| 15x27= 405| 126 cm(14x9) 
 Option 3| 18cm (3x6): 306/18 = 17 steps | 30cm (5x6)| 17x30 = 510cm| 120cm (20x6)
 Option 4| 18cm (2x9) 306/18 = 17 steps | 27cm (3x9)| 18x27= 486cm| 126 cm(14x9) 
 Option 5| 18cm (3x6) 306/18 = 17 steps | 24cm (4x6)| 17x24 = 408cm| 120cm (20x6)

For further research we will only use option 3,4 & 5. Since after discussion we decided to enlarge the floor height.

### First approach
#### Y dimension
Option | Based on stair option | x/y/z sizes| Comment
---------|----------|---------|---------
 Option 1| Option 3  | 120 / 510 / 306 | 510 is quite a big size, but there’s no division possible which leads to an integer multitude of 6. So this will not be a good voxel size. 
 Option 2 | Option 4 | 126 / 486 / 306 -> 126 / 162 / 306 | To make it fitting for human dimensions let’s divide the the y value by 3. The stairs which we calculated earlier will now be 3 voxels. 
 Option 3| Option 5 | 120 / 408 / 306 -> 120 / 102 / 306| The previous voxels will be sufficient for a building with the same height everywhere, but the z value is too big to differ in height through the building.


#### Z dimension
Option | Size | Ratio
---------|----------|---------
 Option 1: based on y dimension option 2 and stairs option 4 divisible by 9| 126/162/306 divide the z value with 2-> 126/162/153 the staircase will now be 3x2 voxels | ratio 1 to 1.3
 Option 2: based on y dimension option 3 and stairs option 5 divisible by 6 | 120/204/102 divide the z value with 3->The staircase will now be 2x3 voxels | ratio 1 to 1.7
 Option 3: based on y dimension option 3 and stairs option 5 divisible by 6| 120/102/306 divide the z value with 3-> the staircase will now be 4x3  | ratio 1 to 0,85


### Definitive approach
Since a different x and y value affected the modularity of the building and spaces were otherwise forced into one particular direction. We have decided after consultation with our tutors to take the same dimension for the x and y value of the voxel.

For the definitive approach towards the voxel size we will use the option for stair dimensions we previously used, since the size was thoroughly thought about in the first approach. Apart from this, as a project group we decided it would be a pragmatic approach to decide our tartan grid based on the thickness of the support structure the building will be made of, to make it fit into the tartan grid.

For this, we made a rough estimation of the area of our building through the Program of Requirements, and how this relates to the available area for our building (obtained through measurements in ArcGIS), we came to a rough estimate of 3 stories, if the available area is maximised, in other words: if all the spaces are summed up. 

Since we have to take into account the varying height in the building, we decided to look at surrounding buildings in the area and the width of the streets and decided upon a maximum of 10 stories high. We estimated that we would need a SHS-HF-300 profile for this height. We chose this profile because it has the same x as y value and based our voxel grid on this. 

Although these dimensions (300x300) are workable, the size is really small and not really algorithm friendly because the building would consist of too many voxels because of the building site's size. 

So let’s take the 300x300 voxels as our meso voxels for the tartan grid. 

If we go back to the stair dimensions which were previously researched and have chosen the following can be said: 
*Rise= 18cm (3x6), 306/18 = 17 steps, tread width= 30cm (5x6), length stairs= 17x30 = 510cm, width staircase= 120cm (20x6)* 

With a stair width of 120cm we could make our voxel size based on 120cm+ meso voxel. But to be on the safe side, and taken into account the railings of the stairs it’s better to take an extra meso voxel for the voxel size. So the voxel size will be 150cm + mesovoxel = 180cm. 

The tartan grid is the place where the structural profiles or the walls can come. 

<img src="https://cdn.discordapp.com/attachments/775754717346791494/782968691113197588/voxel_with_dimensions.jpg">

<img src="https://cdn.discordapp.com/attachments/775754717346791494/782968683973705728/3d_representation_of_grid.jpg">

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803200924239659048/TRAP_2.jpg" alt="collective" style="width:300px;"> 

<img src="https://media.discordapp.net/attachments/775754717346791494/803196044015304734/trap.jpg?width=765&height=541" alt="collective" style="width:300px;"> 
<br>
</br>

## Space criteria 
In order to meet the desired quality as stated in A1_Planning and given through a Program of Requirements, criteria have been set for spaces.
### Criteria to spaces 
The following process has been implemented in order to moderate the input that is necessary in order to work with later on. 
<iframe frameborder="0" style="width:100%;height:800px;" src="https://viewer.diagrams.net/?highlight=000000&edit=_blank&layers=1&nav=1&title=process%20flowchart#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D19GJQO6n8Q_KqAivSHi9RTUaXpw6ekvTw%26export%3Ddownload"></iframe>


### Table/Matrix of relations
The matrix table with connectivity between spaces has been made through the bubble diagram:
<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRqEuxSvQ-N81oncdb5fiRDmU_GB6HNdrPEpVIXcBHApoIXwiqt6-PNKeYbD71hgQ/pubhtml?gid=1256579589&single=true" style="width:150%; height:600px;" frameborder="0">
</iframe>

### Program of Requirements with integration of the voxelsize and layers of spaces
The following definitive Program of Requirements has been formed through the chosen voxelsize and the given Program of Requirements beforehand. 
<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSmJnATJOBL1HdyT0rUhv993Z_nf_2vW9hOOsqYQvNYhO_EVGhiPI_OFTCAWAi-FQ/pubhtml?gid=1171988855&single=true" style="width:150%; height:600px;" frameborder="0">
</iframe>

## Voxelizing the lattice
As the final voxelsize is determined, the location can be voxelized. The given 'compulsory envelope' is being filled with voxels sized 1.8m * 1.8m * 1.8m, which results in the full lattice. 

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803578469933252658/mesh.png">

*compulsory envelope*

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803578468969086976/ful_lowres_lattice.png">

*voxelized envelope* 



Because of the relatively small voxelsize, next to this a second lattice is being generated, with voxelsizes of 15m *15m *15m. This 'low-resolution' lattice could later be used to make the more heavy calculations, and by interpolation of these values a good estimate of the 'high-resolution' voxels is being generated.  

<img src="https://cdn.discordapp.com/attachments/784009094474366977/803578466447786074/ful_lattice.png">

*low-resolution voxelized envelope*
