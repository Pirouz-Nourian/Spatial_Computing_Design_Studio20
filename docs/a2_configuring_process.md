# Process

#### REL chart
With a set program of requirements, we specified each space’s realtive closeness preferences and certain prefered spatial
qualities.

![REL](../img/REL v2.png)

#### Weights and preferences hierarchy
<u>Growth hierarchy</u> 
<br>According to our design strategy with privacy gradients and the decision
to cluster functions around hubs, a hierachy of spaces arises. When the
growth algorithm seeds and grows spaces, the matrix is used to look up
which spaces should grow or “follow” which spaces. However, not every
space finds it important to follow another. Some spaces are dependant on
the location of the hubs but the hubs themselves are not affected by the
spaces following them. This relationship indicated in the matrix by lack
of symmetry across the diagonal.

<p>The following bubble diagram illustrates the meaning of this assymetry along the diagonal in the REL chart. For example, co-cooking area and community garden are connected in the metro diagram , this is also reflected in the REL chart. However, because the co-cooking area indicates that it would need to grow toards the garden, and garden does not iindicate any preference for growing towards the co-cooking, a hieracy arises : co-cokking follows garden, not the other way around.</p>
![Hierarchy](../img/hierarchy.png)

[Hierarchy](../img/hierarchy.png)
<a href="../pdf/hierarchy.pdf" class="image fit"><img src="images/marr_pic.jpg" alt=""></a>

#### Voxel size 
Having set design goals and user perspectives, we chose a voxel size that we consider multifunctional enough to form
spaces with different functions. This voxel size became a base for the voxel cloud used in all following computations.

- Why this size?
    -  Height and width are the same, therefore it is a regular cube
    - A staircase fits inside a single voxel from floor to floor
    - A third of the voxel size is a pleasant width for a small corridor (1080 mm)


- Building regulations:
    - Width stairs: minimum is 800 mm
    - Riser: minimum is 180 mm
    - Tread width: minimum is 220 mm
    - Head room: minimum is 2300 mm

![stair_dimensions](../img/stair_dimensions.PNG)
![stair_3D](../img/stairs_3D.PNG)

#### Notebook Flowchart 
The computation process is reflected in the flowchart. 

For optimization purposes, we used 3 lattices with different voxel sizes. The resulting data was always interpolated for
our main lattice with voxel size 3240x3240.

![Flowchart_notebooks](../img/Flowchart_notebooks.png)

[Flowchart_notebooks](../img/Flowchart_notebooks.png)
<a href="../pdf/flowchart_notebooks.pdf" class="image fit"><img src="images/marr_pic.jpg" alt=""></a>

#### Computation Flowchart

### Static data creation 
#### Solar envelope
>Create an envelope based on solar blockage

The created envelope will be used as the base availability lattice on which all other calculations for static data and the growing of the agends are built upon. 

<img src="../img/Solar envelope in lattice.png" width="500"> 

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

<img src="../img/Solar icons3.png" width="500"> 

#### Solar accessibility
>Ensure spaces get enough sunlight

This data is used for the growing algorithm by certain agents that prefer
a high solar accessibility, for instance: the residential quarters and study
spaces.

<img src="../img/Solar accesibility in lattice.png" width="500"> 

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

<img src="../img/Solar icons.png" width="500"> 

#### Sky view factor
>Ensure functions are able to see enough of the sky
This data is used for the growing algorithm by certain agents that prefer
a high sky view factor, for instance: the office spaces and garden.

<img src="../img/Sky view factor in lattice.png" width="500"> 

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

<img src="../img/Solar icons2.png" width="500"> 


#### Floor level preference 
>Set floor levels for agents
This data is used for the growing algorithm by certain agents that prefer
a proximity to certain floors, for instance: the hub and garden prefer to
be on the ground floor.

<img src="../img/Floor closeness in lattice.png" width="500"> 

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

<img src="../img/Ground floor preference.png" width="500"> 


#### Closeness to the facade (high resolution)
>Ensure access to the facade

This is another parameter to optimize the placement of spaces that need direct daylight or adjacency to the street. 

<img src="../img/closeness to facade.png" width="500"> 

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

<img src="../img/Closeness to facade_icon.png" width="300"> 

#### Closeness to a specific facade (high resolution)

>Orient for site accessability on a specific side

Some spaces and entrances require access to a specific facade based on traffic routes and greenery on the site. While they need to be dajcent to the facade, they do not need to be fixed in a specific place. The data field is used to create axes on each facade, to let the program choose the best location on it.

<img src="../img/closeness to facade.png" width="500"> 

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Availability lattice, custom stencil</td></tr><tr class="even"><td>Output</td><td><p>Specific facade closeness lattice</p></td></tr><tr class="odd"><td>Code</td><td><p><li>efine stencil where only one voxel represents the neighborhood and this neighbor is oriented in the direction of the desired facade</li>
<li>Apply the stencil to the voxel envelope.</li>
<li>Find the number of neighbors for each voxel in the lattice.</li></p>

<p><li>Create a condition for specific facade boundary voxels, where the number of neighbors is < 1</li>
<li>Check envelope with the condition, create a new envelope with only specific facade boundary voxels</li>
<li>Create an adjacency matrix full of 0’s</li>
<li><strong>For</strong> each available voxel in the envelope:<br>Fill in its neighbor’s ID’s in the adjacency matrix</li></p>

<p><li>Compute distances from all voxels to all voxels.</li>
<li>Select the distances between boundary voxels and all other voxels.</li>
<li>Find the minimum distance for all voxels to any specific facade boundary voxel</li>
<li>Add the minimum distance to the corresponding voxel value field.</li>
<li>Map the field distance values from 0 - 1, where 0 is the furthest distance and 1 is the closest to the specific facade</li></p>

</td></tr></tbody></table>

<img src="../img/Closeness to NE facade_icon.png" width="300"> 

#### Quietness from street noise 
>Orient according to traffic noise fall-off

The two main streets around the plot produce significant traffic noise. According to European Environment Agency, these streets produce 50 and 70db of noise. By mapping the noise fall-off from the street, the growth algorithm can take into account the spaces where quietness is especially preferable, such as the library.

<img src="../img/Quietness from street noise_2.png" width="500"> 

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Avalability lattice, meshes representing the streets with different noise levels</td></tr><tr class="even"><td>Output</td><td><p>Quietness from street noise lattice</p></td></tr><tr class="odd"><td>Code</td><td><p>Load several meshes representing streets with different noise levels.

<br>Get all voxel centers as points.</p>
Get all voxel 3d IDs
Initialize a distance lattice full of 0s

<p><strong>For</strong> each voxel:
<br> Calculate the smallest euclidian distance from voxel center to the first street mesh, using trimesh.proximity.
<br>Write the distance value in the distance lattice for the current voxel
<br>Compute the noise dB’s for each voxel in a lattice from the distance lattice using noise formula and noise level for first street</p>

<p>Initialize a summing lattice 
<br>Convert the logarithmic scale into a decimal scale, sum it to the summing lattice<p>

<p>Repeat the for loop for the second street
<br>Convert the logarithmic scale into a decimal scale, sum it to the summing lattice<p>

<p>Compute the aggregated noise lattice by converting the summing lattice back to logarithmic scale<p>

<p>Map the inverse field of noise values to a field of quietness values from 0 - 1, where 0 is the least quiet value and 1 is the quietest value.<p>

</td></tr></tbody></table>

<img src="../img/Quietness from street noise3.png width="300"> 

#### Entrance closeness
>Ensure access to an entrance

To make sure the agents who need to be close to an entrace can grow in that direction, an entrance accessibility lattice must be created.

<img src="../img/entrance access.png" width="500"> 

<table><thead><tr class="header"><th>Pseudocode</th><th></th></tr></thead><tbody><tr class="odd"><td>Input</td><td>Voxelized envelope, entrance locations based on street accessibility</td></tr><tr class="even"><td>Output</td><td><p>Entrance Lattice</p></td></tr><tr class="odd"><td>Code</td><td>Set the entrance voxels based on the entrance locations.
<br>For each non-entrance voxel: 
<br>Find the <strong>closest entrance voxel</strong>
<br>Link the distance to that entrance to that voxel
<br>Convert the distance values into <strong>values between 0 and 1</strong>
<br>Construct the entrance lattice. 
</td></tr></tbody></table>

<img src="../img/Accessibility_coloured.png" width="500"> 

