###NASA

1. TurboDrivers, Athena, Leonides: motor driver boards of different ratings
   1. “Coefficients” loaded on start-up (needs to be done every time logic power is turned on) - otherwise agnostic to what joint they control
2. Robonet Bus (64bit?)
   1. robonet_editor
   1. Synapse board: PCMCIA card to 2-wire bus
3. Shared Memory Transport
   1. smtcore - runs the shared memory instance on the host computer
   1. pysmt - Python wrapper for manipulating SMT values. To be used with great caution when writing, reading OK
4. “Hardware API” - a.k.a. ROS-Control
   1. EffortJointInterface
   1. Interfaces to the sensors
   1. Implementation examples on the wiki, and Greg (MIT) wrote LCM2ROSControl, a simple PID controller wrapper that accepts gains and positions via LCM and outputs state via LCM
5. Simulator: gazebo
   1. [NASA Valkyrie in Gazebo (0 gravity), controlled with director joint control interface](https://www.youtube.com/watch?v=Mf6z-qF-uD4&feature=youtu.be)
   1. [NASA Valkyrie (pinned) in Gazebo, controlled with director joint control interface](https://www.youtube.com/watch?v=e1lDl5IIWOY)
   1. Tipp: Increase solver iterations for stability, or switch from default solver [currently under discussion with JSC, OSRF]
6. Software engineering
   1. Practicing agile via ZenHub
   1. Docker-based build servers continuously building and testing, deploying as internal debian packages
   1. Almost everything in ROS these days
   1. Python VCS to manage dozens of repositories (vcs pull, vcs checkout) - if considering working directly with their ROS packages, use it - otherwise it’s a mess getting all the dependencies
8. Resources
   1. NASA wiki
   1. [Debugging tools](https://github.com/ipab-slmc/wiki/wiki/Valkyrie-debugging-tools)

###IHMC SCS
1. Whole-Body Controller: Angular-momentum & ICP
   1. QP and Inverse Dynamics solver cascaded, input to QP are accelerations
   1. Controller described in journal paper by Twan Koolen et al., IJHR (March  ‘16?)
   1. ICP Papers (2x, we’ve covered them in DSOClets)
   1. Note: The controller described in the paper and the controller implemented diverged a lot! Re-write of controller is under way (to use a single QP, clean-up code) but no ETA.
   1. Will be the one controller we will be running for the next few months/half-year/year
2. SCS = Simulation Construction Set; should be our go-to simulator when executing whole body motion plans, walking etc.
   1. Gazebo/Drake for controller development
   1. SCS for motion planning etc.
3. Runs on robot (“link”), 2 cores reserved for it [keep that in mind when writing own controllers!]
4. Connect to via RemoteVisualizer from Workstation
5. Allows us to easily modify gains etc. from the user interface thanks to Yo variables -- global variables used like a buffer / shared memory
6. Expect 10s of bugs (Marco will cover more what we’ve seen, also notes on our wiki); at the moment poor arm tracking and collapsing torso, leg elasticity, etc.
7. Written in Java, monolithic codebase - everything’s now in the open (see below)
8. Limitations:
   1. Contact points (on the robot) need to be pre-specified, and there are a limited number of those that we can to keep simulation performant
   1. It’s a huge, ‘self-documenting’ codebase
  1. Watch out - there’s no real gitflow (Pull Requests/Issues) going on, commits are directly to the main branch so hard to keep track of what’s going on
9. Resources
   1. IHMC Repository - our fork - How to get started with IHMC’s codebase 


###Drake
1. Simulation (!) and Motion Planning Toolbox
   1. LCM Simulation Node coming soon-ish: pass in your commands via LCM, get state back via LCM
   1. Motion planning: ik, trajectory, etc.; optimisation-based motion planning
2. C++, Matlab, Python (experimental)
   1. Mostly written in (or being converted to) C++, bindings for Matlab (great coverage), Python (in the process of being established) existing
   1. Large set of examples / demos
3. Currently being under a lot of development and refactoring; more resources and generalisation through Toyota project
4. Note: OpenHumanoids/drake is somewhat trailing RobotLocomotion/drake