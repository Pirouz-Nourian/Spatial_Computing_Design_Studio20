<center> <img src="https://cdn.discordapp.com/attachments/784009094474366977/803614295744053258/unknown.png" alt="collective" style="width800px;"> </center>

Apidae is the name of a large  bee family. Bees have a mutualistic relationship with their context, live in communal hives with lots of social interaction but also have individual specific tasks related to their age. Beehives are formed in an instinctively optimized way, by creating the most efficient routes, adapting to wind orientation and other factors. 
Just like the bees, the building has a mutualistic relationship with its context, as the context is to benefit from the placement of the building and the building is a response to the context. The building is communal with the possibility of lots of social interaction, but also the possibility to seclude from the community. The people in our building help each other with specific tasks related to their age. Young people can help elderly with grocery shopping, and the elderly can help the young people with life advice. Just like the beehive, our building is formed in an optimized way. Other than the beehive, in this case it’s done with the usage of scripts.


This is a project repository of Minor Spatial Computing 2020-2021, project group 1. The mid-term and Final submission's process, final products and underlying code/input can be found in this repository. 
________________________________________________

## **_The design challenge_**

We as Project Apidae are requested to design a housing complex incorporating severalcommunal/public facilities for a cooperative live-work-playassociation at location "Rotterdam, The block between Vijverhofstraat, Zomerhofstraat, Schoterbosstraat, and Teilingerstraat". 

The requested housing complex will be the new home of garduate students, young professionals and elderly who are in need of assisted living. The complex has to provide communal and public facilities to ensure collectivity. 

### **_Starting point_**
```html
The following program of requirements has been given:

Housing:

* Student Housing 80 units
* Assisted Living 30 units
* Starter Housing 100 units
* testing spaces


Communal spaces:

* Underground Parking (minimum of 0.5 parking lots per
* apartment)
* Vegetation (minimum 30% of the plot)
* Workshops/Fab-Labs/Co-working Space and Start-up Offices
* Library + Cinematheque + Café/Pub + (pinball) Arcade
* Co-cooking/Restaurant
* Community Centre
* Shop (grocery, tools and crafts)
* (electricity producing/odourless /geek-friendly) Gym


Also, the following design goals have been given:

* Maximum Multi-scale Modularity (Qualitative)
* Excellent Ergonomics (Qualitative)
* Keeping at least the same amount of housing units as before (Quantitative)
* Not blocking direct light for neighbour buildings (Quantitative)
* Max solar gain potential (optional, Quantitative)
* Max greenery (Quantitative)
* Min noise (Quantitative)
* Social integration (Qualitative)
* Rational spectra of privacy and community (Qualitative)
```

The initial scripts can be found [here](https://github.com/shervinazadi/spatial_computing_workshops).
________________________________________________

## **_Why Apidae?_**

As architecture students we often do things for a reason, without even knowing whether it will actually work. We would design our building in a certain shape, to maximize the amount of sunlight coming into the building, but without any scientific evidence. Besides that, the architecture world is quite conservative. Change, no matter the improvement, takes a long time to be accepted. At the same time, the construction industry is worldwide responsible for 40% of the C02-emission [**[Source]**](https://www.scientias.nl/co2-uitstoot-van-de-bouw-bereikt-recordhoogte/#:~:text=Gebouwen%20en%20de%20bouw%20zorgen,van%20de%20bouw%20een%20recordhoogte).
A change in the way we design and form our buildings is necessary. 
This is where the apidae method comes into play. The apidae method scientifically substantiates design choices, makes the process more efficient and modular, while still giving the designer enough room for subjective choices.
________________________________________________

## **_Project Phasing_**
The documentation is divided in 4 phases:
```html
* A1_Planning: 
    Preliminary spatial analysis on the site and idea proposal regarding design goals.
    (KPI, Program of Requirements, Diagrams) with the starting point as described 
    in this home page as input. 

* A2_Configuring:
    Setting up the plot, determining voxel size. In principle, formulating the 
    spatial concept of the building.
    (Lattice contruction and determining voxelsize)

* A3_Massing:
    Computing envelopes and using those in order to logically place functional spaces 
    and form shafts and corridors. After this, allowing the spaces to grow with the 
    given criterias. 
    (Spatial Analysis, Corridors and Shafts, Designing the lattice with voxel 
    removal and ABM growth)

* A4_Forming:
    Finalizing the results with polygonization and designing the borders of the voxels. 
    (Polygonization and renders)

```
