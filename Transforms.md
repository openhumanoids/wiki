**Details about the Valkyrie and Atlas Coordinates and Conventions**

***About Frames***

There are two ways to calculate the position of a part of the robot relative to another:
joint angles, paired with a URDF and a solver for forward kinematics.
frames messages paired with the coordinate_frames block in the cfg file.

*Purpose*

To be able to answer the question ‘what is the transform between BODY and CAMERA_LEFT at time = XXX’. It is used to create the transform of a LIDAR scanner (to accurate 3D point clouds) and also to provide camera and IMU calibrations for state estimation (e.g. pronto).

bot_frames is a C/C++ library which is part of libbot (located in drc/software/externals)
An example query in C/C++ returns the 4x4 transformation matrix between two frames as follows:

double scan_to_local[16];
get_trans_with_utime( bot_frames ,  “SCAN” , "local", msg->utime, scan_to_local);

*Populating Frames*

The cfg file contains a coordinate_frames block which describes the connection between different IMPORTANT frames e.g.
local -> body -> head -> CAMERA_LEFT
Note that not all frames are populated and also that some convienence frames are mentioned e.g. pose_bdi and pose_alt

To populate
* Some of these relative frames are fixed e.g. head -> CAMERA_LEFT. These should agree with the compariable frame in the URDF.
* Others continuously change due to the robot’s motion e.g. body -> head. These are updated by publishing an LCM message with a relative position and orientation transforms (x,y,z and quaternion) to the message specified in the cfg bloc e.g. BODY_TO_HEAD

Ways to publish updates:
* Some sensors publish the update transform directly (e.g. the Multisense SL)
* The root position of the robot is usually provided by POSE_BODY which is published by Pronto - or otherwise published by the state estimator directly.
* Typically frames are provided by joints2frames which solves forward kinematics between two frames and publishes the required transforms e.g. BODY_TO_HEAD


***Issues***

Joints2Frames is currently set up for Atlas but should be modified for valkyrie
I would like to make the frames names better match link names in the urdf, but currently we use CAMERA_LEFT for the left lense of the sensor head and SCAN for the laser mirror.

Checking Transforms***
In the drake-designer, view -> frame visualization. This shows the current frames:
Bot frames come from the above system
Link frames come from the URDF using FK 
Typically these frames should be identical.



*CFG files*
There is one main cfg file for each robot. For MIT’s Atlas robot it is drc_robot.cfg
It includes about 5 files which are are combined into a

The cfg string is published on PARAM_UPDATE channel and various modules use it to store settings. It contains the corrdinate_frames block as well:
* planar_lidars for the Multisense Lidar
* camera - all camera calibrations
* state_estimate - used by Pronto

It also contains a number of unused blocks e.g. network and viewer which were used during the DRC finals.

bot_param can be used by C/C++ to query the config file, for example:

double vxyz = bot_param_get_double_or_fail(param, "state_estimator.legodo.r_vxyz");

***Producing the final output***

The final step in state estimation is outside of pronto/se-fusion. Pronto publishes POSE_BODY which is the position of the pelvis. This is then combined by state-sync (a small process) with the components of the various devices to produce EST_ROBOT_STATE. This is used by all software modules downstream from this to draw the robot.

Currently:
* POSE_BODY  - produced by pronto
 * + ATLAS_STATE  - produced by Atlas driver
 * + MULTISENSE_STATE  - produced by Multisense driver (1)
 * + LEFT_ROBOTIQ_STATE  - ...
 * + RIGHT_ROBOTIQ_STATE  - ...
 * = EST_ROBOT_STATE  - state_sync combines the above into a single message

More general in future:
* POSE_BODY  - produced by pronto
 * + CORE_ROBOT_STATE  - produced by Atlas driver
 * + MULTISENSE_STATE  - produced by Multisense driver (1)
 * + LEFT_ROBOTIQ_STATE  - ...
 * + RIGHT_ROBOTIQ_STATE  - ...
 * = EST_ROBOT_STATE  - state_sync combines the above into a single message

***Glossary***

* URDF: very large text file describing the kinematic tree of a robot. Gives many details about the robot. It does not contain information about parameters or setting for algorithms.
* NB: the val_description directory contains the URDF of the Valkyrie robot. (The valkyrie directory contains a URDF of an older version from NASA)
