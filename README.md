# RaspBuzzTutorial

update in forked ROSBuzz
buzz.bzz
neil.bzz

references: me, benjamin
david, vivek
giovanni, carlo


OLD https://ubuntu-pi-flavour-maker.org/download/ + http://wiki.ros.org/kinetic/Installation/Ubuntu
OLD http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Kinetic%20on%20the%20Raspberry%20Pi


alternative OS options
https://downloads.ubiquityrobotics.com/pi.html <--

prepare the sd card
https://www.sdcard.org/downloads/formatter/
https://www.balena.io/etcher/

sudo crontab to bring date to 2019 (to connect to the internet)

# Xbee setup

Now that you have an assembled Spiri, you can verify that the Xbee module works.
When powering on the drone, after a few seconds you should see a red light on the Xbee shield. If you don’t, there might be some compatibility issues between the TX and the Elroy version you have (see the part about flashing the TX earlier).

## Add the profile
To set the right profile on the Xbee, you should install XCTU on your computer (https://www.digi.com/products/xbee-rf-solutions/xctu-software/xctu#productsupport-utilities).
Using this software, you can load the profile from one of the files provided in the spiri-resources repo (http://git.mistlab.ca/bramtoula/spiri-resources.git) in the  `xbees`folder.
We have one file for the Xbee PRO model « profile_8074.xpro », and one for the Xbee SX « xbee sx.xpro ».


digi xbee resources
http://cms.digi.com/resources/documentation/Digidocs/90001538
http://cms.digi.com/resources/documentation/digidocs/90001458-13/










## Install additional ROS packages (?)
We use additional packages you also need to install on the drone :
`sudo apt-get install ros-kinetic-mavros* ros-kinetic-tf2-geometry-msgs ros-kinetic-image-transport ros-kinetic-cv-bridge libjson-glib-dev`


## Install Buzz

We also need to install Buzz :
```
cd ~/installations
git clone https://github.com/MISTLab/Buzz.git
cd Buzz
mkdir build
cd build
cmake ../src
make
sudo make install
sudo ldconfig
cd ~
```
Now we should have everything needed for our code to work!

## Install and build our drones workspace



install rosbuzz
https://github.com/MISTLab/ROSBuzz/tree/master
https://tecadmin.net/enable-swap-on-ubuntu/
-DSIM=0





We can now download and build our workspace which contains the code we will run :
```
mkdir -p ~/ROS_DRONES_WS/src
cd ~/ROS_DRONES_WS/src
git clone https://github.com/MISTLab/ROSBuzz.git rosbuzz
git clone https://github.com/MISTLab/XbeeMav.git xbeemav
```
We provide files to build the workspace, and a launch file, which are in the `spiri-resources` repo you downloaded earlier. To build :
```
cd ~/ROS_DRONES_WS/
cp ~/installations/spiri-resources/build_spiri.sh ~/installations/spiri-resources/build.sh ~/installations/spiri-resources/spiri.launch .
./build_spiri.sh
```

Make sure you have the following files at the end of the bashrc file (`nano ~/.bashrc`) :
```
source /opt/ros/kinetic/setup.bash
source ~/ROS_DRONES_WS/devel/setup.bash
source ~/ROS_DRONES_WS/src/rosbuzz/misc/cmdlinectr.sh
```

https://askubuntu.com/questions/211716/add-environment-variable-to-bashrc-through-script



## install xbeemav
https://github.com/MISTLab/XbeeMav


## Verifying the database (and take off heights)
Now we have to check that the Xbee is in our database file. To do so, right down the serial number of the Xbee (written on it or found in XCTU).
Then, on the Spiri, check the database.xml file :
`nano ~/ROS_DRONES_WS/src/xbeemav/Resources/database.xml`
If you don’t see the serial number of your Xbee here, you should add it with a new ID number.

- To get xbeemav to ouput a robot id the xbee module S.N. must be set in the ressources/database.xml


If your Xbee was already in the database, you can check the takeoff height associated with it in the following file :
`nano ~/ROS_DRONES_WS/src/rosbuzz/buzz_scripts/include/utils/takeoff_heights.bzz`

If your Xbee was not there and you had to add it, you should add a line in that file setting a takeoff height for the ID you added.
If you had to add your Xbee to the files, it would be a good idea to push the changes you made to the repo.


- reset if they go to sleep

....

# Running the rosbuzz node manually
For this part, you should have :
- An assembled and calibrated Spiri connected to a network.
- A computer connected to the same network as the Spiri.
- The drones workspace built, installed, and sourced.
- An Xbee registered in the database with an appropriate take off height.

To run the rosbuzz node simply use :
`roslaunch ~/ROS_DRONES_WS/spiri.launch`.

After finding GPS and recognizing the Xbee module, you should see logs telling you that the state is « TURNEDOFF ».
From this state, your drone will be able to receive commands from other drones and perform tasks.

One test you can easily make is to open another terminal on the Spiri, switch the control to offboard control with the radio controller, and use the following commands to takeoff and land, while the node is running :
```
takeoff mavros
land mavros
```

- prepare a launch file forcing both device gps position (since you have no localisation) as we do for the groundstation (look in its launch file on github to see how)


- launch xbeemav+rosbuzz on both devices: they should not crash and you should see their xbee blinking (for more debug prints enable the debug mode from the launch file: you will get RAB when a msg is received)


https://www.hq.nasa.gov/alsj/a11/a11trans.html
https://www.hq.nasa.gov/alsj/a11/a11transscript_cm.pdf
