## ROS sdk
http://wiki.ros.org/xsens_mti_driver
```
source devel/setup.bash
roslaunch xsens_mti_driver xsens_mti_multi_nodes.launch
roslaunch xsens_mti_driver xsens_mti_node.launch
roslaunch xsens_mti_driver display.launch
```

### Device info:
- Device: MTi-300-2A8G4, with ID: 03782BC3 (The one on the dog)
- Device: MTi-300-2A8G4, with ID: 03782BC4
- Device: MTi-680G-8A1G6, with ID: 0080005009


### The MTi-1 (Motion Tracker Development Board) is not recognized.
Support for the Development Board is present in recent kernels. (Since June 12, 2015). If your kernel does not support the Board, you can add this manually:


`sudo /sbin/modprobe ftdi_sio $ echo 2639 0300 | sudo tee /sys/bus/usb-serial/drivers/ftdi_sio/new_id`
The device is recognized, but I cannot ever access the device.
Make sure you are in the correct group (often dialout or uucp) in order to access the device. You can test this with:

```
$ ls -l /dev/ttyUSB0

crw-rw---- 1 root dialout 188, 0 May  6 16:21 /dev/ttyUSB0

$ groups

dialout audio video usb users plugdev
```
If you aren't in the correct group, you can fix this in two ways:

1) Add yourself to the correct group. You can add yourself to it by using your distributions user management tool, or call:

`sudo usermod -G dialout -a $USER`
Be sure to replace dialout with the actual group name if it is different. After adding yourself to the group, either relogin to your user, or add the current terminal session to the group:

`newgrp dialout`

2) Use udev rules. Alternatively, put the following rule into /etc/udev/rules.d/99-custom.rules:

`SUBSYSTEM=="tty", ATTRS{idVendor}=="2639", ACTION=="add", GROUP="$GROUP", MODE="0660"`
Change $GROUP into your desired group (e.g. adm, plugdev, or usb).
