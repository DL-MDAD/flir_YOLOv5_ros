# flir_YOLOv5_ros

## 🥢 Dependencies

Clone repo and install [requirements.txt](https://github.com/YeJin20/flir_YOLOv5_ros/blob/main/requirements.txt) in a [Python>=3.7.0](https://projooni.tistory.com/entry/ubuntu%EC%97%90%EC%84%9C-apt-get%EC%9C%BC%EB%A1%9C-python37-pip-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%8B%AC%EB%B3%BC%EB%A6%AD-%EB%A7%81%ED%81%AC-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0) environment, including [PyTorch>=1.7](https://enjoysomething.tistory.com/52)


### 🛹 FLIR Blackfly S
BFLY-PGE-31S4C-C : Blackfly 3.2 MP Color GigE PoE (Sony IMX265), 2048x1536 


Download 'spinnaker-2.0.0.147-aShell64'
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


## 🥢 1. YOLOv5 in docker
```Shell
docker pull osrf/ros:noetic-desktop-full
docker run --rm --gpus all nvidia/cuda:11.8.0-base-ubuntu22.04 nvidia-smi
docker run -it --gpus all --net=host -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v <local_workspace>:/root/share -e DISPLAY=unix$DISPLAY --privileged --name "ROS_noetic" osrf/ros:noetic-desktop-full
```


#### 1. Environment setup
```Shell
cd /root/share
mkdir yolov5_ws && cd yolov5_ws && mkdir src
```

#### 2. add in './bashrc'
```Shell
source /opt/ros/noetic/setup.bash
```

#### 3. build yolov5
```Shell
cd /root/share/yolov5_ws
catkin_make
```

#### 4. Clone the packages to ROS workspace nd install requirement for YOLOv5 submodule:
```Shell
cd <yolov5_ws_workspace>/src
apt-get update && apt-get install -y git
apt-get install -y python3-pip
git clone https://github.com/mats-robotics/detection_msgs.git
git clone --recurse-submodules https://github.com/mats-robotics/yolov5_ros.git 
cd yolov5_ros/src/yolov5
pip install -r requirements.txt # install the requirements for yolov5
```

#### 5. Add in '.bashrc'
```Shell
source /root/share/yolov5_ws/devel/setup.bash
```

#### 6. Build the OS packages:
```Shell
cd /root/share/yolov5_ws
catkin build yolov5_ros # build the ROS package
```

#### 7. Make the Python script executable
```Shell
cd /root/share/yolov5_ws/src/yolov5_ros/src
chmod +x detect.py
```

#### 8. Add in '.bashrc'
```Shell
source /root/share/yolov5_ws/devel/setup.bash
```

In another terminal,
```Shell
docker ps # check docker container
docker commit <CONTAINER_ID> <CUSTOM_NAMES>
```


#### 9. Using custom weights and dataset


* Put your weights into `yolov5_ros/src/yolov5`
* Put the yaml file for your dataset classes into `yolov5_ros/src/yolov5/data`
* Change related ROS parameters in yolov5.launch: `weights`,  `data`


Custom pre-trained model *best.pt* in */root/share/yolov5_ws/src/yolov5_ros/src*


Modify */root/share/yolov5_ws/src/yolov5_ros/launch/yolov5.launch*


<!-- Detection configuration -->
<arg name="weights" default="$(find yolov5_ros)/src/yolov5/korea_light_weight.pt">
<arg name="data" default="$(find yolov5_ros)/src/yolov5/data/korea_light.yaml">
...
<!-- Visualize using OpenCV window -->
<arg name="view_image" default="false"/>


Change the file in `launch/yolov5.launch` to `yolov5.launch`


<!-- ROS topics -->
<arg name="input_image_topic" default="/camera0/image_color"/>


## 🥢 cv_bridge in docker


Compiling ROS cv_bridge with Python3


#### 1. Dependencies
```Shell
sudo apt-get install python3-pip python3-catkin-tools python3-dev python3-numpy
sudo pip3 install rospkg catkin_pkg
```


#### 2. Workspace

Next, create a separate workspace to compile the `bridge_cv` ROS package. We would be naming the directory `cvbridge_build_ws`.


```Shell
mkdir -p ~/share/cvbridge_build_ws/src
cd ~/share/cvbridge_build_ws/src

git clone -b noetic https://github.com/ros-perception/vision_opencv.git
```


#### 3. Change


Therefore, we need to make a slight change to the `cd ~/share/cvbridge_build_ws/src/vision_opencv/CMakeLists.txt`


`find_package(Boost REQUIRED python37)`


to


`find_package(Boost REQUIRED python3)`


#### 4. Compilation


```Shell
cd ~/share/cvbridge_build_ws
catkin config -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.8m -DPYTHON_LIBRARY=/usr/lib/aarch64-linux-gnu/libpython3.8m.so
catkin config --install
```


After the configuration is completed, build the package:


```Shell
catkin build cv_bridge
```


To use the package, you could source it via:


```Shell
source install/setup.bash --extend
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



## 🥢 Issues


### 🛹 QObject::moveToThread:


```Shell
$ pip uninstall opencv-python
$ pip install opencv-python==4.3.0.36
$ pip list|grep opencv-python
opencv-python        4.3.0.36
```
