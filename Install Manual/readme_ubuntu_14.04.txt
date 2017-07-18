#Ubuntu 14.04 LTS Setup Guide

sudo apt-get -y update
#sudo apt-get -y upgrade   #you can skip upgrade of 
sudo apt-get -y install cmake


#---- now install quanergy lider
#https://github.com/QuanergySystems
#username: ***   pass:***
# Device Host Name: 1010C12000833
# Device Static IP is: 10.4.71.246
# subnet mask: 10.4.71.0/255.255.255.0
# gateway: 10.4.71.1
# any self system first set the ip address at edit connection and add new connection with new ip
# eg: 10.4.71.[174|any] subnet mask: 255.255.255.0 gateway: 10.4.71.1 dns server: 10.4.71.1

#----installing quanergy_client procedure

#Install the following libraries (and git)
sudo apt-get install git build-essential libboost-all-dev 

#Install PCL
sudo add-apt-repository ppa:v-launchpad-jochen-sprickerhof-de/pcl
sudo apt-get -y update
sudo apt-get -y install libpcl-all

#Clone the SDK repository
mkdir ~/QuanergySystems
cd ~/QuanergySystems
git clone https://github.com/QuanergySystems/quanergy_client.git
#you might get a prompt for username and password of github
#username: ***   pass:***

#Build quanergy_client code and test application
cd quanergy_client
mkdir build
cd build
cmake ..
make

#Build quanergy_client (SDK) per README Install quanergy_client (SDK)
cd ~/QuanergySystems/quanergy_client/build
sudo make install

#To test, run the test application and follow the usage instructions (Sensor IP address is xx.xx.xx.xx)
./test -host 10.4.71.246

#For documentation, run the following and then open doc/index.html in any browser. this is optional
make doc




#----installing quanergy_client_ros procedure
# TBD
#Install ROS Indigo RVIZ and configure environment
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116
sudo apt-get update
sudo apt-get install -y ros-indigo-rviz ros-indigo-pcl-ros
source /opt/ros/indigo/setup.bash

#Clone the ROS SDK repository

mkdir -p ~/QuanergySystems/catkin_ws/src
cd ~/QuanergySystems/catkin_ws/src
catkin_init_workspace
git clone https://github.com/QuanergySystems/quanergy_client_ros.git
#you might get a prompt for username and password of github
#username: ***   pass:***

#Build quanergy_client_ros code

cd ~/QuanergySystems/catkin_ws
catkin_make

#To test, configure environment and launch node

source /opt/ros/indigo/setup.bash
source ~/QuanergySystems/catkin_ws/devel/setup.bash
roslaunch quanergy_client_ros client.launch host:=<hostname_or_ip>

#To add ROS environment configuration automatically to every future bash session

echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
echo "source ~/QuanergySystems/catkin_ws/devel/setup.bash" >> ~/.bashrc



#----installing q-view procedure
# TBD
#http://downloads.quanergyworks.com
#click on Ubuntu OS 14.04 LTS
#check the readme file here http://downloads.quanergyworks.com/ubuntu/linux_readme.txt
#below steps are there at readme file of quenargy q-view
sudo -i
echo "deb [arch=amd64] http://downloads.quanergyworks.com/qview/ubuntu/ trusty main" >> /etc/apt/sources.list
wget -O - http://downloads.quanergyworks.com/qview/key/quanergy.key | sudo apt-key add -
add-apt-repository ppa:v-launchpad-jochen-sprickerhof-de/pcl
apt-get update
apt-get install -y quanergy-qview



#--- install other packages if needed 
# TBD



#--- install gaze packages

sudo apt-get install libexif-dev -y
sudo apt-get install ntp -y
sudo apt-get install apache2 -y
sudo apt-get install -y xvfb
sudo apt-get install -y php5 libapache2-mod-php5 php5-mcrypt
sudo apt-get install libgtkmm-2.4-dev -y
sudo apt-get install libglademm-2.4-dev -y
sudo apt-get install libgtkglextmm-x11-1.2-dev -y
sudo apt-get install -y nodejs-legacy
sudo apt-get install -y npm
sudo apt-get install -y libconfig++-dev
sudo apt-get install -y libjson0 libjson0-dev
sudo apt-get install -y libjson-glib-1.0-0
sudo apt-get install -y libjson-glib-1.0-common
sudo apt-get install -y libjson-glib-dev
sudo apt-get install -y openvpn
sudo apt-get install -y libjpeg-dev
sudo apt-get install -y libpng++-dev

#some manual edits needed
#libjpeg edit
#to check where is libjpeg
ldconfig -p | grep libjpeg
#then change the /gazeRecord/services/src/Gaze/MakeFile in the program! this is optional because it might already done 

#gcc-5 upgrade needed for ubuntu 14.04
#link https://gist.github.com/beci/2a2091f282042ed20cda
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-5 g++-5

sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 60 --slave /usr/bin/g++ g++ /usr/bin/g++-5



#-------------------------


#Flycapture2-2.9 is ideal for ubuntu 14.04
#download it using https://www.ptgrey.com/Downloads/BySKU?family=FireflyMV&sku=FFMV-03M2C-CS
#using #username: ***   pass:***
#or You can download from GITHUB https://github.com/vidteq/Gaze/raw/master/sdk/flycapture2-2.9.3.43-amd64-pkg.tgz
#download FlyCapture 2.9.3.43 SDK - Linux Ubuntu 14.04 (64-bit) — 04/28/2016 - 11.1706MB
tar -xvf flycapture2-2.9.3.43-amd64-pkg.tgz
cd flycapture2-2.9.3.43-amd64/
sudo sh install_flycapture.sh

#download triclops using https://www.ptgrey.com/Downloads/BySKU?family=Bumblebee2&sku=BB2-08S2C-60
#using #username: ***   pass:***
#or You can download from GITHUB https://github.com/vidteq/Gaze/raw/master/sdk/triclops-3.4.0.8-amd64-pkg.tgz
#download Triclops 3.4.0.8 - Linux Ubuntu 16.04 (64-bit) — 04/28/2017 - 38.996MB
tar -xvf triclops-3.4.0.8-amd64-pkg.tgz
cd triclops-3.4.0.8-amd64/
sudo sh install_triclops.sh

#download Spatial GPS SDK using https://www.advancednavigation.com.au/product/spatial#software
#or You can download from GITHUB https://github.com/vidteq/Gaze/raw/master/sdk/SpatialSDK.zip
#download SpatialSDK.zip
mkdir spatial
cd spatial
unzip ~/Downloads/SpatialSDK.zip
cd C/Dynamic
make



#how to configure the GPS to run without sudo permission, Check below steps
#to check which group the current user is on
groups gaze-t1
#As you've noticed, the /dev/ttyUSB0 device has the group of dialout. All you need to do is add the <user_name> user to the dialout group:
sudo adduser <user_name> dialout


#--- manual changes on code

#--- open <editor> ~/.bashrc
export CVSROOT=":pserver:swuser@10.4.71.121:2401/data/CVS_REPO"
alias cvstt='cvs status 2>/dev/null | grep ^File | grep -v Up-to'
alias wifi="echo *** | sudo -kS touch /etc/usb_modeswitch.d/12d1:1f01; echo *** | sudo -kS usb_modeswitch -J -v 0x12d1 -p 0x1f01;"
alias vpn="echo *** | sudo -kS openvpn --config ~/GazeSystems/vpn/fw-udp-1194-gaze-config.ovpn;"

#--- sudo open <editor> /etc/modules

raw1394

#--- sudo open <editor> /lib/udev/rules.d/40-usb_modeswitch.rules
#this is optional if dongle doesn't work
ATTR{idVendor}=="12d1", ATTR{idProduct}=="15ca", RUN+="usb_modeswitch '%b/%k'"

ATTR{idVendor}=="12d1", ATTR{idProduct}=="1f01", RUN+="usb_modeswitch '%b/%k'"


#--- open <editor> ~/.vnc/xstartup
#this has to be done after setting up of vnc4server
#to start the vnc4server command is: vnc4server
#Add the following line.

gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
#gnome-terminal &

#Save and close it
#then kill the vnc4server server command: vnc4server -kill :<number>
#then start again using command: vnc4server


#--- need other files
#vpn folder

#checkout gaze system from vidteq repository
cvs login
#use password: ***
cvs checkout software/gazeRecord

#
cp config.php ~/GazeSystems/vidteq/software/gazeRecord/www/
cp config.js ~/GazeSystems/vidteq/software/gazeRecord/www/js/




#---- apache setup
ls /var/log/apache2/
sudo gvim /etc/apache2/sites-available/000-default.conf

ServerAdmin webmaster@localhost
#DocumentRoot /var/www/html
DocumentRoot /home/gaze-t1/GazeSystems/vidteq/software/gazeRecord/www
<Directory /home/gaze-t1/GazeSystems/vidteq/software/gazeRecord/www >
   Options FollowSymLinks
   AllowOverride all
   Require all granted
</Directory>


sudo service apache2 restart



#--- install other packages if needed this is optional

sudo apt-cache pkgnames > ~/pkgnames
sudo apt-get -y install openssh-server openssh-client
sudo apt-get -y vim-gtk
sudo apt-get install vnc4server -y
sudo apt-get install tkcvs -y
sudo apt-get install cvs -y
sudo apt-get install -y ubuntu-desktop gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal
sudo apt-get install chromium-browser -y
sudo apt-get install default-jre -y
sudo apt-get install default-jdk -y
sudo add-apt-repository ppa:maarten-baert/simplescreenrecorder
sudo apt-get update
sudo apt-get install -y simplescreenrecorder


-----------------
Makefile changes

1) remove libjpeg on GTK_INCLUDE and GTK_LIBS
2) on INC : add -std=c++11 at the begin and add this one too -I/usr/lib/x86_64-linux-gnu
3) on LIB : add -ljpeg
4) comment full config adding section at Gaze.cpp
