# Driver-Camera
Learn to use camera with odroid on ros melodic.

## First things first

#### Materials List
 - Odroid
 - Decent amount of storage (ex: 32gb micro-sd)
 - Keyboard and mouse
 - Power supply
 - Internet Connection
 - USB camera

#### Installation Materials
 - This repository used ROS melodic
 - This repository used Ubuntu MATE 18.04
 - You will need USB camera drivers installed
 - You will need web_video_server installed
 - You will need usb_cam installed

## Now begin setup
 - First thing you will want to do is make a folder `usb_cam` with the usb_cam drivers and packages installed and another folder inside named `launch`.
 - You will also need a folder titled `web_video_server` with the web_video_server packages installed and another folder inside named `launch`.
 - In the `web_video_server` folder in the `launch` folder you will need to make a launch file named whatever you would like, the name I used it `total_launch.launch`.
 - In the `usb_cam` folder in the `launch` folder you will need a launch file titled again can be title whatever you want, the name I used is `usb_cam.launch`.

#### Setting up the launch files
 - For further information on the parameters you may use in the file titled `usb_cam.launch` visit `http://wiki.ros.org/usb_cam`.
 ##### usb_cam launch file explanation
  - The first thing that denotes a launch file is the opening and closing of the launch file this is done by: `<launch>` and `</launch>`
  - The next step is to in the launch open and call the specific **nodes** that the launch file uses. A node is a process that when used will perform a specific action or calculation denoted by what the node is instructed to do from code. To open a node, you do this `<node` then in this first line also include the name, package, type and in this case output.
  `<node name ="usb_cam" pkg="usb_cam" type="usb_cam_node" output="screen" >` you will notice `pkg` this is a **package** which is basically a directory for a ROS system.
  - The next thing you will want to do is list all parameters that his node will follow. I am not foing to list them all here, but here is an example `<param name="image_width" value="640"/>` so to declare this parameter, you state the name which in this case is the image width and the value which is 640. 
  - Once all parameters are listed, close the node with `</node>` and make sure the launch is closed with `</launch>`.
 ##### total_launch launch file explanation
  - The next part is to create the launch file which will be used on boot. Make sure the `total_launch.launch` file is in the `web_video_server` folder in the `launch` folder.
  - To make this file you use the same `<launch>...</launch>` we used earlier. Now in this file make sure you include the usb_cam launch file so we can use both the web_video_server and usb_cam together. To do this, use  `<include>`, `<include file="$(find usb_cam)/launch/usb_cam-test.launch">` the `$(find usb_cam)` looks for the first instance of a file of that name, the rest is the rest of the location path. Now close the include, `</include>`.
  - Next call the node, `<node name="video_web_server" pkg="video_web_server" type="video_web_server">` with whatever parameter as we did in `usb_cam` then close the node and close the launch.

#### Now terminal commands
 - With the terminal opened, we want to open the launches on boot, so install `rosrun` and use `rosrun robot_upstart install (find web_video_server)/launch/total_launch.launch --job robot_ros --symlink` This will install the launch file we made to now use on boot. You may now save everything and reboot your odroid. You will notice that it works when you visit your specific IP address for your odroid.
 - To disable this:
  - `sudo systemctl disable robot_ros.service`
 - To uninstall this:
  - `rosrun robot_upstart uninstall robot_ros`
