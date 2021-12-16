### https://dev.to/et813/install-docker-and-nvidia-container-m0j
## According to the site,
## Make sure you have installed the NVIDIA driver and Docker 19.03 for your Linux distribution Note that you do not need to install the CUDA toolkit on the host, but the driver needs to be installed.
## ^^ See this for running GUIs

##This is better -- didnt work outof the box
# https://github.com/henry2423/docker-ros-vnc

FROM ubuntu:18.04

# Cloning git repo
RUN apt-get update

# Installing ROS-melodic
RUN apt-get install -y gnupg2
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y lsb-core
ARG DEBIAN_FRONTEND=noninteractive

# INSTALL OTHER NECESSARY PACKAGES
RUN apt-get install -y vim
RUN apt-get install -y tmux
RUN apt-get install -y git
RUN apt-get install -y wget
RUN apt-get update
RUN apt-get install -y python-pip
RUN apt-get install -y libpng16-16
RUN apt-get install -y libjpeg-turbo8
RUN apt-get install -y libtiff5

RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
RUN curl -sSL 'http://keyserver.ubuntu.com/pks/lookup?op=get&search=0xC1CF6E31E6BADE8868B172B4F42ED6FBAB17C654' | apt-key add -
RUN apt update
RUN apt install -y ros-melodic-desktop

# Install Gazebo
RUN sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list' && \
    wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add - && \
    apt-get update && \
    apt-get install -y gazebo9 libgazebo9-dev && \
    apt-get install -y ros-melodic-gazebo-ros-pkgs ros-melodic-gazebo-ros-control

RUN apt-get install -y python-rosdep
RUN rosdep init
RUN rosdep update
RUN echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
RUN apt install -y python-rosinstall python-rosinstall-generator python-wstool build-essential

# Intalling pyton-catkin
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
RUN wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
RUN apt-get update
RUN apt-get install -y python-catkin-tools
RUN apt-get install -y software-properties-common

#Turtlebot3 Dependencies
RUN apt-get install -y ros-melodic-joy ros-melodic-teleop-twist-joy \
    ros-melodic-teleop-twist-keyboard ros-melodic-laser-proc \
    ros-melodic-rgbd-launch ros-melodic-depthimage-to-laserscan \
    ros-melodic-rosserial-arduino ros-melodic-rosserial-python \
    ros-melodic-rosserial-server ros-melodic-rosserial-client \
    ros-melodic-rosserial-msgs ros-melodic-amcl ros-melodic-map-server \
    ros-melodic-move-base ros-melodic-urdf ros-melodic-xacro \
    ros-melodic-compressed-image-transport ros-melodic-rqt* \
    ros-melodic-gmapping ros-melodic-navigation ros-melodic-interactive-markers

RUN sudo apt-get install -y ros-melodic-dynamixel-sdk
RUN sudo apt-get install -y ros-melodic-turtlebot3-msgs
RUN sudo apt-get install -y ros-melodic-turtlebot3
ENV TURTLEBOT3_MODEL=burger

#install Tilix
RUN apt-get update
RUN apt-get install -y tilix

#install net-tools
RUN apt-get install -y net-tools

#install pip and tensorflow % ISSUE: python 3 vs python 2
# RUN apt-get install -y software-properties-common
# RUN add-apt-repository ppa:deadsnakes/ppa -y
# RUN apt-get install python3.8 -y
# RUN apt-get install -y python3-pip
# RUN pip3 install --upgrade pip
# RUN pip3 install tensorflow
# RUN apt-get install -y python3-tqdm
# RUN pip3 install Pillow

#install miniconda
ENV PATH="/root/miniconda3/bin:${PATH}"
ARG PATH="/root/miniconda3/bin:${PATH}"
RUN apt-get update

RUN apt-get install -y wget && rm -rf /var/lib/apt/lists/*

RUN wget \
    https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && mkdir /root/.conda \
    && bash Miniconda3-latest-Linux-x86_64.sh -b \
    && rm -f Miniconda3-latest-Linux-x86_64.sh 
RUN conda --version
RUN conda init

# copy conda dependency yaml file and install requirements
COPY ./lab7785_final.yml .
RUN conda env create -f ./lab7785_final.yml

# install enum
RUN pip install enum34

# Copy gazebo burger(with camera) robot files into image, sean wilson's git repo: https://github.gatech.edu/swilson64/turtlebot3_sim_update 
# COPY /gazeboModel/* /opt/ros/melodic/share/turtlebot3_description/urdf/
#Set some bashrc stuff
# RUN echo 'alias scat="source devel/setup.bash"' >> ~/.bash_aliases
# RUN echo 'alias velkill="rostopic pub -r 10 /cmd_vel geometry_msgs/Twist  "{linear:  {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0,z: 0.0}}""' >> ~/.bash_aliases


#Set Work dir
WORKDIR /root/catkin_ws

CMD ["tail", "-f", "/dev/null"]
