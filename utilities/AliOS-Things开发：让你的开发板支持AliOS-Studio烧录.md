### 目录
* [简介](#简介)
* [准备工作](#准备工作)
* [编写upload命令配置文件](#编写upload命令配置文件)
* [最终效果](#最终效果)
* [其他](#其他)
	* [下载JLink并配置环境变量](#下载JLink并配置环境变量)
* [参考文档](#参考文档)

### 简介

[aos-cube](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-uCube.zh)是[AliOS-Things](https://github.com/alibaba/AliOS-Things)项目开发管理工具（简写命令为aos），具有以下功能：

- 编译代码、Image下载、板子调试。

- 创建模板工程，基于模板做再次开发。

- 支持组件化，获取组件信息，组件的自由组合，满足业务场景的不同需求。


> 如何安装aos-cube请参考[aos-cube安装](https://github.com/alibaba/AliOS-Things/wiki/Quick-Start#install-packages)。


在`AliOS-Things 2.x.x`版本之后，[aos-cube](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-uCube.zh)提供了一套简单易懂的接口可以让开发者很容易适配aos，达到`aos upload`指令下载image，`aos debug`指令启动调试功能。

本文简单介绍一下如何让`aos upload`指令支持你的开发板下载，从而在[AliOS-Studio](https://marketplace.visualstudio.com/items?itemName=alios.alios-studio)中点击upload按钮即可立即下载AliOS-Things编译好的binary，效果如下图所示：<br />
![](https://img.alicdn.com/tfs/TB1V58pu3HqK1RjSZFEXXcGMXXa-1081-792.gif)

> 本文所涉及到的代码在[这里](https://github.com/alibaba/AliOS-Things/tree/rel_3.0.0/build/site_scons)可以找到。

### 准备工作

[pca10040开发板](http://www.keil.com/boards2/nordicsemiconductors/nrf52pca10040/)是由Nordic出品的一款搭载[nRF52832](https://www.nordicsemi.com/Products/Low-power-short-range-wireless/nRF5283)的开发板，板载调试接口为jlink接口，可以通过jlink接口实现image下载，调试程序。

本示例使用pca10040开发板作为示例，实现通过`aos upload`指令调用jlink的程序下载image到pca10040上，**支持windows、linux以及macOS**。

**准备工作**：

> 本功能只适配AliOS-Things 2.x.x版本及以后版本，1.x.x版本目前不支持。


- 参考[AliOS Things 环境配置](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Environment-Setup.zh)安装好AliOS Things的开发环境。

- 下载JLink执行程序并配置好环境变量，具体请参考[下载jlink并配置环境变量](#jlink-config)。

- 更新aos-cube为最新版本：`pip2 install -U aos-cube`。

### 编写upload命令配置文件

`aos upload`会调用upload的配置文件来实现具体的image烧录过程，我们需要编写这个配置文件来达到烧录目的，`aos upload`目前已经支持的开发板可以参考[这里](https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons/upload)，同时，开发者也可以贡献自己适配好的json配置文件到AliOS-Things中。

`AliOS-Things`的源码里面提供了`build/site_scons/gen_upload_configs.py`脚本用来根据填写的内容自动生成json配置文件，具体说明请参考[这里](https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons/)，本示例是参考该规则进行pca10040开发板适配的。

**添加pca10040 upload指令**：
按照[aos指令适配](https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons/)的规则给`gen_upload_configs.py`添加如下内容：

```python
registry_board = {
    ......
    'pca10040': ['pca10040.json'],
    ......
}

pca10040 = {
'cmd': [
    'python',
    '@AOSROOT@/build/site_scons/jlink.py',
    '-d', 'nRF52840_xxAA',
    '-i', 'swd',
    '-f', '@AOSROOT@/out/@TARGET@/binary/@TARGET@.bin',
    '-p', '0x00010000'
],
}
flash_configs['pca10040'] = pca10040
```

> 注：registry_board中的`pca10040`和`flash_configs['pca10040']`里面的`pca10040`一定要是AliOS-Things/board/下面对应的board名称。


其中`jlink.py`脚本是一个专门为`aos upload`编写的python脚本，位于`build/site_scons`中，主要功能是在out目录下生成对应设备的jlink commands文件，并启动jlink下载，[jlink.py下载地址](https://github.com/alibaba/AliOS-Things/blob/rel_3.0.0/build/site_scons/jlink.py)。

**生成对应的json配置文件**：

运行`gen_upload_configs.py`可以自动生成json配置文件：

```bash
$ cd build/site_scons
$ python ./gen_upload_configs.py
```

可以看到在`build/site_scons/upload`目录下有生成`pca10040.json`文件，内容如下：

```json
{
    "cmd": [
        "python",
        "@AOSROOT@/build/site_scons/jlink.py",
        "-d",
        "nRF52840_xxAA",
        "-i",
        "swd",
        "-f",
        "@AOSROOT@/out/@TARGET@/binary/@TARGET@.bin",
        "-p",
        "0x00010000"
    ]
}
```

### 最终效果

```bash
aos upload helloworld@pca10040
```

或者点击AliOS Studio的upload按钮启动下载。

### 其他

#### 下载JLink并配置环境变量

JLink软件包[下载地址](https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack)。请分别下载对应开发环境的软件包。

**windows环境配置**：<br />windows环境下，需要把JLink的bin目录添加到Path环境变量，JLink 默认的bin目录为：`C:\Program Files (x86)\SEGGER\JLink_V640`，具体如何配置Path环境变量请参考：[How to add a folder to `Path` environment variable](https://stackoverflow.com/questions/44272416/how-to-add-a-folder-to-path-environment-variable-in-windows-10-with-screensho)。

> 请注意JLink默认的bin目录中的`JLink_V640`，会根据不同的jlink版本会有所不同。设置完Path环境变量需要重启cmd、bash、vscode等，最好重启电脑。


windows环境下的JLink Commander名称为：`JLink.exe`。

**linux/macOS环境配置**：

linux/macOS环境下的JLink Commander名称为：`JLinkExe`。

### 参考文档

- [segger wiki](https://wiki.segger.com/Main_Page)

- [JLink.exe JLinkGDBServer.exe的参考说明](https://www.segger.com/downloads/jlink/UM08001_JLink.pdf)

- [J-Link_Commander使用说明](https://wiki.segger.com/J-Link_Commander)


