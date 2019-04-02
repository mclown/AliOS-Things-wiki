# 蓝牙设备端开发指南

## <a name="c909ab32"></a>背景介绍

阿里云IoT针对蓝牙设备，提供了一套Breeze方案，可以实现APP-设备-云的完整链路。
本文主要介绍在设备端上，如何让硬件具备蓝牙BLE能力，并与手机蓝牙连接后通过一套SDK建立安全通道，将数据推送给云端。手机端客户端和云端开发请参阅。
链路图如下: 

![00-1550062278267-323ace3b-7a8a-4cf1-9275-843ff6d9ea3c.png | left](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301642-b3e984d5-e46a-436e-b56b-fd89579a2b41.png "00-1550062278267-323ace3b-7a8a-4cf1-9275-843ff6d9ea3c.png")

AliOS Things实现了一套基于蓝牙链路的轻量级安全通路的Breeze SDK，结构框图如下:

![01-1550748266225-5ae4e492-020a-4706-91ae-1aaf8e86b134.png | left | 573x381](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301668-6ed942f2-0366-4e3d-9292-c49251cc2e1b.png "01-1550748266225-5ae4e492-020a-4706-91ae-1aaf8e86b134.png")

## <a name="611d1422"></a>设备端Breeze SDK代码结构

Breeze设备端作为AliOS Things的组件完全开源，AliOS Things最新版本请从[github](https://github.com/alibaba/AliOS-Things/tree/rel_2.1.0)获取，使用请参阅AliOS Things相关[wiki](https://github.com/alibaba/AliOS-Things/wiki)。在AliOS Things的根目录下，代码位于`$(AliOS Things Src)/network/bluetooth/breeze`，如下：

```
.
├── Config.in
├── README.md
├── aos.mk
├── api
├── core
├── include
└── ref-impl
```

其中:

- Config.in //配合meunconfig组件配置
- README.md //readme文档
- aos.mk //makefile文档
- api //目录，暴露给应用的接口:breeze\_export.\* 是Breeze SDK提供的接口; breeze\_awss\_export.\*是蓝牙辅助配网的接口。用户可以依照应用场景选择其中<span data-type="color" style="color:rgb(0, 0, 0)">之一</span>使用。
- core //目录，源码核心实现，包含安全，通道以及扩展指令等的实现。
- include //Breeze SDK移植的头文件。
- ref-impl //参考移植实现，使用AliOS Things OS, BLE协议栈，以及内部mebdtls和SH256的安全部分参考。

## <a name="7860b685"></a>使用开发指南

开发者可以使用默认配置，在已经支持的硬件平台上(如nordic的pca10040/pca10056)直接测试并修改例程，也可以通过SDK提供的HAL接口，对接需要的接口来实现。

### <a name="511de31c"></a>使用AliOS Things配置

- 使用默认配置编译，获取默认配置固件:

```
$aos make breezeapp@pca10040 -c config
$aos make 
```

- 如果想要修改配置，可通过menuconfig来修改并编译

```
$aos make menuconfig
$aos make
```

menuconfig具体重要配置点:

- Application Configuration 界面勾选"Builtin Examples" -> "breezeapp", 根据需要勾选"Enable OTA with Breeze Link"

![03-1550759831482-831428fb-e2e2-445e-a048-747567917e92.png | left | 589x476](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301677-f043acbe-1721-4c30-b830-1a3c593d1f45.png "03-1550759831482-831428fb-e2e2-445e-a048-747567917e92.png")

- BSP Configuration,选择需要的硬件平台，如pca10040

![04-1550759895734-b05d4117-2ca3-4a34-8f84-b37dc086100e.png | left | 591x486](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301690-afd9c95b-3e5c-4660-afd3-c25ed9bb0c19.png "04-1550759895734-b05d4117-2ca3-4a34-8f84-b37dc086100e.png")

- Network Configuration，选择"Breeze SDK",在子菜单选项分别对应：

![05-1550760363141-4d65b55d-67c0-47fa-b156-8069541110d4.png | left | 591x468](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301701-583cee46-2e2e-43b8-b57f-0499d644dde5.png "05-1550760363141-4d65b55d-67c0-47fa-b156-8069541110d4.png")

- 连续安全广播:用以特定安全广播，需配合云端一起使用。
- 设备入网加密方式选择: 一型一密和一机一密设备认证选择。
- 辅助配网功能:用来使能辅助配网相关的配置。
- 链路认证:Breeze链路安全认证。
- 使用默认对接配置:OS使用AliOS Things，Security算法使用内部加密算法，蓝牙使用Blestack。

### <a name="a976132e"></a>移植SDK到第三方平台

参见源代码目录下的ref-impl，将第三方的实现替换此文件夹下的HAL实现:1)OS; 2)BLE协议栈; 3)Security，定义在`$(AliOS Things Src)/network/bluetooth/breeze/include`下，其余部分代码和应用维持不变。

- breeze\_hal\_ble:对接BLE蓝牙协议栈部分接口，包含蓝牙广播，注册Breeze蓝牙服务等。
- breeze\_hal\_sec:对接安全部分，包含AES128 CBC加解密算法，SHA256接口。
- breeze\_hal\_os:对接不同OS的接口，包含OS timer的资源, KV(存储和读取接口)操作等。

## <a name="a4fce0b4"></a>API接口列表

详细定义和说明见【[蓝牙设备端SDK用户编程说明](https://yuque.antfin-inc.com/ilopdoc/guide/eyz31k)】。

## <a name="66ce5462"></a>HAL接口列表

详细定义和说明见【[蓝牙设备端SDK移植接口说明](https://yuque.antfin-inc.com/ilopdoc/guide/hmnca9)】。

## <a name="c80b96d8"></a>调试入云信息和环境搭建

### <a name="5916b051"></a>设备信息申请

由于设备最终要通过云端认证，所以需要申请在云端申请对应的产品认证信息

1. 创建项目，登录[阿里云IoT智能生活开放平台](https://living.aliyun.com/#/)，点击"创建项目"

![06-1550066374598-701a1422-daf9-4023-9541-2ebf60420a58.png | left | 595x266](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301743-b4cb1990-8f95-46a7-a0c1-c7e79e0745b7.png "06-1550066374598-701a1422-daf9-4023-9541-2ebf60420a58.png")

1. 新建产品，在产品界面，点击"新建产品",注意选择节点类型为"设备",是否接入"是"，并接入网关协议为"BLE"。

![07-1550066607647-849d09d8-5d60-4eef-b709-f106cd9b157d.png | left | 599x426](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301753-35be4ee3-24c3-47ef-ac83-ce3f7076e242.png "07-1550066607647-849d09d8-5d60-4eef-b709-f106cd9b157d.png")

1. 在产品定义界面右边可以看到产品的三个信息:Product Key, Product Secret和Product ID，这对于一型一密设备来说已经足够，但一机一密还需要继续点击"设备调试"。

![09-1550067252000-d79b185d-a4d4-4982-8c56-6af479941796.png | left | 602x311](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301788-9cd1249e-e1e0-4a06-acb1-d7a83a832077.png "09-1550067252000-d79b185d-a4d4-4982-8c56-6af479941796.png")

1. 在设备调试界面，点击"新增测试设备",继续生成对应同一类型产品的多个设备，根据"DeviceName"进行区分，Device Secret,至此一机一密需要的五个元素信息都已经获得。

![09-1550067252000-d79b185d-a4d4-4982-8c56-6af479941796.png | left | 637x329](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301788-9cd1249e-e1e0-4a06-acb1-d7a83a832077.png "09-1550067252000-d79b185d-a4d4-4982-8c56-6af479941796.png")



![09-1550067252000-d79b185d-a4d4-4982-8c56-6af479941796.png | left | 636x328](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301788-9cd1249e-e1e0-4a06-acb1-d7a83a832077.png "09-1550067252000-d79b185d-a4d4-4982-8c56-6af479941796.png")

### <a name="f4c92e5e"></a>调试环境搭建

测试用例主要使用Breezeapp并配合手机客户端demo进行测试。

1. 编译breezeapp应用。在`app/example/bluetooth/breezeapp/breezeapp.c`中，将设备信息修改为上一步申请到的元组信息，参考前面编译部分。
2. 烧录固件至设备，并连接串口终端会有如下类似信息(Breeze信息，蓝牙MAC地址，AOS版本等)打印:

![11-1550069466256-e11fd8e6-7e3c-4a7a-900d-bf33a66741b5.png | left | 615x150](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301835-57133646-e450-402d-9372-61d545b6f42c.png "11-1550069466256-e11fd8e6-7e3c-4a7a-900d-bf33a66741b5.png")

1. 手机客户端测试(以IOS为例)

- 登录账号，注意环境需要与设备端五元组信息匹配。
- 打开主界面并扫描蓝牙设备。
- 点击并连接,界面会有连接成功和安全通道建立提示。
- 通过手机端发送数据，设备端回传数据，手机界面会有提示和hex数据显示。



![12-1550069735865-e94db014-25db-4ee3-a546-3ac2b5dd9e66.png | left | 332x574](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301850-ac8ba9b6-9b7b-4bda-b4d4-fb7879198bfc.png "12-1550069735865-e94db014-25db-4ee3-a546-3ac2b5dd9e66.png")

![13-1550069763529-4620c2ab-dd61-48e3-a5ff-0181485381ce.png | left | 334x581](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301873-68453d5e-4969-4240-b098-e28a9b208ee1.png "13-1550069763529-4620c2ab-dd61-48e3-a5ff-0181485381ce.png")



## <a name="8adb8d61"></a>设备端高级应用场景开发指南

在SDK基础建立安全通道基础上，主要有以下两应用场景:1)蓝牙辅助Wifi配网  2)设备OTA。

### <a name="e005a7cc"></a>蓝牙辅助配网

#### <a name="7efcb0ce"></a>使用场景

WiFi设备需要连接WiFi热点（WiFi AP）之后才能与其它设备进行IP通信，对于没有键盘、没有触摸屏、没有Web Server的WiFi IoT设备而言，如何获取WiFi热点的SSID/密码是实现设备远程管理的第一个关键步骤。
我们将WiFi设备获取到WiFi热点的SSID/密码的步骤称为WiFi配网，蓝牙辅助WiFi配网方案__适用于同时支持蓝牙BLE以及WiFi的设备__，该方案通过蓝牙BLE通道将WiFi热点的SSID/密码发送给WiFi设备，从而让WiFi设备可以连接到WiFi AP（通常为WiFi无线路由器）。蓝牙辅助WiFi配网的工作原理示意图如下:

![15-1550123184595-db381a74-a16d-495e-949d-1a3a24e149df.png | left](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301908-1bee2aa6-4dd1-4a75-892a-57cad5989be5.png "15-1550123184595-db381a74-a16d-495e-949d-1a3a24e149df.png")

可以很容易得出蓝牙辅助Wifi配网构成:

- 设备端SDK:设备端硬件支持Wifi/BLE combo芯片,并运行基于Breeze SDK的蓝牙配网例程。
- 配网手机端SDK:手机集成配套的蓝牙配网SDK或者蓝牙配网Demo演示例程。

#### <a name="5e72862b"></a>开发流程

如果您是一个WiFi的模组商或者设备商，您的模组/设备支持蓝牙BLE以及WiFi并且希望集成阿里云IoT提供的蓝牙辅助WiFi配网功能，那么可以按照下面的过程进行模组/设备开发：

![16-1550123823936-75ed2f2d-5abb-40a8-b304-e79dc33f471d.png | left](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301935-0f6317d7-0a25-4f2d-90ec-f81180e4d812.png "16-1550123823936-75ed2f2d-5abb-40a8-b304-e79dc33f471d.png")

- 产品注册。此部分请参考前面章节【调试入云信息和环境搭建】。
- 设备移植。此部分请参考前面章节【移植SDK到第三方平台】。
- 使用SDK/DemoSDK开发。详细指南可参阅【[蓝牙连接开发指南](https://yuque.antfin-inc.com/ilopdoc/guide/ruw3kk)】
- 设备端蓝牙配网调试,使用aos menuconfig来配置:

1. 修改linkkitapp，使能蓝牙配网: 应用目录位于`app/example/linkkitapp`下，默认linkkitapp的蓝牙配网功能是关闭的，需要使能蓝牙配网，选中"Enable Breeze AWSS"。

![17-1551354947355-89eb1afb-027d-4975-96a2-7db70959694e.png | left | 638x360](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301959-68780c25-045b-4b1f-924b-dec91f75d49d.png "17-1551354947355-89eb1afb-027d-4975-96a2-7db70959694e.png")

1. 使能Breeze模块对应的AWSS功能，menuconfig主界面"Network" ->"Breeze SDK"->"Enable AWSS"。

![17-1551354947355-89eb1afb-027d-4975-96a2-7db70959694e.png | left | 642x363](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301959-68780c25-045b-4b1f-924b-dec91f75d49d.png "17-1551354947355-89eb1afb-027d-4975-96a2-7db70959694e.png")

1. 配置meunconfig其他选项。
2. 修改对应的设备信息，在`app/example/linkkitapp/combo/combo_devinfo.h`修改。
3. 编译固件: `aos make`
4. 烧录固件到设备中，连接串口。

- 手机SDK端（以Android平台XlinkSDKDemo为例）

1. 注册并登录账号。
2. 手机连接路由器，确保路由器可以连接阿里云。
3. 根据配网信息生成二维码，可以在第三方二维码生成网站，如[https://cli.im/](https://cli.im/)，二维码格式如下:

![19-1550125830503-0c3dbe00-5a00-4c3a-93cf-1afbeda1ca70.png | left | 422x133](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368301989-837640b5-3f9f-4f04-a2d1-59b4b796b1b9.png "19-1550125830503-0c3dbe00-5a00-4c3a-93cf-1afbeda1ca70.png")

1. 在XlinkSdkDemo底部菜单，"配网"->"统一配网"->"扫码方式配网"
2. 进入二维码扫描界面，扫描上一步生成的包含设备和路由器信息的二维码。
3. 主界面会有相应的设备配网结果提示。



![20-1550125930827-16bc5d11-97fa-4153-9d17-e1d472523d06.png | left | 413x873](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368302033-57c1aeb0-ef21-4b85-839c-0412ef410bcd.png "20-1550125930827-16bc5d11-97fa-4153-9d17-e1d472523d06.png")



![21-1550125998385-b921c870-d72f-4191-8795-03074a622053.png | left | 408x843](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368302048-87cc14c9-86f7-4b13-b008-65453181c333.png "21-1550125998385-b921c870-d72f-4191-8795-03074a622053.png")

### <a name="d00d9752"></a>Breeze设备OTA

在SDK使能OTA情况下，手机Breeze SDK拉取云端的将要升级的固件，通过建立的Breeze通道将设备固件分包传输至设备端，设备端按照OTA的交互对写入对应FLASH分区，完成之后重启设备结束整个OTA流程。

#### <a name="b1e38fa0"></a>OTA编译配置

可以参考breezeapp事例，默认已经使能OTA功能。如果使用meunconfig的话需要打开:
使能"Breezeapp"->"Enable OTA With Breeze Link"。



![22-1551355992710-cc874015-6be4-4ec2-9079-bf3800efb454.png | left | 623x364](https://cdn.nlark.com/yuque/0/2019/png/288262/1552368302089-b0039f44-8209-4c94-8444-0aaee7281ffe.png "22-1551355992710-cc874015-6be4-4ec2-9079-bf3800efb454.png")

#### <a name="07d684fe"></a>OTA对接移植

OTA模块代码实现在`middleware/uagent/ota/ota_ble`下，包含:

```
.
├── Config.in
├── README.md
├── aos.mk
├── inc
│   ├── ota_breeze.h
│   ├── ota_breeze_export.h
│   ├── ota_breeze_plat.h
│   └── ota_breeze_transport.h
└── src
    ├── ota_breeze.c
    ├── ota_breeze_plat.c
    ├── ota_breeze_service.c
    └── ota_breeze_transport.c
```

其中:

- inc:包含了对外的接口和对接API。
- src:OTA内部逻辑的实现。

具体对接函数请参阅[【蓝牙设备端SDK OTA接口说明】](https://yuque.antfin-inc.com/ilopdoc/guide/kqgi8c)

#### <a name="aeaec89a"></a>OTA调试和使用

OTA的具体用法请参见[【蓝牙连接开发指南】](https://yuque.antfin-inc.com/ilopdoc/guide/ruw3kk)中“<span data-type="color" style="color:rgb(0, 0, 0)">运营中OTA配置”</span>章节；

## <a name="adb9e6e5"></a>支持硬件列表

截止到AliOS Things 2.1版本，支持Breeze SDK以及高级特性的硬件平台汇总如下:

| 硬件     | Breeze SDK | 蓝牙配网 | OTA  |
| :------- | :--------- | :------- | :--- |
| pca10040 | Y          | N        | Y    |
| pca10056 | Y          | N        | Y    |
| esp32    | Y          | Y        | N    |

