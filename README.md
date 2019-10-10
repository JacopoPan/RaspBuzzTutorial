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

If you have further questions about this tutorial, contact <jacopo.panerati@robotics.utias.utoronto.ca> or <benjamin.ramtoula@polymtl.ca>

If you have further questions about [ROSBuzz](https://github.com/MISTLab/ROSBuzz) or [XBeeMav](https://github.com/MISTLab/XbeeMav), contact 
<david.st-onge@etsmtl.ca> or <vivek-shankar.varadharajan@polymtl.ca>

If you have further questions about [Buzz](https://github.com/MISTLab/Buzz/), contact 
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
- click the second icon from the top left of XCTU's interface, "Discover radio modules connected to your machine"
- select the appropriate ports (e.g., usbserial-XXXX) and "Next >"
- **if the SX 868 is brand new**, leave the default "Set port parameters"
- **if the SX 868 was previously configured with this profile**, set port parameters to `Baud rate: 230400; Data bits: 8; Parity: None; Stop Bits: 1; Flow control: None`
- click "Finish"
- once found, tick the device(s) and click "Add selected devices"
- select the devices (one by one) on the left panel of XCTU
- on the right panel, locate the small arrow besides the "Profile" icon
- click it and select "Apply configuration profile"
- navigate to file `profile_A007_sseac.xpro`, click "Open"
- the profile will be loaded to the SX 868

A profile for the XBee PRO is also available in this [repository](http://git.mistlab.ca/bramtoula/spiri-resources.git)



# Step 2: Prepare the Raspberry Pi  3s

We now want to install our Raspberry Pi 3 Model B+ with ROS Kinetik Kame. While this can be done in multiple ways (e.g. [with Raspian](http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Kinetic%20on%20the%20Raspberry%20Pi) or using [Ubuntu Mate](https://ubuntu-pi-flavour-maker.org/download/) and the [traditional ROS instructions](http://wiki.ros.org/kinetic/Installation/Ubuntu)), the simplest one is probably to use one of the pre-installed images made available by [Ubiquity Robotics](https://downloads.ubiquityrobotics.com/pi.html).

- [format your Raspberry Pi's SD card ](https://www.sdcard.org/downloads/formatter/) (if necessary)
- flash Ubiquity's image to the card using [Etcher](https://www.balena.io/etcher/) or `gnome-disk-utility`

To avoid certificates validity issues when connecting to the internet, after booting the Raspberry Pi for the first time open a terminal (CTRL+ALT+T) and type 

```
$ sudo crontab -e
```

select your preferred text editor (i.e., `vim`) and add line

```
@reboot date -s "20 JUL 2019 20:17:00"
```

save and close. Reboot. Before moving to the next step, install these additional ROS packages:

`sudo apt-get install ros-kinetic-mavros* ros-kinetic-tf2-geometry-msgs ros-kinetic-image-transport ros-kinetic-cv-bridge libjson-glib-dev`



# Step 3: Install the relevant repositories

These are:

- Buzz (containing the VM interpreting scripts using the Buzz language)
- ROSBuzz (containing the ROS node running the Buzz VM)
- XBeeMav (containing the ROS node allowing Buzz VMs on different Raspberry Pis to communicate through the XBee radios)

### Install Buzz

```
$ git clone https://github.com/MISTLab/Buzz.git
$ cd Buzz
$ mkdir build
$ cd build
$ cmake ../src
$ make
$ sudo make install
$ sudo ldconfig
```

### Install ROSBuzz and XBeeMav

To compile ROSBuzz on a Raspberry Pi 3 Model B+, you'll need a swap file. In a terminal, type: 

```
$ sudo fallocate -l 2G /swapfile
$ chmod 600 /swapfile
$ sudo mkswap /swapfile
$ sudo swapon /swapfile
```

Check the output of `sudo swapon -s` to verify that you now have a 2GB swap file. Then, type `vim /etc/fstab` and append the following line:

```
/swapfile   none    swap    sw    0   0
```

Also edit `sudo vim /etc/sysctl.conf` by appending line:

```
vm.swappiness=10
```

Finally, run:

```
$ sudo sysctl -p
$ echo 'export ROS_PARALLEL_JOBS=-j2' >> ~/.bashrc 
```

Clone and compile the ROSBuzz node:

```
$ mkdir -p ROS_WS/src
$ cd ROS_WS/src
$ git clone https://github.com/JacopoPan/ROSBuzz.git rosbuzz
$ git clone https://github.com/JacopoPan/XbeeMav.git xbeemav
$ cd ..
$ catkin_make -DSIM=0
$ echo 'home/ubuntu/ROS_WS/devel/setup.bash' >> ~/.bashrc 
```

Make sure you have the following files in your `~/.bashrc` file (add them otherwise) :

```
source /opt/ros/kinetic/setup.bash
source home/ubuntu/ROS_WS/devel/setup.bash
```

> Note: in this tutorial we are using forked versions of ROSBuzz and XBeeMav, for the most up-to-date sources, clone `https://github.com/MISTLab/ROSBuzz` and `https://github.com/MISTLab/XbeeMav` instead.

### Adding a never-used-before XBee 

If you want to use an XBee radio that was never used before with `XBeeMav`, you should add its MAC address to the XML database:

```
$ vim ~/ROS_WS/src/xbeemav/Resources/database.xml
```

and add a new line in the form:

```
<Device Address="[Integer-Id]">[MAC-Address]</Device>
```



# Step 4: Run a 2-device demo

- Perform steps 1-3 on 2 Raspberry Pi 3 Model B+
- Connect each of the 2 Pis to a Digi XBee SX (e.g., again using an interface board and USB)

If you are using the sources from `https://github.com/JacopoPan/ROSBuzz` you can run

```
$ roslaunch rosbuzz neil.launch
```

on the first one and 

```
$ roslaunch rosbuzz buzz.launch
```

on the second one. The two devices will re-enact a brief 5-line exchanges from [Apollo 11 transcripts](https://www.hq.nasa.gov/alsj/a11/a11transscript_cm.pdf).

Look at files `ROSBuzz/buzz_scripts/neil.bzz` and `ROSBuzz/buzz_scripts/buzz.bzz` to understand how the communication happens. The message passing is base on two different paradigms:

- broadcast/listen - each Raspberry Pi `broadcast` messages as (key, value) pairs and `listen` to certain keys
- stigmergy - each Raspberry Pi reads and write into a virtual dictionry containing (key, value) pairs

More examples on the use of the Buzz language are available in its [wiki](https://the.swarming.buzz/wiki/doku.php?id=buzz_syntax_cheatsheet)

> Note: files `ROSBuzz/launch/buzz.launch` and `ROSBuzz/launch/neil.launch` contain lines `<arg name="latitude" value="45.0"/>` and `<arg name="longitude" value="73.0"/>` to set "fake" latitude and longitude positions that would prevent ROSBuzz/XBeeMav from working as expected, if missing. Line `<arg name="script" value="script-name"/>` defines which of the scripts in `ROSBuzz/buzz_scripts` is executed.

> Troubleshooting: if an XBee module becomes irresponsive, press the reset buttonon the inteface board.

