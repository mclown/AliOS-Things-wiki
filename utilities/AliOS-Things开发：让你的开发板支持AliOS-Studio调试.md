### 目录

* [简介](#简介)
* [AliOS Studio调试机制](#alios-studio调试机制)
* [准备工作](#准备工作)
* [编写配置文件](#编写配置文件)
	* [debug配置文件说明](#debug配置文件说明)
	* [配置文件参数说明](#配置文件参数说明)
* [启动调试](#启动调试)
* [其他](#其他)
	* [下载JLink并配置环境变量](#下载jlink并配置环境变量)
* [参考文档](#参考文档)

### 简介

在**AliOS-Things 2.1版本**之后，AliOS Studio提供了一套简单易懂的接口可以让开发者很容易适配开发板支持调试功能。可以支持大部分的调试接口：

* ST-Link
* JLink
* CMSIS-DAP

AliOS Studio调试效果如下图所示：

![](https://img.alicdn.com/tfs/TB1aWkRLMHqK1RjSZFEXXcGMXXa-1140-820.gif)

### AliOS Studio调试机制

AliOS Studio主要的功能就是执行aos debug指令，aos debug然后再在后台运行gdb server，然后使用vscode-cpptools的调试能力（gdb client）来实现调试。AliOS Studio调试机制框架图如下图所示：

![undefined](https://img.alicdn.com/tfs/TB1UUDjirH1gK0jSZFwXXc7aXXa-457-540.png) 

本文将详细介绍一下如何让AliOS Studio支持你的开发板调试。

### 准备工作

[pca10040开发板](http://www.keil.com/boards2/nordicsemiconductors/nrf52pca10040/)是由Nordic出品的一款搭载[nRF52832](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF5283)的开发板，板载调试接口为jlink接口，可以通过jlink接口实现image下载，调试程序。
本示例使用pca10040开发板作为示例，实现在`AliOS Studio`中按`F5`即可开始调试pca10040的应用程序，支持windows、linux以及macOS。

#### 准备工作

* 参考[AliOS Things 环境配置](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Environment-Setup.zh)安装好AliOS Things的开发环境。
* 下载JLink执行程序并配置好环境变量，具体请参考[下载jlink并配置环境变量](#下载jlink并配置环境变量)。
* 更新aos-cube为最新版本：pip2 install -U aos-cube。

### 编写配置文件

`aos upload`会调用upload的配置文件来实现具体的image烧录过程，我们需要编写这个配置文件来达到启动gdb server的目的，`aos debug`目前已经支持的开发板可以参考[这里](https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons/debug)，同时，开发者也可以贡献自己适配好的json配置文件到AliOS-Things中。

#### debug配置文件说明

pca10040开发板的debug配置文件如下：

```json
{
    "cmd": [
        {
            "Linux64": "JLinkGDBServerCLExe", 
            "OSX": "JLinkGDBServerCLExe", 
            "Win32": "JLinkGDBServerCL.exe"
        }, 
        "-if", 
        "swd", 
        "-device", 
        "nRF52840_xxAA", 
        "-port", 
        "4242"
    ], 
    "port": 4242, 
    "prompt": "Please INSTALL Jlink Software Package, and ADD JLink to PATH environment, check: www.github.com/alibaba/AliOS-Things/wiki/debug", 
}
```

#### 配置文件参数说明

|参数|说明|默认|
|:---:|---|---|
|`cmd`|运行的指令，指令中支持针对不同PC系统运行不用的命令：`Linux64`，`OSX`，`Win32`，支持多个参数||
|`port`|gdb server监听的端口，,统一为4242|4242|
|`prompt`|当aos debug执行失败的时候，会显示`prompt`，用来提示用户出错原因||

### 启动调试

编写好配置文件之后，进入AliOS Studio，按`F5`即可启动调试。

### 其他

#### 下载JLink并配置环境变量

JLink软件包[下载地址](https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack)。请分别下载对应开发环境的软件包。

##### windows环境配置

windows环境下，需要把JLink的bin目录添加到Path环境变量，JLink 默认的bin目录为：`C:\Program Files (x86)\SEGGER\JLink_V640`，具体如何配置Path环境变量请参考：[How to add a folder to `Path` environment variable](https://stackoverflow.com/questions/44272416/how-to-add-a-folder-to-path-environment-variable-in-windows-10-with-screensho)。

> 请注意JLink默认的bin目录中的JLink_V640，会根据不同的jlink版本会有所不同。设置完Path环境变量需要重启cmd、bash、vscode等，最好重启电脑。

windows环境下的JLink Commander名称为：JLink.exe。

##### linux/macOS环境配置

按照正常的安装流程安装即可，linux/macOS环境下的JLink Commander名称为：JLinkExe。

### 参考文档
* [segger wiki](https://wiki.segger.com/Main_Page)
* [JLink.exe JLinkGDBServer.exe的参考说明](https://www.segger.com/downloads/jlink/UM08001_JLink.pdf)
* [J-Link_Commander使用说明](https://wiki.segger.com/J-Link_Commander)