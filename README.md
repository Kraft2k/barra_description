# barra_description

### Install dependensies

sudo apt install ros-foxy-xacro

sudo apt install ros-foxy-twist-mux

sudo apt install ros-foxy-diagnostic-updater

sudo apt install ros-foxy-tf-transformations

sudo apt install ros-foxy-ros2-control ros-foxy-ros2-controllers

sudo apt install ros-foxy-navigation2 ros-foxy-nav2-bringup

sudo apt install ros-foxy-slam-toolbox

sudo apt install ros-foxy-librealsense2-*

sudo apt install ros-foxy-rqt-image-view

sudo apt install ros-foxy-image-transport-plugins

sudo apt install ros-foxy-v4l2-camera

### On Jetson NX with Ubuntu 20.04 need execute this commands

sudo chmod 666 /dev/ttyACM0

sudo chmod 666 /dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0001-if00-port0

To automate the process

 1. Create a new service file using the following command:
   ```
   sudo vim /etc/systemd/system/chmod-USB.service
   ```

 2. Add the following content to the file:
   ```
    [Unit]
        Description=Set permissions for USB ports
        After=multi-user.target

    [Service]
        Type=oneshot
        ExecStart=/bin/chmod 666 /dev/ttyACM0
        ExecStart=/bin/chmod 666 /dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0001-if00-port0

    [Install]
        WantedBy=multi-user.target

   ```

 3. Enable the service to run at startup using the following command:
   ```
   sudo systemctl enable chmod-USB.service
   ```
 4. Reboot your system, and the command will be executed automatically    after startup.


 
### Build package

colcon build --packages-select barra_description


ros2 launch barra_description launch_sim.launch.py world:=./src/barra_description/worlds/obstacles.world

### Connect joystick

sudo bluetoothctl

last four digits of joystick: 9866

ros2 launch barra_description joystick.launch.py


### Run safety behaviour


(ros2 launch barra_description rplidar.launch.py)

ros2 launch rplidar_ros rplidar.launch.py


ros2 launch barra_description launch_robot.launch.py


ros2 run mobile_base_unit mobile_base




### Simulation

ros2 launch barra_description launch_sim.launch.py world:=./src/barra_description/worlds/obstacle.world


### For mapping

ros2 launch barra_description launch_robot.launch.py


ros2 run mobile_base_unit mobile_base

ros2 launch slam_toolbox online_async_launch.py params_file:=./ros2_ws/src/barra_description/config/mapper_params_online_async.yaml use_sim_time:=false


### For save map 

mkdir maps 

ros2 run nav2_map_server map_saver_cli -f maps/my_map --ros-args -p save_map_timeout:=10000


### For navigation

ros2 launch nav2_bringup navigation_launch.py use_sim_time:=false

add map and add topic /global_costmap


### Camera


ros2 run realsense2_camera realsense2_camera_node


ros2 run realsense2_camera realsense2_camera_node --ros-args -p camera_frame_id:='camera_link_optical'

ros2 run rqt_image_view rqt_image_view

ros2 run image_transport republish compressed raw --ros-args -r in/compressed:=/color/image_raw/compressed -r out:=/camera/image_raw/uncompressed

### Calibration

ros2 launch ball_tracker ball_tracker.launch.py tune_detection:=true detect_only:=true image_topic:=/camera/image_raw/uncompressed

ros2 run ball_tracker detect_ball --ros-args -p tuning_mode:=true -r image_in:=camera/image_raw

ros2 launch ball_tracker ball_tracker.launch.py params_file:=src/barra_description/config/ball_tracker_params_robot.yaml image_topic:=/camera/image_raw/uncompressed


### Follow aruco marker

ros2 run realsense2_camera realsense2_camera_node

ros2 run ros2_aruco aruco_node

ros2 run mobile_base_unit mobile_base

ros2 run mobile_base_unit follow_aruco_marker

### Find aruco markers

ros2 run realsense2_camera realsense2_camera_node

ros2 run ros2_aruco aruco_node

ros2 run mobile_base_unit mobile_base

ros2 run mobile_base_unit find_markers_server

ros2 run mobile_base_unit find_markers_client


### Voice control 

on desktop

rus2 run voice_recognition command_publisher

on rover

ros2 launch barra_description rplidar.launch.py

ros2 run mobile_base_unit mobile_base

ros2 run mobile_base_unit voice_control
