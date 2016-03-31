# SMT and RoboNet tools
These tools are installed on the rt pc on the robot (Link). To SSH into link run ```ssh -YC val@link03```
* ```robonet_editor``` is a RoboNet register/topic display and editor tool. **DO NOT EDIT ANY PROPERTIES UNLESS INSTRUCTED TO DO SO BY NASA!!!**
* ```smtrecorder``` records the topics published on RoboNet into a *.hdf file.
* ```smtscope``` plots real time data like sensor values, torques, ...
* ```hdfviewer``` is an offline HDF log viewing/plotting tool.
* ```turboStream -c [chanel] -n [node] config``` prints configuration parameters of a particular node+channel, e.g. channel=```pelvis```, node=```waist```.
* ```turboStream [node] faults -c -h``` prints a list of faults of a particular actuator/node.
* ```rsmp_showinfo -n [node]``` prints actuator firmware version, e.g. node=```/pelvis/waist```.

##### Exporting SMT topics data to Matlab
The SMT topics contain low level data, like joint torques. To get this data into matlab.
 1. Record a selection of topics using ```smtrecorder``` program. This will produce a *.hdf5 file.
 1. Open the HDF file in the ```hdfviewer```.
 1. Navigate to individual topics on double click on them. This will open the data table for the topic.
 1. Click on ```Table``` -> ```Export data to text file```.
 1

# IHMC tools
IHMC uses a register of global variables called Yo variables. These can be queried, edited, and plotted at runtime. The bottom left corner of the ValkyrieRemoteVisualiser or a simulator window has a search bar. You can search for any Yo variable that has been created in the code. The search is case-insensitive and accepts wildcards. You can then: edit the variable directly in the search results, drag-and-drop the variable onto a plot area (next to the search box), or drag-and-drop a Yo variable onto the panel below the plotting area to edit it later.

List of useful variable for debugging control issues:
* ```k*ArmJointspace``` PID gains of the arm controllers.
* ```zetaArmJointspace``` damping ratio of the arm PID controllers.
* ```maxIntegralErrorArmJointspcae``` integral term clamping of the arm PID controllers.
* ```kp_[joint]``` kp gain of a controlled joint (replace [joint] with a joint name).
* ```kd_[joint]``` kd gain of a controlled joint (replace [joint] with a joint name).
* ```position_error_[joint]``` position error of a controlled joint (replace [joint] with a joint name).
* ```tau_[joint]``` commanded torque of a controlled joint (replace [joint] with a joint name).
* ```raw_q_[joint]``` joint angle reading (replace [joint] with a joint name).
* ```[joint]TrajectoryPosition``` or ```[joint]TrajectoryCurrentPosition``` (depending on the controller used) desired joint angle (replace [joint] with a joint name).

Read more about the controller [here](https://www.researchgate.net/publication/280839675_Design_of_a_momentum-based_control_framework_and_application_to_the_humanoid_robot_Atlas).

##### Recording and editing IHMC logs
IHMC logs are on by default. The Logs get stored on the NAS. Ususally you would want to crop the logs bacause they can be quite large.
To crop an IHMC log file:
 1. Run ```LogVisualizer``` from Eclipse.
 1. Open the *.log file from the session you want to edit (either copy or mount the session directory on you machine).
 1. Use the position slider (just below the rendering window) to examine the log file.
 1. Use ```Mark start``` and ```Mark end``` buttons to create selection.
 1. Use the ```Crop data``` button to save the cropped data to an empty directory.
 1. Restart the LogVisualizer, this time opening the cropped file.

You can examine all Yo variables directly inside the visualizer.

##### Exporting IHMC logs to matlab
To export IHMC logs to MATLAB, start by cropping the data to a very short sequence, then:
 1. Go to the menu ```File``` -> ```Export data``` or click on the export data button (leftmost button on the toolbar above the plots).
 1. Select ```Data``` option, ```Matlab/Octave (*.mat) as the format, and ```All``` as the VarGroup.
 1. Open the *.mat file in matlab.

The *.mat file will contain all the Yo variables. To easily plot various values:
 1. use the Yo variable search tool in the LogVisualizer to find the variable.
 1. Right click on the variable in the results section and ```Copy full name to clipboard```.
 1. Paste this name into matlab, since the full name matches the structure of the matlab, you can use it for plotting (e.g. ```root.valkyrie.robotTime```).