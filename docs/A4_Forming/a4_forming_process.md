# **Polygonization**

## **Perfect shape grasshopper script**
Everything thus far in this process has been modular and efficient. When it came to polygonization, which consists of a lot of manual labour, it did not feel like it was conformed to the ideology of this course. That’s why we decided to make this process more modular and efficient. This originated the beginning of the ‘’perfect shape’’ grasshopper script. The script takes a perfect shape (cube) and cuts it into 152 pieces. It then sorts these pieces based on a distance from a point to the centroid of a piece. With the usage of slider,  a selection can be made for a specific piece and a specific name. After this a directory is selected to save all the files to, by pressing a button. There is also the option to bake every object to their correct layer by pressing a button, as a backup check,  as well as the option and delete all objects.  
For altering the building, the desired geometry should be added onto the “perfect shape”, creating a “new geometry”. This shaped will then also be cut into 152 pieces, and sort based on a distance from the same point to the centroid of the pieces. The “perfect shape” pieces will form a cutting box by which the “new geometry” pieces are cut up. In theory the “new geometry” pieces should be in the same position as the “perfect shape” bounding box for a specific number. For this “new geometry” the only change that must be made to the rest of the script,  is to select a new directory to save the pieces to. This makes the process of generating multiple tilesets extremely efficient. 
This is the theory behind the script. Unfortunately, in practice, there are some challenges to work around. Firstly the “new geometry” should be somewhat cube like. This means that although the added geometry can be asymmetrical, adding/extruding the geometry on an edge can only be done in a symmetrical way, otherwise the box will be cut up into more than 152 pieces. 
The way the 152 pieces are sorted is done by a distance from the piece towards a point. By altering the geometry, these distances do not always stay completely the same though. In 5 “new geometries” tests, in 2 to 5 pieces out of 152 pieces, the order had changed.  This can be solved by manually changing the slider to the correct position. In the 5 test runs the change of the slider was a maximum of 5 up or down.

## **Pseudocode Perfect shape grasshopper script**
*you can zoom in on the flowchart with the bar below that appears when hovering over the diagram*
<iframe frameborder="0" style="width:100%;height:193px;" src="https://viewer.diagrams.net/?highlight=0000ff&edit=_blank&layers=1&nav=1&title=Grasshopperscript.drawio#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1CGnwSmRQPMmgTsY0JmWY-r1alWDR8-cj%26export%3Ddownload"></iframe>

## symmetry = 152 pieces
<img src="https://cdn.discordapp.com/attachments/775754717346791494/803293065761783848/152_pieces.jpg">

## assymetry = more pieces 
<img src="https://cdn.discordapp.com/attachments/775754717346791494/803293112804835328/160_pieces.jpg">



## **Polygonization script**
Out of the possible 24 subtiles, it is only necessary to use 10 of them, the rest could be used for exceptions or when shapes are more complicated. In the 10 subtiles, there are 2 subtiles missing. There is no distinction between floor and roof and no distinction between roof corner and floor corner. As of now if when adding a railing to the roof of the building, this railing will also appear on the bottom of the building. 


<img src="https://cdn.discordapp.com/attachments/775754717346791494/803358655361187871/different_facade_pieces.jpg">
*different facade types*


<img src="https://cdn.discordapp.com/attachments/775754717346791494/803363901935845486/polygonisatie_model.JPG">
*polygonization model*



In the polygonization script itself, there are also still some challenges. Loading in textures and giving it to objects without making a mesh out of it works fine. But when the object file is made a mesh, it loses its texture coordinates. Our group managed to extract texture coordinates from an OBJ and give them to the object that is made a mesh in the jupyter notebook, but the link between textures + texture coordinates and the generated tiles from the subset is missing when exporting the final results or running everything through the poligonization script. Perhaps in the future another group can solve and implement this. 
To give the building a varying appearance, instead of having the same look everywhere throughout the building, stencils were implemented. Due to our floors being 2 voxels high, this brought a challenge. To vary a floor from down to top, it was necessary to design a tileset for the top as well as the bottom of the floor. While selecting these singular voxels as a stencil, another problem appeared. When separating 1 voxel from a bigger number of voxels, it will change from being a wall, to being a corner. The design of the singular voxel will have to be placed on the corner now. Since there is no distinction in the subtiles between a top or a bottom corner, this singular voxel will be symmetrical in both directions. 

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803300933651791902/all_tiles_hoofdstuk_4_polygonisatie.jpg">

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803300928626753566/used_tiles_hoofdstuk_4_polygonisatie.jpg">

<img src="https://cdn.discordapp.com/attachments/775754717346791494/803627397932515333/tiles_changing_hoofdstuk_4_polygonisatie.jpg">



This process is yet to be perfect, but the solutions seems to be not to far away. If the linkage between the textures+ texture coordinates and the generated tiles, the 2 extra subtiles are added, the usage of stencils is improved, this should be a good way to design the exterior of the building in a quick and modular way. 