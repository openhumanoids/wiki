The ```ValkyrieIPABSimulator.java``` is a simulation setup that loads and initializes the Valkyrie robot model in an empty and paused simulation environment and listens to ROS topics for further commands.
**The latest working version of the code is not merged into the repository yet, talk to Vlad to get the code.***

This simulator can be compiled into a monolithic JAR file and used as a binary. To compile the JAR file, go to the ```ValkyrieHardwareDrivers``` directory in your workspace and run ```gradle rosJar```. This will produce a JAR file that contains the revision number in its file name.

Run the simulator either from eclipse or from terminal (in case of the JAR file). There are several options you can specify as command line arguments.

# Command line arguments
 - ```-f```, ```extra-foot-contact-points``` Adds an 8x3 array of simulation foot contact points based on predefined foot contact locations. This is useful for experiments where the robot climbs up/down some steps.
 - ```-s```, ```sdf-contact-points``` Loads contact points from the SDF file (more details below). This option will add the contacts to the list of already existing contacts.
 - ```-S```, ```sdf-contact-points-replace``` Loads contact points from the SDF file (more details below). This option will replace existing contacts.
 - ```-m <file_name>```, ```robotModel <file_name>``` Specifies custom SDF robot file to load. This is for loading robot models, NOT environments!

# Contact points
There are two kinds of contacts in SCS:
 - **Simulation contact points** - these are used for collision checking and they cause the robot to interact with its environment. Since the simulator does not support any other types of collision shapes, this is the only way to make the robot interact with the world.
 - **Controller contacts** - these are virtual points used by the controller to calculate commanded torques. These points don't interact with the world directly, they can't cause collisions, and they may intersect with objects.

The two kinds of contact points may be coinciding but they don't have to, e.g. the foot may consist of an array of 8x3 simulation contact points but only 4 controller contacts in the corners of the foot.

Both kinds of contact points can only be created before the simulation starts. To do this, you can use the command line arguments (see above). To add a predefined array of foot simulation points, use the ```-f-``` switch.

To add custom simulation contact points, use the ```-s``` or ```-S``` command line argument and specify the contact points in the URDF/SDF.
To do this, add collision shapes (e.g. a sphere with radius 0) to any existing links and make sure that the ```name``` tag of this collision shape contains the srting ```sim_contact```. You may add any number of simulation points to arbitrary links this way. Adding these points slows down the simulation, but only the physics solver, the controller (which is the bottle neck) runs at the same speed.

**Adding custom controller contacts is not supported yet.**

# Commanding the simulator via ROS
When the simulator starts, it creates a topic ```/ihmc/valkyrie/api_command```. You can publish messages with the standard string type to this topic to control the behaviour of the simulator.
Following commands are supported:
 - ```simulate``` - starts the simulation.
 - ```stop``` - quits SCS.
 - ```loadsdf <file_name> - loads the environment from an SDF file. You can load the environment at any time, even when the simulation is running (this may make the simulation unstable!).

You can send one message with multiple commands separated with new line characters ```\n```.