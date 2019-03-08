[EN](AliOS-Things-uCube) | 中文

# 目录
- [1 安装](#1-安装)
- [2 使用](#2-使用)
    - [2.1 工程构建](#21-工程构建)
    - [2.2 烧录与调试](#22-烧录与调试)
    - [2.3 串口监控](#23-串口监控)
    - [2.4 帮助信息](#24-帮助信息)
------
**AliOS-Things uCube** 是 AliOS Things 基于命令行的开发管理工具（命令简写为 aos），主要功能包括：工程配置与编译、Image下载调试、其他辅助功能以及与IDE集成。

# 1 安装
uCube 基于 Python（Version：2.7）语言开发，需要有 Python（Version：2.7）开发环境（Python 2.7.14 下验证测试通过）。如开发环境中尚未安装aos-cube，请参考[AliOS Things Environment Setup](AliOS-Things-Environment-Setup)。

# 2 使用
## 2.1 工程构建
AliOS Things 2.1及其后续版本需要先配置、再构建（需要aos-cube 0.3.x）:
```
# 生成最简配置并构建
aos make helloworld@developerkit -c config
aos make

# 自定义配置并构建
aos make menuconfig
aos make
```

AliOS Things 2.0及之前的版本直接执行命令：
```
aos make helloworld@developerkit
```

## 2.2 烧录与调试
构建完成后，连接开发板到开发主机，首先将编译生成的镜像上传到开发板，然后启动调试器：
```
aos upload helloworld@developerkit
aos debug helloworld@developerkit
```

AliOS Things 2.1及其后续版本支持简化的烧录和调试命令行（需要aos-cube 0.3.x），构建完成后直接执行：
```
aos upload
aos debug
```

## 2.3 串口监控
连接串口并查看应用输出和执行过程：
```
$ aos monitor <serial port> <baudrate>
```

## 2.4 帮助信息
其他辅助功能及信息请参考帮助：
```
$ aos -h
$ aos <subcommand> -h
```
