# 🤖 Homework 4: Mobile Robot Navigation 🚀
## 📝 Introduction

The goal of this assignment is to control a mobile robot to follow a trajectory by implementing an autonomous navigation framework.
The key objectives include:
- Building and configuring a simulation world in Gazebo.
- Defining navigation goals and implementing autonomous navigation.
- Mapping the environment using SLAM techniques.
- Implementing vision-based navigation with ArUco markers.  
 
---

### 🔧 1. Create a Gazebo World and Spawn the Mobile Robot 🤖

1️⃣ Launch the Gazebo simulation and spawn the mobile robot in the specified position:

- 📍 **Initial coordinates of the robot:**
  - \( x = -3 \, m \), \( y = 5 \, m \), \( Y = -90^\circ \) (relative to the map frame).
- 🛠️ **World modifications:**
  - Move obstacle 9 to \( x = -3 \, m \), \( y = -3.3 \, m \), \( z = 0.1 \, m \), \( Y = 90^\circ \).

```bash
ros2 launch rl_fra2mo_description gazebo_fra2mo.launch.py 

ros2 run rqt_image_view_ rqt_image_view_
```
### 🧭 2. Static TF Goals and Autonomous Navigation ✅

2️⃣ Set up navigation goals using the Nav2 Simple Commander API.

🎯 **Goal sequence**: Goal 3 ➡️ Goal 4 ➡️ Goal 2 ➡️ Goal 1.

```bash
ros2 launch rl_fra2mo_description gazebo_fra2mo.launch.py 

ros2 run rl_fra2mo_description fra2mo_explore.launch.py

ros2 run rl_fra2mo_description follow_waypoints_goals.py
```

### 🗺️ 3. Map the Environment 🌍

3️⃣ Create and save a **map** of the environment:

``` bash
ros2 launch rl_fra2mo_description gazebo_fra2mo.launch.py

ros2 launch rl_fra2mo_description fra2mo_explore.launch.py

ros2 run rl_fra2mo_description follow_waypoints.py
```
🤖 To save the **pose** 
``` bash
cd src
ros2 bag record -o subset /pose
```
```
ros2 launch rl_fra2mo_description fra2mo_slam.launch.py
```
🗺️ To save the **map**
``` bash
cd src
ros2 run nav2_map_server map_saver_cli -f map
```
### 👁️ 4. Vision-Based Navigation with ArUco Markers 🧩

4️⃣ Use vision for navigation:

- 🎯 **Send** the robot near obstacle 9.
- 🚦 **Detect** the ArUco marker (#1151) using the robot's camera and retrieve its pose.
- 🌍 **Return** the robot to its initial position.
- 📌 **Publish** the marker's pose as a TF frame.

``` bash
ros2 launch rl_fra2mo_description gazebo_fra2mo.launch.py 

ros2 run rl_fra2mo_description static_world_broadcaster.py 

ros2 launch rl_fra2mo_description merge.launch.py 

ros2 run rl_fra2mo_description ARUCO_waypoints.py 

ros2 run tf2_ros tf2_echo world aruco_marker 
```
