EN | [中文](AliOS-Things-uCube.zh)

# Content

- [1 Setup](#1-安装)
- [2 Usage](#2-使用)
    - [2.1 Build Project](#21-)
    - [2.2 Upload and Debug](#22-烧录与调试)
    - [2.3 Monitor Serial Port](#23-串口监控)
    - [2.4 Help](#24-帮助信息)
------

**AliOS-Things uCube** is AliOS-Things command line project management system (abbreviation command is aos). Its functions include configure and build project, image upload and debug ..., and also could be integrated with IDE.

# 1 Setup

uCube development based on Python (Version:2.7) needs Python (Version:2.7) development environment (pass Python 2.7.14 verification test). More information about install aos-cube please refer to [AliOS Things Environment Setup](AliOS-Things-Environment-Setup).

# 2 Usage

## 2.1 Build Project

For AliOS Things 2.1 and later releases, the build steps are configure & build (aos-cube 0.3.x required):
```
# Create minimal configs and build:
aos make helloworld@developerkit -c config
aos make

# Customize configs and build:
aos make menuconfig
aos make
```

For AliOS Things 2.0 and former releases, run command:
```
aos make helloworld@developerkit
```

## 2.2 Upload and Debug

After build completed, connect to development board to upload the image and start debug:
```
aos upload helloworld@developerkit
aos debug helloworld@developerkit
```

For AliOS Things 2.1 and later releases, run the short commands instead after build completed (aos-cube 0.3.x required):
```
aos upload
aos debug
```

## 2.3 Monitor Serial Port
Connect to serial port and check output of application:
```
$ aos monitor <serial port> <baudrate>
```

## 2.4 Help
More functions and information please refer to:
```
$ aos -h
$ aos <subcommand> -h
```
