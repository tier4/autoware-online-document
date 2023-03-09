# Docker installation for quick start

## How to set up a development environment

1. Installing dependencies manually

   - [Install Docker Engine](https://github.com/autowarefoundation/autoware/tree/awsim-stable/ansible/roles/docker_engine#manual-installation)

   - [Install NVIDIA Container Toolkit](https://github.com/autowarefoundation/autoware/tree/awsim-stable/ansible/roles/nvidia_docker#manual-installation)

   - [Install rocker](https://github.com/autowarefoundation/autoware/tree/awsim-stable/ansible/roles/rocker#manual-installation)


## How to build a docker image
  

1. 
   ```
   git clone git@github.com:tier4/autoware-online-document.git
   
   ```



## How to set up a workspace

1. Create the `autoware_map` directory for map data later.

   ```bash
   mkdir ~/autoware_map
   ```

2. Launch a Docker container.

   ```bash
   rocker --nvidia --x11 --user --volume $HOME/autoware_map -- ghcr.io/autowarefoundation/autoware-universe:humble-latest-prebuilt
   ```

   For more advanced usage, see [here](https://github.com/autowarefoundation/autoware/tree/main/docker/README.md).

3. Run Autoware simulator

   Inside the container, you can run the Autoware simulation by following this tutorial:

   [planning simulation]()

   [rosbag replay simulation]().
