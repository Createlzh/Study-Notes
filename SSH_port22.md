# SSH connection reset by IP port 22

> 参考[博客](https://segmentfault.com/a/1190000009567576)

## 问题

* 使用`git clone`时提示`Connection reset by 192.30.255.112 port 22`

```shell
$ git clone git@github.com:Createlzh/OperatingSystem-Notes.git
Cloning into 'OperatingSystem-Notes'...
Connection reset by 192.30.255.112 port 22
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

* 随后使用`ssh -T git@github.com`尝试连接，失败

```shell
$ ssh -T git@github.com
ssh: connect to host github.com port 22: Connection timed out
```



## 解决方法

* 可能是端口被占用了
* 在`./ssh`下找到`config`文件，如果没有就创建一个，复制如下代码

```shell
Host github.com
User git
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```

* 使用`ssh -T git@github.com`尝试连接，连接成功

```shell
$ ssh -T git@github.com
The authenticity of host '[ssh.github.com]:443 ([192.30.255.122]:443)' can't be established.
RSA key fingerprint is SHA256:~~~~.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[ssh.github.com]:443,[192.30.255.122]:443' (RSA) to the list of known hosts.
Hi Createlzh! You've successfully authenticated, but GitHub does not provide shell access.
```