#NASA's instructions to: [Getting-Valkyrie-Walking-With-IHMC's-Controller](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/Getting-Valkyrie-Walking-With-IHMC's-Controller)


***

This article is a step by step run-through on how to turn the Valkyrie on properly and get it ready to receive commands.

Also have a look at NASA's [Booting Up guide](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/Booting%20Up%20Valkyrie).




### Turning on (Hardware-side)
1. Turn on the [power supplies](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/JuiceBoxButtons.png). No particular order for turning on logic or motor power, but:
    1. turn on the powers
    2. when the screens plot `OFF`, press the `out` buttons
1. Turn on Valkyrie: [the switch is located behind the right shoulder, on the backpack](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/RobotPowerButton.png).

### Booting and Calibrating (Software-side)
1. Start the [UI-Builder](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/ui_builder.png): ```ui_builder &``` and load the ``mission_control_val_[UNIT].uic`` (for Edinburgh: ``mission_control_val_D.uic``) configuration file via the <kbd>file</kbd>=><kbd>add tab(s)(config file)</kbd> dialog. **Light will be rainbow or red**
1. <kbd>[Be A Robot](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/ClickBeARobot.png)</kbd>. This will start the communication channels to the operator workstation. Only click on <kbd>Stop Being A Robot</kbd> when done with all experiments as it will shut everything down. **Light turns light purple**
1. <kbd>[Controller Manager: Start](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/ClickControllerManagerStart.png)</kbd>. Controller manager manages ROS control components.
1. <kbd>[Logic Power: On](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/MissionControl.png)</kbd>, this also loads the actuator coefficients and can hence take a little time (wait for the log message to appear). **Light turns green**
1. <kbd>[Start IHMC Controller](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/MissionControl.png)</kbd>
   * **If you want to run the neck controller (IN DEVELOPMENT):** ssh into to link
   * sudo service valkyrie_lcm_distro_bot_lcm_tunnel start
   * roslaunch valkyrie_translator NeckControl.launch
   * from vis04: bot-lcm-tunnel $VAL_LINK_IP -s "DESIRED_NECK_ANGLES"
1. Add the Robot Status tab by going to <kbd>File</kbd>=><kbd>Add Tab</kbd> and load the robot status file. This has to be done after becoming a robot!
1. Turn on the wireless E-Stop by flipping its power switch (lights should be all green).
1. Clear faults/E-Stop in ui_builder.
1. <kbd>[Motor Power: On](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/MissionControl.png)</kbd>.  The capacitors charge taking about 10s. Do NOT servo until it finishes, otherwise you will trigger an overvoltage error and have to restart from the very beginning. **Light will turn purple for 10 seconds, and then blue**
1. Turn on the robot servos to initialize rotors: [Mode Controls panel](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/MissionControl.png). The procedure should be, for each component: 
    1. Servo [COMPONENT]
    2. Park [COMPONENT]
**NOTE** If we servo a component and its rotor initialization does not succeed, we must park immediately otherwise the current supplied increases abruptly and the temperature of the joint raises quickly (always ensure the temperature of all the joints is green in the robot status file). After the robot is parked, 
    3. Initialize missing rotors manually (or try first Initializing Motor Rotors button)
    4. Servo Robot   

### Starting SCS and the Whole Body Controller
Now we turn our attention to Eclipse to start SCS.

1. Start the Remote Visualizer
1. At the bottom of the SCS window, open the Diagnostics tab
1. Ramp up slider 7 (second from the right)
1. Set adaptTorqueOffset to 1 for about 10s, then back to 0
1. Set transferTorqueOffset to 1
1. **Ramp down slider 7**
1. Change to the Startup tab
1. Set calibrateFootForceSensors to 1
1. **Lower the robot down** until the foot have made full contact with the ground
1. Set the requestedHighLevelState to WALKING and then click in another input field. Repeat this (i.e. do it TWICE; there's an IHMC bug)
1. Ramp up slider 8 to the desired CoM height (roughly half way up to start with)
1. **Slowly** transition between the standing/init and the whole body controller by ramping up slider 7

### Enabling ROS communications
1. SSH into Zelda, e.g. for Unit D (Edinburgh): ```ssh -YC zelda```
1. ```cd valkyrie```
1. ```./runROSNetworkProcessor.sh```

### Shutdown
1. Click <kbd>[Stop Being A Robot](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/MissionControl.png)</kbd> in the ui_builder interface, then catch the robot with the lift - do not lift it up before pressing this button or else it will go wild
2. Shutdown on-board pcs:
```
ssh -YC zelda
sudo shutdown now
```
```
ssh -YC link
sudo shutdown now
```
3. Turn off Valkyrie: [the switch is located behind the right shoulder, on the backpack](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/RobotPowerButton.png).

# Other Notes
 * After an E-Stop:
    1. Stop the IHMC Controller.
    1. Stop the Controller Manager.
    1. Stop the Remote Visualizer.
    1. Start the Controller Manager.
    1. Continue from step 5. of [Booting and Calibrating (Software side)](https://github.com/ipab-slmc/wiki/wiki/Valkyrie-Operation/_edit#booting-and-calibrating-software-side).

 * **Troubleshooting**: The first time you turn on Valkyrie and reach step 3. of [Booting and Calibrating (Software side)](https://github.com/ipab-slmc/wiki/wiki/Valkyrie-Operation/_edit#booting-and-calibrating-software-side) (<kbd>[Controller Manager: Start](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/ClickControllerManagerStart.png)</kbd>) you could get a failure message and the Control Manager goes to the state `unknown`. If this is the case, <kbd>[Stop Being A Robot](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/images/MissionControl.png)</kbd> and run in a terminal:
```
pkill -f smt
```
This error is due to an instance of smt which is already running.