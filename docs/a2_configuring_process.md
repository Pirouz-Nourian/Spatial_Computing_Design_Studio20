# Process

> here we are testing collaboration

> Here you should include the process and product of your 2nd activity: **Configuring**

<table><thead><tr class="header"><th>Title</th><th>Configuring (process): Circulation Manifold (product)</th></tr></thead><tbody><tr class="odd"><td>Objective</td><td>Formulate a spatial (topological) concept, design a modular circulation manifold on a pixel/voxel grid.</td></tr><tr class="even"><td>Procedure</td><td><p>Construct a voxelated model of the site with a maximum height of 100 meters. Orient the voxel grid to a global coordinate system (e.g. geographical North-East-West-South). Size the voxels carefully based on the modular height of steps and the length of stair flights and ramps so that they fit in X/Y directions into multiple pixels. Choose the Z size of voxels according to step risers and choose the same size for X and Y as a whole multiple of step threads.</p><p>There are three types of spaces in terms of pedestrian movement in buildings, metaphorically speaking, spaces to <strong>walk</strong> through (e.g. corridors, ramps, and stairs), spaces to <strong>stand</strong> on (e.g. platforms connecting doors to corridors and stairs) and spaces to <strong>sit</strong> on (functional rooms/spaces). Construct a simplified mesh model of all bridges (corridors, ramps, stairs) connected by standing platforms in a modular grid of voxels/pixels. Take into account the free-height necessary for all spaces and pack them into the bounding volume of the building. For every functional space, leave a single pixel as a standing platform and colour it with the corresponding colour.</p></td></tr></tbody></table>


#### Solar envelope

The created envelope will be used as the base availability lattice on which all other calculations for static data and the growing of the agends are built upon. 

![Create an envelope based on solar blockage](../img/Solar envelope in lattice.png)

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Voxelized envelope, context mesh</td></tr><tr class="even"><td>Output</td><td><p>Solar envelope</p></td></tr><tr class="odd"><td>Code</td><td><p>Create a list of all vectors pointing towards the sun locations over the year.</p>

<p><strong>For</strong> all voxels inside of the envelope:
<br><Tab>Cast a ray from the list of sun vectors from the voxel centroid</Tab>
<br><strong>If</strong> the ray intersects with a mesh:
<br>Ignore the ray and continue the loop
<br><strong>Else:</strong>
 <br>Check if the reversed ray intersects with a mesh
 <br><strong>If</strong> this new ray intersects with a mesh:
 <br>Register the intersection for the voxel to a new list
 <br><strong>Else:</strong>
 Register a non intersection to the list</p>

<p><strong>For</strong> each voxel inside the envelope:
<br>Map the amount of intersections in a range between 0 and 1, where
0 means blocking a lot of light for neighbouring buildings and 1 not
blocking any light.</p>

<p>Set a limit to how much light the voxels are allowed to block and create
a new lattice with either True or False values, depending on the amount
of light blocked.</p>

Export this lattice as the new availability lattice.</p></td></tr></tbody></table>

![Solar icons3](../img/Solar icons3.png)

#### Solar accessibility
This data is used for the growing algorithm by certain agents that prefer
a high solar accessibility, for instance: the residential quarters and study
spaces.

![Solar accesibility in lattice](../img/Solar accesibility in lattice.png)

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Solar envelope, context mesh</td></tr><tr class="even"><td>Output</td><td><p>Solar accesibility lattice
</p></td></tr><tr class="odd"><td>Code</td><td><p>Create a list of all vectors pointing towards the sun locations over the year.</p>

<p><strong>For</strong> all voxels inside of the envelope:
<br><Tab>Cast a ray from the list of sun vectors from the voxel centroid</Tab>
<br><strong>If</strong> the ray intersects with a mesh:
<br>Append an intersection to a new list
<br><strong>Else:</strong>
 <br>Append a non intersection to the list

<p><strong>For</strong> each voxel inside the envelope:
<br>Map the amount of intersections in a range between 0 and 1,
where 1 means receiving the most of light and 0 receiving the
least amount of light.</p>

<p>Export the newly created lattice that lists the values of solar accessibility
in a range from 0 to 1.</p></td></tr></tbody></table>

![Solar icons](../img/Solar icons.png)

#### Sky view factor

This data is used for the growing algorithm by certain agents that prefer
a high sky view factor, for instance: the office spaces and garden.

![Sky view factor in lattice](../img/Sky view factor in lattice.png)

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Solar envelope, context mesh, dome mesh</td></tr><tr class="even"><td>Output</td><td><p>Sky view factor lattice</p></td></tr><tr class="odd"><td>Code</td><td><p>Instead of creating a list of vectors pointing towards the sun locations over the year, append the normals of a dome mesh to a list, created to
map the sky in equal proportions. </p>

<p><strong>For</strong> all voxels inside of the envelope:
<br>Cast a ray from the list of sun vectors from the voxel centroid
<br><strong>If</strong> the ray intersects with a mesh:
<br>Append an intersection to a new list
<br><strong>Else:</strong>
<br>Append a non intersection to the list

<p><strong>For</strong> each voxel inside the envelope:
<br>Map the amount of intersections in a range between 0 and 1, where
1 has the least intersections, which means having a high sky view
factor and 0 the opposite.</p>

<p>Export the newly created lattice that lists the values of the sky view factor
in a range from 0 to 1.</p></td></tr></tbody></table>

![Solar icons2](../img/Solar icons2.png)

#### Floor level preference 

This data is used for the growing algorithm by certain agents that prefer
a proximity to certain floors, for instance: the hub and garden prefer to
be on the ground floor.

![Floor closeness in lattice](../img/Floor closeness in lattice.png)

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Solar envelope</td></tr><tr class="even"><td>Output</td><td><p>Floor level preference</p></td></tr><tr class="odd"><td>Code</td><td><ul><li>Create a list of entries based on the height of the imported lattice</li>
<li>Create a matrix that maps the neighbouring entries as if connected from
bottom to top</li>
<li>Select an entry as you would select a floor level (in the visualization it’s 0)</li>
<li>Calculate the distance from that entry to every other one</li>
<li>Map the values from 0 to 1, where 1 is the entry itself and 0 for the entry
that is the furthest from the selected one. Then append them from
bottom to top to in a one dimensional array</li>
<li>Map this array along the z-axis of the entire imported lattice</li>
<li>Multiply this newly created lattice with the solar envelope to set all
unoccupied voxels to 0 and export it</li></ul></td></tr></tbody></table>

![Ground floor preference](../img/Ground floor preference.png)

#### Closeness to the facade
This is another parameter to optimize the placement of spaces that need direct daylight or adjacency to the street. 

![closeness to facade](../img/closeness to facade.png)

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Availability lattice, custom stencil</td></tr><tr class="even"><td>Output</td><td><p>Facade closeness lattice</p></td></tr><tr class="odd"><td>Code</td><td><p><li>Define stencil as Von Neumann neighborhood with top and bottom neighbors removed.</li>
<li>Apply the stencil to the voxel envelope.</li>
<li>Find the number of neighbors for each voxel in the lattice.</li></p>

<p><li>Create a condition for boundary voxels, where the number of neighbors is < 4</li>
<li>Check envelope with the condition, create a new envelope with only boundary voxels</li>
<li>Create an adjacency matrix full of 0’s</li>
<li><strong>For</strong> each available voxel in the envelope:<br>Fill in its neighbor’s ID’s in the adjacency matrix</li></p>

<p><li>Compute distances from all voxels to all voxels.</li>
<li>Select the distances between boundary voxels and all other voxels.</li>
<li>Find the minimum distance for all voxels to any boundary voxel.</li>
<li>Add the minimum distance to the corresponding voxel value field.</li>
<li>Map the field distance values from 0 - 1, where 0 is the furthest distance and 1 is the closest</li></p>

</td></tr></tbody></table>

![Closeness to facade_icon](../img/Closeness to facade_icon.png)

#### Quietness from street noise 
The two main streets around the plot produce significant traffic noise. According to European Environment Agency, these streets produce 50 and 70db of noise. By mapping the noise fall-off from the street, the growth algorithm can take into account the spaces where quietness is especially preferable, such as the library.

![Quietness from street noise](../img/Quietness from street noise.png)

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Avalability lattice, meshes representing the streets with different noise levels</td></tr><tr class="even"><td>Output</td><td><p>Quietness from street noise lattice</p></td></tr><tr class="odd"><td>Code</td><td><p>Load several meshes representing streets with different noise levels.
<br>Get all voxel centers as points.</p>

<p><strong>For</strong> each voxel:
<br>Calculate the smallest euclidian distance from voxel center to the first street mesh, using trimesh.proximity.
<br>Using the inverse square law, calculate noise values from the acquired distance and data of level of noise on the street. 
<br>Add the noise value to the corresponding voxel in the value field.</p>


<p>Map the inverse field of noise values to a field of quietness values from 0 - 1, where 0 is the least quiet value and 1 is the quietest value.</p>

<p>Repeat quietness value field construction for the second street.
<br>Combine the quietness value fields by choosing the lowest quietness values for each point in the field.</p>

</td></tr></tbody></table>

![Closeness to NE facade_icon](../img/Closeness to NE facade_icon.png)

#### Entrance accessibility
To make sure the agents who need to be close to an entrace can grow in that direction, an entrance accessibility lattice must be created.

![entrance access](../img/entrance access.png)

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Voxelized envelope, entrance locations based on street accessibility</td></tr><tr class="even"><td>Output</td><td><p>Entrance Lattice</p></td></tr><tr class="odd"><td>Code</td><td>Set the entrance voxels based on the entrance locations.
<br>For each non-entrance voxel: 
<br>Find the <strong>closest entrance voxel</strong>
<br>Link the distance to that entrance to that voxel
<br>Convert the distance values into <strong>values between 0 and 1</strong>
<br>Construct the entrance lattice. 
</td></tr></tbody></table>

![Quietness from street noise (2)](../img/Quietness from street noise (2).png)
