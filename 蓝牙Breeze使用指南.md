# 背景介绍
设备硬件具备蓝牙BLE能力，在与手机蓝牙连接后通过一套SDK建立安全通道，将数据推送给云端。如下:

![Breeze topology](https://img.alicdn.com/tfs/TB1mKLyOSzqK1RjSZFjXXblCFXa-516-208.png)

AliOS Things实现了一套基于蓝牙链路的轻量级安全通路的Breeze SDK，结构框图如下:

![Breeze framework](https://img.alicdn.com/tfs/TB1pxjzONTpK1RjSZFMXXbG_VXa-1094-728.png)

设备端Breeze SDK代码结构
组件代码位于`$(AliOS Things Src)/network/bluetooth/breeze`，如下：
```.
├── Config.in
├── README.md
├── aos.mk
├── api
├── core
├── include
└── ref-impl
```

其中:
* Config.in //配合meunconfig组件配置
* README.md //readme文档
* aos.mk //makefile文档
* api //目录，暴露给应用的接口:breeze_export.* 是Breeze SDK提供的接口; breeze_awss_export.*是蓝牙辅助配网的接口。用户可以依照应用场景选择其中之一使用。
* core //目录，源码核心实现，包含安全，通道以及扩展指令等的实现。
* include //Breeze SDK移植的头文件。
* ref-impl //参考移植实现，使用AliOS Things OS, BLE协议栈，以及内部mebdtls和SH256的安全部分参考。

# 使用开发指南
开发者可以使用默认配置，在已经支持的硬件平台上(如nordic的pca10040)直接测试并修改例程，也可以通过SDK提供的HAL接口，对接需要的接口来实现。
## 使用AliOS Things配置
* 使用默认配置编译，获取默认配置固件:
```
$aos make breezeapp@pca10040 -c config
$aos make 
```



