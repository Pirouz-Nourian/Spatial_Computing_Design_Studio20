# Process

#### Flexibility
<img src="../img/Sustainability.PNG" width="700">


#### Modularity
Keeping with our main goal of creating a sustainable and modular system, we designed individual voxels as spatial modules that can be interconnected to create spaces with different programs. We aimed for a design which chould be easily adapted if and when the buiding had to change, meaning modules could be added or removed or their function could be easily changed.
<img src="../img/modularity.PNG" width="700">

#### Modular system
The system consists of 4 parts: the permanent structure and the adaptable facade and infill.
<br><img src="../img/Modular system.PNG" width="400">

#### Construction
<img src="../img/exploded axo_1.PNG" width="800">

##### Wooden joint
<br><img src="../img/joint.png" width="500">

##### Structure and Infill
<br><img src="../img/interior wall_1.png" width="700">

#### Modular interior tiles

<br><img src="../img/Tiles icon.jpg" width="500">

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>envelope lattice, several custom tile sets</td></tr><tr class="even"><td>Output</td><td><p>floorplan</p></td></tr><tr class="odd"><td>Code</td><td><p>The tiles are created with an underlying system similar to that often seen
in tile based board games. The square voxel is subdivided in three parts
along each edge. One of these subdivisions is equal to the width of a
small corridor or door.</p>

<p>These three parts are then labeled as either a door, wall or open space.
By combining different tiles that match the corresponding edge types,
different spaces can be created from simple tiles.</p>

<p>By then also listing the function type of each tile, such as the entrance
or kitchen (E & K), limitations and recommendations could be added to
the code which tiles can connect to which tiles. Due to time limitations
this is something that we have not developed yet, but could be an interesting concept for peers following this course over the following years.</p>
</td></tr></tbody></table>


#### Facade tiles 
##### The tiling system
The facade tiles are designed to tile vertically and create patterns. Whenever new modules would be added to the structure, new tiles from different tile set could be used for each new addition. This way the building facade would enrich over time. It is another way to emphasize the building’s adaptability and long lifecycle.

<img src="../img/computing-diagram_forming_time-development.png" width="600">

##### Tile creation
<img src="../img/Tile creation.PNG" width="700">

##### Poligonization
<img src="../img/computing-diagram_forming.png" width="600">

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>envelope lattice, several custom tile sets</td></tr><tr class="even"><td>Output</td><td><p>an .obj of a tiled facade</p></td></tr><tr class="odd"><td>Code</td><td>Load envelope lattice
<br>Remove interior voxels by creating a Von Neumann stencil to detect
neighbours 
<br>Remove voxels whose neighbour count is <=5

<p>Extract cube lattice from envelope lattice
<br>Tile the envelope lattice with tileset1 
<br>Select vertical slices in the lattice whose tiles to replace 
<br>Tile selected slices with tileset 2</p>

<p>Export tiled facades</p>

</td></tr></tbody></table>

##### Tiles voxelized envelope
<img src="../img/Tiled voxelized envelope.PNG" width="700">
This voxelized envelope is from a previous iteration from the growth algorithm.
