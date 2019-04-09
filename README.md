# TURTLEBOT3-MAPPING-MOVING
How-to tutorial for using ROS with turtlebot3 and getting the turtlebot3 to map its surroundings while moving around

For more information on how to use the turtlebot3 please visit http://emanual.robotis.com/docs/en/platform/turtlebot3/overview/


##############################################################################################################################
##############################################################################################################################


** REQUIREMENTS **

For this tutorial you will need to have:
----------------------------------------

HARDWARE:
---------
- turtlebot3 ("burger") kit

- openCR board (if your turtlebot3 kit does not already come with one)

- raspberry pi 3 Model B (Model B+ is NOT recommended as it is not yet compatible with the software we will be using)

- SD card with MINIMUM of 8GB

- wireless access point (that can be either a cell phone "hot spot", wireless router, or another raspberry pi configured as a 
  wireless AP) internet connection is required for the downloads but is not necessary once all downloads are completed.

- A laptop or other PC to be used as a REMOTE PC for running ROS (Robot Operating System)


SOFTWARE:
---------
- (Linux Mint 18 "Sarah" - Cinnamon). The ISO can be downloaded from here http://mirrors.seas.harvard.edu/linuxmint/stable/18/linuxmint-18-cinnamon-64bit.iso

- Arduino IDE, which can be dowloaded from here https://www.arduino.cc/en/Main/Software

- Raspberry pi disk image zip file, can be downloaded from this link http://www.robotis.com/service/download.php?no=730 this 
  disk image comes with ROS packages pre-installed.

- BalenaEtcher, for flashing the SD card disk image, can be downloaded from this link https://www.balena.io/etcher/


##############################################################################################################################
#############################################################################################################################


** SETUP **

go to http://emanual.robotis.com/docs/en/platform/turtlebot3/opencr_setup/#opencr-setup and finish the "SETUP" section for the Turtlebot3, Remote PC, and OpenCR board.

1. on your remote pc (make sure it is running Linux Mint 18) open a terminal with "Ctrl + Alt + t"

2. type "roscore" hit Enter

3. open another terminal and ssh into the raspberry pi using "ssh pi@xxx.xxx.x.xxx" replace all the x with your raspberry pi IP
   address. 
   If you don't know the IP of your raspberry pi you can connect the pi up to a tv monitor (via HDMI), keyboard, and mouse, and
   then go here https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address for
   instructions on how to find it.

4. you will be prompted to enter your password to the raspberry pi. The password should be "turtlebot".

5. once you are connected to the raspberry pi, type "roslaunch turtlebot3_bringup turtlebot3_core.launch &"   then hit Enter

6. the last command you entered will launch the "core" node which starts serial communication between the pi and openCR board.
   Give the last command a few seconds to finish running.

7. once you see "Calibration End" hit Enter.

8. now type "roslaunch turtlebot3_bringup turtlebot3_lidar.launch &" this will launch the lidar sensor node, if the sensor was 
   not spinning before it should now start to spin.

9. now go back to your Remote PC and open another terminal window by pressing "Ctrl + Alt + t"

10. in the new terminal type "export TURTLEBOT3_MODEL=burger"   hit Enter

11. now type "roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping"    hit Enter

12. in a few seconds your should see the rviz monitor open and a map being created by the trutlebot3.

13. open yet another terminal window by typing "Ctrl + Alt + t"

14. in the new terminal type "export TURTLEBOT3_MODEL=burger"   hit Enter

15. now type "roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch"

16. you should now be able to send movement commands to the turtlebot3 by pressing 'w' (forwards), 'x' (backwards), 'a' (right), 
    'd' (left), and 's' (stop)

17. AVOID VIGOROUS MOVEMENT! such as changing the speed too quickly or rotating too fast. Doing this can cause problems and  
    possibly result in the raspberry pi losing connection with the openCR board in which case the turlebot3 will stop responding
    to new commands and will continue running the last movement command it recieved and it will also stop generating the map.

18. in the event that you do lose your connection or the turtlebot3 stop responding to movement commands, 
    type "sudo pkill -f serial_node.py" then hit Enter to kill the serial node. 
    Then type "rosrun rosserial_python serial_node.py /dev/ttyACM0" hit Enter to relaunch the serial node and reestablish
    communications.


