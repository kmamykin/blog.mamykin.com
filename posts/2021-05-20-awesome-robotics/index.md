---
permalink: /posts/awesome-robotics/
title: "Awesome robotics"
author: "Kliment Mamykin"
date: "2021-05-20"
image: ''
tags:
- ROS
- ROS2
---

This is a bi-product of my deep dive to understand the current state of affairs in robotics.
It's not an exhaustive list of all subject areas but rather a list of starting points I found useful to explore further. 
Hopefully this will be helpful to somebody who is diving into robotics for the first time.

## Runtime/Frameworks

* https://www.ros.org/
    * Docs: http://docs.ros.org/
* https://google-cartographer-ros.readthedocs.io/en/latest/index.html# SLAM, Navigation
* https://moveit.ros.org/ motion planning, manipulation, 3D perception, kinematics, control and navigation
* https://navigation.ros.org/
* https://opencv.org/ OpenCV Vision/Perception
* https://pointclouds.org/
* https://zenoh.io/blog/2021-04-28-ros2-integration/ Low weight, performant messaging library. DDS alternative for pub/sub, able to bridge to DDS
* https://web.casadi.org/ nonlinear optimization and algorithmic differentiation for numerical optimal control

## Simulation & Visualization
* http://gazebosim.org/
  * https://hub.docker.com/_/gazebo/ Nice description how to use gazebo
* https://ignitionrobotics.org/home
* https://allisonthackston.com/articles/ignition-vs-gazebo.html
* https://cyberbotics.com/ 
* https://plotjuggler.io/ Visualization tool of rosbags and topics streams
* https://gym.openai.com/

## Web Tools
* http://robotwebtools.org/tools.html
* https://webviz.io/
* https://foxglove.dev/ https://github.com/foxglove/studio
* https://github.com/rapyuta-robotics/zethus Realtime robot data visualization in the browser (React.js)
* https://discourse.ros.org/t/robostack-using-ros-alongside-the-conda-and-jupyter-ecosystems-on-any-linux-macos-windows-arm/20250 
* https://medium.com/robostack/cross-platform-conda-packages-for-ros-fa1974fd1de3 ^^ same as above Conda distribution for ROS binary packages + Jupyter

## Robots
* Robodog + Robomaker https://aws.amazon.com/robomaker/
    * https://github.com/aws-robotics
    
* Mindstorm EV3 http://wiki.ros.org/Robots/EV3
    * https://www.instructables.com/ROS-Robot-With-Lego-EV3-and-Docker/
    * https://github.com/srmanikandasriram/ev3-ros
    * https://www.ev3dev.org/
    * http://hacks4ros.github.io/h4r_ev3_ctrl/
    * https://github.com/ev3dev/brickstrap Brickstrap is a tool for turning Docker images into bootable image files for embedded systems.
    * https://github.com/rancher/buildroot/tree/master/board/lego/ev3
    
## Drones
* https://px4.io/ - Open Source Autopilot Designed For Scale

## Embedded 
* https://buildroot.org/ - Linux based
* https://www.yoctoproject.org/ - Linux based
* https://www.freertos.org/ - for super tiny controllers
  * https://github.com/hashmismatch/freertos.rs FreeRTOS in Rust


## Courses
* https://www.autoware.org/awf-course - Self-Driving Cars with ROS and Autoware
* http://underactuated.csail.mit.edu/ Video lectures from MIT
* https://www.theconstructsim.com/ Courses + IDE (pretty cool) with a Free individual plan
  * https://www.youtube.com/playlist?list=PLK0b4e05LnzYGvX6EJN1gOQEl6aa3uyKS ROS Development Studio tutorials
* https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/
* http://www.clearpathrobotics.com/assets/guides/melodic/ros/

## Events/Videos
* https://discourse.ros.org/t/ros-world-talks-are-posted/17436
* https://www.redhat.com/en/open-source-stories/robots/breaking-the-wheel 

## Rust and Robots
* https://robotics.rs/
* https://github.com/twistedfall/opencv-rust

## Data
* https://github.com/6RiverSystems/rosbag2bigquery
* https://6river.com/data-driven-robotics-leveraging-google-cloud-platform-and-big-data-to-improve-robot-behaviors/

## Challenges and Competitions
* https://subtchallenge.world/home

## Notable companies and organizations
* https://www.openrobotics.org/

## Docker
* https://github.com/osrf/docker_images Official ROS/ROS2/Gazebo images
* https://roboticseabass.wordpress.com/2021/04/21/docker-and-ros/ 
* https://wiki.ros.org/docker/Tutorials/Compose
* https://wiki.ros.org/docker/Tutorials/GUI
* https://hub.docker.com/_/ros

## Miscelaneous
* https://github.com/S2-group/icse-seip-2020-replication-package/blob/master/ICSE_SEIP_2020.pdf How do you architect your robot?
