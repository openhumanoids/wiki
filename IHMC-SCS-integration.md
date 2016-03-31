
# Compiling interactive simulator program
To spawn the simulator with the Valkyrie robot model in an empty environment, launch the: ValkyrieIPABSimulator.java (CTRL+SHIFT+R to search for file).
This program will start the simulator and subscribe to /ihmc/valkyrie/api_command ROS topic. Command messages for starting/stopping the simulation and loading the environment from a SDF file will be published to this topic.

The ValkyrieIPABSimulator can be launched from eclipse (see [Installing IHMC's Atlas and Valkyrie Codebase](https://github.com/ipab-slmc/ipab-distro/wiki/Installing-IHMC%27s-Atlas-and-Valkyrie-Codebase#download-from-eclipse-website-juno-ide-for-java-developers)) but it can also be compiled into a monolithic JAR file and launched from the terminal. To create the JAR file run:
```Shell
# cd into the ValkyrieHardwareDrivers project in the Eclipse workspace
cd ~/workspace/ValkyrieHardwareDrivers
# Compile the simulator program using Gradle
gradle rosJar
```

This will compile a JAR file that will include all the dependencies. This JAR file can be moved to any location but is requires the configuration files, namely IHMCNetworkParameters.ini.
To launch the simulator run:
```Shell
cd ~/workspace/ValkyrieHardwareDrivers
java -jar lib/ValkyrieIPABSimulator-32172.jar
```

The number appended to the file name is the SVN revision number. To checkout a different revision see [Installing IHMC's Atlas and Valkyrie Codebase](https://github.com/ipab-slmc/ipab-distro/wiki/Installing-IHMC%27s-Atlas-and-Valkyrie-Codebase#other).

**Note:** The ValkyrieIPABSimulator is not yet integrated into the IHMC code base yet (as of 18/08/2015). Ask Vlad for the JAR file or for patches to compile the code yourself.

# Controlling SCS from Drake Designer
SCS can be now launched from the bot-procman-sheriff. The command is stored in a shell script in drc/software/ipab/config/runscs.bash. This script is set up to run the simulator with a specific revision number. This ensures that the Designer launches a stable version of the simulator. Ideally, the revision number won't change often so the file will only change with major simulator updates. The drc/software/ipab/config/valkyrie_v2.pmd script launches the simulator along side the Drake Designer.

The simulator listens to a ROS topic and a LCM2ROS translator has been provided. Here is an example of loading a SDF file in the Drake Designer, and commanding SCS to load the same file and starting the simulation:
```Python
filename='path_to_file.sdf'

# Load the scene
from ddapp import sceneloader
sc=sceneloader.SceneLoader()
sc.loadSDF(filename)

# Command SCS
import ipab
msg=ipab.scs_api_command_t()
msg.command="loadSDF "+filename+"\nsimulate"
lcmUtils.publish('SCS_API_CONTROL', msg)
```

The command message is a string and it can contain the following commands:
```Python
msg.command="loadSDF file_name" # To load the SDF file.
msg.command="simulate" # To start the simulation.
msg.command="stop" # To shut down SCS
```
You can concatenate commands with new line characters "\n".
The stop command is useful for running unit tests, as it allows to shut down the simulator at the end of the test. All simulations start in a paused state to allow for loading the environment before the robot starts moving.

**Note:** To load SDF files, you have to add paths to gazebo resources into your $GAZEBO_RESOURCE_PATH environmental variable. Add the following line to your ~/.bashrc to set the default paths:
```Shell
source /usr/share/gazebo/setup.sh
export GAZEBO_RESOURCE_PATH=$GAZEBO_RESOURCE_PATH:~/.gazebo/models/
```
When the sceneloader can't find the resources, it skips them silently. As a result, the scene will remain empty. This only applies if you are using any resources in the SDF file (this is usually true for SDFs created in Gazebo).

# Support 
* Hip Chat: https://hipchat.ihmc.us/g9zneimT1
* Doug Stephens: @DougieFresh

# Implemented 
* /atlas/inputs/ihmc_msgs/HandPosePacketMessage
  * Purpose: Move the arm to a joint configuration or move the hand to a pose
* /atlas/inputs/ihmc_msgs/ComHeightPacketMessage
  * Purpose: Change the CoM as required
* /atlas/inputs/ihmc_msgs/FootstepDataListMessage
  * Purpose: Send a set of footstep positions for execution
* /atlas/inputs/ihmc_msgs/FootstepDataMessage
  * An individual footstep. Not transmitted
* /atlas/inputs/ihmc_msgs/PauseCommandMessage
  * Purpose: stops the robot walking

# Not Implemented Yet
* /atlas/inputs/ihmc_msgs/FootPosePacketMessage
* /atlas/inputs/TimestampedPoseFootStepGenerator
* /atlas/inputs/ihmc_msgs/ChestOrientationPacketMessage
* /atlas/inputs/ihmc_msgs/HeadOrientationPacketMessage
* /atlas/inputs/ihmc_msgs/LookAtPacketMessage
* /atlas/inputs/ihmc_msgs/PelvisPosePacketMessage
