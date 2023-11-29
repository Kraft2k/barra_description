# barra_description

sudo chmod 666 /dev/ttyACM0

sudo chmod 666 /dev/serial/by-id/usb-Silicon_Labs_CP2102_USB_to_UART_Bridge_Controller_0001-if00-port0

colcon build --packages-select barra_description

sudo bluetoothctl

connect 90:E9:D2:3B:C6:AF

ros2 run zuuu_hal hal

ros2 launch barra_description launch_robot.launch.py

ros2 launch barra_description rplidar.launch.py

ros2 launch barra_description joystick.launch.py

ros2 launch slam_toolbox online_async_launch.py params_file:=./src/articubot_one/config/mapper_params_online_async.yaml use_sim_time:=false

