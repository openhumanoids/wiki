March 2016

### Purpose:

To validate performance of the IHMC walking controller with the NASA Valkyrie in terms of the walking speed parameters: swing speed, transfer time and step length

### Tool description:

This tool generates a custom footsteps plan. The user can specify:
* number of steps (Num Steps)
* lateral gap between right and left footstep (Step Width)
* forward distance between consecutive steps of the same foot (i.e. between two left footsteps) (Forward Step)
  * (This is twice the distance called "Forward step" in the Drake footstep planner)
  * 0.6m corresponds to the heel of the leading foot being about 0.02m beyond the other foot's toe
* transfer time that is the lateral balance time from one leg to the other (IHMC Transfer Time)
* swing time that is the time of a step forward (IHMC Swing Time)

The default, maximum and minimum values have been set depending on those in ValkyrieWalkingControllerParameters.java.

### Using the tool:
1. Open the procman-sheriff and start the ui from the menu bar. Note that Director should be started using the command 
```
director -c $DRC_BASE/software/config/val_sim_scs/robot.cfg -val2 --startup $DRC_BASE/software/modules/ohdirector/runstartup.py
```
2. In the Task Panel select the `Manual Walking` tab
3. Click on `Footsteps Plan`
4. Customize the walking plan using the options on the left side of the panel
5. Click on `EXECUTE Plan`

## Tests

**Set Up:**
 * COM height: slider 8 half way up to value 60
    * TODO: determine corresponding fixed point
 * Number of Steps: 6
 * 3 dimensions:
    * Transfer Time (columns)
    * Swing Time (rows)
    * Forward Step (boxes). Values taken into account: 0.35 m (default) to 0.60 m (maximum). 

**Notation:** 
 * F = Forward motion
 * B = Backward motion
 * V = Validated by assumption (cell not tested)
 * X = Failure by assumption (cell not tested)

***

**Notes:**
 * For each cell, I tried to find the maximum length forward step Valkyrie can perform. I increased the length from 0.35 m up to a failure.
 * _Success up to_ means that Valkyrie completed a walking plan of 6 steps. However, smooth walking performances can be achieved for values below the upper threshold (< 0.60 m).   

***

**Results:**

| Swing\Transfer |2.0 s          |1.5 s          |1.0 s          |0.75 s         |0.5 s          |0.25 s         |
|----------------|:--------------|:--------------|:--------------|:--------------|:--------------|:--------------|
|**1.5 s**       | _**Success:**_<br>F up to `0.60 m`<br>B up to `0.60 m`<br> | V | V | V | _**Comments:**_<br>Cell not tested but transfer value validated up to `0.45 m` | _**Failure:**_<br>F `0.35 m`<br>B `0.35 m`<br>**Comments:** Transfer time not supported |
|**1.2 s**       | V | _**Success:**_<br>F up to `0.60 m`<br>B up to `0.60 m` | V | _**Success:**_<br>F up to `0.60 m`<br>B up to `0.60 m`<br> | _**Success:**_<br>F up to `0.60 m`<br>B up to `0.45 m`<br>_**Failure:**_<br>B `0.60 m`<br>**Comments:**<br>B right foot steps on the left one during swing causing failure | X |
|**1.0 s**       | V | V | V | _**Success:**_<br>F up to `0.50 m`<br>B up to `0.35 m`<br>_**Failure:**_<br>B `0.45 m`<br>**Comments:**<br>F & B hesitates on the second swing right leg, B this hesitation causes a failure | _**Success:**_<br>F up to `0.35 m`<br>_**Failure:**_<br>B `0.35 m` | X |
|**0.9 s**       | V | V | _**Success:**_<br>F up to `0.60 m`<br>B up to `0.60 m`<br>**Comments:**<br>F & B the hip shakes, the walking is not smooth and stable | _**Success:**_<br>F up to `0.45 m`<br>B up to `0.45 m`<br>_**Failure:**_<br>F `0.50 m`<br>**Comments:**<br>balance lost during hesitation on right foot swing | _**Success:**_<br>F up to `0.35 m`<br>_**Failure:**_<br>B `0.35 m`<br>**Comments:**<br>F hesitates on the second swing right leg, B loses equilibrium laterally during transfer and falls | X |
|**0.6 s**       | X | _**Failure:**_<br>F `0.35 m`<br>B `0.35 m`<br>**Comments:** Swing time not supported | _**Failure:**_<br>F `0.35 m`<br>B `0.35 m`<br>**Comments:** Swing time not supported | X | _**Failure:**_<br>F `0.35 m`<br>B `0.35 m`<br>**Comments:** Swing time not supported, it did not even move, immediate failure | X |

***

**Conclusion:**
With this test we achieved acceptable performance with Transfer Time `0.75 s`, Swing Time `1.2 s`, length of each step `0.60 m` for 6 steps. However, we were pushing the robot performance in terms of walking speed so the walking gaits were not very smooth (i.e. watching it walking I could see some hesitation). The robot can perform smoother walking gaits with this timings if step length `<0.60 m`. Note that a Swing Time 0.2 seconds longer (1.0 s --> 1.2 s) is not perceived by the audience but makes the difference for the robot (failure --> success). 
