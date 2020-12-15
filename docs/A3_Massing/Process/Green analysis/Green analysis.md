# Green Analysis

In this analysis we wanted to create some attraction to green spaces in the direct neighborhood of the envelope. The eventual goal is to minimize the graph distance of a voxel which is in need of green space, and the green space its self. To accomplish this we used the notebook offered by Shervin Azadi.

### Park
The first green space we defined is the small park between the Almondestraat and the Schoterbosstraat. We did this by extracting the index number 422, which represents the park, from the distance matrix.

``` python
# select the corresponding row in the matrix
park_dist = dist_mtrx[422]

# find the maximum valid value
max_valid = np.ma.masked_invalid(park_dist).max()

# set the infinities to one more than the maximum valid values
park_dist[park_dist == np.inf] = max_valid + 1

```
![title](../img/Park.png)

### Raingarden
The same process we repeated for the second green space which is the Raingarden on the south east side of the envelope.

``` python
# select the corresponding row in the matrix
park_dist = dist_mtrx[506]

# find the maximum valid value
max_valid = np.ma.masked_invalid(park_dist).max()

# set the infinities to one more than the maximum valid values
park_dist[park_dist == np.inf] = max_valid + 1

```

### Hofbogen
For the third green space The Hofbogen, we adjusted the code a little bit since we wanted to select a complete row of voxels.

**1.3 Select Hofbogen Voxels**
``` python
p = pv.Plotter(notebook=True)

# initialize the selection lattice
base_lattice = avail_lattice * 0 - 1

# init base flat
base_flat = base_lattice.flatten().astype(int)

# Set the grid dimensions: shape + 1 because we want to inject our values on the CELL data
grid = pv.UniformGrid()
grid.dimensions = np.array(base_lattice.shape) + 1
# The bottom left corner of the data set
grid.origin = base_lattice.minbound - base_lattice.unit * 0.5
# These are the cell sizes along each axis
grid.spacing = base_lattice.unit 

# adding the boundingbox wireframe
p.add_mesh(grid.outline(), color="grey", label="Domain")

# adding the avilability lattice
init_avail_lattice.fast_vis(p)

# adding axes
p.add_axes()
p.show_bounds(grid="back", location="back", color="#aaaaaa")

def create_mesh(value):
    i = int(value)
    # init base flat
    base_copy = np.copy(base_lattice)
    base_copy = base_copy * 0 - 1
    base_copy[:,i,1] = 0
    base_new = base_copy
    # base_flat = base_lattice.flatten().astype(int)
    # base_flat = base_flat * 0 - 1
    # base_flat[i] = 0 
    # base_new = base_flat.reshape(base_lattice.shape)
    # Add the data values to the cell data
    grid.cell_arrays["Selection"] = base_copy.flatten(order="F").astype(int) # Flatten the array!
    # filtering the voxels
    threshed = grid.threshold([-0.1, 0.9])
    # adding the voxels
    p.add_mesh(threshed, name='sphere', show_edges=True, opacity=1.0, show_scalar_bar=False)

    return

p.add_slider_widget(create_mesh, [0, base_lattice.shape[1]], title='1D Index', value=0, event_type="always", style="classic", pointa=(0.1, 0.1), pointb=(0.9, 0.1))
p.show(use_ipyvtk=True)

```

**1.4 Construct Distance to Hogbogen Lattice**

``` python
base_lattice = avail_lattice * 0
# voxel selection
base_lattice[:,0,1] = 1
base_flat = base_lattice.flatten()
vox_interest = np.where(base_flat == 1)
print(vox_interest)
# print(dist_mtrx[[51,682]])
# [dist_mtrx[51], dist_mtrx[682]]
dist_interest = dist_mtrx[vox_interest]
```
**1.4 Construct Distance to Hogbogen Lattice**
``` python
# find the maximum valid value
max_valid = np.ma.masked_invalid(dist_interest).max()

# set the infinities to one more than the maximum valid values
dist_interest[dist_interest == np.inf] = max_valid + 1
```
``` python
min_dist = np.min(dist_interest, axis=0)
```

![title](../img/Hofbogen.png)

## Roof garden acces

Besides the three main green spaces in the near neighborhood of the envelope, there are more green space to which function could have a connection namely the roof gardens and the gardens inside of the building. These gardens don’t have a fixed location in the building and could potentially change constantly. The relation between a function and a roof garden is therefore called a dynamic relation. 
To accomplish this we altered the code given to us by Shervin Azadi a little bit. Within the notebook W+2_mcda_seed_allocation before the agent for loop we updated the dynamic field.

### Program.csv

space_name, space_id, ent_acc, sun_acc, lobby_acc, roof_garden_acc,  workshop_acc
lobby, 0, 1.0, 0.0, 1.0, 0.0, 0.0
roof_garden, 1, 0.0, 1.0, 0.0, 1.0, 0.0
workshop, 2, 0.5, 0.5, 0.0, 1, 1.0

<table><thead><tr class="header"><th>space_name</th><th>space_id</th><th>ent_acc</th><th>sun_acc</th><th>lobby_acc</th><th>lobby_acc</th><th>lobby_acc

</th></tr></thead><tbody><tr class="odd"><td>180mm x 240mm</td><td>(2 x 180) + (1 x 240) = 610</td><td>3:4</td><td>6</td><td>900x900 mm
</th></tr></thead><tbody><tr class="odd"><td></td><td></td><td></td><td></td><td>1800x1800 mm
</th></tr></thead><tbody><tr class="odd"><td></td><td></td><td></td><td></td><td>3600x3600 mm</td></tr><tr></tbody></table>

### 1.2 Running the simulation

  agn_acc_list = []
    # for each agent
    for a_id in range(agn_num):
        # find which voxel occupied by destination function (roof garden) 
        a_locs = agn_locs[a_id]
        # organize 3d indices vertically
        a_locs_array = np.array(a_locs).T
        # extract 1d index for all the locations of this agent
        a_locs_1d = np.ravel_multi_index(a_locs_array, avail_lattice.shape)

        # extract distance field to those voxels from the distance matrix 
        voxel_dists = dist_mtrx[a_locs_1d]

        # combine dist fields of all voxels of this agent
        combined_dists = voxel_dists.min(axis=0)
        max_valid = np.ma.masked_invalid(combined_dists).max()
        # set the infinities to one more than the maximum valid values
        combined_dists[combined_dists == np.inf] = max_valid + 1
        # mapping the values from (0, max) to (1, 0)
        mapped_dists = 1 - combined_dists / max_valid

        # constructing the lattice
        agn_acc_lattice = tg.to_lattice(mapped_dists.reshape(avail_lattice.shape), avail_lattice)

        agn_acc_list.append(agn_acc_lattice)

    # combine agn_dist fields with base env info
    env_info = env_info_base + agn_acc_list

### 1.2 Running the simulation

![title](../img/W+2_mcda_seed_allocation_atraction.PNG)

In this visualisation we see that the workshop agent is...