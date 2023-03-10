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

   1. [planning simulation](https://autowarefoundation.github.io/autoware-documentation/main/tutorials/ad-hoc-simulation/planning-simulation/)

   2. [rosbag replay simulation](https://autowarefoundation.github.io/autoware-documentation/main/tutorials/ad-hoc-simulation/rosbag-replay-simulation/).

   3. AWSIM simulation

      ### A. Preparation
      
         Before trying AWSIM simulation, you have to once exit the Docker container by `exit` command.

         Required PC specs
         - CPU: 6cores and 12thread or higher
         - GPU: RTX2080Ti or higher

         
         Localhost Settings
         ```bash
         if [ ! -e /tmp/cycloneDDS_configured ]; then
            sudo sysctl -w net.core.rmem_max=2147483647
            sudo ip link set lo multicast on
            touch /tmp/cycloneDDS_configured
         fi
         ```
         
         Download AWSIM binary
         ```bash
         sudo apt install libarchive-tools -y
         cd $HOME
         wget -qO- https://github.com/tier4/AWSIM/releases/download/v1.1.0/AWSIM_v1.1.0.zip | bsdtar -xvf-
         chmod +x ./AWSIM_v1.1.0/AWSIM_demo.x86_64
         ```
         
         Download a map
         ```bash
         wget -qO- https://github.com/tier4/AWSIM/releases/download/v1.1.0/nishishinjuku_autoware_map.zip | bsdtar -xvf-
         mv nishishinjuku_autoware_map ~/autoware_map
         ```

      ### B. Run simulation

         #### Terminal 1

         Launch a Docker container
         ```bash
         rocker --nvidia --x11 --user --privileged --network=host --volume $HOME/autoware_map --volume /tmp -- ghcr.io/tier4/online:humble-awsim-stable-prebuilt-cuda
         ```

         Launch Autoware
         ```bash
         export ROS_LOCALHOST_ONLY=1
         export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
         export RCUTILS_COLORIZED_OUTPUT=1

         ros2 launch autoware_launch e2e_simulator.launch.xml \
         vehicle_model:=sample_vehicle \
         sensor_model:=awsim_sensor_kit \
         map_path:=$HOME/autoware_map/nishishinjuku_autoware_map
         ```

         If you feel it consumes memory too much, you can turn off Displays > Map >  PointCloudMap on Rviz panel.
         ![map_pointcloud_off](https://user-images.githubusercontent.com/42209144/224251072-e568405d-de19-4b5e-a86f-709ce12fed3b.png)

         #### Terminal 2

         Launch a Docker container
         ```bash
         rocker --nvidia --x11 --user --privileged --network=host --volume $HOME/autoware_map --volume $HOME/AWSIM_v1.1.0 --volume /tmp -- ghcr.io/tier4/online:humble-awsim-stable-prebuilt-cuda
         ```

         Launch AWSIM
         ```bash
         export ROS_LOCALHOST_ONLY=1
         export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
         export RCUTILS_COLORIZED_OUTPUT=1

         cd $HOME
         ./AWSIM_v1.1.0/AWSIM_demo.x86_64
         ```

      ### C. Enjoy self-Driving simulation

         Please refer to "5. Let's run the self-Driving simulation" in [AWSIM Quick Start Demo](https://tier4.github.io/AWSIM/GettingStarted/QuickStartDemo/).

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
