# flir_YOLOv5_ros

## 🥢 Dependencies

Clone repo and install [requirements.txt](https://github.com/YeJin20/flir_YOLOv5_ros/blob/main/requirements.txt) in a [Python>=3.7.0](https://projooni.tistory.com/entry/ubuntu%EC%97%90%EC%84%9C-apt-get%EC%9C%BC%EB%A1%9C-python37-pip-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%8B%AC%EB%B3%BC%EB%A6%AD-%EB%A7%81%ED%81%AC-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0) environment, including [PyTorch>=1.7](https://enjoysomething.tistory.com/52)


### 🛹 FLIR Blackfly S
BFLY-PGE-31S4C-C : Blackfly 3.2 MP Color GigE PoE (Sony IMX265), 2048x1536 


Download 'spinnaker-2.0.0.147-amd64'
```Shell
./install_spinnaker.sh # install
sudo spinview # check camera connection
```


### 🛹 ROS Melodic
#### 1. Setup your sources.list
```Shell
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/rost-latest.list'
```


#### 2. Set up your keys
```Shell
sudo apt install curl
curl -s https://raw.githubsercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```


#### 3. Installation
```Shell
sudo apt update
sudo apt install ros-melodic-desktop-full
apt search ros-melodic
```


#### 4. Environment setup
```Shell
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
source /opt/ros/melodic/setup.bash
```


#### 5. Dependencies for building packages
```Shell
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
sudo apt install python-rosdep
sudo rosdep init
rosdep update
```


## 🥢 pointgrey_camera_driver for ROS
```Shell
sudo apt-get install ros-melodic-pointgrey-camera-driver
```

## 🥢 FLIR_camera_driver for ROS
```Shell
cd ~/catkin_ws/src && git clone https://github.com/ros-drivers/flir_camera_driver.git
cd ~/catkin_ws/ && catkin_make
source ~/catkin_ws/devel/setup.bash
roslaunch spinnaker_camera_driver camera.launch
```


## 🥢 YOLOv5 in docker
```Shell
docker pull yejin21/flir_yolov5_ros
docker run -it --gpus all --net=host -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v <local_workspace>:/root/share -e DISPLAY=unix$DISPLAY --privileged --name <container_name> yejin21/flir_yolov5_ros
```


## 🥢 Launch


### 🛹 Terminal 1


Modify "camera_serial" in <ros_workspace>/src/flir_camera_driver/spinnaker_camera_driver/launch.camera.launch
```Shell
cd <ros_workspace>
source ~/catkin_ws/devel/setup.bash
roslaunch spinnaker_camera_driver camera.launch
```


### 🛹 Terminal 2


```Shell
docker run -it --gpus all --net=host -v <LOCAL_DIRECTORY>:/root/share --previleged --name <NEW_NAMES> <CUSTOM_NAMES>
source /root/share/yolov5_ws/devel/setup.bash
```


```Shell
cd /root/share/yolov5_ws
roslaunch yolov5_ros yolov5.launch
```



