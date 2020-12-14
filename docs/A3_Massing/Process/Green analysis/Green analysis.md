# Green Analysis

In this analysis we wanted to create some atraction to green spaces in the direct neighborhood of the envelope. The eventual goal is to minimize the graph distance of a voxel which is in need of green space, and the green space its self. To accomplish this we used the notebook offered by Shervin Azadi.

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

