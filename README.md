# master-thesis-wrk
## 1. Introduction  
this 
## 2. Usage
This repository uses the `mir robot` as a simulation example.  
Build the relevant repository through the following URL：
```
https://github.com/match-ROS/match_mobile_robotics.git
```
and package `fpp_launch`, `fpp_msgs`, `fpp_ros` and `fp_utils` from the following repository:
```
https://github.com/match-ROS/formation-path-planning
```
### 2.1 Add path planner plugin
Browse into `your_catkin_ws/src/match_mobile_robotics/mir/mir_navigation/launch` and open the file `move_base.launch`.  
Add this code :
```
<param name="base_global_planner" value="path_planner/SplinedRelaxedAStar" />
<param name="base_global_planner" value="path_planner/SplinedRelaxedAStar" />
<param name="base_global_planner" value="PRM_planner/PRMPlannerROS" />
<param name="base_global_planner" value="RRTstar_planner/RRTstarPlannerROS" />
```
under the code line:
```
<node pkg="move_base" type="move_base" respawn="false" name="move_base_node" output="screen" clear_params="true">
```
What's more, copy the file `SplinedRelaxedAStar.yaml` from `splined_relaxed_a_star/config` folder   
into `your_catkin_ws/src/match_mobile_robotics/mir/mir_navigation/config/Planner/GlobalPLanner` folder. 
### 2.2 Run robot
In the launch file `run_robot.launch` you can change the map and start pose of the robot.  
```
roslaunch data_analysis run_robot.launch
```
### 2.3 Data record
If you want to save the planned path information for processing and analysis later,  
you can select the corresponding planner's command line from the following command：
```
rosbag record /move_base_node/SplinedRelaxedAStar/plan -O new.bag
rosbag record /move_base_node/VoronoiPlannerROS/plan -O new.bag
rosbag record /move_base_node/PRMPlannerROS/plan -O new.bag
rosbag record /move_base_node/RRTstarPlannerROS/plan -O new.bag
```
A data package named "new.bag" will be generated.
### 2.4 Set goal point
You can use launch file `send_goal.launch` to set the goal point precisely:
```
roslaunch data_analysis send_goal.launch
```
### 2.5 Data analysis
If a data bag is recorded, use the corresponding command of the planner to generate a file named "plan" in .cvs format.
```
rostopic echo -b new.bag -p /move_base_node/SplinedRelaxedAStar/plan > plan.csv
rostopic echo -b new.bag -p /move_base_node/VoronoiPlannerROS/voronoi_path > plan.csv
rostopic echo -b new.bag -p /move_base_node/PRMPlannerROS/plan > plan.csv
rostopic echo -b new.bag -p /move_base_node/RRTstarPlannerROS/plan > plan.csv
```
Then you can get length, smoothness and clearance of the planned path:
```
roslaunch data_analysis data_analysis.launch
```
The address of the file "plan" needs to be changed in this launch file to your own folder address.
