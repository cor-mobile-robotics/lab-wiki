# VRPN Mocap ROS 2 Bridge

This page explains how to use the `vrpn_mocap` bridge to publish tracked pose, twist, and acceleration as ROS topics:

1. How to connect ROS 2 to the Vicon system through VRPN.
2. How to isolate a robot and laptop on a shared Wi-Fi network using a custom ROS domain.

## 1. Overview
Once the Vicon PC runs and successfully tracks one or more objects, as explained in [Vicon Motion Capture](mocap.md), a tracked object named `<tracker_name>` is published as:
- `/vrpn_mocap/<tracker_name>/pose`
- `/vrpn_mocap/<tracker_name>/twist`
- `/vrpn_mocap/<tracker_name>/accel`

## 2. Installing `vrpn_mocap` from binary release
On Ubuntu with ROS 2:
```
sudo apt install ros-<rosdistro>-vrpn-mocap
```


## 3. Starting the VRPN client
After sourcing ROS2, run:
``` 
ros2 launch vrpn_mocap client.launch.yaml server:=192.168.0.232 port:=3883
```
Assuming `192.168.0.232` is the IP adress of the of the Vicon PC, as explained in [Network](network.md), and `3883` the port.

All actively tracked objects on the Vicon PC will now be published as:
- `/vrpn_mocap/<tracker_name>/pose`
- `/vrpn_mocap/<tracker_name>/twist`
- `/vrpn_mocap/<tracker_name>/accel`

Where <tracker_name> is the object's name on the Vicon PC. After starting, you can verify topics appear using:

```
ros2 topic list | grep vrpn
```


### 3.1 Parameters
Useful launch arguments may be:
- `update_freq` (double) – frequency at which tracked object topics are published, default value is 100. In some cases, for example when recording a rosbag, it may be useful to reduce this.
- `frame_id` (string) – frame name of the fixed world frame (default: `"world"`)
- `sensor_data_qos` – use best-effort QoS for the VRPN data stream; set to `false` to use the system default QoS, which is reliable (default: `true`)
- `use_vrpn_timestamps` (bool) – use timestamps coming from VRPN. This ensures that the interval between frame timestamps is not modified (default: `false`)


Example:
```
ros2 launch vrpn_mocap client.launch.yaml server:=192.168.0.232 port:=3883 update_freq:=15.0
```

A more detailed explanation of parameters and the default launch file is provided at https://docs.ros.org/en/humble/p/vrpn_mocap/index.html. 


A more detailed explanation of parameters and the default launch file is provided at https://docs.ros.org/en/humble/p/vrpn_mocap/index.html.

## 4. ROS domain setup on MRL Wi-Fi network
When multiple people use ROS 2 on the same Wi-Fi network, topics may be shared across all users on the LAN. Set a `ROS_DOMAIN_ID` to specify which devices should be able to discover each other. For example, when using:
- a robot
- a laptop

Run, in every terminal on both devices:
```
export ROS_DOMAIN_ID=42
```
To set the domain id to 42. You can also add this to `~/.bashrc` on both devices. 


### 4.1 Troubleshooting
Setting up the domain ID can be a hassle. Consult https://docs.ros.org/en/eloquent/Tutorials/Configuring-ROS2-Environment.html for more information. Alternatively, use ChatGPT for troubleshooting. Two or three points that may be worth considering, at your own risk, are:

- Restarting the ROS background service on the robot when topics are not visible, e.g. with `sudo systemctl restart mirte-ros`
- Ensuring `ROS_LOCALHOST_ONLY` is set to inactive (`0` instead of `1`) using: `export ROS_LOCALHOST_ONLY=0`
- Restarting the daemon on your PC with `ros2 daemon stop` and then `ros2 daemon start`

## 5. Acknowledgement
The VRPN client was developed by a third party and is publicly available via https://github.com/vrpn/vrpn.
