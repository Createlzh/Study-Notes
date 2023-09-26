# Ubuntu 20.04安装docker版微信

### 基于鱼香ROS一键安装脚本桌面版

* github：https://github.com/fishros/install
* web：https://fishros.com/#/fish_home



### 基于deepin操作系统的docker-wechat

##### 出处

* github：https://github.com/top-bettercode/docker-wechat
* dockerhub：https://hub.docker.com/r/bestwu/wechat

##### 安装步骤

1. 允许所有用户访问X11服务，运行命令：

   ```bash
   xhost +
   ```

2. 安装docker

3. 解决非root用户权限问题

   ```bash
   # 1.切换root用户
   $ sudo su
   # 2.添加用户到docker用户组，将{username}替换为用户名
   $ sudo gpasswd -a {username} docker
   # 如果提示没有docker组，先建立该用户组
   $ sudo groupadd docker
   # 查看/etc/group,确定是否存在docker组
   $ cat /etc/group | grep docker
   # 3.添加后重新登陆到你的用户组，执行命令查看是否包含docker组
   $ groups
   # 4.重启docker
   $ sudo service docker restart
   # 5.新开终端，查看权限
   $ docker images
   # 6.如果提示unix /var/run/docker.sock权限不够，则修改/var/run/docker.sock权限
   $ sudo chmod a+rw /var/run/docker.sock
   ```

4. 拉取docker镜像

   ```bash
   sudo docker pull bestwu/wechat
   ```

5. 运行镜像

   ```bash
   sudo docker run -d --name wechat --device /dev/snd --ipc="host"\
     -v /tmp/.X11-unix:/tmp/.X11-unix \
     -v /home/zhliao/WeChat/.WeChatFiles:/WeChatFiles \
     -v /home/zhliao/WeChat/:/home/wechat \
     -e DISPLAY=$DISPLAY \
     -e XMODIFIERS=@im=ibus \
     -e QT_IM_MODULE=ibus \
     -e GTK_IM_MODULE=ibus \
     -e AUDIO_GID=`getent group audio | cut -d: -f3` \
     bestwu/wechat
   ```

   > 这里修改`-e DISPLAY=unix$DISPLAY`为`-e DISPLAY=$DISPLAY`，否则容器图形界面不显示

   ```bash
   docker run -d --name wechat --device /dev/snd \
   -v /tmp/.X11-unix:/tmp/.X11-unix \
   -v $HOME/WeChatFiles:/WeChatFiles \
   -e DISPLAY=$DISPLAY \
   -e XMODIFIERS=@im=ibus \
   -e QT_IM_MODULE=ibus \
   -e GTK_IM_MODULE=ibus \
   -e AUDIO_GID=`getent group audio | cut -d: -f3` \
   -e GID=`id -g` \
   -e UID=`id -u` \
   bestwu/wechat
   ```



### 基于Windows的盒装微信

##### 出处

* dockerhub：https://hub.docker.com/r/zixia/wechat

##### 安装步骤

1. 获取脚本

   ```bash
   wget https://raw.githubusercontent.com/huan/docker-wechat/master/dochat.sh
   ```

2. 安装

   ```bash
   sudo bash dochat.sh
   ```

3. 报错提示“docker: Error response from daemon: could not select device driver "" with capabilities: [[gpu]].”

   > docker19 及之后的版本中使用 nvidia gpu 已经不需要单独安装 nvidia-docker 了，这已经被集成到了 docker 中。
   >
   > 要使用宿主机的 GPU，需要在 docker run 的时候添加 --gpus [xxx] 参数。但是，在我们刚刚安装好 docker 并构建好镜像之后，直接这样运行是有问题的。
   >
   > 实际上，我们在通过 --gpus 参数来使用宿主机的 GPU 时，需要先安装一个英伟达的容器运行时。
   >
   > 另外需要注意的是，这个东西是不能直接 apt install，会报找不到该软件，需要先添加英伟达的 apt 软件源。
   >
   > https://nvidia.github.io/nvidia-container-runtime/

   ```bash
   # 添加GPGkey
   sudo curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey | \
     sudo apt-key add -
   distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
   # 添加apt源
   sudo curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list | \
     sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
   # 更新apt源
   sudo apt update
   # 安装
   sudo apt install nvidia-container-runtime
   ```

   > 安装apt install这一步可能很慢，实际上可以直接到官网下载deb包手动`dpkg -i [deb]`安装
   >
   > https://github.com/NVIDIA/libnvidia-container/tree/gh-pages/stable
   >
   > 进入ubuntu20.04文件夹后，发现deb被指向为ubuntu18.04版本，进入18.04文件夹后下载相应的deb包并安装：
   >
   > ```bash
   > libnvidia-container1_1.9.0-1_amd64.deb
   > libnvidia-container-tools_1.9.0-1_amd64.deb
   > nvidia-container-toolkit_1.9.0-1_amd64.deb
   > nvidia-docker2_2.10.0-1_all.deb
   > ```

4. 报错提示无法创建目录

   ```bash
   Disabling patch for /home/user/.wine/drive_c/users/user/AppData/Roaming/Tencent/WeChat ...
   Disabling patch for /home/user/.wine/drive_c/users/user/Application Data/Tencent/WeChat ...
   mkdir: 无法创建目录 “/home/user/.wine/drive_c/users/user/Application Data/Tencent”: 权限不够
   
   ```

   修改$HOME目录下DoChat文件夹的owner：

   ```bash
   # /home/zhliao/Dochat
   # sudo chown -R $USER:$USER ./DoChat
   sudo chown -R $(whoami) $HOME/DoChat/
   ```

5. 报错提示：libGL error，X Error of failed request

   ```bash
   [DoChat] WeChat 3.3.0.115
   [DoChat] Starting...
   libGL error: No matching fbConfigs or visuals found
   libGL error: failed to load driver: swrast
   X Error of failed request:  GLXBadContext
     Major opcode of failed request:  152 (GLX)
     Minor opcode of failed request:  6 (X_GLXIsDirect)
     Serial number of failed request:  285
     Current serial number in output stream:  284
   [DoChat] WeChat.exe exit with code 0
   [DoChat] Found new version?
   [DoChat] WeChat.exe exited
   ```

   > 参考 https://github.com/huan/docker-wechat/issues/50

   ```bash
   # 安装nvidia-docker2
   $ sudo apt install nvidia-docker2
   # 重启docker
   $ sudo systemctl restart docker
   ```

   ```bash
   # 采用如下指令，增加了指令：--gpus=all --env="NVIDIA_DRIVER_CAPABILITIES=all"
   docker run \
     --name DoChat \
     --rm \
     -i \
     \
     -v "/home/zhliao/DoChat/WeChatFiles/":'/home/user/WeChat Files/' \
     -v "/home/zhliao/DoChat/ApplicationData":'/home/user/.wine/drive_c/users/user/Application Data/' \
     -v /tmp/.X11-unix:/tmp/.X11-unix \
     \
     -e DISPLAY \
     \
     -e XMODIFIERS=@im=ibus \
     -e GTK_IM_MODULE=ibus \
     -e QT_IM_MODULE=ibus \
     -e GID="$(id -g)" \
     -e UID="$(id -u)" \
     \
     --ipc=host \
     --privileged \
     \
     --gpus=all --env="NVIDIA_DRIVER_CAPABILITIES=all" \
     \
     zixia/wechat:3.3.0.115 bash
   ```

   

   



