## 步骤

### 下载/安装 Visual Studio Code
https://code.visualstudio.com/

### 安装 AliOS Studio 插件

![](https://img.alicdn.com/tfs/TB1l4kMnbvpK1RjSZPiXXbmwXXa-1231-848.jpg)

> 如果已有安装，请确保 alios-studio 插件版本升级到 0.8.0 以上

### 开发环境准备

#### Linux/Mac中安装aos-cube

安装：
- [Python 2](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)

安装 python pip 包管理器，然后安装 `aos-cube` 到全局环境:
```
$ pip install --upgrade setuptools
$ pip install --upgrade wheel
$ pip install aos-cube
```
>如遇网络问题可参考[这里](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Linux-Environment-Setup#%E5%AE%89%E8%A3%85python%E5%92%8Caos-cube)

#### Windows中安装aos-cube
可以直接使用我们准备好的一键安装包：http://p28phe5s5.bkt.clouddn.com/setup_windows.zip  
也可以手动安装以下软件/工具:  
- [Python 2](https://www.python.org/downloads/)
- [Git (带 Git Bash)](https://git-scm.com/downloads)

Python 2 和 Git 安装完成以后，在 Git Bash 中安装 `aos-cube` 和`相关的packages`:
```
$ pip install --upgrade setuptools
$ pip install --upgrade wheel
$ pip install --user aos-cube
```

>细节可参考 [AliOS-Things-Windows-Environment-Setup](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Windows-Environment-Setup) 

**`[注意]`** 请确定下在运行完 `pip install aos-cube`后`esptool, pyserial, scons` 和 `aos-cube` 全部被成功安装, 如果失败需要自行一步一步的安装这些依赖包.

```bash
如果只是升级aos-cube，可以按下述步骤:

$ pip install --upgrade setuptools
$ pip install --upgrade wheel
$ pip install --upgrade aos-cube
```

#### AliOS Studio一键安装aos-cube

AliOS Studio也支持一键安装aos-cube，如下图所示：

> `AliOS Studio`一键安装的`aos-cube`是安装在虚拟python环境里面的([virualenv](https://virtualenv.pypa.io/en/latest/))，在vscode的终端里面能够正常使用`aos-cube`，其他终端无法正常使用`aos-cube`。

![](https://img.alicdn.com/tfs/TB1zVnemSzqK1RjSZFLXXcn2XXa-1141-820.gif)

### 下载 AliOS Things 代码

从GitHub克隆：
`git clone https://github.com/alibaba/AliOS-Things.git`  
或者从国内镜像站点：
`git clone https://gitee.com/alios-things/AliOS-Things.git`

### 开始上手

这里我们基于 Starter Kit 开发板来介绍 AliOS Studio 的使用，其他板子可以此类推。  
对应视频 [使用 AliOS Studio 开始 AliOS Things 开发](http://v.youku.com/v_show/id_XMzU3OTE2MzI1Ng==.html)

#### 编译 helloworld

1. 在 Visual Studio Code 中打开下载好的 AliOS Things 代码目录
![](https://img.alicdn.com/tfs/TB1cPDapwmTBuNjy1XbXXaMrVXa-2032-1170.png)
2. 所有功能都集中在下方工具栏中  
小图标从左至右功能分别是 `编译` `烧录` `串口工具` `创建工程` `清除`  
![](https://img.alicdn.com/tfs/TB1WPzapwmTBuNjy1XbXXaMrVXa-286-22.png)  
左侧的 `helloworld@starterkit` 是编译目标，格式遵循 `应用名字@目标板名字` 的规则，点击它可以依次选择应用和目标板。  
![](https://img.alicdn.com/tfs/TB1Rt2gpwmTBuNjy1XbXXaMrVXa-2032-1170.png)
3. 编译目标确定以后，点击 ![](https://img.alicdn.com/tfs/TB1qR_UpuSSBuNjy0FlXXbBpVXa-24-22.png) 开始编译。  
编译过程如果发现缺少 toolchain 则会自动下载解压到正确位置。

#### 烧录到目标板

1. 通过 USB Micro 线缆连接好 Starter Kit 与电脑
2. 点击下方工具栏闪电图标完成固件烧录
![](https://img.alicdn.com/tfs/TB1YOvEprGYBuNjy0FoXXciBFXa-2032-1170.png)

#### 串口工具

点击下方工具栏插头图标打开串口，连接目标板，第一次连接会提示填写串口设备名和波特率，再次点击  
可以看到 `app_delayed_action` 在1秒时启动，每5秒触发一次，也可以在这里输入命令进行交互。  
**这里如果打开串口出错，请注意你的用户是否有串口访问权限**
![](https://img.alicdn.com/tfs/TB1CfefpTtYBeNjy1XdXXXXyVXa-2032-1170.png)

### 调试
细节参考 [Starter-Kit-Tutorial#调试](Starter-Kit-Tutorial#调试)  
或视频 [使用 AliOS Studio 开始 AliOS Things 调试](http://v.youku.com/v_show/id_XMzU3OTE5ODE1Ng==.html)

### 其他

#### AliOS Studio 命令列表

按<kbd> Ctrl-Shift-P </kbd> 打开vscode的命令面板，输入 `alios-studio`可以看到`AliOS Studio`支持的命令：

![](https://img.alicdn.com/tfs/TB1idGckQvoK1RjSZFNXXcxMVXa-748-412.png)

命令说明：

|命令|描述|下方工具栏图标|
|:---|:---|:---:|
|`Build`|编译： `aos make app@board`|![](https://img.alicdn.com/tfs/TB14LGAkNnaK1RjSZFBXXcW7VXa-25-22.png)|
|`Change build target`|改变编译目标：app和board|![](https://img.alicdn.com/tfs/TB15HqhkMHqK1RjSZFEXXcGMXXa-25-22.png)|
|`Clean`|清除: `aos make clean`|![](https://img.alicdn.com/tfs/TB1_PN_kSrqK1RjSZK9XXXyypXa-25-22.png)|
|`Config Serial Monitor`||
|`Connect device` |打开串口: `aos monitor`|![](https://img.alicdn.com/tfs/TB1oSSckHvpK1RjSZPiXXbmwXXa-25-22.png)|
|`Install aos-cube`|参考：[AliOS Studio一键安装aos-cube](#alios-studio一键安装aos-cube)|-|
|`List Device`|列出所有串口|-|
|`Manage Account`|管理阿里云账号|-|
|`Probe Device`|-|-|
|`Technical Support`|打开**钉钉**|
|`Upload`|上传固件到开发板： `aos upload app@board`|![](https://img.alicdn.com/tfs/TB1gwqakNTpK1RjSZR0XXbEwXXa-25-22.png)|

#### AliOS Studio 快捷键

|按键|执行命令|描述|
|:-|:-|:-:|
|<kbd>shift+alt+b</kbd>|`alios-studio.build`|编译|
|<kbd>shift+alt+c</kbd>|`alios-studio.clean`|清除|
|<kbd>shift+alt+u</kbd>|`alios-studio.upload`|上传固件|

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