# Drone_Competition
Autonomous Drone Simulation Competition

# PC Recommended Specifications
- Intel Core i7-10750H 10th Generation, 5.00GHz or higher
- 16 GB DDR4 RAM or higher
- 500 GB SSD or higher
- NVIDIA GeForce GTX 1660 Ti or higher

# PC Required Specifications
- OS : Ubuntu 18.04
- external graphics card is necessary

# Install Guide
## Download
```
git clone https://github.com/rladntjd/Drone_Competition.git
cd ~/Drone_Competition
``` 

## 1. Install Ros Melodic  

```
sudo chmod +x ros_melodic_install.sh
./ros_melodic_install.sh
```  

## 2. Install Unreal Engine  

Make sure you are registered with Epic Games. This is required to get source code access for Unreal Engine.  

* Just follow the "1 - Required Setup process" on [Link](https://docs.unrealengine.com/4.26/en-US/SharingAndReleasing/Linux/BeginnerLinuxDeveloper/SettingUpAnUnrealWorkflow/) to register git.  
* Don't download Git of Unreal Engine. it is included in the code that we distribute.  

```
sudo chmod +x Unreal_install.sh
./Unreal_install.sh
```  

* you’ll see a box asking to register Unreal Engine file types. You want to select “Yes”.  


## 3. Install AirSim  

```
sudo chmod +x AirSim_install.sh
./AirSim_install.sh
```

## 4. Install mavros  
 
```
sudo chmod +x mavros_install.sh
./mavros_install.sh
``` 

## 5. Install px4   

```
sudo chmod +x px4_install.sh
./px4_install.sh
```


## 6. Install depth_noise

Just double-click ros-melodic-depth-noise_2.0.1-0bionic.amd64.deb

or 
```
sudo depkg -i ros-melodic-depth-noise_2.0.1-0bionic.amd64.deb
```
## 7. Setting and Download map  

* Setting file for drone competition.

This file is used to set visual sensors and connenct simulation with px4 firmware.
Put this file at Documents\AirSim

* Visual sensor specification

front_center_custom

RGB, 480X320, FOV : 90, 0.5m above from the drone center

Depth, 480X320, FOV : 90, 0.5m above from the drone center


Link : https://drive.google.com/file/d/1D6OVH1roItq2vfi0X-5JGv8ekbsZEBCL/view?usp=sharing

* Competition map

![메시](https://user-images.githubusercontent.com/71123229/126067216-e9c53062-49d4-4abf-aa73-8f6194c59800.JPG)

Link : https://drive.google.com/file/d/1qiP-0VZRr7KU1f5E6bB4Gw-2pXdjbp8U/view?usp=sharing

* Need to turn off 'Use Less CPU when in Background' option at Edit - Editor preferences - General - Performance

# Start Guide  

## 1. Open Map  
Run drone_proto.uproject file and click 'play' for start simulation.  


## 2. Open 4 terminals respectively
### 1) terminal 1 : Airsim  

```
cd ~/AirSim/ros
source devel/setup.bash
roslaunch airsim_ros_pkgs airsim_node.launch
```

### 2) terminal 2 : PX4
```
cd ~/PX4-Autopilot
make px4_sitl_default none_iris
```
### 3) terminal 3 : Mavros
```
roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"
```  

### 4) terminal 4 : Depth Noise
```
roslaunch depth_noise depth_simple.launch
```  

# Notice

## Noise is added

Due to gaussian random noise is added to rotor thrust the drone will vibrate.

[![noise](https://user-images.githubusercontent.com/63506664/126486798-18f3ef42-eb66-4146-b4dd-b3503e12f063.jpg)](https://youtu.be/c1Q5NXIVQUM)

gaussian noise added

[![no noise](https://user-images.githubusercontent.com/63506664/126478305-ac0b8d20-eaef-4038-8308-4df2b9a2b864.jpg)](https://youtu.be/ebIxCsg4aP4)

noise not added

## Camera Intrinsic Parameter on Airsim

{ fx, fy, cx, cy } = { 240, 240, 240, 160 }

No distortion Parameter


## Subscriber & Publisher

https://microsoft.github.io/AirSim/airsim_ros_pkgs.html#using-airsim-ros-wrapper

Given topics

RGB image topic from Simulation Drone
```
/airsim_node/drone_1/front_center_custom/Scene
```
Depth image topic from Simulation Drone
```
/simulation/depth_image
```

Depth camera can measure distance upto 13m. Depth noise is added

http://wiki.ros.org/mavros

- /mavros/home_position/home Topic will be provided for Home Positioning

- Positioning data such as GPS is NOT provided. Use visual odometry or other pose estimation algorithm.


## Information about map

### Bird's-eye view of map
![map](https://user-images.githubusercontent.com/71123229/126286328-36528943-e347-44a6-b9eb-af90eeec14c8.png)

### Information about gates
![gate](https://user-images.githubusercontent.com/71123229/126285768-7512f0b0-0a2c-44ac-aa6f-bc16918a08a4.png)

## PX4 Offboard control Example
### C++ example
https://docs.px4.io/master/en/ros/mavros_offboard.html
### Python example
https://github.com/rladntjd/px4_offboard_example.git



