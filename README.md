# ROS Navigation of Summit Robot

In this project, I used Summit robot (http://www.robotnik.eu/mobile-robots/summit-xl/) in Gazebo for autonomous navigation around a room.

This project can be divided into following parts:

 1. Generating a Map of the environment

This is done in ```my_summit_mapping``` package. The launch file of this package launches the ```slam_gmapping``` node. The gmapping ROS package is an implementation of a specific SLAM algorithm called gmapping (https://www.openslam.org/gmapping.html). After launching this node, the next step is to move around in the environment and create a map. To launch the keyboard teleop, execute the following command:

```roslaunch summit_xl_gazebo keyboard_teleop.launch```

After creating a map, execute the following command to save it.

```rosrun map_server map_saver -f name_of_map```

2. Robot Localization

This next step is to localize the robot in the above created map. This is done through a package called ```my_summit_localization```. This package launches ```amcl_noce```.

The AMCL (Adaptive Monte Carlo Localization) package provides the amcl node, which uses the MCL system in order to track the localization of a robot moving in a 2D space. This node subscribes to the data of the laser, the laser-based map, and the transformations of the robot, and publishes its estimated position in the map.

This package also contains a custom service called ```/record_spot```. This service can be called to save the position and orientation of the robot in a .txt file.

3. Path Planning System

Path planning is done by package called ```my_summit_path_planning```. This package launches the ```move_base``` node.

4. Central package that interacts with the Navigation Stack

The package called ```my_summit_navigation``` contains a service that will contain a service that will take a string as input. This string will indicate one of the labels that were saved in when calling the previously created ```/record_spot```.  When this service receives a call with the label, it will get the coordinates (position and orientation) associated with that label, and will send these coordinates to the ```move_base``` action server. If it finished correctly, the service will return an "OK" message. An action client that will send goals to the move_base action server.


![Robot Arm](https://i.ibb.co/RSp84xH/Screenshot-at-Jul-21-10-16-42.png)
