### Suggestions to NASA
- add a hardware wire between the two ptgrey cameras in the chest
- new ui_builder with state and operator projection


### Repos

for drcbo/main-distro i made an access change:
- drc-pull remains read
- drc-push and drcbot-push teams are now both read, were write

Our code Issues:
- multisense joint angle missing in est robot state


### NASA Plans
Have had incremental partnerships with darpa, gm, nasa iss
robolegs was a previous project before valkyrie

every cargo vehicle has kit to fix the exercise equipment
typically 5 days down time in 1000 for about 30 different elements of the iss (over 5-10 yrs)

#####Space Robotics Challenge
* Virtual comp environments released in sim april 2016
* sim comp qual jun 2016 *
* sim comp task finalised sep 2016 *
* src virt comp dec 2016 *
* hardware locked may 2017 *
* physical comp finalised 2017 * 
* src competition dec 2017 *
* <possibly have another comp or other robots after that> 

(* to be approved) osrf are involved

#####SRC Hosts:
* network access to robot by jsc and hosted team
* 15 days access per month for hosted team

### Hardware
* knees joints force sensing have resolution issues
* 1.8kWh lithium ion pack
* 2 individual batteries for motor and logic
* battery mass simulator is a lithium weight
* hands not finished for a few months
* Suggestion: add a hardware wire between the two ptgrey cameras in the chest

Two onboard computers:
* link - real-time linux. ihmc controller
* zelda - perception computer. 'network processor' for comms to remote machines

IMU is lord microstrain 3dm gx4 15: http://www.microstrain.com/inertial/3dm-gx4-15
* Use ETH ROS driver
* microcontroller on imu has limited baud so not all data can be shipped to us (without changing baud).
* imu runs onboard bias estimation and reports the estimated 
* accelerometer values corrected for this
* The difference between the usb and the RS232 is in the com side of the imu. For using the RS232 the baud rate must be specified. Additionally we will run through the RS232 interface through a turbodriver and then that will use robonet to move data to the computer.
* When the usb is plugged into the usb it creates a virtual serial port inside the computer to expose the data (which is why a baud rate must be specified). We want to move away from this since it requires the computer ports to interrupt which limits the number of imus that can be used.
* Issue with IMU: because IMU is mounted on plastic cover (3D printed), so the magnitude of vibration is very large. It destroys the internal Kalman filter of the IMU. On the contrary, the metal part of the robot has large inertia and is a rigid piece. They are working on getting metal component for mounting IMUs. 

upgrades mentioned by nasa:
- strain gauges on the wrists
- force control of the wrists
- control interface to the head and hands: currently being developed to mimic the turbo drivers 
- upgrading of the the imu
- integrating the active ir projector into the sensor head
- further pitching the sensor head within the outer head

Kit to Be Delivered by NASA:
- Will provide a network server
- juice box
- 12tb NAS
- Two pelvis and back mounts in 80-20 mount will be provided. we just need to fix that to the gantry

Kim: nasa will be paying for the shipping for the src hosts
- insurance issues to be confirmed


### NASA Software
* 400 unit tests, 6 people working on software
* Docker - testing suite for unit tests. Developed by Kody
* https://github.com/NASA-JSC-Robotics/valkyrie/wiki
* Use zenhub issue tracking (swim lanes) for reporting problems
* python-vcs to keep complex, multi-repository workspaces in sync and maintain sanity
* build server building debians of stable branches of all repositories, then distribution as PPA internally

Code and Dev issues:
* hand, neck control not exposed to ihmc yet

Software Releases:
* Aurora nov 10, 2015
* bart, jan 13, 2016
* cartoon, march/april
* daria, tbd

### IHMC Software
* Operator UI will not be is not open source. But it was suggested that we could use it as part of our agreement

##### IHMC Controller
* Sylvin could easily add a mid-level interface mode to accept streaming joint accelerations. the advantage of this is that this is a direct input to the QP as opposed to something like torques. Torques are the output after passing through the inverse dynamics module (after the QP?). This mode would be useful for dynamic motion planning/execution e.g. dynamic mesh

Improving walking performance at training:
* increase tf time and swing time to defaults used by IHMC
* reduced nom step width to 0.23

##### Controller Fixes
* Feet were sliding when standing. kdzangularhold was set to 100 to hold the feet in place on the ground


### Customized IHMC gains
Use the Yo variable widget on the bottom left side of the Simulator/RemoteVisualizer to tune IHMC control gains.
You can either search for the variables and change them directly, or you can drag-and-drop each variable into the panel below the plot area. To change a value, confirm the change by pressing enter or selecting another text field on the scree.
***Never tune gains on the robot without exploring it in the simulator first!*** 

***Arm gains***

Set gains in this order:

 1. ```zetaArmJointspace``` = 0.9
 1. ```kpArmJointspace``` = 200.0
 1. ```maxIntegralErrorArmJointspace``` = 5000.0
 1. ```kiArmJointspace``` = 200.0

***Pelvis gains***

Set gains in this order:

Lower damping ration decreases oscillations.
 1. ```zetaXYAngularChestOrientation``` = 0.6
 1. ```kpXYAngularChestOrientation``` = 120.0


### Control notes

#####Alex’s Notes (to be merged with Maurice’s notes)
**QP info** Current whole body solver is made by QP solver + inverse dynamics solver, in a cascaded 	manner. Some issues of tuning the null space and conjunct different 	pieces of code is foreseen, not recommended for 3rd party control engineer to debug. In the future, 	there will be a newer version of one QP centralized QP to solve whole body. Currently cvxgen is the qp solver, but it clearly has 	its own limitations. (The QP feeds in accelerations)

**Structure bending:** foot displacement can be between 3-5 cm, it is claimed by IHMC that bending is caused by leg structure, not the harmonic gear deflection since there is direct measurement of joint position 	after the HD gear. The compensation of the leg structure bending can be problematic since it is not well tuned, it can possibly cause 	problems for the control of other joints or kinematic chains. 	

**The torso orientation computation** the position computation uses the encoder (mostly likely the motor encoder), and the angular velocity is done 	by extracting the relative rate from 2 IMUs. Code is in: JointStateUpdater.java/IMUBasedJointVelocityEstimator.java

There is an optical sensor located the end of the linear actuator in torso that gives more ideal force measurement than the FT sensor. But the only problem is that the spring can get stuck sometimes (purely mechanical limitation/issue) which downgrades the torque 	measurement.

It will be interesting if we could look at the transformation from the linear motion of the linear actuators to the rotational motion of the torso, to see if it will be reasonable to use motor velocity to compute link angular velocity. 	

#####Sylvain comments on the issues:
**Oscillation issue** (leg differential force), most likely the delay of velocity signal due to filtering and lack of compensation for structure deformation

**Jerky motion when start UI motion planning** Typical interpolation problem of using the current motion position instead of the latest desired position, so the interpolation creates discontinuity thus a jump. There is a quick fix that can be done by Sylvain, but this issue is minor at the moment.

**Torso orientation error/drifting** It is caused by the joint tracking error combined with gains (effort =Kp*error), so it generates large acceleration demand in arm acceleration that could result in torso yawing, but we have not fully confirmed this, only hypothesis.

#####Maurice Notes:
Sylvain expects there to be 10s of bugs still in the controller

number one issue at moment is torque-based deflection in hip. can be visualized by (1) putting hip frame into upper right plot of UI, driving it to be over the inner edge of the foot (so that that foot has most of the force on it). This is clearly not correct - maybe 3 cm away from what model should do. Sylvan will work on a torq

**Chest rotation velocity sensing** is currently An issue:
* currently pair of pelvis/chest imus used to calculate rotation rates of chest
* motor encoders converted to linear velocity through linkage. high resolution, however sylvan doesn’t use it
  - because he doesn’t trust the calculation of the joint angle/velocity from the motor encoder and bar linkage. possibly also some deformation
  - another sensor measures spring deflection for effort. would like to use this
* link encoder measures the angle directly, so resolution

**Joint filtering:** (you can study this from SCS logs)
- measured leg encoder positions are filtered with the torque deformation model (same model we used for Atlas)
- NASA provides velocity estimates.
- currently the (filtered) position and (nasa) velocity are out of phase. the velocity leads the position
- as a result this can cause out of phase control e.g. the hopping from left foot to right repeatedly
- sylvan wants to filter the velocity in the same manner as the positions to avoid this

implementation of controller and published algorithm differ quite a bit currently: while improving the current implementation also working in parallel on a new version which will be closer to description. CvxGen is current QP solver, considering moving to QP-OSS solver

ACTION: add a bitbucket request to have a boolean switch between:
- adding the current desired to the incoming arm plan so that the controller can blend into the plan. Problem with this is that when the sample time in the plan is short, the controller must blend a big difference - causing a whole body shake. (the whole body shake is because the QP tries to use the whole body (esp torso) counteract the jerk at the arm joints (current case)
- OR when an incoming plan is received, drop the desired and use the for plan sample. The expected result will be that the arm will droop at the start of the plan. If well tuned this might not be very noticeable (to be implemented)

**Observation on the robot:** When allowing arm j2 to feather down from horizontal to vertical you can feel a tic/contact force as the joint rotates. JOSH: this is a torque ripple from the harmonic motor. the number of tics is exactly the gear ratio. you will see this change for different joints. 

(Alex comment: harmonic gear assembly is tricky since there are only few teeth engaged, usually it requires highly professionals to do careful assembly with visual inspection. However, the NASA design is very compact and due to the design, the insertion of harmonic gear is done blindly, and only tested and evaluated by backdriving the joint. So it is possible that it is misaligned by one tooth, but it should be easy to diagnose by measurement, see link here: http://www.vibanalysis.co.uk/vibanalysis/gears/gears.html )

#####20 Jan, 2016 Alex' notes 

**Provisional solutions (not tested by us yet)** Add compensation for elasticity for the joint velocity, which is supposed to remove the oscillation problem; Apply feedforward velocity term in arm tracking, similar to what is done in atlas. This slightly reduces the tracking error in arm joints.

**Explanation of the torso yawing motion** The steady state errors in arm joints result in significant joint acceleration, which will be directly fed into the cascaded whole-body solver, where the whole body inverse dynamics outputs unreasonable torso torque in order to achieve the high arm joint accelerations. So the issue is triggered by a sequence of side effects.


**Explanation of always falling to right side:** Structure bening is not compensated at all for either legs. So what the robot senses about the COM position is not really where it is. So I reckon the real COM is off the measurement outward for about 1cm, that is why it diverges during quasi-static right stance motion.

#####21-22 Jan, 2016 Alex' notes 
- The new code (joint elasticity compensation, and feedforward velocity for arm joints) does not necessarily improve the torso collapsing problem. The lateral oscillation triggered by pulling back seems better, it happened only once now.

- The arm gains are completely disabled to zero, and the torso still collapses. So the previous explanation is completely not correct. It is not related with the ‘large’ arm joint accelerations, there are other unknown reasons in the control.

- The torso collapsed during the last run of ihmc walking demo.

### Calibration
- IHMC say joint calibration accuracy: 0.01 is very bad, 0.005 is ok, 0.003 target
- joint torque offsets (estimated before runs by IHMC) are on the order 1-5 Nm
- IHMC can write a torque offset file to XML:
 - beside 'send torque offsets' is 'Print torque offsets' in the ihmc ui which does this
- NASA will provide a ROS service call to tare the torque sensors
- More details of joint torque calibration (removal of torque offset) In the mid-ware level, there are xml files that contain the ID of each joint and the torque offset. These can be loaded into the turbo driver when the robot powers up; this can be done but not done yet. Jordan is working on that. How IHMC software does it is to move all joints to specific positions, hold statically for a while, extract the difference between the desired integral torque from PID and the real torque measurement as the torque offset, and load these offsets somewhere inside the IHMC software and used later in the control code. So, it is not yet written elsewhere in any external config file or xml file. But technically, this can be done. Jordan commented that if we do so, probably we only need to run the torque calibration procedure only once per month.  

###Our Software
Easier way to not build Exotica: place an empty file named CATKIN_IGNORE in each of the directories, e.g.:
* touch $DRC_BASE/catkin_ws/src/exotica/CATKIN_IGNORE
* touch $DRC_BASE/catkin_ws/src/exotica-dev/CATKIN_IGNORE

Joint Streaming from Drake Joint 
* cd /oh-distro/software/director/src/python/tests
* python testAtlasCommand.py --base
* On robot or base machine (if LCM tunnel to LCM2ROSControl on link): ``cddrc ; cd director ; directorPython src/python/tests/testAtlasCommand.py --robot``
* Then use director to send trajectory plans or use the joint control interface directly: ``cddrc ; cd director ; directorPython src/python/tests/testAtlasCommand.py --base`` (will open a GUI)
* ``FEEDBACK_TORQUE`` is the channel to receive the commanded torque
* ``COMMAND_FEEDBACK`` is the channel where ``ROBOT_COMMAND`` is echoed back with the correct utime


Diff needed during joint streaming mode:
* -def convertStateMessageToDrakePose(msg, strict=True):
* +def convertStateMessageToDrakePose(msg, strict=False):

***
### Analysing IHMC log files
- Start "LogVisualizer"

### On-board Computer Setup
- **LINK** is the real-time computer on-board the valkyrie running the JSC system software, JSC ros-control, and IHMC whole-body controller, as well as any third-party controllers that we are developing. **2 cores are blocked in the OS for the IHMC whole-body controller**. **ZELDA** is to run own code, e.g. perception. Runs the ROS wrapper and IHMC Network Processor API

### Low-level debugging and control with Shared Memory Transport
- ``smtcore`` runs the shared memory instance on the computer
- robonet is a MLWDS bus
- ``robonet_editor`` gives access to robonet status and control registers through a GUI (useful for finding errors). Should not be used for control
- ``cfgLoader -i devel/share/val_description/instance/instances/instance_files/test_bench.yml`` - loads the test bench coefficients ("coeff file") into the TurboDrivers. This is also the "under the hood" programme that is called from the ui_builder interface as part of the Logic Power On/Load Coefficients command
- ``turboCmd [list of actuators] faults -c`` - lists faults, clears; when all green we are ready to servo
- Checking faults in robonet_editor: StatReg2, make sure RotorInit=1 or move it slightly
- Hardware API: torques are exposed via EffortJointInterfaces
- SMT can be directly accessed and modified from within python (``import pysmt as smt``); okay for testbench and single joint usage; not okay for full robot or limp control; requires the ``nasa_val_embedded_utils``
- ``smttopic list`` - lists all SMT topics
- Reference persons for SMT: Will (?), James Holley

### Running your own controller on LINK
- ROS Control Controller Manager: ``roslaunch val_deploy val_controller_manager.launch model:=model/urdf/valkyrie_C_no_hands.hw.urdf``, followed by launching your controller launch file to attach/load it as a plugin

***

### Topics During Training


#####Getting GIT (via ssh protocol) through NASA's firewall
Add the following lines into ```~/.gitconfig```:
```
[credential]
	helper = cache --timeout=36000
[url "https://github.com/"]
    insteadOf = git@github.com:
[url "https://github.com/"]
    insteadOf = ssh://git@github.com/
[url "https://github.com/"]
    insteadOf = git://github.com
[url "https://bitbucket.org/"]
    insteadOf = git@bitbucket.org:
```
You can now clone your repository using the ssh urls. These will get replaced with http urls on the fly.
You will be asked to enter your github username and password. This will be cached for 10 hours (36000s).


#####Networking During Training
Using network manager create interface with:
* MTU: 1500 bytes, Method: Manual, address 10.185.0.150, netmask 255.255.255.0, gateway 10.66.171.1

IP addresses:
* left operator station: 10.185.0.103
* link?: 10.185.0.30
* zelda?: 10.185.0.31
* multisense: 10.185.0.32

Add to bashrc:
* export ROS_IP=10.185.0.150
* export ROS_MASTER_URI=http://10.185.0.30:11311
* export LCM_DEFAULT_URL="udpm://239.255.76.76:7676?ttl=1"

Changing this setting was suggested but probably not necessary:
- in this file /etc/sysctl.conf set the following
- net.ipv4.conf.default.rp_filter=0
- net.ipv4.conf.all.rp_filter=0