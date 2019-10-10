# RaspBuzzTutorial

The objective of this tutorial is to show how one can use jointly use:

- the [Raspberry Pi 3 Model B+](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/),
- the [Buzz scripting language](https://the.swarming.buzz/wiki/),
- the [Robotic Operating Systems](https://www.ros.org/about-ros/) (ROS)
- and, [Digi XBee SX 868](https://www.digi.com/products/embedded-systems/digi-xbee/rf-modules/sub-1-ghz-modules/digi-xbee-sx-868) RF modules


as a simple, customizable, and low-cost platform for multi-robot systems and IoT applications.

# Main steps

This tutorial contains 4 steps:

- prepare the XBee modules;
- prepare the Raspberry Pi 3s;
- install the relevant repositories;
- run a 2-device demo.

For additional information on this tutorial contact <jacopo.panerati@robotics.utias.utoronto.ca> or <benjamin.ramtoula@polymtl.ca>

For additional information on [ROSBuzz](https://github.com/MISTLab/ROSBuzz) and [XBeeMav](https://github.com/MISTLab/XbeeMav) contact 
<david.st-onge@etsmtl.ca> or <vivek-shankar.varadharajan@polymtl.ca>

For additional information on [Buzz](https://github.com/MISTLab/Buzz/) contact 
<giovanni.beltrame@polymtl.ca> or <cpinciroli@wpi.edu>

# Step 1: Prepare the XBee modules

Get the XBee configuration profile.

```
git clone https://github.com/JacopoPan/RaspBuzzTutorial.git
cd RaspBuzzTutorial
```

To set the right profile on the Xbee, you should install XCTU on your computer (https://www.digi.com/products/xbee-rf-solutions/xctu-software/xctu#productsupport-utilities).
Using this software, you can load the profile from one of the files provided in the spiri-resources repo (http://git.mistlab.ca/bramtoula/spiri-resources.git) in the  `xbees`folder.
We have one file for the Xbee PRO model « profile_8074.xpro », and one for the Xbee SX « xbee sx.xpro ».

# Step 2: Prepare the Raspberry Pi  3s

# Step 3: Install the relevant repositories

# Step 4: Run a 2-device demo
