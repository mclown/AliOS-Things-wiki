## 1.介绍
如下图流程所示:AliOS Things OTA提供云端一体化的安全升级服务，与阿里云安全服务器KPM直接对接，在云端完成秘钥及证书管理，数据签名，下发公钥到设备端，设备端完成固件签名验证，整个流程云端一体化提供服务，集成开发及操作非常简单。

![](https://gw.alicdn.com/tfs/TB1Zo3IhuH2gK0jSZJnXXaT1FXa-940-809.png)
## 2.云端
登录：[https://iot.console.aliyun.com/product](https://iot.console.aliyun.com/product)
选择：监控运维---->固件升级---->安全升级；会弹出如下框图，选择对应要启用安全升级的产品，点击"安全升级"使状态为“开”，并点击“复制”，既可以获取公钥；
![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2019/png/109397/1552038316038-a2a2091c-8761-4f54-9e52-930cca8598e0.png#align=left&display=inline&height=647&name=image.png&originHeight=809&originWidth=940&size=45823&status=done&width=752)

## 3.设备端
将步骤2获取的公钥放入AliOS Things：aos\middleware\uagent\ota\ota_core\verify\ota_public_key_config.h中，将固件编译出来，烧录即可；
## 4.触发功能
完成以上3个步骤后，安全升级的配置已完成。如果设备在线，登录云端，进入“监控运维”，选择“固件升级”，找到对应的设备，点击“验证固件”，即可触发和验证安全升级；
