# DLP平台Docker镜像配置

#### 镜像导入

* 使用`docker load`才能正常使用环境变量中的conda路径

  * **`docker save`**：将一个**镜像**导出为文件
  * **`docker load`**：将上文件导入为一个镜像，会保存该镜像的的所有历史记录
  * `docker export`：将一个**容器**导出为文件
  * `docker import`：将容器导入成为一个新的镜像，会丢失所有元数据和历史记录

  > 两者的区别在于容器快照将会丢弃所有的历史记录和元数据信息，而镜像存储文件将保存完整记录，体积也会更大。此外从容器快照文件导入时，也可以重新指定标签等元数据。



#### 启动容器并配置

* 使用test_withgpu_image:v1文件load后，需要`docker run -it test_withgpu_image:v1 /bin/bash`启动容器

* 进入容器后，使用如下命令安装：

  ```bash
  apt-get update
  apt install libgl1-mesa-glx
  apt install libglib2.0-dev
  pip install numpy==1.24.3
  ```

* 激活conda环境：

  * 执行`conda init`，提示需要重启shell
  * 使用`exit`退出镜像，随后`docker ps -a`查看容器id
  * 使用`docker exec -it [container_id] bash`重新进入容器
    * 如果报错”Container xxxx is not running“，则需要执行`docker start [container_id]`



#### 保存容器

* 将上述操作过的容器保存为镜像：`docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`

  ```bash
  docker commit a402f test_image:latest
  ```



#### 远程jupyterlab服务器

* 启动镜像生成容器时需要使用`-p HostPort:ContainerPort`配置好端口映射：

  ```bash
  docker run -it -p 7777:22 -p 8888:8889 --privileged=true test_image:latest /bin/bash
  ```

  > 22给ssh连接（也可以不开），8889为下文中配置的容器里jupyterlab端口号，8888为本地主机的端口
  >
  > 远程电脑通过访问`本地主机ip:8888`就能访问容器中的jupyterlab（需要将容器中jupyterlab的端口号设置为8889）

* 安装jupyterlab和vim

  * `pip install jupyterlab`或者`conda install jupyterlab`

    > 使用pip install jupyterlab并完成下述步骤后，执行jupyterlab报错
    >
    > ```bash
    > Traceback (most recent call last):
    >   File "/opt/conda/lib/python3.8/runpy.py", line 194, in _run_module_as_main
    >     return _run_code(code, main_globals, None,
    >   File "/opt/conda/lib/python3.8/runpy.py", line 87, in _run_code
    >     exec(code, run_globals)
    >   File "/opt/conda/lib/python3.8/site-packages/ipykernel_launcher.py", line 15, in <module>
    >     from ipykernel import kernelapp as app
    >   File "/opt/conda/lib/python3.8/site-packages/ipykernel/kernelapp.py", line 43, in <module>
    >     from traitlets.utils import filefind
    > ImportError: cannot import name 'filefind' from 'traitlets.utils'
    > ```
    >
    > 解决办法：
    >
    > ```bash
    > pip3 install traitlets==5.1.1
    > pip3 install pygments==2.4.1
    > ```

  * `python`进入python环境，执行完后退出python环境

    ```python
    from jupyter_server.auth import passwd
    #部分博客执行#from notebook.auth import passwd
    #提示报错：ModuleNotFoundError: No module named ‘notebook‘
    #因为没有安装jupyter，而是安装的jupyterlab，需要执行#python -m pip install jupyter
    
    passwd()
    #提示输入密码，输入完后会生成一段加密的密码，需要复制一下字符串
    #'argon2:$argon2id$v=19$m=10240,t=10,p=8$W+FBAePTux9gQE9amb+CCA$EBVgf+dV6VVQmxRPzZcjV4MluHNBG0itoqoWrBBWuc0'
    ```

  * `apt-get install vim`安装vim

  * `jupyter lab --generate-config`生成配置文件

  * 编辑生成的文件`vim ~/.jupyter/jupyter_lab_config.py`

    ```bash
    # 将ip设置为*，意味允许任何IP访问
    c.ServerApp.ip = '*'
    # 这里的密码就是上边我们生成的那一串，粘贴一下
    c.NotebookApp.password = 'argon2:$argon2id$v=19$m=10240,t=10,p=8$s2p+nnqfOxjz3/MAy4cEFA$qONBqNzpkKeLcysVuN1l8Q'
    
    # 服务器上并没有浏览器可以供Jupyter打开
    c.ServerApp.open_browser = False
    # 监听端口设置为8888或其他自己喜欢的端口，因为上文将容器的8889映射到了本地主机的8888端口，因此这里设置成8889
    # 这样的话远程机器通过访问本地主机的ip:8888就可以访问容器里的jupyterlab
    c.ServerApp.port = 8889
    # 允许远程访问
    c.Server.allow_remote_access = True
    ```
  
  * 配置完成之后就可以启动jupyter lab

    ```bash
    jupyter lab --allow-root
    ```
  
  * 此时在电脑浏览器中输入`本地主机ip:8888`就可以访问jupyterlab，密码为passwd()时设置的密码



#### 已启动容器新增端口映射：待验证

1. 使用**docker ps -a**命令找到要修改容器的**CONTAINER ID**

2. 运行以下命令，进入该容器目录

   ```bash
   docker inspect【CONTAINER ID】| grep Id
   #或者docker ps -a
   cd /var/lib/docker/containers
   #这条指令可能会提示没有权限访问docker文件夹，可以先使用sudo -s，再执行cd指令
   ```

3. 停止容器，并停止主机docker服务

   ```bash
   docker stop [容器id]
   systemctl stop docker
   ```

4. 进入2得到的文件夹内，修改hostconfig.json 和 config.v2.json

   ```bash
   #修改hostconfig.json的PortBindings值，追加新的端口
   "PortBindings": {
        "22/tcp": [
            {
                "HostIp": "",
                "HostPort": "7777"
            }
        ],
        "8889/tcp": [
            {
                "HostIp": "",
                "HostPort": "8888"
            } 
        ]
    }
   ```

   ```bash
   #修改config.v2.json里Config->ExposedPorts和NetworkSettings->Ports
   "Config": {
       ....
       "ExposedPorts": {
           "22/tcp": {},
           "8889/tcp": {}
       },
       ....
   },
   
   "NetworkSettings": {
   	....
   	"Ports": {
        	"22/tcp": [
            	{
                "HostIp": "",
                "HostPort": "7777"
            	}
       	 ],
       	"8889/tcp": [
            	{
                "HostIp": "",
                "HostPort": "8888"
            	} 
        	]
       }
       ....
    }
   ```

   

#### jupyterlab报错

* 使用pip install jupyterlab并完成下述步骤后，执行jupyterlab报错

  ```bash
  Traceback (most recent call last):
    File "/opt/conda/lib/python3.8/runpy.py", line 194, in _run_module_as_main
      return _run_code(code, main_globals, None,
    File "/opt/conda/lib/python3.8/runpy.py", line 87, in _run_code
      exec(code, run_globals)
    File "/opt/conda/lib/python3.8/site-packages/ipykernel_launcher.py", line 15, in <module>
      from ipykernel import kernelapp as app
    File "/opt/conda/lib/python3.8/site-packages/ipykernel/kernelapp.py", line 43, in <module>
      from traitlets.utils import filefind
  ImportError: cannot import name 'filefind' from 'traitlets.utils'
  ```

  解决办法：

  ```bash
  pip3 install traitlets==5.1.1
  pip3 install pygments==2.4.1
  ```

* 执行jupyterlab报错：

  ```bash
  Traceback (most recent call last):
    File "/opt/conda/bin/jupyter-lab", line 6, in <module>
      from jupyterlab.labapp import main
    File "/opt/conda/lib/python3.8/site-packages/jupyterlab/labapp.py", line 13, in <module>
      from jupyter_server.serverapp import flags
    File "/opt/conda/lib/python3.8/site-packages/jupyter_server/serverapp.py", line 38, in <module>
      from jinja2 import Environment, FileSystemLoader
    File "/opt/conda/lib/python3.8/site-packages/jinja2/__init__.py", line 12, in <module>
      from .environment import Environment
    File "/opt/conda/lib/python3.8/site-packages/jinja2/environment.py", line 25, in <module>
      from .defaults import BLOCK_END_STRING
    File "/opt/conda/lib/python3.8/site-packages/jinja2/defaults.py", line 3, in <module>
      from .filters import FILTERS as DEFAULT_FILTERS  # noqa: F401
    File "/opt/conda/lib/python3.8/site-packages/jinja2/filters.py", line 13, in <module>
      from markupsafe import soft_unicode
  ImportError: cannot import name 'soft_unicode' from 'markupsafe' (/opt/conda/lib/python3.8/site-packages/markupsafe/__init__.py)
  ```
  
  解决办法：
  
  ```bash
  #https://stackoverflow.com/questions/72191560/
  #降低markupsafe版本
  pip install markupsafe==2.0.1
  ```
  
  
  
  