    Current model used by IHMC: V1_sim_shells_no_arms.sdf or V1_sim_shells.sdf
    You can tell SCS to use a different model by changing the config in the class called ValkyrieConfigurationRoot
    This is also the model loaded in to the controller. So changing it here changes it everywhere
    
    IHMC side:
    IHMC has both URDF and SDF files.:
    - SDF (invented for Gazebo, standard maintained by OSRF) has tags for simulation only but IHMC also use it for the robot
    - URDF is our preference.
    
    Originally they generated the SDF from URDF using gzsdf (a gazebo tool) but since then
    they have made mass parameter adjustments. They hope to get back to having a single URDF
    gold standard, but for now, the SDF is the gold standard. Nonetheless we will use the
    closest URDF because the changes are minimal.
    
    V1_sim.[sdf|urdf] is almost identical to the Drake model. They don't use that one.
    V1_sim_shells.[sdf|urdf] is similar but has extra mass which was the missing leather shells
    V1_sim_shells_no_arms.[sdf|urdf] is a variant without arms.
    
    Drake side: (~/drc/software/models/valkyrie)
    V1_sim.urdf is what I got from NASA in April 2014
    V1_sim_mit.urdf is the version I made to be 
    
    I made these changes:
    Rename every joint to match Atlas e.g. UpperNeckExtensor to neck_cy. I WANT TO AVOID DOING THIS AGAIN
    Change the mesh file paths. This fine
    Remove '/' from leading joint names due to a drake bug. This is needed for now
    
    So the process is to take V1_sim_shells.urdf 
    - write a script to make V1_sim_mit.urdf
    - use urdf_to_graphviz to check that its a proper urdf
    - modify the director_config.json file as needed
    - launch drake-designer and make sure it loads
    - write a script to convert V1_sim_mit.urdf to  V1_sim_mit_drake.urdf
    - Both of these scripts should use the code in /drc/software/models/atlas_v4/mit_modifications/ for inspiration
    - Remember that we are likely to periodically get an update of the robot model so we want to be able to update easily