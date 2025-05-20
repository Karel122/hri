ROS1 docker with robotblockset_python, franka_ros and robot_module_msgs preinstalled. ROS_MASTER_URI and ROS_IP can be set from .env file.

It runs jupyterlab in the background. Password can be set in .env file.

NOTEBOOK_ROOT_DIR sets jupyter notebook's root directory. Default is /rbs_ws/src.

If ADDITIONAL_COMMAND=0, then any additional command provided will be passed as arguments (argv) to Jupyter Lab when it is run.
If ADDITIONAL_COMMAND=1, Jupyter Lab will run in the background (with nohup or similar), and an additional command can be started after Jupyter Lab is launched.

You can use ros_entrypoint.sh instead if you don't want to run Jupyter.

# Clone instructions

This repo uses submodules for external dependencies, therefore you have to use --recurse-submodules flag while clonining:

```bash
git clone --recurse-submodules  https://repo.ijs.si/hcr/rbs-docker
```


# Usage with docker compose

```bash
docker compose up -d
```
You don't have to build docker image (it will be done first time you run compose).


# Usage with Rocker

Rocker is a tool that enables running Docker images with dcustomized local support, such as GPU acceleration support. It also facilitates cleaner file permissions by injecting user-specific files for more controlled mounting of file volumes. See https://github.com/osrf/rocker

Installation:
```bash
pip install rocker
```

First, build docker image:
```bash
docker build . -t rbs-docker-jupyter
```

Hint: You can also reuse rbs-docker-jupyter:latest built by docker compose.


Intel GPU:
```bash
rocker --devices /dev/dri --x11 --user --network=host --env-file .env --image-name rbs_rocker rbs-docker-jupyter:latest
```

NVIDIA GPUs:
```bash
rocker --nvidia --user --network=host --env-file .env --image-name rbs_rocker rbs-docker-jupyter:latest
```

Optionally, you can also specify:
```bash
--volume /path-to/host_folder:/container_folder
--home # entire user directory
```

