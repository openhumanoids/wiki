* Hephaestus: checkout manager, resolves dependencies and creates eclipse projects
* Eclipse: IDE
* Gradle: Dependency Managment, build system
* Confluence: Wiki
* Fisheye: Code browser
* Bamboo: Integrated Testing
* Stash: git server (self-hosting/offline equivalent to bitbucket)
* Jura: (internal) ticketing system
* Bitbucket: Location of opensource code. Hosts a mirror of Stash
* Open source tickets: use bitbucket for these.


#FAQ and Tips
##Building/Checkout
- **To update:** 
    - 1. **Pull new projects/remove old projects:** Run Hephaestus from terminal where you have exported the following variable: ```export SWT_GTK3=0```. In Hephaestus, wait for synchronisation to finish and resolve any problems by clicking on the problem description to the left of the file cabinet icon. After resolving each problem like this, Hephaestus will run synchronization, which may take a long time (wait). If projects are being removed, make a note of them, because they will have to be removed from Eclipse manually later. If there are no problems, continue with the next step.
    - 2. **Import project changes:** In eclipse menu, navigate to ```File > Import```. In the import window, choose ```Gradle > Gradle project```. The ```Root folder``` should be set to ```_workspace``` (in your local path). Click on ```Build Model```, select all projects that have been added click ```Finish```.
    - 3. **Remove old projects:** If any projects were completely removed by Hephaestus, they would still exists in the Eclipse environment. Remove them manually now. Alternatively, you can remove all projects and re-import them.
    - 4. **Update existing projects:** Select all the projects in the project explorer > Right click > ```Team``` > ```Update to Head``` (or ```Update to Version``` for a specific revision number).
    - 5. **Resolve Errors:** Select all projects > Right Click > ```Gradle``` > ```Refresh Dependencies```
- **Number of compile/dependency errors appear in the 'Problems' tab**
  - Go to main menu > ```Project``` > uncheck ```Build Automatically```.
  - Go to main menu > ```Project``` > ```Clean``` and clean the whole workspace.
  - Go to main menu > ```Project``` > ```Build All``` and repeat this until all the problems disappear. Each build should get a rid of couple of dependecy problems.
  - Go to main menu > ```Project``` > check ```Build Automatically```.
- **Illegal Entry in Gradle Dependencies error**
  - Make the listed directories: 
  - cd ~/workspace_/_workspace/build & mkdir resources/test -p & mkdir classes/test -p
  - This is a gradle bug

## Operating Sim and IHMC's UI

* Operate ValkyrieObstacleCourseDemo or AtlasObstacleCourseDemo
* disable the ros module (if necessary), the ros module transmits perception to the ui but not will not control robot over ROS when IHMC-UI is active0
* press both clear buttons in lidar control to reset ground to the feet, then press enable to see data
  * quad tree for terrain and footsteps
  * oct tree for point clouds and manipulation
* right click in ui to lay footsteps, spacebar to execute

to walk more quickly:
- press 'longer steps' button
- in 'walking' tab reduce swing time and transition time


# Tips
* "Bad file descriptor" error ... restart
* Example benchmark: IHMC UI says 'frames per second ~30' when nVidia is working properly.
  * nvidia 980 graphics card recommended
* The simulation speed can be increased by changing the value set for 'getSimulateDT()' in each of the robot model Java files (Warning, high values will crash the simulation).
* To have the walking controller plot in the SCS GUI track a particular variable (and use it to center the graph visualisations), pass the variable name (e.g. "Center Of Mass") as an additional third parameter to VisualizerUtils.createOverheadPlotter() in DRCSimulationFactory.java

### Run time fixes
- **Dealing with 'out of memory' and Java heap space problems**  
- ***Solution 1***  
Instead of having to increase the memory in the JVM on a per project basis, set a system wide setting for all application executions.
  - Go to Window->Preferences->Java->Installed JREs
  - Select the JRE you're using, click Edit
  - Go to the line for Default VM Arguments and insert -Xmx8g to increase the ram to 8 GB. (alternatively -Xmx16g for 16 GB)
  - Click Finish -> Apply -> Save   
- ***Solution 2*** 
  - Set 'scsInitialSetup.setSimulationDataBufferSize(#)' (where # is some number) to a lower value. The default buffer value is 16000 which can be reduced significantly without a noticeable effect on simulation.

# Key Bindings
* 'x' will hide extra buttons