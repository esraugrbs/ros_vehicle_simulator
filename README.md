# Vehicle Simulation (ROS Melodic + Gazebo 9)

This package supplies everything needed to spin-up a 1 ∶ 5-scale autonomous ground-vehicle inside Gazebo and test mapping, localisation and basic navigation algorithms.

## Purpose
The goal of this project is to **generate 2-D maps by fusing real-time LIDAR scans with odometry while the simulated vehicle drives around the environment**.  
By validating SLAM and navigation in simulation first, you can iterate quickly before deploying code to the physical robot.

## Key Components
| Folder | Purpose |
|--------|---------|
| **urdf/**          | Modular **Xacro** model of chassis, differential-drive wheels, caster wheels and a 2-D LIDAR sensor |
| **launch/**        | *vehicle_gazebo.launch* (spawns robot & controllers), *mapping_rviz.launch* (starts **slam_gmapping** + TF + RViz), *move_base.launch* (Navfn + DWA planners) |
| **config/**        | PID settings for wheel-velocity controllers in `controllers.yaml` |
| **worlds/**        | `Map_world.world` – simplified track for rapid testing |
| **CMakeLists.txt / package.xml** | Declare all ROS dependencies and MIT licence |

## Features
* **Real-time 2-D SLAM** using *slam_gmapping*; publishes `/map` and TF tree (`map ↔ odom ↔ base_link`).
* **Goal-directed navigation** – *move_base* pairs Navfn (global) with DWA (local) planners to reach goals set in RViz.
* **Independent wheel control** via `velocity_controllers/JointVelocityController` (tunable PID).
* **Rapid iteration**: run everything in simulation before the physical robot is ready.

## Prerequisites
| Software | Version |
|----------|---------|
| Ubuntu   | 18.04 (tested) |
| ROS      | **Melodic** |
| Gazebo   | **9** |
| Catkin deps | `roscpp`, `gazebo_ros`, `gazebo_ros_control`, `controller_manager`, `joint_state_controller`, `velocity_controllers`, `xacro` |

## Build & Launch
```bash
# Clone into your catkin workspace
cd ~/catkin_ws/src
git clone https://github.com/<your-org>/vehicle_sim.git
cd ..
catkin_make            # or catkin build
source devel/setup.bash

# 1. Spawn the robot
roslaunch vehicle_description vehicle_gazebo.launch

# 2. Start SLAM + RViz (new terminal)
roslaunch vehicle_description mapping_rviz.launch

# 3. Enable autonomous navigation (optional, new terminal)
roslaunch vehicle_description move_base.launch
