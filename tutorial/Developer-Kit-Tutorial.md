## IoT案例
### 1, 基于**Developer Kit开发板**快速搭建'实时查询天气'的物联网应用。
* 技术创新点：
   * QR设备配网、钉钉推送设备上线通知
   * 轻量级嵌入式设备 + 阿里云API云市场的天气API
   * 开发简单上手快、思路/方案拓展性强
* 参考[快速搭建文档](https://yq.aliyun.com/articles/720391)  
* 效果展示  
  ![demo_show](https://img.alicdn.com/tfs/TB13NF4iQL0gK0jSZFAXXcA9pXa-296-390.png)

    
   

## 注意
> 本文主要介绍**Developer Kit开发板**相关内容，如果使用**Starter kit开发板**请查看[这里](https://github.com/alibaba/AliOS-Things/wiki/Starter-Kit-Tutorial.zh).  
## 介绍
>职责1：诺行负责开发板所有内容，若涉及到诺行无法解答的AliOS Things相关问题，诺行有责任将其引导到下述OS的钉钉群，或用户自行搜索加入  
>职责2：AliOS Things官方钉钉群支持解答OS及其应用的相关问题，同时还部分支持与阿里云产品打通的端侧问题

* Develoeprkit开发板由[诺行](http://www.notioni.com)负责设计/生产/维护(**阿里提供OS，该板子处于[认证](https://github.com/AITC-LinkCertification/AITC-Manual/wiki/AliOS-Things%E8%AE%A4%E8%AF%81)过程中**)，部分常规开发板资料阿里同学整理如下：
  > **注：** 遗留较多硬件问题且**诺行不愿**在开发板出厂时修复；开发板相关文档较乱；
     * [板子硬件资源](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Developer-Kit-Hardware-Guide)  
     * [硬件自测文档](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Developer-Kit-User-Basic-Operation-Guide)
     * [开发板原理图和生产软件bin文件](http://www.notioni.com/#/source)，恢复出厂设置方法：烧录出厂bin文件  
     * wifi模组固件升级，查看[这里](https://github.com/alibaba/AliOS-Things/wiki/wifi_upgrade_guide.md)确认是否需要升级
     * 熟悉其他诺行提供的开发板资料，若遇到开发板问题请在诺行钉钉群at群主唐浩寻求帮助

* AliOS Things支持该开发板，并提供相关的[应用示例](#应用示例)，由于该开发板**尚未**通过阿里[认证](https://github.com/AITC-LinkCertification/AITC-Manual/wiki/AliOS-Things%E8%AE%A4%E8%AF%81)，亦可查看AliOS Things支持的[更多开发板](https://github.com/alibaba/AliOS-Things/tree/master/board)
* 支持方式
   * 诺行开发板钉钉群（**负责开发板所有内容**，请直接联系群主唐浩）：  
        * 诺行开发板1~技术钉钉群，群号：23193725
        * 诺行开发板2~技术钉钉群，群号：23167859
   * AliOS Things钉钉群 (**负责OS及其相关应用支持**, 进群查看群公告)：
        * AliOS Things-Banana技术交流钉钉群，群号：23130352
        * AliOS Things-技术交流钉钉群，群号：11748797
## 板载硬件资源
![board_res](https://img.alicdn.com/tfs/TB17oEJgQvoK1RjSZFNXXcxMVXa-674-508.png)  
**主要模块(自上而下):**
* 传感器(上图从左到右):
     - 加速度计acc 和 陀螺仪gyro，LSM6DSLTR, vendor:ST
     - 压力presure，BMP280，vendor:bosch
     - 光强light和接近光proximity，LTR-553ALS-WA, vendor:LITE-ON
     - 湿度Humi和温度Temp, SHTC1, vendor:Sensirion
     - IR Detector, PT26-21B/CT, vendor:Everlight 
     - IR Emitter, IR26-61C/L510/R/2G(MI), vendor:Everlight 
     - 磁力计mag，MMC3680KJ，vendor:Memsic
* 摄像头：0.3M Pixels, YL-F170066-0329, vendor:深圳影领  
* TFT LCD: 1.3', 240*240pixels, SPI，H13TS43A 深圳汉禹  
* Audio Codec:  Cortex-M0, 145KB EPROM,12KB SRAM,up to 50MHz, 芯唐电子  
* MCU：STM32L496VGTx, Cortex-M4, 最高主频80Mhz，320KB SRAM,1MB Flash, vendor: ST  
* 按键*3，分别是按键A, M, B
* WIFI模组：BK7231。vendor:Beken  

## AliOS Things
#### 开发前提  
 * [AliOS Things文档入口](https://dev.iot.aliyun.com/doc/detail/aliosthings/3.0.13#index.html)搭建环境  
* 参考[cube安装使用](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-uCube.zh) 和 [AliOS Things 2.1构建与配置](https://github.com/alibaba/AliOS-Things/wiki/AliOS-Things-Build-Configuration.zh)   
#### 应用示例
| **Demos/Examples** | **Comments** | **Status** |
| --------       | -------- | -------- |
| helloworld       | 入门，串口打印log | [文档介绍](https://github.com/alibaba/AliOS-Things/tree/developer/example/helloworld) |
| developerkitgui       | LCD显示 | developer分支 |
| dk.dk_camera       | 拍照，存储，显示 | developer/master分支, [文档](https://github.com/alibaba/AliOS-Things/tree/rel_2.1.0/app/example/dk/dk_camera) |
| dk.dk_audio      | 录音、回放，识别 | developer/master分支, [文档](https://github.com/Notioni/Codes/tree/master/Developer%20Kit/Codec%E5%9B%BA%E4%BB%B6) | 
| qr               | 二维码     | developer分支，功能：如果扫到二维码后，串口会将二维码打印出来。按B键可以在LCD上显示二维码    |
| udataapp               | sensor相关     | developer分支     |
| mqttapp               | mqtt测试    | developer分支     |
| linkkitapp      | 一键配网（受限），建议等2.1正式版本发布  | developer分支  |
| simple_mqtt               | 升级版helloworld     | developer分支，[文档](https://linkdevelop.aliyun.com/device-doc#aos-helloworld.html)     |
| ldapp      | LCD，传感器，云端  | developer分支，[文档](https://github.com/alibaba/AliOS-Things/tree/developer/example/ldapp/README.md)  |
| linkdevelop.env_monitor | 环境监控   | developer分支，[文档](https://linkdevelop.aliyun.com/device-doc#aos-env_monitor.html)  |
| linkdevelop.device_ctrl | 设备控制  | developer分支，[文档](https://linkdevelop.aliyun.com/device-doc#aos-device_control.html)  |
| linkdevelop.hmi  | 人机交互  | developer分支，[文档](https://linkdevelop.aliyun.com/device-doc#aos-hmi.html)  |

>Note: 诺行维护的'PCIe接口 + 外扩4G/NB等模块' 参考[这里](https://github.com/alibaba/AliOS-Things/wiki/developerkit_pcie_page)
## 附录

 