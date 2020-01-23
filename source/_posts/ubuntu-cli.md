---
title: Ubuntu CLI
date: 2020-01-11 15:56:40
categories:
	- dev
tags:
	- ubuntu
	- shell
	- usage
---

## scp: 文件传输

### from local to server

```shell
scp -r {your folder or file} {username}@{ip address}:{folder in server}
```

### from server to local

```
scp -r {username}@{ip address}:{folder in server} {your folder or file}
```

<!-- more -->

## 网络连接

强制重启后可能导致没有网络连接，使用下列命令可恢复
``` shell
sudo service NetworkManager stop
sudo rm /var/lib/NetworkManager/NetworkManager.state
sudo service NetworkManager start
```

## zombie

杀死僵尸进程

```shell
ps aux | grep "Z"  # find zombie progress
kill -9 $(ps -A -ostat,ppid | grep -e '[zZ]'| awk '{ print $2 }')
```

## iconv: 格式转换

```shell
iconv -f UTF-8 -t GBK {from-file} -o {to-file}
```

## fdisk & parted: 挂载 2T+ 硬盘

```shell
sudo fdisk -l  # find the unmounted disk, say /dev/sdb or so
# WARNING: Carefully check which dir you should part!
sudo parted /dev/sdb  # enter parted
(parted) mklabel gpt
(parted) mkpart primary 1 100%
# Warning: The resulting partition is not properly aligned for best performance.
# Ignore/Cancel?
# if you meet this problem, change 1 to another value like 0 or so.
(parted) print  # check the result of patition
(parted) quit  # quit
sudo mkfs.ext4 -F /dev/sdb
# auto mount the disk
sudo blkid  # copy the UUID of mounted disk
sudo vim /etc/fstab # add a line of UUID of disk, and new disk is mounted to /disk
UUID=******** /disk ext4 defaults 0 0
reboot
# chmod a+rw /disk  make sure what you are doing before run this command!
```

## [Error: watch xxx ENOSPC](https://yq.aliyun.com/articles/600560)

```shell
echo fs.inotify.max_user_watches=666666 \
	| sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

**错误原因**
　　就是一个程序监控的文件数量超出了设定值，这行命令就是把设定的值改大一些

**解释一下这行命令的意思**
将 `echo fs.inotify.max_user_watches=666666` 的输出内容，
也就是＂fs.inotify.max_user_watches=666666＂通过｜传给后面的命令，然后tee将前面的传过来的内容输出到标准输出的同时，追加到文件file中。如果文件不存在，则创建；如果已经存在，就在末尾追加内容，而不是覆盖，最后是系统重新加载配置文件，使更改生效．
**大家可以到`/proc/sys/fs/inotify`下查看自己更改的值**

## Tmux

```shell
tmux at -t <name>  # re-enter a session
tmux new -s <name>  # create a session named <name>
tmux ls  # list all sessions
tmux kill-session -t <name>  # remove session named <name>

ctrl+b+d # leave the session

ctrl+b+$ # rename current session
ctrl+b+s # list sessions

ctrl+b+c # create window
ctrl+b+, # rename current window
ctrl+b+w # list windows
ctrl+b+& # close current window

ctrl+b+% # vertically split the window, get a new pane
ctrl+b+\" # horizontally split the window, get a new pane
ctrl+b+Arrow # shift from pane
ctrl+b+q # show the number of panes, press <number> to specified pane
ctrl+b+x # close the pane

ctrl+b+[ # view history of terminal
```

## Docker

### .dockerignore

```shell
.git
```

### Dockerfile

#### build

```shell
docker build -t <image>[:<tag>] . # default tag is latest
```

### stop container(s)

```shell
docker stop <container ID> # stop specified container
# stop container which match the regex
docker stop $(docker ps | awk '/regex/ print $1') 
```

### remove container(s)

```shell
docker rm <container ID> # remove specified container
docker rm $(docker ps -a -p) # remove all containers
```

### remove image(s)

```shell
docker rmi <image> # remove specified imgae
docker rm $(docker image ls) # remove all images
```

### create a container

```shell
# create a container and run backend
docker run -d -it --name="specified-name" <image> 
```

### enter a container

```shell
docker attach <container ID> # enter a running container
```

### exit from a container

```
ctrl+p, ctrl+q
```

## crontab

```shell
service cron start // 启动服务
service cron stop // 关闭服务
service cron restart // 重启服务
service cron reload // 重新载入配置
```

### 定时任务

```shell
0 5 * * * command args...
```

### 开机启动

```shell scrpt
@reboot command args...
```

## -no-pie: x-executale

在编译成可执行文件时在最后添加 `-no-pie` 参数。

## 清理磁盘

### 清理日志

```shell
journalctl --vacuum-size=10M
```

## dpkg

```shell
sudo dpkg -i <*.deb>  # install
sudo apt -f install  # fix the dependency
```

### dpkg: 处理软件包 xxxxxx (-- configure)时出错

```shell
sudo mv /var/lib/dpkg/info /var/lib/dpkg/info_old # 现将info文件夹更名
sudo mkdir /var/lib/dpkg/info # 再新建一个新的info文件夹
sudo apt-get update && sudo apt-get -f install # 更新源列表，更新软件
sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info_old # 执行完上一步操作后会在新的info文件夹下生成一些文件，现将这些文件全部移到info_old文件夹下
sudo rm -rf /var/lib/dpkg/info # 把自己新建的info文件夹删掉
sudo mv /var/lib/dpkg/info_old /var/lib/dpkg/info # 把以前的info文件夹重新改
```

## PID

### 查看端口号对应的 PID

```shell
lsof -i:8888
```

### 杀死 PID 对应的程序

```shell
kill -9 PID
```

## 查看文件（夹）大小

```shell
ls -lh filename  # 文件
du -sh  # 当前文件夹大小
du -h --max-depth=1  # 当前文件夹及子目录大小
```

## 查看进程

```shell
ps aux | grep "some message"
```

## grep

- -P 使用正则表达式
- -o 只输出匹配到的结果

### 前瞻、后顾

```shell
echo "Here is a string, and Here is another" | grep -o -P "(?<=Here).*(?=string)" 
# is a string, and Here is another #greedy
echo "Here is a string, and Here is another" | grep -o -P "(?<=Here).*?(?=string)" 
# is a 
# is another
```

## awk

### 处理列数据

第一列是字符串，第二列是数字，以下代码找出第二列的值 `>= 10^20` 的列

```shell
cat columns.csv | awk -F "\t" '{if ($2 >= 10^20) printf("%s\t%f\n", $1, $2)}'
```

### 交换列数据

以下代码交换第一列和第二列的顺序

```shell
cat columns.csv | awk -F "," '{printf("%f\t%s\n", $2, $1)}'
```

## Zsh & Oh my zsh

```shell
sudo apt install -y git
sudo apt install  -y zsh
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
chsh -s /usr/bin/zsh
reboot
```

### fix .zsh_history

```shell
# Fixes a corrupt .zsh_history file

mv ~/.zsh_history ~/.zsh_history_bad
strings ~/.zsh_history_bad > ~/.zsh_history
fc -R ~/.zsh_history
rm ~/.zsh_history_bad
```

## Typora

```shell
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

# add Typora's repository
sudo add-apt-repository 'deb http://typora.io linux/'
sudo apt-get update

# install typora
sudo apt-get install typora
```

## Markdown

### 分页

```html
<div STYLE="page-break-after: always;"></div>
```

## Fcitx wubi

```shell
sudo apt-get install fcitx-wubi
```

## Ubuntu:18 Server

### 配置网络

```shell
sudo vim /etc/netplan/***.yaml
```

### 重启服务

```shell
service systemd-resolved start
```

### 加入开机启动

```shell
systemctl enable systemd-resolved
```

## Error

### E: 无法获得锁 /var/lib/apt/lists/lock - open (11: 资源暂时不可用)

```shell
sudo rm /var/lib/apt/lists/lock
```

## 安装新字体

```shell
wget https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete%20Mono.otf
sudo mkdir -p /usr/share/fonts/custom
sudo mv Monaco.ttf /usr/share/fonts/custom
sudo chmod 744 /usr/share/fonts/custom/Monaco.ttf

sudo mkfontscale  #生成核心字体信息
sudo mkfontdir
sudo fc-cache -fv
```

