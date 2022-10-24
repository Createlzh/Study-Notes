# Centos7 Xfce桌面安装+VNC远程访问

>  https://blog.csdn.net/sinat_40471574/article/details/104509302
>
>  https://blog.51cto.com/u_9691128/4580184
### 一、 安装Xfce

**1.  安装X Window图形用户接口：**

```
yum groupinstall "X Window system"
```

**2.  查看Xfce包名：**

```
yum grouplist
```

![image-20221024170139017](C:\Users\zhliao\Downloads\csdn_py_jb51\Centos7 Xfce桌面安装+VNC远程访问.assets\image-20221024170139017.png)

**如果这里没有Xfce，需要安装额外包yum源：**

```shell
yum install epel-release -y
```

安装结束后就有xfce了

**3.  安装Xfce：**

```
yum groupinstall Xfce
```

**4.  更新图形界面target：** 

```
sudo systemctl isolate graphical.target
```

**5.  中文设置：**

安装字体楷体

```
yum install cjkuni-ukai-fonts
```

查看当前语言

```
echo $LANG
```

如果不是中文则手动设置为中文

```
vi /etc/locale.conf
```

将内容改为

```
LANG=zh_CN.UTF-8
```

刷新字体缓存

```
fc-cache
```

**6.  设置默认启动方式： **

```
设置默认启动为图形化界面
systemctl set-default graphical.target


设置默认启动为文本模式
systemctl set-default multi-user.target 


```

完成后输入reboot重启



###  二、 VNC远程访问(ROOT用户)

**1.  安装VNC server：**

执行以下命令安装vnc-server：

```
yum install vnc-server -y
```

编辑文件

```
vim /etc/sysconfig/vncservers
```

在文件内添加以下内容，设置用户和分辨率：

```
VNCSERVERS=“1:root”
VNCSERVERARGS[1]="-geometry 1028x960"
```

输入vncpasswd设置密码

```
vncpasswd
```

 编辑文件

```
vim /root/.vnc/xstartup
```

替换内容为

```
#!/bin/sh
#Uncomment the following two lines for normal desktop:
unset SESSION_MANAGER
#exec /etc/X11/xinit/xinitrc
[ -x /etc/vnc/xstartup ] &amp;&amp; exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] &amp;&amp; xrdb KaTeX parse error: Expected 'EOF', got '&amp;' at position 59: …config -iconic &amp;̲ #xterm -geome…VNCDESKTOP Desktop"&amp;
#twm &amp;
startxfce4 &amp;
```

给文件添加可执行权限 

```
chmod a+x xstartup
```

 **2.  配置VNC服务：（使用root用户）**

编辑服务窗口文件

```
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
vi /etc/systemd/system/vncserver@:1.service
```

在第39行后添加User=root，否则会报错Job for vncserver@:1.service failed because the control process exited with error code. See "systemctl status vncserver@:1.service" and "journalctl -xe" for details.

将43，44行的&lt;USER&gt;替换为root。

![image-20221024170126126](C:\Users\zhliao\Downloads\csdn_py_jb51\Centos7 Xfce桌面安装+VNC远程访问.assets\image-20221024170126126.png)

**3.  启动vnc server： **

刷新服务、设置自启、启动服务

```
systemctl daemon-reload

systemctl enable vncserver@:1.service  #设置该1号窗口服务为开机自启

systemctl start vncserver@:1.service
```

启动vnc

```
vncserver
```

**检查端口连接情况**

```
netstat -lnpt
```

此时5901号端口应正常

<img src="C:\Users\zhliao\Downloads\csdn_py_jb51\Centos 7.5 轻量级桌面系统Xfce安装  + Windows 10 VNC远程访问.assets\20200229195346731.jpg" >

**4.   防火墙设置**

我们需要配置防火墙来让 VNC 服务正常工作：

```shell
firewall-cmd --permanent --add-service vnc-server

systemctl restart firewalld.service
```

**5.   连接服务器：**

下载安装vnc viewer

![image-20221024170115051](C:\Users\zhliao\Downloads\csdn_py_jb51\Centos7 Xfce桌面安装+VNC远程访问.assets\image-20221024170115051.png)

输入服务器地址和密码即可连接



### 三、配置第二个VNC（非ROOT用户sbeam）

1.修改文件

```shell
vim /etc/sysconfig/vncservers

VNCSERVERS=“1:root 2:sbeam”
VNCSERVERARGS[1]="-geometry 1028x960"
VNCSERVERARGS[2]="-geometry 1028x960"
```

**使用sbeam用户登录**，输入vncpasswd设置密码

```
vncpasswd
```

 编辑文件

```
vim /home/sbeam/.vnc/xstartup
```

替换内容为

```
#!/bin/sh
#Uncomment the following two lines for normal desktop:
unset SESSION_MANAGER
#exec /etc/X11/xinit/xinitrc
[ -x /etc/vnc/xstartup ] &amp;&amp; exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] &amp;&amp; xrdb KaTeX parse error: Expected 'EOF', got '&amp;' at position 59: …config -iconic &amp;̲ #xterm -geome…VNCDESKTOP Desktop"&amp;
#twm &amp;
startxfce4 &amp;
```

给文件添加可执行权限 

```
chmod a+x xstartup
```

 **2.  配置VNC服务：**

编辑服务窗口文件

```
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:2.service
vim /etc/systemd/system/vncserver@:2.service
```

在第39行后添加**Type=simple，User=sbeam**，否则会报错Job for vncserver@:2.service failed because the control process exited with error code. See "systemctl status vncserver@:2.service" and "journalctl -xe" for details.

将43，44行的&lt;USER&gt;替换为sbeam。

![image-20221024170126126](C:\Users\zhliao\Downloads\csdn_py_jb51\Centos7 Xfce桌面安装+VNC远程访问.assets\image-20221024170126126-1666606235977-4.png)

**3.  启动vnc server： **

可以先关闭之前的vncserver即1号窗口

```shell
vncserver -kill :1
```

刷新服务、设置自启、启动服务

```
systemctl daemon-reload

systemctl enable vncserver@:2.service  #设置该2号窗口服务为开机自启

systemctl start vncserver@:2.service
```

启动vnc

```
vncserver //启动所有桌面
vncserver :2 //启动2桌面
```

**检查端口连接情况**

```
netstat -lnpt
```

此时5902号端口应正常

<img src="C:\Users\zhliao\Downloads\csdn_py_jb51\Centos 7.5 轻量级桌面系统Xfce安装  + Windows 10 VNC远程访问.assets\20200229195346731.jpg" >

 **4.  防火墙已经配置**



### 四、VNC配置说明

https://wenku.baidu.com/view/90ea55584bd7c1c708a1284ac850ad02de80079c.html



### 五、 其它配置

**1.  如需加密vnc连接可参考：**https://blog.csdn.net/qq_39052513/article/details/100272502

![image-20221024170101036](C:\Users\zhliao\Downloads\csdn_py_jb51\Centos7 Xfce桌面安装+VNC远程访问.assets\image-20221024170101036.png)

**2.   Xfce插件安装：**https://www.jianshu.com/p/95c24f4d96e3

**3.  Xfce桌面美化：**https://blog.csdn.net/qq_43901693/article/details/103102528

PS：美化资源下载网站需要科学上网访问
