# Conda&pip Linux Installation&Configuration Guide

* Anaconda3
  * https://www.anaconda.com/download
  * https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/
* Miniconda3：https://docs.conda.io/projects/miniconda/en/latest/



### 手动安装

1. 进入上述网站，下载对应版本miniconda.sh
2. 执行`sudo bash miniconda.sh`
3. 输入"yes"之后选择安装路径，除了默认路径`$HOME/miniconda3/`以外，可以选择`/opt/miniconda3`
4. 是否需要init，选择"yes"



### 自动安装

执行官方快速安装指令

```bash
mkdir -p ~/miniconda3

wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh

bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3

rm -rf ~/miniconda3/miniconda.sh
```



### 安装后测试

1. 重新打开终端，如果报错没有conda指令：

   ```bash
   ~/miniconda3/bin/conda init bash
   ~/miniconda3/bin/conda init zsh
   ```

   > `~/miniconda3/`指代miniconda安装路径，如果安装在`/opt/miniconda`则为
   >
   > `/opt/miniconda3/bin/cinda init bash`



### conda换源

##### 暂时换源

如果我们不想修改配置，只是在下载某些包时使用国内的源，可以使用`--channel`参数指定

```bash
#使用清华源镜像下载numpy包
conda install --channel https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main numpy
```



##### 查看现在状态

默认状态下，查看现在的源`conda config --show`

> 可以使用`-h`查看参数说明，如`conda config -h`

```bash
$ conda config --show
  ...
  changeps1: True
  channel_alias: https://conda.anaconda.org
  channel_priority: flexible
  channel_settings: []
  channels:
    - defaults
  client_ssl_cert: None
  client_ssl_cert_key: None
  clobber: False
  conda_build: {}
  create_default_packages: []
  croot: /home/zhliao/conda-bld
  custom_channels:
    pkgs/main: https://repo.anaconda.com
    pkgs/r: https://repo.anaconda.com
    pkgs/pro: https://repo.anaconda.com
  custom_multichannels:
    defaults: 
      - https://repo.anaconda.com/pkgs/main
      - https://repo.anaconda.com/pkgs/r
    local: 
  debug: False
  default_channels:
    - https://repo.anaconda.com/pkgs/main
    - https://repo.anaconda.com/pkgs/r
  default_python: 3.11
  default_threads: None
  deps_modifier: not_set
  dev: False
  disallowed_packages: []
  download_only: False
  dry_run: False
  enable_private_envs: False
  env_prompt: ({default_env}) 
  # 对应conda虚拟环境的路径
  envs_dirs:
    - /home/zhliao/.conda/envs
    - /opt/miniconda3/envs
  ...
```

##### 通过.condarc文件配置

1. 默认状态下通过新建`.condarc`文件配置源

   ```bash
   # 换回默认源（清除所有用户添加的镜像源路径，只保留默认的路径）
   conda config --remove-key channels
   # 创建文件
   vim ~/.condarc
   ```

   > 这里其实删除了`channels`下的镜像源，但`default_channels`等其他源没有被删除

   在`.condarc`文件中加入如下内容（清华镜像源）：

   ```bash
   channels:
     - defaults
   show_channel_urls: true
   default_channels:
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
   custom_channels:
     conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
   ```

2. 修改成功后：

   * 使用`conda config --show channels`会发现只显示了上述内容的`channels`下的源
   * 使用`conda config --show default_channels`会发现可以显示`default_channels`下的源

   * 使用`conda config --show-sources`则会显示$HOME/.condarc文件中的配置

   * 使用`conda config --show`会显示完整字段

   ```bash
   # 只显示channels
   $ conda config --show channels
   channels:
     - defaults
   
   # 只显示default_channels 
   $ conda config --show default_channels
   default_channels:
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
   
   # 只显示.condarc文件内容
   $ conda config --show-sources
   ==> /home/zhliao/.condarc <==
   channels:
     - defaults
   custom_channels:
     conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
   default_channels:
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
   show_channel_urls: True
   
   # 完整配置
   $ conda config --show
   ...
   channels:
     - defaults
   client_ssl_cert: None
   client_ssl_cert_key: None
   clobber: False
   conda_build: {}
   create_default_packages: []
   croot: /home/zhliao/conda-bld
   custom_channels:
     anaconda/pkgs/main: https://mirrors.tuna.tsinghua.edu.cn
     anaconda/pkgs/free: https://mirrors.tuna.tsinghua.edu.cn
     anaconda/pkgs/r: https://mirrors.tuna.tsinghua.edu.cn
     conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
   custom_multichannels:
     defaults: 
       - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
       - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
       - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
     local: 
   debug: False
   default_channels:
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
   ...
   ```

##### 通过add增加源

可以直接通过`conda config --add <channel> <https://xxx>`指令将源添加到`<channel>`：

```bash
# 先删除原先配置
$ rm ~/.condarc

# 添加源
$ conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
$ conda config --add default_channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/

# 此时会发现前者加入到了channels中，后者在default_channels中
$ conda config --show-sources
==> /home/zhliao/.condarc <==
channels:
  - https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
  - defaults
default_channels:
  - https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
```

##### 源

```bash
# 中科大镜像源
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/
 
# 阿里镜像源
conda config --add channels https://mirrors.aliyun.com/pypi/simple/
 
# 豆瓣的python的源
conda config --add channels http://pypi.douban.com/simple/ 
```

### 其他指令

```bash
# 显示检索路径，每次安装包时会将包源路径显示出来
conda config --set show_channel_urls yes
conda config --set always_yes True

# 默认不自动激活base环境
conda config --set auto_activate_base false

#执行以下命令清除索引缓存，保证用的是镜像站提供的索引
conda clean -i
```



### pip换源

##### 临时换源

使用`-i`指令指定安装时所使用的源

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple package_name
```



##### 永久换源

使用`pip config set global.index-url https://xxxxx`

```bash
$ pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
Writing to /home/zhliao/.config/pip/pip.conf
```

> 这里可以看到配置文件的路径位于`$HOME/.config/pip/pip.conf`

国内源

```bash
# 国内源：
阿里云 http://mirrors.aliyun.com/pypi/simple/ 
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/ 
豆瓣(douban) http://pypi.douban.com/simple/ 
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/ 
中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```



##### 编辑配置文件

在`$HOME/.config/pip/pip.conf`文件中

```shell
# 阿里源
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
disable-pip-version-check = true          #取消pip版本检查，排除每次都报最新的pip，非必须
 
[install]
trusted-host=mirrors.aliyun.com

# 在[install]设置：install_lib = ~/usr/lib/pythonxxx/site-packages，可以指定pip安装包路径
```



##### conda与pip

在未激活任何环境时

```bash
$ which pip
/usr/bin/pip
```

在激活了conda环境时

```bash
$ which pip
/opt/miniconda3/bin/pip
```

