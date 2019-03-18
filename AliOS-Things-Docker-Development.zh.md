# 使用 Docker 开发

本文推荐docker下进行日常开发和编译。
Docker基本命令参考：[这里](https://yq.aliyun.com/articles/514781?spm=a2c4e.11153940.blogcont66163.15.7dbb6131aN6rf4) 

## Linux环境开发
适用于Linux环境开发者，有专门代码服务器或虚拟机

#### 安装docker

```plain
$ sudo apt-get install docker-ce 
```
#### 获取docker镜像

```plain
$ docker pull registry.cn-hangzhou.aliyuncs.com/alios_things/rtos:latest   
```
#### 启动docker

为其命名alios-docker: 
```plain
$ docker run -it --privileged --name alios-docker registry.cn-hangzhou.aliyuncs.com/alios_things/rtos /bin/bash
```
__注意：__
1）退出docker后，可用以下命令重新启动已有docker：
```plain
$ docker container start -ia alios-docker
```
2）镜像更新前，需要备份有用的代码，重新获取新镜像并启动

## Mac环境开发

适用于无专门代码服务器或虚拟机，习惯mac本机开发

### 适用场景一：有linux经验，代码编辑与编译均可在docker环境下进行
#### 下载mac环境下的docker工具包并安装
打开[https://docs.docker.com/toolbox/overview/#whats-in-the-box](https://docs.docker.com/toolbox/overview/#whats-in-the-box)

![](https://img.alicdn.com/tfs/TB1EEwyLSzqK1RjSZFpXXakSXXa-781-253.png)

安装后打开Toolbox的DOCKER CLI， 实际上进入mac 的Terminal, 再继续下面的操作
#### 获取docker镜像
```plain
$ docker pull registry.cn-hangzhou.aliyuncs.com/alios_things/rtos:latest
```
#### 启动docker
为其命名alios-docker：
```plain
$ docker run -it --privileged --name alios-docker registry.cn-hangzhou.aliyuncs.com/alios_things/rtos bin/bash
```
#### 获取代码
1) [组件化工具](https://aliosthings.iot.aliyun.com/aos/download)获取：按需选择适当组件获取需要的代码
     本地zip文件，或 wget http链接(根据所选组件生成源码下载的链接) 获取
2) github获取：将获取全量代码，不推荐
      git clone命令

__注意：__
1) 退出docker后，可用以下命令重新启动已有docker：
```plain
$ docker container start -ia alios-docker
```
2) 需要烧机时，配置docker USB设备，参考Windows下的[USB设备配置](#配置docker_usb设备)，启动docker命令中无需-v参数

### 适用场景二：代码编辑和调试在mac，编译和烧录在docker下进行
与mac场景一比较，需要建立共享目录，完成代码在两种环境中的共享。
#### 下载docker工具
同上
#### 获取docker镜像
同上
#### 获取代码
推荐[组件化工具](https://aliosthings.iot.aliyun.com/aos/download)获取，将zip包解压到本机某个目录下， 如 /Users/xxx/alios
#### 启动docker
为其命名alios-docker，并指定本机与docker的目录映射关系：
使用-v 参数  -v <本机代码所在目录名>:<docker中映射名>
```plain
$ docker run -it --privileged --name alios-docker -v /Users/xxx/alios:/workspace registry.cn-hangzhou.aliyuncs.com/alios_things/rtos bin/bash
```
至此，可以达到对/Users/xxx/alios中的代码进行本地编辑和调试，而编译时， 转入docker中的/workspace下，执行 aos make <app>@<board>

__注意：__
需要烧机时，配置docker USB设备，参考Windows下的[USB设备配置](#配置docker_usb设备)， 启动docker命令中-v 参数稍有差别

## Windows环境开发
适用于无专门代码服务器或虚拟机， 习惯Windows本机开发
### 适用场景一：有linux经验，可工作于linux虚拟机
#### 下载Windows环境下的docker工具包并安装
打开[https://docs.docker.com/toolbox/overview/#whats-in-the-box](https://docs.docker.com/toolbox/overview/#whats-in-the-box)下载windows下的docker工具


![](https://img.alicdn.com/tfs/TB1PFQILIfpK1RjSZFOXXa6nFXa-1111-329.png)

Toolbox将默认安装VirtualBox，之后打开Docker Quickstart Terminal进行下面的操作
#### 获取docker 镜像
```plain
$ docker pull registry.cn-hangzhou.aliyuncs.com/alios_things/rtos:latest
```
#### 启动docker
```plain
$ docker run -it --privileged --name alios-docker registry.cn-hangzhou.aliyuncs.com/alios_things/rtos bash
```
#### 获取代码
1) [组件化工具](https://aliosthings.iot.aliyun.com/aos/download)获取：按需选择适当组件获取需要的代码
    本地zip文件，或 wget http链接(根据所选组件生成源码下载的链接) 获取
2) github获取：将获取全量代码，不推荐
     git clone命令

__注意：__
1）退出docker后，可用以下命令重新启动已有docker：
```plain
$ docker container start -ia alios-docker
```
 2）需要烧录设备时， 参考Windows场景二中的[USB设备配置](#配置docker_usb设备)，docker启动时，无需-v参数

### 适用场景二：代码编辑和调试在windows，编译和烧录在docker下进行
与场景一比较，需要建立共享目录，完成代码在两种环境中的共享。
#### 下载docker工具
同上
#### 获取docker镜像
同上
#### 获取代码
推荐[组件化工具](https://aliosthings.iot.aliyun.com/aos/download)获取，将zip包解压到本机某个目录下， 如d:\work
#### 启动docker, 带目录共享能力
__方式一：创建共享目录的docker__
 执行如下脚本，按要求copy命令并执行后即可
[set_share_folder.zip](http://alios-things-public.oss-cn-hangzhou.aliyuncs.com/set_share_folder.zip)

例如：需要做 d:\work与 docker中的/workspace的目录共享，执行脚本后，命令如下：
1）脚本执行后，自动停在虚拟机终端：
```plain
$ sudo mkdir –parents /d/work
$ sudo mount –tvboxsf d/work /d/work/
$ exit
```
2）退出后回到windows命令行，启动AliOS docker：
```plain
$ docker run -it --privileged --name alios-docker -v /d/work:/workspace/ registry.cn-hangzhou.aliyuncs.com/alios_things/rtos bash
```
这样就启动了一个AliOS Things的docker, 之后可以在docker环境里进行编译开发.

__注意：__Windows下取消共享目录命令：
```plain
C:/Program Files/Oracle/VirtualBox/VBoxManage.exe sharedfolder remove default --name d/work
```

__方式二：利用samba服务的docker__

1）__创建存储volume__, 便于与docker下的目录共享：
```plain
$ docker volume create --name aos-vol
```

2）__启动docker__：为其命名alios-docker，并-v指定volume目录映射到docker中的/workspace
```plain
$ docker run -it --privileged -p445:445 --name alios-docker -v aos-vol:/workspace/ registry.cn-hangzhou.aliyuncs.com/alios_things/rtos bash
```

3）__安装samba并配置,   __此时已进入docker环境：
```plain
$ apt install samba
$ vim /etc/samba/smb.conf   #在文件最后添加以下内容
[alios]
   path = /workspace
   public = yes
   case sensitive = yes
   map archive = no
   only guest = yes
   writable = yes
   force user = aosuser
   force group = aosuser
$ groupadd -g 1000 aosuser             #添加分组，与上面指定的分组名保持一致
$ useradd -m -u 1000 -g 1000 aosuser   #添加用户，与上面指定的用户名保持一致
```

4）启动samba daemon:
```plain
$ /usr/sbin/smbd
```

5）获取docker的虚拟IP： 回到Docker Quickstart Terminal里查看ip
```plain
$ docker-machine ip
192.168.99.100
```

6）配置网络连接：在windows下按win+R键，调出运行窗口，输入ip


![](https://img.alicdn.com/tfs/TB1HEwyLSzqK1RjSZFpXXakSXXa-519-268.png)

此时，在windows下可访问docker里的内容， 亦可随意添加内容到docker目录里：

![](https://img.alicdn.com/tfs/TB16CAJLNYaK1RjSZFnXXa80pXa-1016-229.png)


#### 配置docker_usb设备
注：如果无烧录需求， 可忽略此步骤

1）打开Docker Quickstart Terminal （Mac时，可直接使用mac terminal），停止缺省虚拟机：
```plain
$ docker-machine stop default
```
2）打开VirtualBox的管理界面，设置->usb设备->点选“启用usb控制器”->添加usb设备->确认


![](https://img.alicdn.com/tfs/TB1VowyLSzqK1RjSZFpXXakSXXa-1351-504.png)

3）重新运行缺省虚拟机：
```plain
$ docker-machine start default
```
或者打开一个新的Docker Quickstart Terminal
```plain
$ docker-machine ssh default
```

4）检测usb设备：连线后使用下面命令进行检测
```plain
$ dmesg | grep1 usb
........
[   19.942282] usb 1-2: cp210x converter now attached to ttyUSB0
```
 
5）欲使用usb设备的功能，启动docker时需添加新的启动参数--privileged，而-v参数根据目录共享需要添加：
```plain
$ docker run -it --privileged --name alios-things -v /d/work:/workspace/ registry.cn-hangzhou.aliyuncs.com/alios_things/rtos bash
```

6）编译代码，使用aos命令烧写: 比如烧写适配developerkit板子的helloworld应用的image：
```plain
$ aos upload helloworld@developerkit
.......
[INFO]: Firmware upload succeed!
```
