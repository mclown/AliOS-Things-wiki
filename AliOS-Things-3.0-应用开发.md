### 目录

* [应用开发框架介绍](#应用开发框架介绍)
* [使用条件](#使用条件)
* [快速开始](#快速开始)
    * [第一步：下载AliOS Things 3.0源码](#第一步下载alios-things-30源码)
    * [第二步：添加AOS_SDK_PATH环境变量](#第二步添加aos_sdk_path环境变量)
    * [第三步：AliOS Studio中创建应用工程](#第三步alios-studio中创建应用工程)
* [编译、烧录、调试](#编译烧录调试)
* [其他说明](#其他说明)
* [参考文档](#参考文档)


### 应用开发框架介绍

应用开发框架介绍。

### 使用条件

* `AliOS Things >= 3.0`。
* `aos-cube >= 0.3.7`。

### 快速开始
#### 第一步：下载AliOS Things 3.0源码

* 到开源地址：https://github.com/alibaba/AliOS-Things 下载AliOS Things完整源码。
* 也可以到：https://aliosthings.iot.aliyun.com 定制你的AliOS Things源码。

#### 第二步：添加AOS_SDK_PATH环境变量

添加`AOS_SDK_PATH`系统环境变量，指向AliOS Things 3.0源码路径，`aos-cube`会根据`AOS_SDK_PATH`环境变量来定位AliOS Things源码。不同系统添加环境变量的方式不同，详细添加方法见[如何添加AOS_SDK_PATH环境变量](#如何添加AOS_SDK_PATH环境变量)。


#### 第三步：AliOS Studio中创建应用工程

创建示例：

![](https://img.alicdn.com/tfs/TB18vKRhG67gK0jSZFHXXa9jVXa-1205-707.gif)

### 编译、烧录、调试

应用工程中，AliOS Studio也支持编译、烧录、调试等功能。AliOS Studio的详细使用文档请参考[AliOS Studio](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Studio)。

### 其他说明

##### windows上添加AOS_SDK_PATH环境变量

详细方法请参考[windows系统如何设置添加环境变量？](https://jingyan.baidu.com/article/47a29f24610740c0142399ea.html)。

**查看环境变量是否生效：**
重启终端，输入以下命令，返回AliOS Things源码路径就说明设置成功：
* PowerShell中运行：`$env:AOS_SDK_PATH`。
* CMD中运行：`echo %AOS_SDK_PATH%`。
* git bash中运行：`echo %AOS_SDK_PATH`。

##### ubuntu上添加AOS_SDK_PATH环境变量

详细方法请参考[ubuntu－设置系统环境变量](https://www.jianshu.com/p/12fbfa8c7489)。
**查看环境变量是否生效：**
重启终端，输入`echo $AOS_SDK_PATH`，返回AliOS Things源码路径就说明设置成功。

##### macOS上添加AOS_SDK_PATH环境变量

详细方法请参考[Mac 中环境变量的配置和理解](https://blog.csdn.net/u010416101/article/details/54618621)。

**查看环境变量是否生效：**
重启终端，输入`echo $AOS_SDK_PATH`，返回AliOS Things源码路径就说明设置成功。

### 参考文档

* [AliOS Things环境搭建](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Environment-Setup)
* [AliOS Things构建系统](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-build-system.zh)
* [看见源码：立刻体验AliOS Things 3.0 ！](https://developer.aliyun.com/article/719492)
