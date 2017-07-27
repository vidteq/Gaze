#Ubuntu 16.04 LTS Setup Guide
# Commented line need not be exercised (#) its optional or reference

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
sudo apt-get -y install git build-essential libboost-all-dev 

#Install PCL
#----install pcl dependency
#https://askubuntu.com/questions/916260/how-to-install-point-cloud-library-v1-8-pcl-1-8-0-on-ubuntu-16-04-2-lts-for/916261

#INSTALLATION

#Install oracle-java8-jdk:

sudo add-apt-repository -y ppa:webupd8team/java && sudo apt update && sudo apt -y install oracle-java8-installer

#press ok and yes later while install

#Install various dependencies and pre-requisites:

sudo apt -y install g++ cmake cmake-gui doxygen mpi-default-dev openmpi-bin openmpi-common libusb-1.0-0-dev libqhull* libusb-dev libgtest-dev
sudo apt -y install git-core freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libphonon-dev libphonon-dev phonon-backend-gstreamer
sudo apt -y install phonon-backend-vlc graphviz mono-complete qt-sdk libflann-dev libflann1.8 libboost1.58-all-dev

cd ~/Downloads
wget http://launchpadlibrarian.net/209530212/libeigen3-dev_3.2.5-4_all.deb
sudo dpkg -i libeigen3-dev_3.2.5-4_all.deb
sudo apt-mark hold libeigen3-dev

wget http://www.vtk.org/files/release/7.1/VTK-7.1.0.tar.gz
tar -xf VTK-7.1.0.tar.gz
cd VTK-7.1.0 && mkdir build && cd build
cmake ..
make                                                                   
sudo make install

#Install Point Cloud Library v1.8:

cd ~/Downloads
wget https://github.com/PointCloudLibrary/pcl/archive/pcl-1.8.0.tar.gz
tar -xf pcl-1.8.0.tar.gz
cd pcl-pcl-1.8.0 && mkdir build && cd build
cmake ..
make
sudo make install

#Perform some clean up:

cd ~/Downloads
rm libeigen3-dev_3.2.5-4_all.deb VTK-7.1.0.tar.gz pcl-1.8.0.tar.gz
sudo rm -r VTK-7.1.0 pcl-pcl-1.8.0

#VALIDATION

cd ~
mkdir pcl-test && cd pcl-test

#--- Create a CMakeLists.txt file: and copy paste below section

cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
project(pcl-test)
find_package(PCL 1.2 REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_executable(pcl-test main.cpp)
target_link_libraries(pcl-test ${PCL_LIBRARIES})

SET(COMPILE_FLAGS "-std=c++11")
add_definitions(${COMPILE_FLAGS})

#---Create a main.cpp file: and copy paste below section

#include <iostream>

int main() {
    std::cout << "hello, world!" << std::endl;
    return (0);
}

#Compile:

mkdir build && cd build
cmake ..
make

#Test:

./pcl-test

#Output -> hello, world!

/bin/rm -rf ~/pcl-test

#-------------

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

#Build quanergy_client (SDK) per README Install quanergy_client (SDK) this is optional
cd ~/QuanergySystems/quanergy_client/build
sudo make install

#To test, run the test application and follow the usage instructions (Sensor IP address is xx.xx.xx.xx)
./test -host 10.4.71.246

#For documentation, run the following and then open doc/index.html in any browser. this is optional
#make doc




#----installing quanergy_client_ros procedure : ROS is optional procedure 

#Install ROS Indigo RVIZ and configure environment
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
#sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
sudo apt-get update

--------------
sudo apt-get install -y ros-kinetic-desktop-full
or
sudo apt-get install -y ros-kinetic-rviz ros-kinetic-pcl-ros
-----------

source /opt/ros/kinetic/setup.bash

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

source /opt/ros/kinetic/setup.bash
source ~/QuanergySystems/catkin_ws/devel/setup.bash
roslaunch quanergy_client_ros client.launch host:=<hostname_or_ip>

#To add ROS environment configuration automatically to every future bash session

echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
echo "source ~/QuanergySystems/catkin_ws/devel/setup.bash" >> ~/.bashrc



#----installing q-view procedure is optional procedure 
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
sudo apt-get install php7.0 -y
sudo apt-get install php7.0-gd -y
sudo apt-get install libapache2-mod-php7.0 -y
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


#------------------

#Flycapture2-2.10 is ideal for ubuntu 16.04
#download it using https://www.ptgrey.com/Downloads/BySKU?family=FireflyMV&sku=FFMV-03M2C-CS
#using #username: ***   pass:***
#or You can download from GITHUB https://github.com/vidteq/Gaze/raw/master/sdk/flycapture2-2.10.3.266-amd64-pkg.tgz
#FlyCapture 2.10.3.266 SDK - Linux Ubuntu 16.04 (64-bit) — 04/13/2017 - 14.1020MB or any latest for 16.04
cd ~/Downloads
wget https://github.com/vidteq/Gaze/raw/master/sdk/flycapture2-2.10.3.266-amd64-pkg.tgz
tar -xvf flycapture2-2.10.3.266-amd64-pkg.tgz
cd flycapture2-2.10.3.266-amd64/
sudo sh install_flycapture.sh
cd ~/Downloads
/bin/rm -r flycapture2-2.10.3.266-amd64-pkg.tgz flycapture2-2.10.3.266-amd64


#download triclops using https://www.ptgrey.com/Downloads/BySKU?family=Bumblebee2&sku=BB2-08S2C-60
#using #username: ***   pass:***
#or You can download from GITHUB https://github.com/vidteq/Gaze/raw/master/sdk/triclops-3.4.0.8-amd64-pkg.tgz
#download Triclops 3.4.0.8 - Linux Ubuntu 16.04 (64-bit) — 04/28/2017 - 38.996MB
cd ~/Downloads
wget https://github.com/vidteq/Gaze/raw/master/sdk/triclops-3.4.0.8-amd64-pkg.tgz
tar -xvf triclops-3.4.0.8-amd64-pkg.tgz
cd triclops-3.4.0.8-amd64
sudo sh install_triclops.sh
cd ~/Downloads
/bin/rm -r triclops-3.4.0.8-amd64-pkg.tgz triclops-3.4.0.8-amd64


#download Spatial GPS SDK using https://www.advancednavigation.com.au/product/spatial#software
#or You can download from GITHUB https://github.com/vidteq/Gaze/raw/master/sdk/SpatialSDK.zip
cd ~/Downloads
wget https://github.com/vidteq/Gaze/raw/master/sdk/SpatialSDK.zip
#download SpatialSDK.zip
mkdir SpatialSDK
cd SpatialSDK
unzip ../SpatialSDK.zip
cd C/Dynamic
make
cd ~/Downloads
/bin/rm -r SpatialSDK SpatialSDK.zip


#how to configure the GPS to run without sudo permission, Check below steps
#to check which group the current user is on
groups <user_name>
#As you've noticed, the /dev/ttyUSB0 device has the group of dialout. All you need to do is add the <user_name> user to the dialout group:
sudo adduser <user_name> dialout






#--- install other packages if needed this is optional

sudo apt-cache pkgnames > ~/pkgnames
sudo apt-get -y install openssh-server openssh-client
sudo apt-get -y install vim-gtk
sudo apt-get install vnc4server -y
sudo apt-get install tkcvs -y
sudo apt-get install cvs -y
sudo apt-get install chromium-browser -y

sudo add-apt-repository ppa:maarten-baert/simplescreenrecorder
sudo apt-get update
sudo apt-get install -y simplescreenrecorder
sudo apt-get install -y ubuntu-desktop gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal



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

mkdir ~/Documents/vidteq
cd ~/Documents/vidteq
#checkout gaze system from vidteq repository
cvs login
#use password: ***
cvs checkout software/Gaze

#
cp ~/Documents/vidteq/software/Gaze/gazePkg/manualInstallation/www/config.php ~/Documents/vidteq/software/Gaze/www/
cp ~/Documents/vidteq/software/Gaze/gazePkg/manualInstallation/www/js/config.js ~/Documents/vidteq/software/Gaze/www/js/




#---- apache setup
ls /var/log/apache2/
sudo gvim /etc/apache2/sites-available/000-default.conf

ServerAdmin webmaster@localhost
#DocumentRoot /var/www/html
DocumentRoot /home/<userName>/Documents/vidteq/software/gazeRecord/www
<Directory /home/<userName>/Documents/vidteq/software/gazeRecord/www >
   Options FollowSymLinks
   AllowOverride all
   Require all granted
</Directory>


sudo service apache2 restart




#--- install packages below is optional if needed install
sudo apt-get install default-jre -y
sudo apt-get install default-jdk -y
sudo apt-get install -y gparted


#---- for vpn copy directory
sudo cp ~/Documents/vidteq/software/Gaze/gazePkg/manualInstallation/vpn ~/Downloads/
#then open 
gvim ~/Downloads/vpn/fw-udp-1194-gaze-config.ovpn
#and change line number 13 last path to /home/gaze/Downloads/vpn/openvpnpass.txt

#------------------
#--- then copy service to /etc/systemd/system
sudo cp ~/Documents/vidteq/software/Gaze/service/linuxService/etc/systemd/system/gaze-left-machine-check.service /etc/systemd/system/
#--- then copy service to /usr/local/bin
sudo cp -rf ~/Documents/vidteq/software/Gaze/service/linuxService/usr/local/bin/gaze-services to /usr/local/bin/

sudo chmod +x /usr/local/bin/gaze-services/gaze-left-machine-check.sh
sudo chmod +x /usr/local/bin/gaze-services/gaze-right-machine-check.sh

#--- to run the service cmd's
#to enable service
#sudo systemctl enable gaze-left-machine-check.service
#to start service
#sudo systemctl start gaze-left-machine-check.service
#check service status
#sudo systemctl status gaze-left-machine-check.service

#stop service
#sudo systemctl stop gaze-left-machine-check.service
#to disable service
#sudo systemctl disable gaze-left-machine-check.service

#------------------
#for ip config of system and mobile dongle
#create new ethernet ip for both
#and edit connection with IPv4 settings 
#change method to manual
#pass static ip like 10.4.71.* netmask 255.255.255.0 gateway 10.4.71.1
#last DNS servers: 10.4.71.1

#similar to dongle with static ip 192.168.0.100 netmask 255.255.255.0 gateway 192.168.0.1
#last dns server 192.168.0.1

#----------------
