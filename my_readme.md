## Steps for installation

```sh
mkdir -p face_ws/src
cd face_ws/src
git clone https://github.com/RachiketArya/homer_robot_face.git
cd ..
```
(Cloning of another face-package may be needed: homer_tts)

Following are instrcutions from a stack exchange [answer](https://askubuntu.com/questions/1082722/unable-to-locate-package-libesd0-dev-on-ubuntu-18-04):

```sh
sudo nano /etc/apt/sources.list
```

If you are running anything above Ubuntu 16.04, add the following lines at the end and press <kbd>Ctrl</kbd> + <kbd>C</kbd>:
```sh
deb http://us.archive.ubuntu.com/ubuntu/ xenial main universe
deb-src http://us.archive.ubuntu.com/ubuntu/ xenial main universe
```

If the above fails, edit the two lines above as follows:
```sh
deb [arch=amd64,i386] http://us.archive.ubuntu.com/ubuntu/ xenial main universe
deb-src [arch=amd64,i386] http://us.archive.ubuntu.com/ubuntu/ xenial main universe
```

Then run:
```sh
sudo apt-get update && sudo apt-get install libesd0-dev
```

Finally, in the workspace, run the following:
```sh
rosdep install --from-paths src --ignore-src --rosdistro=melodic
```

Now, in line 29 of ```CMakeLists.txt```, add ```-fopenmp``` so the the line looks like the following:
```sh
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -fopenmp")
```

Now run the following in the root folder of the workspace:

```sh
catkin_make
source devel/setup.bash
roslaunch homer_robot_face robot_face.launch
```


## For text to speech
Install the necessary packages:
```sh
sudo apt-get install libttspico-utils
```
Clone the following package in ```[workspace]/src```:
```sh
git clone https://github.com/RachiketArya/homer_tts.git
```

Temporary fix for build error in ```MarryTTs.cfg```:
uncomment line 13 and comment line 48

After ```catkin_make``` and sourcing the environment, run:
```sh
roslaunch homer_tts homer_pico2wave.launch
```

## Note
To change the face to male, the path of the config file needs to be changed in the ```MainWindow.cpp``` file Line 57.
To change the character expression, publish the character faces listed in ```TalkingHead.cpp``` (":), :( etc") on the topic ```/robot_face/text_out``` and then publish on ```/robot_face/talking_finished``` to "clear the buffer". Or publish directly onto ```/speak/goal``` if the text to speech has been launched.
After launching text to speech node, publish to ```/speak/goal``` a string of words separated only by " ".


For a new raspberry pi 4 with the ubiquity 2020 16.04 image installed, do the following to enable auto-login:

In Ubuntu 18.04 (Xubuntu Minimal Desktop), create /etc/lightdm/lightdm.conf and add the following:

# /etc/lightdm/lightdm.conf
[SeatDefaults]
autologin-user=<username>
autologin-user-timeout=0
I couldn't get it working through the lightdm.conf.d folder.

# For script to work on booting up:
Download the package containing gnome-session-propoerties and open it to add the startup command saying /bin/bash [location].launch

And add the following command:
```sh
/bin/bash -c "sleep 15 && /bin/bash /home/ubuntu/homer_face.sh"
```


ORRRR

Follow the two links:
https://www.stuffaboutcode.com/2012/06/raspberry-pi-run-program-at-start-up.html
https://wiki.debian.org/LSBInitScripts


Useful commands for pi:
alsamixer (for audio)
sudo raspi-config

## Connect ROS on two macines
1. Make sure both the machines have the same identification of the ros master by running the following command (with the IP address of the machine running the ROS master) on both the machines:

```ROS_MASTER_URI=http://172.16.30.103:11311```

2. On the machine that is not running the ROS master, correct the face that the machine's address can't be resolved by running the following command:

```export ROS_IP=172.16.30.223```

More help can be found at [ROS Network Setup](http://wiki.ros.org/ROS/NetworkSetup) page.

Meanwhile for openeing a netcat chat across the machines, start listening on a particular port on the first machine:

```netcat -l 1234```

and subscribe to that machine's chat on another machine via:

```netcat [IP ADDRESS] 1234```

eg.

```netcat 172.16.30.223 1234```

# Useful commands:
```sh
ps auxf
ls /proc/[PID]
cat /usr/sbin/homer_face-start
```

To set the IP address manually:
```sh
sudo ip addr 192.168.1.113/32 dev eth0
sudo ifconfig eth0 netmask 255.255.255.0
```


## Following is probably the best idea to change the IP address

Or change the following file:
```sh
sudo nano /etc/network/interfaces
```

```sh
# The primary network interface
auto eth0
iface eth0 inet static
address 192.168.2.33
gateway 192.168.2.1
netmask 255.255.255.0
network 192.168.2.0
broadcast 192.168.2.255
```
and restart the networking
```sh
sudo /etc/init.d/networking restart
```



# Current Workflow:
1. Set ROS_MASTER_URI and ROS_IP
2. Run multisim.launch
3. Run UI Node
4. Run magni compartment node

TODO: Configuration of the other Raspberry Pi for compartment