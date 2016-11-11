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
