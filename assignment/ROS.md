### How to Install ROS　Kinetic

1. 首先为了在Ubuntu 16下安装ROS，必须先让系统接收这个软件的相关资源包。

  `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list`

2. 然后设置keys

  ```sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
  ```

3. 接下来就可以直接安装了

  * 首先更新系统软件

    `sudo apt-get update`

  * 选择Desktop-Full Install 安装

    `sudo apt-get install ros-kinetic-desktop-full`

4. 如果要使用ROS的话，必须先初始化rosdep。rosdep能够让你更加方便的安装你所需要的系统依赖文件。

  `sudo rosdep init`
  `rosdep update`

5. 设置ROS环境

  `echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc`

6. 安装rosinstall工具，这是一个经常在ROS使用到的命令行工具

`sudo apt-get install python-rosinstall`

到此ROS安装完毕。

### Cartpgrapher 的安装

不多说，直接贴代码，看不懂

```
# Install wstool and rosdep.
sudo apt-get update
sudo apt-get install -y python-wstool python-rosdep ninja-build

# Create a new workspace in 'catkin_ws'.
mkdir catkin_ws
cd catkin_ws
wstool init src

# Merge the cartographer_ros.rosinstall file and fetch code for dependencies.
wstool merge -t src https://raw.githubusercontent.com/googlecartographer/cartographer_ros/master/cartographer_ros.rosinstall
wstool update -t src

# Install deb dependencies.
rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y

# Build and install.
catkin_make_isolated --install --use-ninja
source install_isolated/setup.bash
```

网络出了点问题，所以跑demons没做完，以下是2d过程中的截图

![2D](https://raw.githubusercontent.com/Scintillium/Res/master/DOL-RES/2D.png)
