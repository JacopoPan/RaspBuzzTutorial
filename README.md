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
$ git clone https://github.com/JacopoPan/RaspBuzzTutorial.git
$ cd RaspBuzzTutorial/
```

File `profile_A007_sseac.xpro` in folder `RaspBuzzTutorial` can be uploaded to each of your [XBee SX 868](https://www.digi.com/products/embedded-systems/digi-xbee/rf-modules/sub-1-ghz-modules/digi-xbee-sx-868) radios by following these steps:

- install Digi's [XCTU](https://www.digi.com/products/embedded-systems/digi-xbee/digi-xbee-tools/xctu#productsupport-utilities) (tested with XCTU 6.4.3 on OS X Mojave and Ubuntu 16.04)
- connect your XBee SX 868 to the machine running XCTU through USB (using, for example, the interface board included in the development kit, part: [XK8X-DMS-0](https://www.digikey.com/product-detail/en/digi-international/XK8X-DMS-0/602-2117-ND/))
- click the second icon from the top left of XCTU, "Discover radio modules connected to your machine"
- select the appropriate ports (e.g. usbserial-XXXX) and "Next >"
- **if the SX 868 is brand new**, leave the default "Set port parameters"
- **if the SX 868 was previously configured with this profile**, set port parameters to `Baud rate: 230400; Data bits: 8; Parity: None; Stop Bits: 1; Flow control: None`
- click "Finish"
- once found, thick and choose "Add selected devices"
- select the device on the left panel of XCTU
- on the right panel, locate the arrow besides the "Profile" icon
- click it and select "Apply configuration profile"
- navigate to file `profile_A007_sseac.xpro`, click "Open"
- the profile will be loaded to the SX 868

A profile for the XBee PRO is also available in this [repository](http://git.mistlab.ca/bramtoula/spiri-resources.git)

# Step 2: Prepare the Raspberry Pi  3s

We now want to install our Raspberry Pi 3 Model B+ with ROS Kinetik Kame. While this can be done in multiple ways (e.g. [with Raspian](http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Kinetic%20on%20the%20Raspberry%20Pi) or using [Ubuntu Mate](https://ubuntu-pi-flavour-maker.org/download/) and the [traditional ROS instructions](http://wiki.ros.org/kinetic/Installation/Ubuntu)), the simplest one is probably to use one of the pre-installed images made available by [Ubiquity Robotics](https://downloads.ubiquityrobotics.com/pi.html).

- [format your Raspberry Pi's SD card ](https://www.sdcard.org/downloads/formatter/) (if necessary)
- flash Ubiquity's image to the card using [Etcher](https://www.balena.io/etcher/)

To avoid certificates validity issues when connecting to the internet, after booting the Raspberry Pi for the first time open a terminal (CTRL+ALT+T) and type 

```
$ sudo crontab -e
```

select your preferred text editor (i.e., `vim`) and add line

```
@reboot date -s "20 JUL 2019 20:17:00"
```

save and close. Reboot.

Before moving to the next step, install these additional ROS packages:
`sudo apt-get install ros-kinetic-mavros* ros-kinetic-tf2-geometry-msgs ros-kinetic-image-transport ros-kinetic-cv-bridge libjson-glib-dev`

# Step 3: Install the relevant repositories

# Step 4: Run a 2-device demo
