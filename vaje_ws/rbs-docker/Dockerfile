FROM osrf/ros:noetic-desktop-full

# Set environment variable for non-interactive apt installs
ENV DEBIAN_FRONTEND=noninteractive

# change the default shell to Bash
SHELL [ "/bin/bash" , "-c" ]

# Install necessary tools
RUN apt-get update && \
    apt-get install -y python3-pip python-is-python3 python3-catkin-tools curl gnupg2 neovim bash-completion iputils-ping  python3-tk less texlive-xetex texlive-fonts-recommended texlive-plain-generic  xclip && \
    rm -rf /var/lib/apt/lists/*

# Init rosdep
RUN rosdep update

# Setup environment
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

# create a ROS workspace
RUN mkdir -p /rbs_ws/src/

COPY franka_ros/franka_msgs /rbs_ws/src/franka_msgs
COPY franka_ros/franka_description /rbs_ws/src/franka_description
COPY franka_ros/franka_gripper /rbs_ws/src/franka_gripper
# Build only msgs for franka_gripper
COPY mods /tmp/mods
RUN mv /tmp/mods/franka_gripper/CMakeLists.txt /rbs_ws/src/franka_gripper/CMakeLists.txt && rm -rf /tmp/mods
COPY robot_module_msgs /rbs_ws/src/robot_module_msgs
COPY robotblockset_python /rbs_ws/src/robotblockset_python
COPY configs /rbs_ws/src/configs

RUN cd /rbs_ws/src/robotblockset_python && \
    python3 -m pip install -e .
RUN source /opt/ros/noetic/setup.bash && \
    cd /rbs_ws && \ 
    catkin build

# jupyter
RUN pip install jupyter jupyterlab jupyter-collaboration ipympl scipy

# jupyter & ros entrypoint
COPY jupyter_entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

