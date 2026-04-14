# Use ROS Humble base image as the starting point
FROM ros:humble-ros-base

# Disable interactive prompts during apt operations
ENV DEBIAN_FRONTEND=noninteractive

# Update apt cache and install basic compilation dependencies usually needed for ROS 2
RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    build-essential \
    ros-humble-foxglove-bridge \
    && rm -rf /var/lib/apt/lists/*

# Pre-source the ROS Humble workspace on bash invocation
RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc

# Set the default working directory aligned with our compose mount
WORKDIR /robot-ws

# Keep standard bash running
CMD ["bash"]
