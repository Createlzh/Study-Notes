# nav_follow proj3/4板端运行

1. `roslaunch cam_sync.launch` // 打开深度相机

2. 新建终端 `cd nav_follow/nav_follow_3_npu`

3. 注意npu版本需要先`sudo modprobe rknpu`

4. `catkin_make`

  > 如果报错提示 CMake Error Unable to find either executable 'empy' or Python module 'em'
  >
  > 安装sudo apt install python3-empy
  >
  > 如果仍然提示报错，`catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3`

4. `source devel/setup/sh` // 添加到bash环境

5. `rosrun aikit_cv aikit_cv_node`  // 可用tab补全



