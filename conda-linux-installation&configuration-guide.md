# Conda Linux Installation&Configuration Guide

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



### 安装后

1. 重新打开终端，如果报错没有conda指令：

   ```bash
   ~/miniconda3/bin/conda init bash
   ~/miniconda3/bin/conda init zsh
   ```

   > `~/miniconda3/`指代miniconda安装路径，如果安装在`/opt/miniconda`则为
   >
   > `/opt/miniconda3/bin/cinda init bash`