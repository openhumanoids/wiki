# Installation of IHMC Codebase
### Install Eclipse
* Installation assumes 14.04 Ubuntu. Java JDK8 is required - to install JDK8 on Ubuntu 14.04 LTS, please follow [these instructions](http://ubuntuhandbook.org/index.php/2015/01/install-openjdk-8-ubuntu-14-04-12-04-lts/). If in doubt, check which version of Java you are using: ``java -version``
* [Download from Eclipse website Mars IDE for Java Developers](http://www.eclipse.org/downloads)
* Unpack and install (i.e. move) into /opt/eclipse
  * Create simlink: ``sudo ln -s /opt/eclipse/eclipse /usr/local/bin/eclipse``
* To enable this in Eclipse, when running one of the test programs mentioned below in Eclipse:
  * go ```Preferences``` , ```Java``` and ```Compiler``` and set the compiler to 1.8
  * To import the 1.8 compiler import from /usr/lib/jvm/java-8-openjdk-amd64 (This bit is incomplete)
* As of Feb 2016: a fresh Ubuntu 14.04 comes with 1.8, meaning the above steps isn't necessary

### Clone the IHMC Open Robotics Software Repository
 * Clone the IHMC Open Robotics Software repository: 
```Shell
git clone git@github.com:ihmcrobotics/ihmc-open-robotics-software.git --branch develop _IHMCRobotics
```
 * Open Eclipse and go to <kbd>File</kbd> => <kbd>Import</kbd> => <kbd>Gradle Project</kbd>. Make sure to use version 2.11 of the Gradle plugin. Use the built in ```Buildship``` gradle plugin, not the third party one. If the welcome page shows up, just press next, then use the path where you cloned the repository as the ```Project root directory```. Then click on ```Next```. 
* Choose the ```Gradle wrapper (recommended)``` and click ```Next``` again. Gradle will now load all the projects. Click ```Finish``` when done.

Out of date notes:
* Note: The Eclipse Buildship Gradle plugin may cause the import process to fail. This is due to conflicting versions of gradle. There are two possible solutions (both may be required depending on the version of your Eclipse installation):
* Update the gradle plugin: From main menu: ```Help``` -> ```Eclipse Marketplace...```, search for "buildship" plugin, click on ```Installed``` to go to plugin details, and ```Update```.
* When importing the projects, specify the gradle version on the ```Import Options``` page of the import wizard. Version ```2.11``` is known to work.

### Setup your NetworkParameters.ini:
After importing the code into Eclipse, open the file listing
* For simulations on you computer run: us.ihmc.communication.configuration.NetworkParametersCreator and click [save]. The default values point to localhost.

# Running SCS

### Running a test program
* Ctrl+Shift+T to search for files, jump to arbitrary files by file name using Ctrl+Shift+R
* Open ValkyrieFlatGroundWalkingTrack or AtlasFlatGroundWalkingTrack
* use "java application" for the "run as"
* The SCS system will startup and the robot will drop to the ground and balance
* pressing "|>o" plays the simulation. And [] pauses
* In the variable search panel (bottom left), search for "walk" and set to 1/true
* The robot will randomly walk around

### To run Valkyrie paired with Drake Designer UI
* To use the IHMC ROS Api you need to tell SCS your ROS UI details. Its sufficient to run NetworkParametersCreator and click [save]. This will point to localhost

Having [installed the MITDRC](https://github.com/openhumanoids/main-distro/blob/master/README.rst) and IHMC codebases. These instructions will
set up the bridge between the two system (through IHMC's ROS API)

* Start a roscore in a terminal
* In a second terminal open and launch the Drake Designer using procman:
  * ``cd ~/ipab-distro/drc/software/config/val_sim_scs``
  * ``bot-procman-sheriff -l robot.pmd``
  * Launch the 'start_ui' script to launch the UI, planners and other utilities

Launch SCS from Eclipse
* alt-ctrl-t "ValkyireIPABSimulator"
* Run as -> Java Application
* The robot will drop to the ground and balance in SCS and in Drake-designer. You can walk around and move the arms using the designer

Terminating:
* Press the red button in eclipse
* To shut down Drake processes 'select all', and then right-click 'stop'
* Remember to start bot-procman first and shut it down last as IHMC/SCS needs the ros bridge to stay running

Using your own simulation environment:
* Also see [Compiling interactive simulator program](https://github.com/ipab-slmc/ipab-distro/wiki/IHMC-SCS-integration#compiling-interactive-simulator-program)


### List of useful IHMC test or demo programs
* ValkyrieFlatGroundWalkingTrack and AtlasFlatGroundWalkingTrack: a basic test of the walking control
* AtlasDemo01ROSAPISimulator, ValkyrieROSAPISimulator, AtlasFinalsROSAPISimulator: The basic Simulator with a ROS API
* ValkyrieIPABSimulator: a variant of the above described above, maintained by Vlad
* ValkyrieObstacleCourseDemo and AtlasObstacleCourseDemo: the IHMC UI controlling the robot

# Other
**Debugging**
* Some of our sims just require a great deal of RAM, ~4GB
* HOWTO-Increasingthedefaultmaximumheapsize-280115-1101-2.pdf.


# DEPRECATED
The instructions below were from the era when we still used Hephaestus

* Help -> Eclipse Marketplace
* Install Subclipse (1.10+) including restart
* Allow subversion to interact with gnome despite a bug ("Subversion 1.6 constains a bug that causes ...")
* Install "Gradle IDE Pack" from Marketplace
* Uncheck everything but: Enide Gradle, Gradle IDE and Minimalist Gradle Editor

### Clone the IHMC Open Robotics Software Repository
 * Clone the IHMC Open Robotics Software repository as a local workspace, e.g. to ~/workspace: ```git clone git@bitbucket.org:ihmcrobotics/ihmc-open-robotics-software.git workspace```
 * Open Eclipse and go to <kbd>File</kbd> => <kbd>Import</kbd> => <kbd>Gradle Project</kbd>, then select the workspace folder/git repository and create a folder ``_workspace`` underneath the folder that you have just cloned and click on <kbd>Build Model</kbd>
 * Once the model has been build, click on <kbd>Select All</kbd>, and then <kbd>Finish</kbd> to import

### Opening up Eclipse
* Launch Eclipse, and in the dialog where it asks for a Workspace location at start point it to the Workspace you just created.
* File -> Import -> Gradle Project.
* At the top is a text field to point it to a "Model", you will want to point it at the folder in your workspace that begins with an underscore (it should be _<Workspace Name>).
* Select that, click Build Model, and if all is well you should get no errors.  Select all the projects and finish.
* The Eclipse package explorer will list about 25 packages: Atlas at the top and ValkyrieHardwareDrivers at the bottom.

### Checkout IHMC codebase
Hephaestus 0.4.8beta requires Java 8 (as will all future IHMC code).

* Download Hephaestus (IHMC tool for code managment):

    https://s3-us-west-2.amazonaws.com/ihmc/hephaestus/Hephaestus-0.4.8-beta-linux.jar

* Enable permissions: ``chmod +x Hephaestus----.jar``
* right click on the .jar, Open With your JRE (alternatively you can run it from the command line using "java -jar <file>")
  * Press 'continue' if it complains about a GTK3 Warning
* If needed, the Username is ReadOnlyGuest and password is IHMC????z (ask Vlad or Maurice for password)
* Select all available projects
* Use this as your workspace location: /home/yourname/workspace
* Then click "create workspace"
* Code checkout begins and can then take 5-10 mins. (Typical workspace directory size is 1.7GB)
* Once your checkout is complete you can close Hephaestus.

**Team Synchronize (updating your SVN)**
* In Eclipse, in the top right, there's a little four paned box with a Plus next to the word "Java", hit this to add a new Perspective and select the "Team Synchronize" perspective.
* In the left pane, in that little "Synchronize" view, click the farthest left button that's a yellow cylinder and a window, choose "SVN", then select all and proceed
* And after this sync is done it should show all of your incoming and outgoing changes, there's a button that looks like a piece of paper with an arrow pointing to the left, if you hover over it it'll show a tooltip that says "Update All Incoming Changes", click that.
* The Gradle dependencies don't always update properly after checking out new code. To fix this, select all projects in the package explorer > right click > Gradle > Refresh all.

**Checkout a specific revision**
* In eclipse, select all projects in the package explorer > right click > Team > Switch to another branch/tag/revision
* On top of the window, just under the url, uncheck "Switch to HEAD revision" and type the revision number in the box provided. Click OK.
* The revision will be checked out.
* The Gradle dependencies don't always update properly after checking out new code. To fix this, select all projects in the package explorer > right click > Gradle > Refresh all.