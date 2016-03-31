To log all variables from the robot you can setup an external logger. Instructions are provided on this page.

####Step-by-step guide

#####Logger computer system requirements

1. Tons of hard drive space
   1. The logger can take 100GB/hour easily, depending on the number of variables and video streams.
2. 4 GB RAM (8 GB when more than one video stream is captured)
3. 2.5GHz or faster Intel i7 or Xeon E3, E5 or E7
   1. 1 + n cores, where n is the number of video streams
4. (optional) BlackMagic Decklink Mini Recorder capture cards
   1. https://www.blackmagicdesign.com/products/decklink
   1. The other Decklink capture cards might work, but are not tested yet.

#####Setting up a logging computer
1. Install Ubuntu 14.04
2. Install BlackMagic software
> Download the Desktop Video software from the Blackmagic site
> https://www.blackmagicdesign.com/support/family/capture-and-playback
> Use at least version 10.5.4 Update. You do not need the SDK, just the plain Desktop Video product. The registration has a "Download only" link in the bottom left to bypass.

3. Update Blackmagic firmware for each Decklink card (first card is 0, second 1, etc).
   1. BlackmagicFirmwareUpdater update [Decklink card]
4. Reboot the computer
5. Publish the logger
   1. Clone IHMCOpenRoboticsSoftware
   1. cd IHMCOpenRoboticsSoftware/RobotDataCommunication
   1. ./publishLogger.sh [user]@[host]
6. Create ~/.ihmc/IHMCNetworkParameters.ini
logger=[logger host name or IP, reachable from robotController]
robotController=[robot controller hostname or IP, reachable from logger]
7. Create the directory to log to
   1. mkdir ~/robotLogs
   1. Instead of making the ~/robotLogs this can also be a symlink to a NAS.

#####Setting up robot controller
The robot controller is responsible for determining what Decklink cards are recording during a logging session. This is configured in ~/.ihmc/IHMCNetworkParameters.ini.

To make the logger work, IHMCNetworkParameters.ini needs a "logger" entry and optionally has a "loggedCameras" entry.
> logger=[logger host name or IP, reachable from robotController]
> loggedCameras=[comma separated list of Decklink card ID's to capture]

The Decklink cards are numbered sequentially starting from 0. Video resolution is auto-detected. If you want to log Decklink cards 0 and 2, set"loggedCameras=0,2". If you only want to log Decklink card 1, set "loggedCameras=1". If you do not want to log any cameras, remove the
loggedCameras line or set it to an empty string.

#####Starting the logger
1. Login the logging computer
2. cd RobotDataCommunication/bin
3. ./RobotDataCommunication

The logger will listen for controller sessions coming online. It will dispatching logging sessions accordingly. There is no need to restart the logger.