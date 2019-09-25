[EN](Quick-Start) | 中文

# 快速开始

本文通过直接在一台 linux 机器上运行 AliOS Things 来带您快速体验它。  
如果您在 Windows 或 Mac 上工作，也可以使用我们提供的[IDE](AliOS-Things-Studio)或使用[docker环境](AliOS-Things-Docker-Environment-Setup)进行开发。

## 配置环境

请直接前往[环境配置](AliOS-Things-Environment-Setup#1-system-environment-setup)选择适合自己开发方式的环境安装。

您也可以尝试一键安装脚本[Setup Script for Linux/Mac](https://alios-things-public.oss-cn-hangzhou.aliyuncs.com/setup_linux_osx.sh),
或者按以下命令手动安装依赖的软件包
例：在一台 Ubuntu 16.04 LTS (Xenial Xerus) 64-bit PC 上
```bash
sudo apt-get install -y python
sudo apt-get install -y gcc-multilib
sudo apt-get install -y libssl-dev libssl-dev:i386
sudo apt-get install -y libncurses5-dev libncurses5-dev:i386
sudo apt-get install -y libreadline-dev libreadline-dev:i386
sudo apt-get install -y python-pip
sudo apt-get install -y minicom
```
## 安装 aos-cube
首先, 用 python 包管理器 `pip` 来安装 `aos-cube` 和`相关的依赖包`在全局环境，以便于后续使用 AliOS Things Studio 进行开发。
```bash
$ pip install setuptools
$ pip install wheel
$ pip install aos-cube
```
> 请确认`pip`环境是基于 Python 2.7 的。如果遇到权限问题，可能需要 `sudo` 来执行。

## 下载代码

```bash
git clone https://github.com/alibaba/AliOS-Things.git -b rel_xxx (xxx指发布版本号，如3.0.0、2.1.0等)
```

从AliOS Things Release 2.1开始，可以使用[Component Tool](https://aliosthings.iot.aliyun.com/aos/download) 获取最小代码集到本地，便于快速开发

## 编译运行

AliOS Things 2.1及其后续版本(需要aos-cube 0.3.x)，更多配置参见[这里](AliOS-Things-Build-Configuration.zh)
```bash
cd AliOS-Things
aos make helloworld@linuxhost -c config && aos make
./out/helloworld@linuxhost/binary/helloworld@linuxhost.elf
```

AliOS Things 2.0及之前的版本：
```bash
cd AliOS-Things
aos make helloworld@linuxhost
./out/helloworld@linuxhost/binary/helloworld@linuxhost.elf
```

## 效果

可以看见 `app_delayed_action` 在1秒时启动，每5秒触发一次。
```bash
$ ./out/helloworld@linuxhost/binary/helloworld@linuxhost.elf
 [   1.000]<V> AOS [app_delayed_action#9] : app_delayed_action:9 app
 [   6.000]<V> AOS [app_delayed_action#9] : app_delayed_action:9 app
 [  11.000]<V> AOS [app_delayed_action#9] : app_delayed_action:9 app
 [  16.000]<V> AOS [app_delayed_action#9] : app_delayed_action:9 app
 ```
