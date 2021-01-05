# Process
> This is our process and product of the 1st activity: **Planning**

<table><thead><tr class="header"><th>Title</th><th>Planning (process): Programme of Requirements &amp; Network (product)</th></tr></thead><tbody><tr class="odd"><td>Objective</td><td>Formulate the design problems, form a programme of requirements, form a network, formulate your design principles and the idea (spatial sequences/experience/stories visible in a network).</td></tr><tr class="even"><td>Procedure</td><td><p>Describe the hierarchy of design decisions, formulate design goals, define design principles, identify stages in the design process that could be supported by algorithms, draw a flowchart to reflect on these steps and their connections and update it every week.</p><p>Develop a programme of requirements, an idea (encapsulating the added value of the building and what is going to be unique about it in terms of human experiences) and a corresponding network indicating the main trips inside the building to be facilitated by direct connections matching with the scenarios envisaged in the idea. Formulate the design principles indicating what is a good shape for the building given operational, climatic, or structural aspects.</p></td></tr></tbody></table>

## The brief

#### The design problems

The objective for this course was to develop and create housing, work and recreational spaces. This has to be done in a way that is both beneficial to the users and neigbouring community, while also taking sustainablity into account. 
<br />

#### Programme of requirements

The design brief states that the plot in [Rotterdam, The block between Vijverhofstraat, Zomerhofstraat, Schoterbosstraat, and Teilingerstraat](https://www.google.com/maps/place/Startup+Noord/@51.9292516,4.4767546,373m/data=!3m2!1e3!4b1!4m5!3m4!1s0x47c434a9cf625753:0xb8615a4c444b9d57!8m2!3d51.9292516!4d4.4779013) is to be redeveloped. Here we could choose between the compulsory or optional plot as shown in the figure below. 

![the Plot](../img/plot.png)

The brief given could be alterted to some extend, so we changed it to the following:

- Housing:
    - Student housing 80 units
    - Assisted living 30 units
    - Starter housing 100 units

- Communal Spaces:
    - Underground parking (0.5 parking lots per apartment or more)
    - Communal garden
    - Workshop
    - Common room (co-cooking)
    - Study space
    - Bike parking (1 per resident)

- Public Spaces:
    - Shared car parking
    - Hub
    - Community center
    - Librairy and music rooms
    - Offices
    - Gym
    - Makerspace
    - Shop(s)
    - Coffee corner
    - Restaurant(s)
    - Bike parking 

<br />
<br />

## The development
<br />

#### Our vision
Having visited the site and conducted research on the context and local communities, we stated a vision for the plot itself and how the plot would fit in the larger urban scale. We sought to adress some of the most pressing issues in Rotterdam nowadays : housing shortage and sustainability. Our vison is as follows :

> **Densify the city with sustainable living and working space, which benefit both the user and neigbouring community, with tailored modular and flexible units.**

<br />

#### Design goals

1.  <u>Creating clusters</u>
    <br>The residential functions are <strong>clustered</strong> around their preferred communal node (for example, the study space). This way they are more <strong>accessible</strong> to those that use them the most, while also <strong>separating</strong> the users with different lifestyles.

    <img src="../img/design_goal_1.png" width="250">

2.  <u>Separating public/private</u>
    <br>A privacy gradient ensures <strong>separation</strong> between the public and private areas inside the building, while in between communal areas serve as <strong>transition</strong>. This way the residents can enjoy a peaceful and quiet living space, without them having to worry about <strong>noise</strong> or compromised <strong>privacy</strong>.

    <img src="../img/placeholder.png" width="250">

3.  <u>Outdoor Garden</u>
    <br>All residential units are <strong>connected</strong> to the central communal garden. This way, they all have access to a pleasant open and green area to relax in. Furthermore, commuting through it <strong>stimulates encounters</strong> between neighbours

    <img src="../img/design_goal_3.png" width="250">

4.  <u>Activating the street</u>
    <br>The Vijverhofstraat is ‘activated’ with <strong>opportunities</strong> for people to dine and shop there. this aligns with the city’s plan to turn the old metroline into a <strong>‘Highline’</strong>. This contributes to the amount of <strong>visitors</strong> and significance of the area.

    <img src="../img/design_goal_4.png" width="250">

    
<br />

#### Network

The first step of design involved creating a network graph reflecting main trips inside the building based on our design goals:

<!-- [this one](https://miro.com/app/board/o9J_lfSaOlk=/). -->
![Proposed model](../img/Network_graph.png)

While designing the graph and human experiences reflected by it, we referred to our design goals.

1. The residential functions are clustered around communal nodes that are most relevant to the resident group. (Students' living next to study spaces, assisted living next to workshops etc.)

2. To ensure a transition between very private and very public spaces, several routes are made between the residential part of the builing and communal / private working area, while the most public area on the street connects to the communal/ private working are with one main route for everyone to use. 

2. Implementing the 2nd and 3rd design goals of creating gradients between the public and the private, the residents' living spaces and communal spaces are centered around the more quiet and peaceful garden, while the more public functions are centered around the hub.

3. Shops and restaurants are not directly connected to the Hub or the Garden but to the street. This way the most public area is distinguished, thus inviting people to dine and shop in the open and lively part of the building. This aligns with the 4th goal of activating the street.
<br />
<br />

#### REL chart and preferences

The relationship and preference matrix is used to seed agents, grow the spaces and determine a hierrachy of spaces.

 Hierarchy

According to our design strategy with privacy gradients and the decision to cluster functions around hubs, a hierachy of spaces arises. When the growth algorithm seeds and grows spaces, the matrix is used to look up which spaces should grow or "follow" which spaces. However, not every space finds it important to follow another. Some spaces are dependant on the location of the hubs but the hubs themselves are not affected by the spaces following them. This relationship indicated in the matrix by lack of symmetry across the diagnol.

![Proposed model](../img/Matrix.png)

<br />
<br />

#### Computational Process Flowchart
![Proposed model](../img/process (1).png)

