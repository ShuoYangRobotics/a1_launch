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