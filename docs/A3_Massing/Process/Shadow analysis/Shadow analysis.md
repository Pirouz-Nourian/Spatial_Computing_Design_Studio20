# Shadow analysis 

After removing the voxels with too little sun access, we have analysed the shadow casting of the new envelope. 

A part of the building envelope would provide shade for the surroundings. Because the building has a high height and is close to other buildings, we have removed the voxels that have a major influence on the shadow on certain parts of the immediate context.

Just like in the sun analysis, we compute many intersections. This time, however, we are computing them from the sky towards the voxels. In the code, the directions of the sun vectors are opposite to those in the sun analysis.

``` python
# constructing the sun direction from the sun vectors in a numpy array
sun_dirs = np.array(sun_vectors)

```

Because we want to keep the park behind the building, we also think it is important that this remains a nice green place. That is why we have taken this into account in our analysis. we have changed the immediate context In Rhino and placed a building with a height of 1.75 meters (human height) on the park. You can clearly see the difference in the colors, and so in the shadow casting of the envelope. 

**Analysis without taking the park into account**
![Title](../img/shadow_no_park.jpg)

**With taking the park into account**
![Title](../img/shadow1.jpg)

We have removed the voxels that have over 40 percent of intersections and cause most of the shadow on the surroundings. We continue the next analysis with the newly created envelope.

**New envelope**
![Title](../img/shadow2.jpg)