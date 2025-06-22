# ROS Setup Instructions

This document outlines the steps to set up and run a ROS (Robot Operating System) environment with specific configurations for a robot system. Ensure all commands are executed in separate terminals, and set the `ROS_IP` environment variable in each terminal.

## Prerequisites
- Set the `ROS_IP` environment variable in **all terminals**:
  ```bash
  export ROS_IP=192.168.1.28  # Replace with your network IP
  ```

## Steps

1. **Start the ROS Master**
   ```bash
   roscore
   ```

2. **Connect the Robot Serial Port (RSP) with ROS**
   ```bash
   rosrun rosserial_python serial_node.py tcp
   ```

3. **Publish the `motor_speed` Topic**
   ```bash
   rosrun control motor_controller.py
   ```

4. **Launch Freenect for Depth Camera**
   ```bash
   roslaunch freenect_launch freenect.launch depth_registration:=true
   ```

5. **Publish Static Transform between `camera_link` and `base_link`**
   ```bash
   rosrun tf2_ros static_transform_publisher 0 0 0 0 0 0 1 base_link camera_link
   ```

6. **Run RTAB-Map for SLAM**
   ```bash
   roslaunch rtabmap_ros rtabmap.launch rtabmap_args:="--delete_db_on_start" rtabmapviz:=false rviz:=true frame_id:=base_link
   ```

7. **Launch TEB Local Planner for Navigation**
   ```bash
   roslaunch control nav.launch
   ```

8. **Control the Robot with Keyboard**
   ```bash
   rosrun control manuel_control.py
   ```

## Notes
- Ensure the IP address (`192.168.1.28`) matches your network configuration.
- Each command should be run in a separate terminal with `ROS_IP` set.
- Verify that all required ROS packages (`rosserial_python`, `freenect_launch`, `rtabmap_ros`, `tf2_ros`, and `control`) are installed and configured properly.
