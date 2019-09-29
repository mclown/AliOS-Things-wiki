### 1 环境准备
* 获取代码：[https://github.com/alibaba/AliOS-Things](https://github.com/alibaba/AliOS-Things) 分支：rel_3.0.0
* 选定APP：在AliOS-Things/app/example/中linkkitapp和otaapp都支持OTA功能，本文以linkkitapp为例介绍;
* 选定board: 在AliOS-Things/board/中有很多板子都支持OTA功能，本文以developerkit为例介绍;
* 选定云端平台：阿里云有两个平台支持AliOS Things OTA功能：[物联网平台](http://iot.console.aliyun.com/)或[智能生活开放平台](https://living.aliyun.com/)，本文以物联网平台为例介绍；
### 2 使用流程
AliOS Things 支持Windows和Linux编译，本文以Linux编译环境为例：

- **选择app和board**

输入命令：
aos make distclean /*清除之前配置*/
aos make linkkitapp@developerkit -c config /*配置app为linkkitapp board为developerkit*/

- **选择OTA组件及功能**

输入命令：aos make menuconfig, 如下图：
选择顺序：Middleware  Configuration  --->uAgent Configuration  --->-*- OTA Features  ---> OTA Features
![image.png](https://gw.alicdn.com/tfs/TB1YE3ZhuL2gK0jSZFmXXc7iXXa-957-771.png)

相关功能介绍如下：

    [ ]   OTA Secure Downloading Mode        /*默认支持http下载，选中此项将支持https下载模式*/ 
    [ ]   OTA via uAgent                     /*默认不支持uAgent模式升级，选中支持uAgent方式升级*/
    [ ]   RSA Verify Support                 /*默认不支持安全升级，选中支持安全升级*/
    (5)   OTA Download Retry Count           /*默认网络异常重试5次，可以根据需要自定义次数*/
    (20000) OTA Download Timeout(ms)         /*OTA下载过程监控时间默认是20s,支持自定义*/
    (512) OTA Download Block Size(bytes)     /*OTA下载时获取数据buf大小默认是512字节，支持自定义*/
    (1024) OTA write flash cache size(bytes) /*OTA写flash缓存大小默认是1k，支持自定义*/
    [ ]   BLE OTA Support                    /*BLE OTA功能选项*/
完成配置后保存退出

- **编译固件**

编译命令：aos make 编译完成后，生成的固件在out/linkkitapp@developerkit/binary目录下，如下图：
![image.png](https://gw.alicdn.com/tfs/TB1Pa71hET1gK0jSZFrXXcNCXXa-696-234.jpg)

linkkitapp@developerkit.bin烧录到板子上，linkkitapp@developerkit_ota.bin用于上传云端，但上传云端固件的版本号要求高于烧录在板子上的固件版本号，因此需要生成一个高版本的固件；

- **固件版本号更改及云端操作**

更改build/build_rules/aos_target_config.mk文件中的app-1.0.0-$(CURRENT_TIME)为app-2.0.0-$(CURRENT_TIME)编译，如下图：
![image.png](https://gw.alicdn.com/tfs/TB1MG34hAT2gK0jSZPcXXcKkpXa-929-741.png)

复制如上图标记的版本号，登录[物联网平台](http://iot.console.aliyun.com/)平台，按如下图顺序操作
![image.png](https://gw.alicdn.com/tfs/TB19Ow1hEH1gK0jSZSyXXXtlpXa-1881-835.png)

点击新增固件后，如下图:

![image.png](https://gw.alicdn.com/tfs/TB1bgZ2hET1gK0jSZFrXXcNCXXa-771-808.png)

点击确定后，选择验证固件即可开始固件升级；升级结果可以点击“查看”获取详细结果；物联网平台的OTA操作,可参考文档[阿里云物联网平台固件升级文档](https://help.aliyun.com/document_detail/58328.html); 智能生活平台的OTA操作,可参考[阿里云智能生活开放平台固件升级文档](https://living.aliyun.com/doc#fxvw5z.html)；

注：在做OTA之前确保设备端已连接云端

附件:
1. 安全升级
* [**OTA安全升级**](https://yuque.antfin-inc.com/kqoe59/agqw03/nzqe07)
2. 差分升级
* [**AliOS Things OTA差分工具使用指南**](https://yuque.antfin-inc.com/kqoe59/wmmz9s/hp0c1c)

