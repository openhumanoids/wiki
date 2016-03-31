# Installing the Xilinx software
The Xilinx installation files are stored on the treminator server at ```valkyrie/xilinx/```.
1. ```sudo apt-get install fxload libusb-dev```
1. Copy all files onto your computer. Make sure you have 30GB of disk space free after you have moved all the files over.
1. Extract ```Xilinx_ISE_DS_14.7_1015_1-1.tar```, ```cd``` into the destination directory and run ```sudo ./xsetup```
1. Go through the installations screens and selevt ```Install drivers``` on the screen with installation options. Choose ```/opt/Xlinx``` (selected by default) as the installation directory.

The installation may fail when installing the drivers. If this happens:
1. Copy unpack the ```install_drivers.tar.gz``` file from terminator server.
1. 
```bash
cd usb-driver
sudo ./setup_pcusb /opt/Xlinx/17.7/ISE_DS/ISE
make
sudo cp libusb-driver.so /opt/Xlinx
```
1. Reboot your machine

If this doesn't work, follow [these instructions](https://forums.xilinx.com/t5/Installation-and-Licensing/ISE14-3-device-driver-intall-failed-on-Ubuntu-12-04/m-p/287377#M4060)

# Flashing Synapse firmware onto the FPGA
1. Connect the Xlinx programmer to USB and to the Synapse card. The connection is successful when the LED on the programmer turns green.
1. Open a terminal and run ```source /opt/Xilinx/14.7/ISE_DS/settings64.sh```, then start the ```impact``` software. Cancel any popus that appear on startup.
1. Double click on ```Boundary scans``` item in the ```iMPACT flows``` list on the left side of the window.
1. Go to menu > ```Output``` > ```Cable Auto Connect```
1. Right click on the main window area and select ```Initialize chain```, answer ```No``` to the Auto Assign Configuration File dialog window that pops up. Click ```OK``` in the Programming dialog window that pops up after that.
1. Double ckick on the ```SPI/BPI``` dashed box above the Xlinx chip icon in the main window area.
1. Select the ```synapse.mpi``` file provided by NASA.
1. In the ```Select Attached SPI/BPI``` window, select: ```BPI PROM```, ```SPANSIONS29GL01GP```, and ```16``` for the data width. Confirm to close the window.
1. Now right click on the green ```Flash``` box and select ```Program```. Click ```Ok``` to start programming. This will take several minutes. A message confirming success will appear in the the main window at the end.

# Installing NASA software
1. Install and boot into kernel 3.13.0-39-lowlatency:
```bash
sudo apt-get install linux-headers-3.13.0-39-lowlatency linux-image-3.13.0-39-lowlatency
```
1. Make sure your user has sudo rights. Also, add your user to the ```dailout``` group (use ```usermod -aG dialout <user_name>``` and log out for this to take effect).
1. Install the dependencies as described [here](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/Setup-Valkyrie-Developer-Environment).
1. Go through steps ```Pre-requisites``` and ```Setup``` from the [Valkyrie Source Code](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/Valkyrie-Source-Code) wiki.
1. Run 
```bash
vcs import --input ~/val_workspaces/public_full_workspace.yaml ~/val_indigo/src/
source ~/.bashrc
```
1. Finish the setup by following ```Final Steps``` from the [Valkyrie Source Code](https://github.com/NASA-JSC-Robotics/valkyrie/wiki/Valkyrie-Source-Code) wiki.

# Setting up Robonet communication
1. Turn on logic power for the arm (yellow LED on the Athena board will turn on - visible with the covers off).
1. In 1st terminal window: run ```smtcore``` (no output is expected).
1. In 2nd terminal window: run ```control_bench``` which is an alias for ```control -r val_indigo/install/share/nasa_val_config_files/robonet_schedules/forearm_testbed.yaml```. Create the alias in your ```~/.bash_aliases``` if you want to use it. The ```forearm_testbed.yaml``` file was creates specifically for use of the left arm with the test bed. Other files can be loaded instead (e.g. right arm).
1. At this point, a green LED next to the yellow one will turn on. If it doesn't comms are off.
1. Run ```smteditor``` to read/edit values.
