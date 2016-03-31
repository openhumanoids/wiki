1. badArmTrajectoryPacketHandling: 
  - fixes issue 17. trajectory handling not correct of ihmc
  - out of joint limits not properly parses
  - cleared the joint trajectory of a single joint
  - which created qDD = NaN and then fall

2. jerk across arm plans - possibly only when moving arms and not with WBP
  - Solution: UoE added offset to timesteps at start of plan

3. Bias correction fixed by IHMC