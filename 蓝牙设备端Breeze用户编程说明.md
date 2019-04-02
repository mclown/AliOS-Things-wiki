# 背景

Breeze协议提供基于蓝牙链路连接阿里云的安全通道和服务，并且提供蓝牙辅助配网功能(通过BLE链路获取AP的SSID和password)。基本SDK API在breeze\_export.h中，蓝牙配网的接口API在breeze\_awss\_export.h中。

## 接口API

### breeze\_start

启动breeze SDK服务。用户使用此接口初始化和启动breeze服务。

__参数__

| __名称__  | __类型__       | __描述__                                           |
| :-------- | :------------- | :------------------------------------------------- |
| dev\_conf | device\_config | 初始化Breeze SDK的信息，包含设备信息，回调函数等。 |

__返回值__

`0` 成功, `-1` 失败。

### breeze\_end

停止breeze服务，用户调用此接口停止breeze服务。

__参数__

无。

__返回值__

`0` 成功, `-1` 失败。

### breeze\_post

推送设备端状态数据至移动端，使用BLE indicate方式。

__参数__

| __名称__ | __类型__   | __描述__           |
| :------- | :--------- | :----------------- |
| buffer   | uint8\_t\* | 数据指针。         |
| length   | uint32\_t  | 数据长度，byte数。 |

__Return__

`0` 成功, 其他错误值 失败。

### breeze\_post\_fast

和`breeze_post`类似，推送设备端状态数据至移动端，区别在于使用BLE notify方式。

__参数__

| __名称__ | __类型__   | __描述__           |
| :------- | :--------- | :----------------- |
| buffer   | uint8\_t\* | 数据指针。         |
| length   | uint32\_t  | 数据长度，byte数。 |

__Return__

`0` 成功, 其他错误值 失败。

### breeze\_post\_ext

设备端上报带有cmd字段的数据至移动端。

__参数__

| __名称__ | __类型__   | 描述                    |
| :------- | :--------- | :---------------------- |
| buffer   | uint8\_t\* | 数据指针。              |
| length   | uint32\_t  | 数据长度，byte。        |
| cmd      | uint8\_t   | 推送给移动端的cmd类型。 |

__返回值__

`0` 成功, 其他错误值 失败。

### breeze\_append\_adv\_data

广播内容增加用户自定义数据。

__参数__

| __名称__ | __类型__   | __描述__           |
| :------- | :--------- | :----------------- |
| data     | uint8\_t\* | 数据指针。         |
| len      | uint32\_t  | 数据长度，byte数。 |

__返回值__

无。

### breeze\_restart\_advertising

SDK重启蓝牙广播。

__参数__
无。

__返回值__
无。

### breeze\_awss\_init

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口对蓝牙配网SDK进行初始化。在用户业务逻辑初始化阶段调用。</span></span>

__参数__

| __名称__ | __类型__             | __描述__                                                     |
| :------- | :------------------- | :----------------------------------------------------------- |
| cb       | apinfo\_ready\_cb    | 为设备完成WiFi信息（SSID、密码）获取后的回调函数，由用户定义/提供，并由SDK完成调用。 |
| info     | breeze\_dev\_info\_t | 为设备信息，包括ProductID，Product Key，Product Secret，Device Name，Device Secret等字段，由用户提供。 |

__返回值__

无。

### breeze\_awss\_end

停止蓝牙配网服务。

__参数__
无。

__返回值__
无。