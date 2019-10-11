### 目录
* [介绍](#介绍)
* [安装](#安装)
	* [下载并安装 Visual Studio Code](#下载并安装-visual-studio-code)
	* [安装 AliOS Studio 插件](#安装-alios-studio-插件)
	* [安装 aos-cube](#安装-aos-cube)
* [使用](#使用)
	* [AliOS-Studio 工具栏](#alios-studio-工具栏)
	* [编译 - Build](#编译---build)
	* [烧录 - Upload](#烧录---upload)
	* [串口监控 - Monitor](#串口监控---monitor)
	* [调试 - Debug](#调试---debug)
* [更多说明](#更多说明)
	* [AliOS Studio 命令列表](#alios-studio-命令列表)
	* [AliOS Studio 快捷键](#alios-studio-快捷键)
* [配置文件说明](#配置文件说明)
	* [launch.json](#launchjson)
	* [settings.json](#settingsjson)
	* [tasks.json](#tasksjson)
* [其他功能](#其他功能)
	* [AliOS Things 3.0 应用开发](#alios-things-30-应用开发)
	* [鼠标移到AliOS Things的API上会显示API说明链接](#鼠标移到alios-things的api上会显示api说明链接)
	* [转换TSL json文件为C代码文件](#转换tsl-json文件为c代码文件)
* [附录](#附录)
	* [添加自定义task](#添加自定义task)
* [常见问题](#常见问题)

### 介绍

`AliOS Studio`是一套基于vscode的开发环境，支持**windows**、**linux**、**macOS**。`AliOS Studio`有以下功能：

- 极佳开发体验、简单操作界面
- 支持AliOS Things应用开发
- 代码补全、索引、提示等
- 编译/下载/调试 AliOS Things
- 适配多种开发板
- 串口工具、TSL转换工具等

### 安装

#### 下载并安装 Visual Studio Code

访问 [https://code.visualstudio.com/](https://code.visualstudio.com/) 下载并安装vscode。

#### 安装 AliOS Studio 插件

打开vscode，按照下图所示安装`AliOS Studio`插件：

![](https://img.alicdn.com/tfs/TB1l4kMnbvpK1RjSZPiXXbmwXXa-1231-848.jpg#width=)

#### 安装 aos-cube

`AliOS Studio` 依赖 `aos-cube`，如果想要手动安装 `aos-cube` 的话，请参考 [System environment setup](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Environment-Setup#1-system-environment-setup)，同时`AliOS Studio`也支持一键安装`aos-cube`，如下图所示：

> 使用AliOS Studio一键安装功能首先需要安装python2.7和pip。

![](https://img.alicdn.com/tfs/TB1udE8d_Zmx1VjSZFGXXax2XXa-1140-820.gif#width=)

> `AliOS Studio`一键安装的`aos-cube`是安装在虚拟python环境里面的([virualenv](https://virtualenv.pypa.io/en/latest/))，在vscode的终端里面能够正常使用`aos-cube`，其他终端无法正常使用`aos-cube`。


### 使用

#### AliOS-Studio 工具栏

`AliOS Studio`的主要功能都集中在vscode**下方工具栏**中，小图标从左至右功能分别是`创建应用工程` `编译` `烧录` `串口工具`  `清除`。
> **注：** 当用vscode打开了AliOS Things源码或者应用工程时，才会显示全部的工具图标。

<div align="center">
<img src="https://img.alicdn.com/tfs/TB1mePriEH1gK0jSZSyXXXtlpXa-998-76.jpg" width="480">
</div>

左侧的`helloworld@developerkit`是编译目标，格式遵循`应用名字@目标板名字`的规则，点击它可以依次选择应用和目标板。

#### 编译 - Build

点击`编译目标`选择应用和目标板，点击编译图标进行编译：

![](https://img.alicdn.com/tfs/TB1bUxrLwHqK1RjSZFgXXa7JXXa-1140-820.gif#width=)

#### 烧录 - Upload

1. 通过 USB Micro 线缆连接好开发板和电脑
2. 点击下方工具栏闪电图标完成固件烧录：

![](https://img.alicdn.com/tfs/TB1iq8rLr2pK1RjSZFsXXaNlXXa-1140-820.gif#width=)

[这里](https://github.com/alibaba/AliOS-Things/tree/rel_2.1.0/build/site_scons/upload)可以看到目前支持烧录（upload）的开发板，如果想要自己添加开发板支持，请参考：

- [https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons](https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons)

- [让你的开发板支持AliOS Studio烧录](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things%E5%BC%80%E5%8F%91%EF%BC%9A%E8%AE%A9%E4%BD%A0%E7%9A%84%E5%BC%80%E5%8F%91%E6%9D%BF%E6%94%AF%E6%8C%81AliOS-Studio%E7%83%A7%E5%BD%95)


#### 串口监控 - Monitor

1. 通过 USB Micro 线缆连接好开发板和电脑

2. 点击下方工具栏插头图标打开串口。第一次连接会提示填写串口设备名和波特率，再次点击可以看到串口输出，同时也可以在这里输入命令进行交互。

> 这里如果打开串口出错，请注意你的用户是否有串口访问权限。

#### 调试 - Debug

按`F5`或者点击菜单栏`Debug` > `Start Debugging`进入调试模式：

![](https://img.alicdn.com/tfs/TB1cNRSLxjaK1RjSZKzXXXVwXXa-1140-820.gif#width=)

[这里](https://github.com/alibaba/AliOS-Things/tree/rel_2.1.0/build/site_scons/debug)可以看到目前支持调试（debug）的开发板，如果想要自己添加开发板支持，请参考：

- [https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons](https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons)。

- [让你的开发板支持AliOS Studio烧录](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things%E5%BC%80%E5%8F%91%EF%BC%9A%E8%AE%A9%E4%BD%A0%E7%9A%84%E5%BC%80%E5%8F%91%E6%9D%BF%E6%94%AF%E6%8C%81AliOS-Studio%E8%B0%83%E8%AF%95)

参考视频：[使用 AliOS Studio 开始 AliOS Things 调试](http://v.youku.com/v_show/id_XMzU3OTE5ODE1Ng==.html)。

##### 设置优化等级

使用调试功能，最好设置优化等级为`-Og`或者`-O0`，否则会出现函数跳转异常、单步调试异常、变量optimize-out等问题。设置优化等级：

- AliOS Things 2.1版本以前：手动更改`build/aos_toolchain_arm-none-eabi.mk` 中的`COMPILER_SPECIFIC_OPTIMIZED_CFLAGS`变量为-Og 或者 -O0。
- AliOS Things 2.1版本及以后：使用命令`aos make BUILD_TYPE=debug`即可。你也可以参考[配置项：task.json](#tasksjson)中的说明，更改默认的Build选项。

### 更多说明

#### AliOS Studio 命令列表

按 `Ctrl-Shift-P` 打开vscode的命令面板，输入 `alios-studio`可以看到`AliOS Studio`支持的命令：

![](https://img.alicdn.com/tfs/TB1lEW_MZbpK1RjSZFyXXX_qFXa-1439-838.png)

命令说明：

| 命令 | 描述 | 工具栏 |
| --- | --- |:---:|
| `Build` | 编译： `aos make app@board` | ![](https://img.alicdn.com/tfs/TB14LGAkNnaK1RjSZFBXXcW7VXa-25-22.png#width=) |
| `Change build target` | 改变编译目标：app和board | ![](https://img.alicdn.com/tfs/TB15HqhkMHqK1RjSZFEXXcGMXXa-25-22.png#width=) |
| `Clean` | 清除: `aos make clean` | ![](https://img.alicdn.com/tfs/TB1_PN_kSrqK1RjSZK9XXXyypXa-25-22.png#width=) |
| `Config Serial Monitor` |  |  |
| `Connect device` | 打开串口: `aos monitor` | ![](https://img.alicdn.com/tfs/TB1oSSckHvpK1RjSZPiXXbmwXXa-25-22.png#width=) |
| `Install aos-cube` | 参考：[AliOS Studio一键安装aos-cube](#alios-studio%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85aos-cube) | - |
| `List Device` | 列出所有串口 | - |
| `Manage Account` | 管理阿里云账号 | - |
| `Probe Device` | - | - |
| `Technical Support` | 打开**钉钉** | - |
| `Upload` | 上传固件到开发板： `aos upload app@board` | ![](https://img.alicdn.com/tfs/TB1gwqakNTpK1RjSZR0XXbEwXXa-25-22.png#width=) |
| `OTA(Developerment Over-The-Air)` | 一键OTA功能 | - |
| Show welcome page | 显示welcome页面 | - |

#### AliOS Studio 快捷键

默认快捷键：

| 按键 | 执行命令 | 描述 |
| --- | --- | --- |
| `shift+alt+b` | `alios-studio.build` | 编译 |
| `shift+alt+c` | `alios-studio.clean` | 清除 |
| `shift+alt+u` | `alios-studio.upload` | 上传固件 |

也可以在`keybindings.json`中自定义自己喜欢的按键组合：

```json
[
    {
      "command": "alios-studio.build",
      "key": "shift+alt+b"
    },
    {
      "command": "alios-studio.clean",
      "key": "shift+alt+c"
    },
    {
      "command": "alios-studio.upload",
      "key": "shift+alt+u"
    }
]
```

### 配置文件说明

在AliOS Things源码或者应用工程中，都有`.vscode/`目录，该目录下面都有3个json文件，这些json文件分别配置不一样的功能：

- `launch.json` - 设置调试参数
- `settings.json` - AliOS Studio配置选项
- `tasks.json` - 设置tasks参数（包括编译、烧录、串口监控、清除等tasks）

> AliOS-Things 2.1版本以后，新增加了一个`.TAGS.AOS.DB` 文件，该文件是符号表数据库。

#### launch.json

AliOS Studio依赖[C/C++插件](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)提供的调试能力，使用`launch.json`来配置调试参数，launch.json的详细配置说明请参考：[vscode-cpptools/launch.md](https://github.com/Microsoft/vscode-cpptools/blob/master/launch.md)。

**每次更改编译目标(`app@board`)的时候，都会同步更新launch.json。**

launch.json 中的关键配置项如下如所示：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            ......
            "program": "${workspaceRoot}/out/helloworld@cy8ckit-149/binary/helloworld@cy8ckit-149.elf",
            "miDebuggerServerAddress": "localhost:4242",
            "setupCommands": [
                ......
                {
                    "text": "target remote localhost:4242"
                }
                ......
            ],
            "osx": {
                "miDebuggerPath": "arm-none-eabi-gdb"
            },
            "linux": {
                "miDebuggerPath": "arm-none-eabi-gdb"
            },
            "windows": {
                "miDebuggerPath": "arm-none-eabi-gdb.exe"
            }
        }
    ]
}
```

##### 配置项说明
| 名称 | 说明 |
| --- | --- |
| program | gcc编译出来的elf文件，位于`out/app@board/binary/app@board.elf` |
| miDebuggerServerAddress 和<br />setupCommands | 配置gdb的连接端口，不同的gdb server使用不同的端口 |
| miDebuggerPath | gdb执行文件路径 |

#### settings.json

一般情况下无需更改settings.json的内容，AliOS Studio会根据配置自动更新。

```json
{
    "aliosStudio.inner.yosBin": "aos",
    "aliosStudio.hardware.board": "developerkit",
    "aliosStudio.name": "helloworld",
    "aliosStudio.aosVersion": "2.1.0",
    "C_Cpp.default.browse.databaseFilename": "${workspaceRoot}/.vscode/.TAGS.AOS.DB"
}
```

> 该配置项为AliOS Things 2.1.0版本中的配置。

##### 配置项说明
| 配置项 | 说明 |
| --- | --- |
| `yosBin` | - |
| `hardware.board` | 编译的目标开发板 |
| `name` | 编译的目标应用 |
| `aosVersion` | AliOS Things 2.1.0版本及以后新增该配置选项，标志当前代码的版本号。 |
| `C_Cpp.default.browse.databaseFilename` | 配置符号表数据库保存路径 |

#### tasks.json

> vscode 的 tasks.json 官方说明请参考[https://code.visualstudio.com/Docs/editor/tasks](https://code.visualstudio.com/Docs/editor/tasks)。task的属性请参考：[https://code.visualstudio.com/Docs/editor/tasks#_custom-tasks](https://code.visualstudio.com/Docs/editor/tasks#_custom-tasks)。

tasks.json 用来描述当前支持哪些tasks，比如点击工具栏的编译按钮(![](https://img.alicdn.com/tfs/TB14LGAkNnaK1RjSZFBXXcW7VXa-25-22.png#width=))实际上就是执行`tasks.json`中的`alios-studio: Make`任务。

##### tasks.json的任务说明

| label | 说明 | 工具栏 |
| --- | --- |:---:|
| alios-studio: Make | 编译代码 | ![](https://img.alicdn.com/tfs/TB14LGAkNnaK1RjSZFBXXcW7VXa-25-22.png#width=) |
| alios-studio: Upload | 上传代码到开发板 | ![](https://img.alicdn.com/tfs/TB1gwqakNTpK1RjSZR0XXbEwXXa-25-22.png#width=) |
| alios-studio: Serial Monitor | 启动串口工具 | ![](https://img.alicdn.com/tfs/TB1oSSckHvpK1RjSZPiXXbmwXXa-25-22.png#width=) |
| alios-studio: Clean | 清除代码目标文件 | ![](https://img.alicdn.com/tfs/TB1_PN_kSrqK1RjSZK9XXXyypXa-25-22.png#width=) |
| alios-studio: OTA | 一键OTA功能 |  |

当然，你也可以在tasks.json中添加自己的任务，然后依次点击vscode菜单栏的Terminal > Run Task... ，即可看到你配置的导出IAR工程的task：<br />
![](https://img.alicdn.com/tfs/TB1wujGH6TpK1RjSZKPXXa3UpXa-718-288.png#width=)

更多的自定义task可以参考 [附录](#附录) > [添加自定义task](#添加自定义task)。

### 其他功能

#### AliOS Things 3.0 应用开发

AliOS Things 3.0版本于9月27日在云栖大会正式发布，在新版本中带来了全新的应用开发框架，帮助用户快速构建自己的应用。使用户可以更专注于自身应用的开发。开发者可以在AliOS Studio中快速的创建应用工程：

> 要求 AliOS Things >= 3.0.0 和 aos-cube >= 0.3.7。

![](https://img.alicdn.com/tfs/TB18vKRhG67gK0jSZFHXXa9jVXa-1205-707.gif)

#### 鼠标移到AliOS Things的API上会显示API说明链接

为了方便开发者尽快熟悉AliOS Things API，当鼠标移到AliOS Things的API上就会显示`查看AliOSThings 官方API文档`：

![](https://img.alicdn.com/tfs/TB1p3RKHVzqK1RjSZFCXXbbxVXa-917-350.png#width=)

#### 转换TSL json文件为C代码文件

[物的模型(TSL)](https://help.aliyun.com/document_detail/73727.html?spm=a2c4g.11186623.6.566.62742ab2c8pTxa) 是阿里云IOT平台很重要的一个概念，是一个数据模型，它是物理空间中的实体，如传感器、车载装置、楼宇、工厂等在云端的数字化表示。`AliOS Studio` 提供了一个高效的方法可以快速的把TSL json文件转换为C代码文件，右键json文件，然后选中`Convert TSL json to C string` 即可转换：

![](https://img.alicdn.com/tfs/TB17h0GHVYqK1RjSZLeXXbXppXa-1140-820.gif#width=)

### 附录

#### 添加自定义task

##### 添加task - 导出IAR/MDK工程：

```json
{
  "label": "alios-studio: Export IAR Project",
  "type": "shell",
  "command": "aos",
  "args": [
    "make",
    "IDE=iar"
  ],
  "presentation": {
    "focus": true
  }
}
```

##### 添加task - 多线程编译：

```json
{
  "label": "alios-studio: Parallel Build",
  "type": "shell",
  "command": "aos",
  "args": [
    "make",
    "JOBS=8"
  ],
  "presentation": {
    "focus": true
  }
}
```

##### 添加task - 编译debug类型固件：

该固件配合调试功能。

```json
{
  "label": "alios-studio: Build Debug",
  "type": "shell",
  "command": "aos",
  "args": [
    "make",
    "BUILD_TYPE=debug"
  ],
  "presentation": {
    "focus": true
  }
}
```


### 常见问题

##### Visual Studio Code is unable to watch for file changes in this large workspace

> 针对Linux系统，windows和mac不会出现这种情况。

该错误在 linux系统上比较常见，主要是因为linux系统最大可监听文件数有限制。linux系统默认系统可监听文件数为8192个，`AliOS-Things`的源码比较大，文件数远远大于8192个，此时vscode无法监听所有的文件改动，导致AliOS Studio 插件会工作不正常，报如下错误：

![](https://img.alicdn.com/tfs/TB1xxKIororBKNjSZFjXXc_SpXa-374-89.jpg)

**解决办法：**
此时需要设置**linux系统最大可监听文件数**。

使用如下命令查看当前可监听文件数：
```sh
cat /proc/sys/fs/inotify/max_user_watches
```
编辑文件：`/etc/sysctl.conf`，然后增加如下行：
```sh
fs.inotify.max_user_watches=524288
```
使用如下指令生效：
```sh
sudo sysctl -p
```

> Arch Linux 用户请参考此[链接](https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers).


 更多细节请参考：["Visual Studio Code is unable to watch for file changes in this large workspace" (error ENOSPC)](https://code.visualstudio.com/docs/setup/linux#_visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace-error-enospc)。

##### Workspace is too large to watch for file changes

和上面的问题一样：[Visual Studio Code is unable to watch for file changes in this large workspace](#visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace)

##### SyntaxError: .vscode\launch.json: Unexpected token / in JSON at position 4378

请不要在 `.vscode/tasks.json` 或者 `.vscode/launch.json`中添加注释。

##### 调试模式，提示gdb is not signed

![](https://img.alicdn.com/tfs/TB1pCaehKL2gK0jSZPhXXahvXXa-860-354.png)

试试换个toolchain，或者删除这个toolchain，让aos-cube自己下载toolchain。

