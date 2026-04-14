# Robot Workspace (`robot-ws`)

This is the primary ROS 2 (Humble) workspace governing the low-level nodes for the SBC (Single Board Computer) deployed on the robot (e.g., Radxa Zero).

It encapsulates robust sensor interfacing, footprint publication, inverse/forward kinematics (odometry), motor driving, and advanced reinforcement-learning driven stabilization policies.

## 🏗️ Structure

Included submodules orchestrate various components of the robot:
- **`ddsm115_driver`**: Interfaces with the DDSM115 direct-drive motors.
- **`imu_driver`**: Connects and publishes raw IMU readings.
- **`odometry_node`**: Fuses sensor logic into robust kinematics data.
- **`robot_tf_publisher`**: Coordinates standard static/dynamic `tf2` transforms and URDF definitions.
- **`robot_launch`**: The central launch nexus encompassing logic to boot single or composed modular sets of nodes.
- **`stabilization_controller`**: Real-time balancing proxy utilizing ONNX-runtime evaluated AI models for stabilization.
- **`ydlidar_ros2_driver`**: Customized code tailored for the TMini Laser Scanner (replaces the standard YDLidar node).

## 🚀 Compilation & Development

You can natively compile the system on a matching `aarch64` / `x86_64` machine or utilize the supplied Docker configuration.

### Using Docker (Highly Recommended)
We provide a simple Dockerization technique to ensure compiling code always perfectly mirrors the robot's ecosystem environment.

1. **Spin up the Docker Container**:
   We recommend using `docker compose` to effortlessly mount this workspace and boot into it:
   ```bash
   docker compose run --rm robot-dev
   # Alternatively via run: docker run --rm -it -v $(pwd):/robot-ws -w /robot-ws ros:humble-ros-base bash
   ```
2. **Compile the Workspace**:
   Inside the Docker container terminal (which automatically sources the base ROS distribution), simply invoke `colcon`:
   ```bash
   colcon build --symlink-install
   ```

## 🎮 Launching the Robot

Always source your newly compiled workspace configurations before attempting a launch:
```bash
source install/setup.bash
```

To initialize the primary process grouping, utilize the core components residing in `robot_launch`:
```bash
ros2 launch robot_launch robot.launch.py
```

### Launch Arguments
You can easily pass CLI arguments to tailor behavior:
- `enable_motors` (Default: `true`) — Boots up motor communications via DDSM115 Node and associated Odometry Node.
- `enable_rl_balancer` (Default: `false`) — Unlocks the AI balancing mechanics utilizing the ONNX runtime module.

**Example**:
```bash
ros2 launch robot_launch robot.launch.py enable_rl_balancer:=true
```
