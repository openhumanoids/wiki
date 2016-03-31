I usually make a persistent connection for when I want to use the sensor on my laptop.

Create a new connection from the Network Manager, say called "multisense". Settings should be:
* General: don't automatically connect when available
* Ethernet: set MTU=9000
* ipv4 settings:
  *    method=Manual
  *    ip address = 10.66.171.xx (not 21)
  *    netmask = 255.255.255.0
  *    gateway = 10.66.171.1

Then save settings and switch to the new multisense network (with the ethernet cable from the sensor connected to the computer). Alternatively you can run ifconfig with these options on eth0 or appropriate network interface.

Run "multisense-lcm-driver" (most options are the same as before, but run --help to see the list).

Multisense Dimensions: http://files.carnegierobotics.com/products/MultiSense_SL/MultiSense_SL_interface.pdf