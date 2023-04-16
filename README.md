# Docker installation and Autoware simulation

## How to set up a development environment

1. Install dependencies manually

   - [Install Docker Engine](https://github.com/autowarefoundation/autoware/tree/awsim-stable/ansible/roles/docker_engine#manual-installation)

   - [Install NVIDIA Container Toolkit](https://github.com/autowarefoundation/autoware/tree/awsim-stable/ansible/roles/nvidia_docker#manual-installation)

   - [Install rocker](https://github.com/autowarefoundation/autoware/tree/awsim-stable/ansible/roles/rocker#manual-installation)


## How to run a simulator

1. Create the `autoware_map` directory for map data later.

   ```bash
   mkdir ~/autoware_map
   ```

2. Launch a Docker container using pre-built image

   ```bash
   rocker --nvidia --x11 --user --volume $HOME/autoware_map -- ghcr.io/tier4/online:humble-awsim-stable-prebuilt-cuda
   ```

3. Run Autoware simulator

   Inside the container, you can run the Autoware simulation by following this tutorial:

   1. [planning simulation](docs/planning_simulation.md)

   2. [rosbag replay simulation](docs/rosbag_replay_simulation.md)

   3. [AWSIM simulation](docs/awsim_simulation.md)

## How to build a docker image locally

1. Build a Docker image
   
   ```bash
   cd $HOME
   git clone git@github.com:tier4/autoware-online-document.git autoware
   cd autoware
   chmod +x -R docker
   ./docker/build.sh
   ```

2. Use the Docker image
   
   You can use your Docker image built locally by replacing \
   `ghcr.io/tier4/online:humble-awsim-stable-prebuilt` (pre-built) 
   with `ghcr.io/tier4/online:humble-awsim-stable` (locally built).
   Keep in mind that you should source setup.sh by `source ~/autoware/install/setup.bash`
