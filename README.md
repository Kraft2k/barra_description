# barra_description

On Jetson NX with Ubuntu 20.04 need execute this commands

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
   sudo systemctl enable chmod-ttyACM0.service
   ```
 4. Reboot your system, and the command will be executed automatically    after startup.


 



colcon build --packages-select barra_description

sudo bluetoothctl

connect 90:E9:D2:3B:C6:AF

ros2 run zuuu_hal hal

ros2 launch barra_description launch_robot.launch.py

ros2 launch barra_description rplidar.launch.py

ros2 launch barra_description joystick.launch.py

ros2 launch slam_toolbox online_async_launch.py params_file:=./src/barra_description/config/mapper_params_online_async.yaml use_sim_time:=false
