# flir_YOLOv5_ros

🥢 Dependencies
Clone repo and install [requirements.txt](https://github.com/YeJin20/flir_YOLOv5_ros/blob/main/requirements.txt) in a [Python>=3.7.0](https://projooni.tistory.com/entry/ubuntu%EC%97%90%EC%84%9C-apt-get%EC%9C%BC%EB%A1%9C-python37-pip-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%8B%AC%EB%B3%BC%EB%A6%AD-%EB%A7%81%ED%81%AC-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0) environment, including [PyTorch>=1.7](https://enjoysomething.tistory.com/52)

🛹 FLIR Blackfly S
BFLY-PGE-31S4C-C : Blackfly 3.2 MP Color GigE PoE (Sony IMX265), 2048x1536 

Download 'spinnaker-2.0.0.147-amd64'
'''Shell
./install_spinnaker.sh # install
sudo spinview # check camera connection
'''

🛹 ROS Melodic
1. Setup your sources.list
'''Shell
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/rost-latest.list'
'''

2. Set up your keys
'''Shell
sudo apt install curl
curl -s https://raw.githubsercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
'''

3. Installation
'''Shell
sudo apt update
sudo apt install ros-melodic-desktop-full
apt search ros-melodic
'''

4. Environment setup
'''Shell
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
source /opt/ros/melodic/setup.bash
'''

🥢 pointgrey_camera_driver for ROS
'''Shell
cd ~/catkin_ws/src && git clone https://github.com/ros-drivers/pointgrey_camera_driver.git
cd ~/catkin_ws/ && catkin_make
'''

🥢 FLIR_camera_driver for ROS
'''Shell
cd ~/catkin_ws/src && git clone https://github.com/ros-drivers/flir_camera_driver.git
cd ~/catkin_ws/ && catkin_make
roslaunch spinnaker_camera_driver camera.launch
'''

🥢 YOLOv5 in docker
'''Shell
docker pull osrf/ros:noetic-desktop-full
docker run --rm --gpus all nvidia/cuda:11.8.0-base-ubuntu22.04 nvidia-smi
docker run -it --gpus all --net=host -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v <local_workspace>:/root/share -e DISPLAY=unix$DISPLAY --privileged --name "ROS_noetic" osrf/ros:noetic-desktop-full
'''

1. Environment setup
'''Shell
cd /root/share
mkdir yolov5 && cd yolov5 && mkdir src
'''

2. add in './bashrc'
'''Shell
source /opt/ros/noetic/setup.bash
'''

3. build yolov5
'''Shell
cd ~/catkin_ws
catkin_make
'''

4. Clone the packages to ROS workspace nd install requirement for YOLOv5 submodule:
'''Shell
cd <ros_workspace>/src
apt-get update && apt-get install -y git
apt-get install -y python3-pip
git clone https://github.com/mats-robotics/detection_msgs.git
git clone --recurse-submodules https://github.com/mats-robotics/yolov5_ros.git 
cd yolov5_ros/src/yolov5
pip install -r requirements.txt # install the requirements for yolov5
'''

5. Add in '.bashrc'
'''Shell
source /root/share/catkin_ws/devel/setup.bash
'''

6. Build the OS packages:
'''Shell
cd <ros_workspace>
catkin build yolov5_ros # build the ROS package
'''

7. Make the Python script executable
'''Shell
cd <ros_workspace>/src/yolov5_ros/src
chmod +x detect.py
'''
