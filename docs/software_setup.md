## Overview
Our goal is to build A1 robot as a powerful research platform for rough terrain locomotion. The robot runs following programs at the same time on a single NUC (must be i7 or higher). 

1. A balancing and velocity tracking controller. The controller code is at [A1_Ctrl](https://github.com/paulyang1990/A1_Ctrl). 
2. A vision based state estimator. The estimator code is at [VILO](https://github.com/paulyang1990/tightly-coupled-visual-inertial-leg-odometry)


Next we introduce all the necessary setup steps

## Development Computer System Requirement
We use the development computer to test the robot controller in simulation. It can just be standard laptop or desktop. 
* Laptop or desktop i7 or better chip
* Install Ubuntu 18.04
* Install ROS melodic
* Install Gazebo 8 or 9 
  
## Hardware Computer System Requirement
We use the hardware computer to test the robot controller on hardware robot. It is mounted on the robot and connects to A1 via LAN cable.

* A Intel NUC computer with i7 or better chip
* Install Ubuntu 18.04
* Install ROS melodic
* Install [Realsense driver and Realsense ROS](https://github.com/IntelRealSense/realsense-ros). Be careful about software versions. We recommend RealSense ROS v2.2.22 and LibRealSense v2.42.0.

## Controller Installation
The controller runs inside a [docker](https://www.youtube.com/watch?v=rOTqprHv1YE). It is just an architecture choice to make the controller easy to run on multiple platforms. The A1_Ctrl is able to control hardware, Gazebo simulation, and Nvidia Issac Sim simulation. Our typical workflow is first setup the docker environment on a desktop and tune the controller in Gazebo/Isaac Sim. Then setup the docker on the robot onboard computer, use the same control code with different parameter file (yaml) to control the hardware robot. 

To install the controller, read the README.md file of A1_Ctrl and build docker first. We provides two docker options ([docker](https://github.com/paulyang1990/A1-Docker/blob/main/docker/Dockerfile) \& [hardware_docker](https://github.com/paulyang1990/A1-Docker/tree/main/hardware_docker)), the only difference is the first dockerfile using ssh and deamon setup which allows [clion integration](https://www.jetbrains.com/help/clion/remote-projects-support.html#remote-toolchain). 

## Vision-based Estimator Installation
read the README.md file of the [VILO](https://github.com/paulyang1990/tightly-coupled-visual-inertial-leg-odometry)


## Controller \& Vision Estimator Integration
The A1_Ctrl has a simple EKF for velocity and position estimation, in most cases it is good enough. 

The VILO additionally provides global consistent position estimation and sparse mapping. 

To see how VILO work, a lot of dependeancies must be installed on the computer. 

* [a1_launch](https://github.com/paulyang1990/A1_launch)
* [A1_visualizer](https://github.com/paulyang1990/A1_visualizer)
* [tightly-coupled-visual-inertial-leg-odometry](https://github.com/paulyang1990/tightly-coupled-visual-inertial-leg-odometry)
* [transform_graph](https://github.com/jstnhuang/transform_graph)
* [unitree_ros](https://github.com/paulyang1990/unitree_ros)
* [VINS-Fusion](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion)

You may notice the unitree ros is installed in docker eariler when you built the docker. Here we need it again because the A1_visualier use the unitree A1 urdf. If you don't want to go through the unitree_ros package installation process (it is very complicated). You can modify [A1_visualier to use a local urdf model](https://github.com/paulyang1990/A1_visualizer/blob/main/src/hardware_a1_visualize.cpp#L116) 


After everything is properly installed, start the pipeline:
1. (terminal 1) docker start a1_cpp_ctrl_docker
2. (terminal 1) ssh root@localhost -p2233
3. (terminal 2) roscore
4. (terminal 3) roslaunch a1_launch front_camera_read.launch
5. (terminal 4) roslaunch a1_launch hardware_a1_vilo_together.launch
6. (terminal 1) roslaunch a1_cpp a1_qp_ctrl.launch type:=hardware