<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Breeze SDK是阿里巴巴IoT事业部提供的基于蓝牙协议栈(aos组件自带的或者厂商自己的)上层提供的一套安全接入的解决方案，并支持通过蓝牙通道支持wifi辅助配网功能，本文对Breeze SDK的HAL接口进行说明，厂商对接时可以参考本文档。</span></span>

## Breeze目录结构

Breeze位于`network/bluetooth/breeze`，目录结构如下：

```plain
.
├── Config.in
├── README.md
├── aos.mk
├── api
├── core
├── hal
├── include
└── ref-impl
```

其中：

- Config.in : 配合menuconfig的配置文件。
- README.md：Breeze SDK说明文档。
- aos.mk文件：Breeze SDK Makefile。
- api目录：包含用户编程接口的定义和实现。
- core: 源码实现。
- include：Breeze SDK对接的头文件定义。
- ref-impl目录：提供了在AliOS Things OS， BLE协议栈，以及内部AES和SHA256安全接口的参考实现。

## HAL接口说明

Breeze SDK的HAL接口分为协议栈、OS、安全三个部分，这些接口的定义在`network/bluetooth/breeze/include/`目录中：

- __breeze\_hal\_ble.h__
  定义了蓝牙配网SDK对接到不同厂商蓝牙协议栈的接口，参考实现位于`network/bluetooth/breeze/hal/ble/breeze_hal_ble.c`文件；
- __breeze\_hal\_os.h__
  定义了蓝牙配网SDK对接到不同OS系统的接口，基于AliOSThings的参考实现位于`network/bluetooth/breeze/hal/ble/breeze_hal_os.c`，用户如果使用AliOSThings时，该部分接口不需要对接；
- __breeze\_hal\_sec.h__
  定义了蓝牙配网SDK对接到不同安全算法实现的接口，基于mbedtls的参考实现位于`network/bluetooth/breeze/hal/ble/breeze_hal_security.c`文件，用户使用AliOSThings时，该部分接口不需要对接。

## 蓝牙协议栈HAL列表 

### <span data-type="color" style="color:#262626">ble_stack_init</span>

该接口完成对BLE协议栈的初始化。

__参数__

<div class="bi-table">
  <table>
    <colgroup>
      <col width="112px" />
      <col width="150px" />
      <col width="489px" />
    </colgroup>
    <tbody>
      <tr>
        <td rowspan="1" colSpan="1">
          <div data-type="p"><strong>名称</strong></div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p"><strong>类型</strong></div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p"><strong>描述</strong></div>
        </td>
      </tr>
      <tr>
        <td rowspan="1" colSpan="1">
          <div data-type="p">ais_init</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">ais_bt_init_t</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">指定了蓝牙service和characteristics，以及他们的权限属性等相关设置。具体包括： -uuid_svc：AIS服务的uuid（type、value）信息； -rc/wc/ic/nc/ wwnrc：AIS服务的characteristic的属性信息； -on_connected:蓝牙连接时间的回调函数。
          </div>
          <div data-type="p">-on_disconnected：蓝牙断开连接事件的回调函数。</div>
        </td>
      </tr>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">init_done</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">stack_init_done_t</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">协议栈初始化完成回调函数，需要在蓝牙协议栈完成之后主动调用传入成功参数AIS_ERR_SUCCESS(0)。</div>
        </td>
      </tr>
    </tbody>
  </table>
</div>

__返回值__

<span data-type="color" style="color:#262626">该函数成功时返回</span><span data-type="color" style="color:#262626"><code>AIS_ERR_SUCCESS</code></span><span data-type="color" style="color:#262626">，错误发生时返回对应的错误编码（breeze_hal_ble.h）中。</span>

### <span data-type="color" style="color:#262626">ble_stack_deinit</span>

<span data-type="color" style="color:#262626">该接口负责协议栈停止和资源销毁等工作。</span>。

__参数__
无。

__返回值__
<span data-type="color" style="color:#262626">该函数成功时返回</span><span data-type="color" style="color:#262626"><code>AIS_ERR_SUCCESS</code></span><span data-type="color" style="color:#262626">，错误发生时返回对应的错误编码（breeze_hal_ble.h）中。</span>

### ble\_send\_notification

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口通过notify方式发送数据。</span></span>

__参数__

| __名称__ | __类型__   | __描述__               |
| :------- | :--------- | :--------------------- |
| p\_data  | uint8\_t\* | 发送数据的buffer地址。 |
| length   | uint16\_t  | 数据长度。             |

__返回值__
<span data-type="color" style="color:#262626">该函数成功时返回</span><span data-type="color" style="color:#262626"><code>AIS_ERR_SUCCESS</code></span><span data-type="color" style="color:#262626">，错误发生时返回对应的错误编码（breeze_hal_ble.h）中</span>。

### <span data-type="color" style="color:rgb(25, 31, 37)"><span data-type="background" style="background-color:rgb(255, 255, 255)">ble_send_indication</span></span>

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口通过indicate方式发送数据。</span></span>

__参数__

| __名称__ | __类型__   | __描述__                                                    |
| :------- | :--------- | :---------------------------------------------------------- |
| p\_data  | uint8\_t\* | 发送数据的buffer地址。                                      |
| length   | uint16\_t  | 数据长度。                                                  |
| txdone   | callback   | 回调处理函数，由HAL实现代码在发送indication数据完毕后调用。 |

__返回值__
<span data-type="color" style="color:#262626">该函数成功时返回</span><span data-type="color" style="color:#262626"><code>AIS_ERR_SUCCESS</code></span><span data-type="color" style="color:#262626">，错误发生时返回对应的错误编码（breeze_hal_ble.h）中</span>。

### ble\_disconnect

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口断开已有的蓝牙连接。该函数断开当前的蓝牙连接，与</span></span><span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>ble_stack_deinit</code></span></span><span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">的区别是，后续还可以进行连接。</span></span>

__参数__

| __名称__ | __类型__ | __描述__                                                     |
| :------- | :------- | :----------------------------------------------------------- |
| reason   | uint8\_t | SDK断开蓝牙连接原因，注意非蓝牙spec规定的断开原因，如Remote User Terminated Connect（0x13）、Connection Accept Timeout Exceeded（0x10）等，需要在实现中对接做一次映射。例如SDK传入AIS\_BT\_REASON\_REMOTE\_USER\_TERM\_CONN(0x00)，在实现的时候映射成BT\_HCI\_ERR\_REMOTE\_USER\_TERM\_CONN(0x13)。 |

__返回值__

无。

### ble\_advertising\_start

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口开启蓝牙广播。</span></span>

__参数__

| __名称__ | __类型__            | __描述__                                                     |
| :------- | :------------------ | :----------------------------------------------------------- |
| adv      | ais\_adv\_init\_t\* | 输入参数adv指定了所需的广播信息，包括flag、设备名、厂商数据段内容。 |

__返回值__
<span data-type="color" style="color:#262626">该函数成功时返回</span><span data-type="color" style="color:#262626"><code>AIS_ERR_SUCCESS</code></span><span data-type="color" style="color:#262626">，错误发生时返回对应的错误编码（breeze_hal_ble.h）中</span>。

### ble\_advertising\_stop

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口停止蓝牙广播。</span></span>

__参数__
无。

__返回值__
<span data-type="color" style="color:#262626">该函数成功时返回</span><span data-type="color" style="color:#262626"><code>AIS_ERR_SUCCESS</code></span><span data-type="color" style="color:#262626">，错误发生时返回对应的错误编码（breeze_hal_ble.h）中</span>。

### <span data-type="color" style="color:rgb(38, 38, 38)">ble_get_mac</span>

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口用于获取蓝牙设备的MAC地址。</span></span>

__参数__

| __名称__ | __类型__   | __描述__                                                     |
| :------- | :--------- | :----------------------------------------------------------- |
| mac      | uint8\_t\* | 用于存储获取到的蓝牙MAC地址，6字节二进制格式：0xAA，0xBB，0xCC，0xDD，0xEE，0xFF（对应MAC地址“AA:BB:CC:DD:EE:FF”） |

__返回值__
<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该函数成功退出是返回</span></span><span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><code>AIS_ERR_SUCCESS</code></span></span><span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">，错误发生时返回Breeze错误编码。</span></span>

## OS HAL列表

OS HAL接口提供以下功能：

- 定时器支持；
- 系统重启接口；
- 系统时间戳获取和睡眠接口；
- Key-Value键值读取接口。

### os\_timer\_new

该接口用于创建定时器。

__参数__

| __名称__ | __类型__         | __描述__                 |
| :------- | :--------------- | :----------------------- |
| timer    | os\_timer\_t\*   | 定时器指针。             |
| cb       | os\_timer\_cb\_t | 定时器超时回调处理函数。 |
| arg      | void\*           | 回调函数函数参数。       |
| ms       | int              | 超时时间。               |

__返回值__
成功时时返回0，否则返回非0值。

### os\_timer\_start

该接口用于启动定时器<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__     | __描述__     |
| :------- | :----------- | :----------- |
| timer    | os\_timer\_t | 定时器指针。 |

__返回值__
成功时返回0，否则返回非0值。

### os\_timer\_stop

该接口用于停止定时器<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__     | __描述__     |
| :------- | :----------- | :----------- |
| timer    | os\_timer\_t | 定时器指针。 |

__返回值__
成功时返回0，否则返回非0值。

### os\_timer\_free

该接口用于销毁定时器资源<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__     | __描述__     |
| :------- | :----------- | :----------- |
| timer    | os\_timer\_t | 定时器指针。 |

__返回值__
成功时返回0，否则返回非0值。

### os\_reboot

该接口用于重启OS<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__
无。

__返回值__
无。

### os\_msleep

该接口触发系统/进程进行睡眠<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__ | __描述__             |
| :------- | :------- | :------------------- |
| ms       | int      | 休眠时长，单位为ms。 |

__返回值__
无。

### os\_now\_ms

该接口用于获取系统当前时间戳（从系统启动开始计数）<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__
无。

__返回值__
系统时间戳，单位为ms。

### os<span data-type="color" style="color:rgb(38, 38, 38)">_kv_set</span>

该接口用于设置永久存储Key-Value键值。<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__     | __描述__            |
| :------- | :----------- | :------------------ |
| key      | const char\* | 键名（字符串）。    |
| value    | const void\* | 键值（任意值）。    |
| len      | int          | 键值长度。          |
| sync     | int          | 预留参数，暂时传入1 |

__返回值__
成功时返回0，否则返回非0值。

### os<span data-type="color" style="color:rgb(38, 38, 38)">_kv_get</span>

该接口用于读取永久存储Key-Value键值。

__参数__

| __名称__    | __类型__     | __描述__         |
| :---------- | :----------- | :--------------- |
| key         | const char\* | 键名（字符串）。 |
| buffer      | void\*       | 键值（任意值）。 |
| buffer\_len | int\*        | 键值长度指针。   |

__返回值__
成功时返回0，否则返回非0值。

### os<span data-type="color" style="color:rgb(38, 38, 38)">_kv_del</span>

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口用于删除永久存储Key-Value键值</span></span>。

__参数__

| __名称__ | __类型__     | __描述__         |
| :------- | :----------- | :--------------- |
| key      | const char\* | 键名（字符串）。 |

__返回值__
成功时返回0，否则返回非0值

### os<span data-type="color" style="color:rgb(38, 38, 38)">_rand</span>

<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">该接口用于产生一个整型的随机值</span></span>。

__参数__
无。

__返回值__
随机有符号整型值。

## 安全相关的HAL 

### sec\_sha256\_init

该接口对sha256模块进行初始化<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__    | __描述__                                      |
| :------- | :---------- | :-------------------------------------------- |
| ctx      | SHA256\_CTX | SHA256结构体指针,资源申请和销毁由调用者负责。 |

__返回值__
无。

### sec\_sha256\_update

计算给定数据的SHA256值

__参数__

| __名称__ | __类型__    | __描述__                       |
| :------- | :---------- | :----------------------------- |
| ctx      | SHA256\_CTX | 初始化返回的SHA256结构体指针。 |
| data     | const BYTE  | 要进行SHA256更新的数据指针。   |
| len      | size\_t     | 数据长度。                     |

__返回值__
无。

### sec\_sha256\_final

sha256模块完成SHA256计算接口<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__    | __描述__                       |
| :------- | :---------- | :----------------------------- |
| ctx      | SHA256\_CTX | 初始化返回的SHA256结构体指针。 |
| hash     | BYTE        | 签名输出的保存数组。           |

__返回值__
无。

### sec\_aes128\_init

该接口对AES128加解密算法流程进行初始化<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__       | __描述__                       |
| :------- | :------------- | :----------------------------- |
| key      | const uint8\_t | AES128算法要求的key输入。      |
| iv       | const uint8\_t | AES128 CBC算法要求输入的iv值。 |

__返回值__
`void`指针，指向初始化后的context，后续加解密流程中使用该返回值

### sec\_aes128\_destroy

销毁AES128加解密算法流程，释放相关资源<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__ | __类型__ | __描述__                      |
| :------- | :------- | :---------------------------- |
| aes      | void\*   | 接口初始化之后返回的context。 |

__返回值__
成功时返回0，否则返回-1。

### sec\_aes128\_cbc\_encrypt

调用该接口进行AES128 CBC加密计算<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__   | __类型__     | __描述__                                                   |
| :--------- | :----------- | :--------------------------------------------------------- |
| aes        | void\*       | 初始化之后返回的context。                                  |
| src        | const void\* | 指针指向需要加密的数据。                                   |
| block\_num | size\_t      | 需要加密的数据块数（16字节为一块，不足16字节按一块计算）。 |
| dst        | void\*       | 解密后数据的存储地址                                       |

__返回值__
成功时返回0，否则返回-1。

### sec\_aes128\_cbc\_decrypt

调用该接口进行AES128 CBC解密计算<span data-type="color" style="color:rgb(38, 38, 38)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。</span></span>

__参数__

| __名称__   | __类型__     | __描述__                                                   |
| :--------- | :----------- | :--------------------------------------------------------- |
| aes        | void\*       | 初始化之后返回的context。                                  |
| src        | const void\* | 指针指向需要加密的数据。                                   |
| block\_num | size\_t      | 需要加密的数据块数（16字节为一块，不足16字节按一块计算）。 |
| dst        | void\*       | 解密后数据的存储地址                                       |

__返回值__
成功时返回0，否则返回-1。
#### 