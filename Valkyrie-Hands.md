**Email on Thursday, November 05, 2015 1:12 PM**

Hi Jordan,

We have some UK collaborators who are keen to do advanced grasping with Valkyrie.

They have a few queries, can you answer them:
- what is the control interface for the hands i.e. can you do torque or current control of the fingers or is it just position
- has anyone used the tactile sensors on the fingers? Or are the tactile sensors even on the newer models?
- I remember someone saying that the CamBoards (?) cameras at the thumbs weren't great. Any idea how bad? Can they produce greyscale images?
If you have any other info about the hand design or your testing of them that would be great. I think you were changing the material when I was there in June.

One of the groups wants to install their own hands. 

**Reply 1**

From: Lack, Jordan T. (JSC-ER411)[Jacobs Technology, Inc.] 

Maurice,

I am in Korea for humanoids, but I will answer what I can and have CC'd Lyndon Bridgwater(our hand designer) to clean up where I am unable to help.

- So far we have only done position control. I know torque control is not available, but its possible current control is I am not sure. I know we have not tried current control though.
- We had a co-op do some looking into the pressure sensors in the fingers if thats what you are referring to. I don't really recall his findings but I seem to recall hearing that they saturate easily and due to the stuff they are encapsulated in, their behavior is pretty different from one sensor to the next.
- I think we took the camboards out of the hands but I could be wrong there. I am not sure what they are capable of, but Lyndon could probably give you the exact model of the sensor and you could maybe look it up.
- On installing of their own hands: I am only speaking for myself here and Lyndon could better answer this, but I doubt that would really be an option. We have talked about offering a more commercially available hand as an option, but it would involve us designing a completely new forearm and wrist I think. The way the forearms/hands/wrists are packaged together, you can't just yank a hand off and attach a new one. 

Hopefully this helps, if I have said something incorrect or left out some info help me out Lyndon! 

Thanks,
Jordan

**Reply 2**

Hi Maurice, 
Much of what Jordan responded is correct.  Here are a few extra details. 
 There is likely a chance for current based control for the finger actuators.  Currently there is no option for direct tension or torque control for the fingers of Valkyrie like we can with Robonaut 2. 

The finger sensors are installed and functional, though they are a little sensitive and saturate under high loads.  But they are robust and don't break due to those high loads.  The primary focus that our intern was looking at the sensors was to investigate the use of the sensors for vibration sensing, to sense slipping.  The sensors do not have a good bandwidth and as such don't work well for this application. 

As for the camboard nanos, they are not being installed in the new Valkyrie systems. Though everything is still scared to receive them.  As for the quality, the cameras gave 640 x 480 depth map and gray scale.  General decent camera, though the installation location cause issues.  The thumb and hand cover nearly half of the visible workspace of the camera. This is not entirely a bad thing though working with it was not that useful during our initial activities.  

With regard to the development of unique hands for Valkyries the interface at the J4/J5 separation is well defined and we have looked into allowing this to be a module up for teams to work with. Though the integrated wrist roll, pitch, and yaw make simple hand replacements a little more difficult. 

If there are any other details you have a question on I am happy to answer. 

Regards, 

Lyndon 



Yeah we can current limit the fingers. Does that answer your question? I am not 100% sure I understand what your asking, but its possible the answer is "it doesn't know when to stop moving". We were probably just doing position control and there are stretchy tendons in there to keep it from crushing things in most cases. We haven't picked up any fragile things though, so we haven't had to deal with cases in which we needed to be careful not to grasp to firmly. 

Jordan