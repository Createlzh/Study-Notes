# ros_installation_guide

1. `sudo apt install ros-neotic-desktop-full` // 安装ROS，注意neotic对应ubuntu20.04

    > 如果提示 E: Unable to correct problems, you have held broken packages
    >
    > 参考 [Unable to correct problems, you have held broken packages](https://askubuntu.com/questions/223237/unable-to-correct-problems-you-have-held-broken-packages)
    >
    > 1. `dpkg --get-selections | grep hold` 查看held的包
    >
    > 2. `sudo apt install -f`
    >
    > 3. `sudo apt install update` `sudo apt install upgrade`
    >
    > 4. `sudo apt autoremove`
    >
    > 5. Select the *Fix Broken Packages* option in Synaptic package manager. Run the following commands to install Synaptic.
    >
    >    ```
    >    sudo apt update  
    >    sudo apt upgrade   
    >    sudo apt install synaptic  
    >    ```
    >
    >    Open Synaptic and in Synaptic select *Edit* -> ***Fix Broken Packages*** and then repeat *Edit* -> *Fix Broken Packages* a second time.
    >
    >    In Synaptic in the left pane click the *Custom Filters* button which is marked by the mouse cursor in the below screenshot. From the list in the top left corner select *Broken*. In the center pane will be listed any broken packages that still need to be repaired.
    >
    > 6. 最终解决方案：修改apt源
    >     `sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak`
    >     `sudo vim /etc/apt/sources.list`
    >
    >   ```bash
    >   deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
    >   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
    >   deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
    >   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
    >   deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
    >   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
    >   deb https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
    >   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
    >   deb https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
    >   deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
    >   ```
    >
    >   `sudo apt update`
    >
    >   `sudo apt upgrade`
    
    2. 刷新环境变量
    
       `echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc`
    
       `source ~/.bashrc`
    
    3. 运行`roscore`测试
    
    4. `sudo apt-get install ros-noetic-realsense2-camera`
       `sudo apt-get install ros-noetic-realsense2-description`

