# AliOS Studio

EN | [中文](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Studio)

`AliOS Studio` is a AliOS Things development environment based on vscode, and is available for **Windows**, **macOS** and **Linux**. `AliOS Studio` supports:

* Excellent development experience, simple UI.
* Language support for C includes Auto-completion, Symbol searching, Go to/Peek Definition/Declaration, etc.
* Compile, upload and debug with AliOS Things.
* Support many embedded boards.
* uDevice Center, Serial tool, TSL Convert tool etc.

## Install

### Install Visual Studio Code

Download and Install [vscode](https://code.visualstudio.com/).

### Install AliOS Studio

Open vscode, Install `AliOS Studio` extension:

![image | left](https://img.alicdn.com/tfs/TB1_CUrMHvpK1RjSZFqXXcXUVXa-1142-821.jpg "")

### Install aos-cube

`AliOS Studio` depends on `aos-cube`, check [aos-cube](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-uCube) to Install. `AliOS Studio` also supports install `aos-cube`, show like this:

> NEED to install python2 and pip first !, check [System envirmonent setup](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Environment-Setup#1-system-environment-setup).

![image | left](https://img.alicdn.com/tfs/TB1udE8d_Zmx1VjSZFGXXax2XXa-1140-820.gif "")

> Install `aos-cube` By `AliOS Studio`, `aos-cube` will be installed in virtual python envirmonent([virualenv](https://virtualenv.pypa.io/en/latest/)), , `aos-cube` works well in vscode terminal, but not working in other terminal like `cmd` or `powershell`.

## Quick Start

### AliOS Studio statusbar

The main features of `AliOS Studio` displays in vscode statusbar on the bottom. the icons is `Build`, `Flash`, `serial tool`, `Clean` from left to right.

![image | left](https://img.alicdn.com/tfs/TB1QmxlLrvpK1RjSZPiXXbmwXXa-452-37.jpg "")

### Build

`helloworld@developerkit` is the build target in statusbar, which `helloworld` is the app name, `developerkit` is the board name. You can choose **app** and **board** by click it and build target like this:

![image | left](https://img.alicdn.com/tfs/TB1bUxrLwHqK1RjSZFgXXa7JXXa-1140-820.gif "")

### Flash

1. Connect board and PC by USB Micro cable.
2. click **Flash** icon to Flash binary to board.

> If not set the serial port number and the baudrate, click **Flash** icon will show the quickpick to pick serial port and input baudrate.

![image | left](https://img.alicdn.com/tfs/TB1iq8rLr2pK1RjSZFsXXaNlXXa-1140-820.gif "")

[Here](https://github.com/alibaba/AliOS-Things/tree/rel_2.1.0/build/site_scons/upload) you can see `AliOS Studio` board list that supports **Flash**. If you want your board supports `AliOS Studio` Flash, refer to:

* [https://github.com/alibaba/AliOS-Things/tree/master/build/site\_scons](https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons)

### Debug

In vscode, Press `F5` or click `Debug` > `Start Debugging` will enter in debug mode:

![image | left](https://img.alicdn.com/tfs/TB1cNRSLxjaK1RjSZKzXXXVwXXa-1140-820.gif "")

[Here](https://github.com/alibaba/AliOS-Things/tree/rel_2.1.0/build/site_scons/upload) you can see `AliOS Studio` board list that supports **Debug**. If you want your board supports `AliOS Studio` Debug, refer to:

* [https://github.com/alibaba/AliOS-Things/tree/master/build/site\_scons](https://github.com/alibaba/AliOS-Things/tree/master/build/site_scons)。

Reference video: [使用 AliOS Studio 开始 AliOS Things 调试](http://v.youku.com/v_show/id_XMzU3OTE5ODE1Ng==.html).


##### Set Optimization Level

Best to set the optimization level to `-Og` or `-O0` when debugging, otherwise there will be problems such as function jump exception, single-step debugging exception, variable optimize-out. Set the optimization level:
* For AliOS Things < 2.1.0: modify `COMPILER_SPECIFIC_OPTIMIZED_CFLAGS` to -Og or -O0 in `build/aos_toolchain_arm-none-eabi.mk`.
* For AliOS Things >= 2.1.0: add `BUILD_TYPE=debug` when make: `aos make BUILD_TYPE=debug`.

## Others

### AliOS Studio Commands list

Press <kbd>Ctrl-Shift-P</kbd> to open the command palette and type alios-studio:

![image | left](https://img.alicdn.com/tfs/TB1D5FBH9rqK1RjSZK9XXXyypXa-717-413.jpg "")

|command|description|Status bar icons|
|:---|:---|:---:|
|`Build`|Build AliOS Things: `aos make app@board`|![](https://img.alicdn.com/tfs/TB14LGAkNnaK1RjSZFBXXcW7VXa-25-22.png)|
|`Change build target`|Change build target, including app and board|![](https://img.alicdn.com/tfs/TB15HqhkMHqK1RjSZFEXXcGMXXa-25-22.png)|
|`Clean`|Clean : `aos make clean`|![](https://img.alicdn.com/tfs/TB1_PN_kSrqK1RjSZK9XXXyypXa-25-22.png)|
|`Upload`|Upload firmware to device: `aos upload app@board`|![](https://img.alicdn.com/tfs/TB1gwqakNTpK1RjSZR0XXbEwXXa-25-22.png)|
|`Config Serial Monitor`|-|-|
|`Connect device` |Execute command: `aos monitor`|![](https://img.alicdn.com/tfs/TB1oSSckHvpK1RjSZPiXXbmwXXa-25-22.png)|
|`Install aos-cube`|Check [Install aos-cube](#install-aos-cube)|-|
|`List Device`|List all the serial port|-|
|`Manage Account`|Manage aliyun account|-|
|`Probe Device`|-|-|
|`Technical Support`|Open **dingtalk**|-|
|`OTA(Developerment Over-The-Air)`|developerment over-the-air|-|
|`Show welcome page`|show welcome page|-||


## Keyboard Shortcut

AliOS Studio default keybinding:

|key|command|description|
|:-|:-|:-:|
|<kbd>shift+alt+b</kbd>|`alios-studio.build`|build|
|<kbd>shift+alt+c</kbd>|`alios-studio.clean`|clean|
|<kbd>shift+alt+u</kbd>|`alios-studio.upload`|upload|

Use the following to embed a AliOS Studio shortcut in `keybindings.json`. Replace with your preferred key bindings:

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


### Will display api-link when hover on AliOS Things APIs

To help developers to be familiar with AliOS Things APIs faster, `AliOS Studio` will display api-link when hover on AliOS Things APIs:

![](https://img.alicdn.com/tfs/TB1p3RKHVzqK1RjSZFCXXbbxVXa-917-350.png)

### Convert TSL json file to C String file

[Thing Specification Language (TSL)](https://help.aliyun.com/document_detail/73727.html?spm=a2c4g.11186623.6.566.62742ab2c8pTxa) is a data model that digitizes a physical entity and constructs the entity data model in IoT Platform.
`AliOS Studio` offer an efficient way to convert TSL json file to C file.
right-click json file, and click `Convert TSL json to C string` to convert TSL json file to C file like this:

![](https://img.alicdn.com/tfs/TB17h0GHVYqK1RjSZLeXXbXppXa-1140-820.gif)


## Configuration Files

There is 3 configuration files In AliOS Things source code:

* `launch.json` - configure debugging options
* `settings.json` - configure `AliOS Studio` options
* `tasks.json` - tasks list including build, flash, serial tool, clean etc.

> For AliOS-Things 2.1.0, there is an symbol database named `.TAGS.AOS.DB`.


### launch.json

AliOS Studio using [vscode-cpptools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) to debug AliOS Things, which you can configure debug options in `launch.json`. refer to [vscode-cpptools/launch.md](https://github.com/Microsoft/vscode-cpptools/blob/master/launch.md) for more `launch.json` configure options.

**AliOS Studio will update `launch.json` every time when you change build target.**

`launch.json` mainly options:

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

#### Mainly Options

|option|description|
|---|---|
|`program`|ELF file to debug, like `out/app@board/binary/app@board.elf` |
|`miDebuggerServerAddress` and `setupCommands`|GDB connect port, Different gdb servers use different ports|
|`miDebuggerPath`|The path to the debugger (such as gdb) |

### settings.json

`AliOS Studio` will update `settings.json` automatically. but you can modify `settings.json` by yourself.

```json
{
    "aliosStudio.inner.yosBin": "aos",
    "aliosStudio.hardware.board": "cy8ckit-149",
    "aliosStudio.name": "helloworld",
    "aliosStudio.aosVersion": "2.1.0",
    "C_Cpp.default.browse.databaseFilename": "${workspaceRoot}/.vscode/.TAGS.AOS.DB",
    "aliosStudio.iot.deviceName": "test_01",
    "aliosStudio.iot.productKey": "a1MZxOdcBnO"
}
```

> This is `settings.json` in AliOS Things 2.1.0.

#### options

| option | description |
| --- | --- |
| `yosBin` | - |
| `hardware.board` | board of build target |
| `aosVersion` | AliOS Things version |
| `C_Cpp.default.browse.databaseFilename` | symbol database path |
| `iot.deviceName` | deviceName for OTA(Developerment Over-The-Air) |
| `iot.productKey` | productKey for OTA(Developerment Over-The-Air) |

### tasks.json

> Official description about vscode `task.json` please refer to [https://code.visualstudio.com/Docs/editor/tasks](https://code.visualstudio.com/Docs/editor/tasks)。options of task please refer to [https://code.visualstudio.com/Docs/editor/tasks#\_custom-tasks](https://code.visualstudio.com/Docs/editor/tasks#_custom-tasks)。

`AliOS Studio` using `tasks.json` to do many works such as build, flash, clean.

#### tasks list

|label|description| statusbar icons|
|---|---|:---:|
| alios-studio: Make | build | ![](https://img.alicdn.com/tfs/TB14LGAkNnaK1RjSZFBXXcW7VXa-25-22.png) |
| alios-studio: Upload | flash | ![](https://img.alicdn.com/tfs/TB1gwqakNTpK1RjSZR0XXbEwXXa-25-22.png) |
| alios-studio: Serial Monitor | serial tool | ![](https://img.alicdn.com/tfs/TB1oSSckHvpK1RjSZPiXXbmwXXa-25-22.png) |
| alios-studio: Clean | build | ![](https://img.alicdn.com/tfs/TB1_PN_kSrqK1RjSZK9XXXyypXa-25-22.png) |
| alios-studio: OTA | OTA(Developerment Over-The-Air) | -|

You can add custom tasks by yourself in `tasks.json`, and run task by click menubar `Terminal` > `Run Task...`:

![image | left](https://img.alicdn.com/tfs/TB1wujGH6TpK1RjSZKPXXa3UpXa-718-288.png "")

There are more tasks for AliOS Things in [Appendix](#appendix) > [Add Custom Tasks](add_custom_tasks).
## Appendix

### Add Custom Tasks

#### Custom Tasks - Export IAR/MDK project:

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

#### Custom Tasks - Parallel Build:

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

#### Custom Tasks - Build For Debug:

```json
{
  "label": "alios-studio: Build For Debug",
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

## FAQ

#### Visual Studio Code is unable to watch for file changes in this large workspace
> This error only appears on **linux**

check [Visual Studio Code is unable to watch for file changes in this large workspace](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Studio#visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace)

#### Workspace is too large to watch for file changes
same problem as [Visual Studio Code is unable to watch for file changes in this large workspace](#visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace)

#### SyntaxError: .vscode\launch.json: Unexpected token / in JSON at position 4378

Don't add comments in `.vscode/tasks.json` or `.vscode/launch.json`.

## Known Issues

## Report Issues
Report this problem to us: https://github.com/alibaba/AliOS-Things/issues
