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
* 如果想要修改配置，可通过menuconfig来修改并编译
```
$aos make menuconfig
$aos make
```
menuconfig具体重要配置点:
* Application Configuration 界面勾选"Builtin Examples" -> "breezeapp", 根据需要勾选"Enable OTA with Breeze Link"

![app_pic](https://img.alicdn.com/tfs/TB1s_fEOHrpK1RjSZTEXXcWAVXa-970-784.png)

* BSP Configuration,选择需要的硬件平台，如pca10040

![board_cfg](https://img.alicdn.com/tfs/TB107PEOMHqK1RjSZFEXXcGMXXa-932-766.png)

* Network Configuration，选择"Breeze SDK",在子菜单会有对应一下选项分别对应：

![network_cfg](https://img.alicdn.com/tfs/TB1koPyOH2pK1RjSZFsXXaNlXXa-925-733.png)

* 连续安全广播:用以特定安全广播，需配合云端一起使用。
* 设备入网加密方式选择: 一型一密和一机一密设备认证选择。
* 辅助配网功能:用来使能辅助配网相关的配置。
* 链路认证:Breeze链路安全认证。
* 使用默认对接配置:OS使用AliOS Things，Security算法使用内部加密算法，蓝牙使用Blestack。

## 移植Breeze到第三方平台
参见源代码目录下的ref-impl，将第三方的实现替换此文件夹下的HAL实现:1)OS; 2)BLE协议栈; 3)Security，定义在`$(AliOS Things Src)/network/bluetooth/breeze/include`下，其余部分代码和应用维持不变。
* breeze_hal_ble:对接BLE蓝牙协议栈部分接口，包含蓝牙广播，注册Breeze蓝牙服务等。
* breeze_hal_sec:对接安全部分，包含AES128 CBC加解密算法，SHA256接口。
* breeze_hal_os:对接不同OS的接口，包含OS timer的资源, KV(存储和读取接口)操作等。
详细接口定义和说明详见<HAL接口列表>

# API接口列表


# HAL接口列表


# 调试入云信息和环境搭建
## 设备信息申请
由于设备最终要通过云端认证，所以需要申请在云端申请对应的产品认证信息
* 创建项目，登录阿里云IoT智能生活开放平台，https://living.aliyun.com/#/，点击"创建项目"

![create_product](https://img.alicdn.com/tfs/TB1HlfAOOrpK1RjSZFhXXXSdXXa-2366-1054.png)

* 新建产品，在产品界面，点击"新建产品",注意选择节点类型为"设备",是否接入"是"，并接入网关协议为"BLE"

![cfg_product](https://img.alicdn.com/tfs/TB1K6bGOFzqK1RjSZFCXXbbxVXa-1910-1360.png)

* 在产品定义界面右边可以看到产品的三个信息:Product Key, Product Secret和Product ID，这对于一型一密设备来说已经足够，但一机一密还需要继续点击"设备调试"

![3elements](https://img.alicdn.com/tfs/TB1hgS4OSzqK1RjSZPcXXbTepXa-2580-1300.png)

* 在设备调试界面，点击"新增测试设备",继续生成对应同一类型产品的多个设备，根据"DeviceName"进行区分，Device Secret,至此一机一密需要的五个元素信息都已经获得

![devname](https://img.alicdn.com/tfs/TB14SjDOFzqK1RjSZFoXXbfcXXa-2432-1256.png)

![new_product_success](https://img.alicdn.com/tfs/TB1s8zHOQPoK1RjSZKbXXX1IXXa-750-407.jpg)

## 调试环境搭建
测试用例主要使用Breezeapp并配合手机客户端demo进行测试。
* 编译breezeapp应用。在app/example/bluetooth/breezeapp/breezeapp.c中，将设备信息修改为上一步申请到的元组信息，参考前面编译部分。
* 烧录固件至设备，并连接串口终端会有如下类似信息(Breeze信息，蓝牙MAC地址，AOS版本等)打印:

![hello_print](https://img.alicdn.com/tfs/TB1lijEOMHqK1RjSZFPXXcwapXa-1244-302.png)

* 手机客户端测试(以IOS为例)
 * 登录账号，注意环境需要与设备端五元组信息匹配。
 * 打开主界面并扫描蓝牙设备。
 * 点击并连接,界面会有连接成功和安全通道建立提示。
 * 通过手机端发送数据，设备端回传数据，手机界面会有提示和hex数据显示。

![discover_1](https://img.alicdn.com/tfs/TB1alTtOSrqK1RjSZK9XXXyypXa-936-1620.png)

![connected](https://img.alicdn.com/tfs/TB1_mfwOSzqK1RjSZPxXXc4tVXa-928-1616.png)

# 设备端高级应用场景开发指南
在SDK基础建立安全通道基础上，主要有以下两应用场景:1)蓝牙辅助Wifi配网  2)设备OTA。

## 蓝牙辅助配网

[蓝牙辅助配网详细说明。](https://github.com/alibaba/AliOS-Things/wiki/蓝牙辅助WiFi配网开发说明)

## Breeze设备OTA
在SDK使能OTA情况下，手机Breeze SDK拉取云端的将要升级的固件，通过建立的Breeze通道将设备固件分包传输至设备端，设备端按照OTA的交互对写入对应FLASH分区，完成之后重启设备结束整个OTA流程。
## OTA编译配置
可以参考breezeapp事例，默认已经使能OTA功能。如果使用meunconfig的话需要确认如下设置:
* APP, Application Configuration-> Selete APP(Builtin Examples) -> breezeapp -> enable ota with breeze link

![ota_1](https://img.alicdn.com/tfs/TB1DobDONTpK1RjSZFMXXbG_VXa-1492-791.jpg)

* Middleware Configuration ->uAgent ->OTA ->ota_ble

![ota_2](https://img.alicdn.com/tfs/TB1fSHOONjaK1RjSZFAXXbdLFXa-1492-786.jpg)

## OTA对接移植
OTA模块代码实现在middleware/uagent/ota/ota_ble下，包含:

```
.
├── aos.mk
├── Config.in
├── inc
│   ├── ota_breeze_export.h
│   ├── ota_breeze.h
│   ├── ota_breeze_os.h
│   ├── ota_breeze_plat.h
│   └── ota_breeze_transport.h
├── README.md
└── src
    ├── ota_breeze.c
    ├── ota_breeze_os.c
    ├── ota_breeze_plat.c
    ├── ota_breeze_service.c
    └── ota_breeze_transport.c
```

其中:
* inc:包含了对外的接口和对接API。
* src:OTA内部逻辑的实现。

# 支持硬件列表
截止到AliOS Things 2.1版本，支持Breeze SDK以及高级特性的硬件平台汇总如下:

![support_1](https://img.alicdn.com/tfs/TB1offchDZmx1VjSZFGXXax2XXa-865-339.jpg)

蓝牙配网在硬件版本支持情况如下:

![support_2](https://img.alicdn.com/tfs/TB1HrrzOSzqK1RjSZFLXXcn2XXa-1093-364.jpg)