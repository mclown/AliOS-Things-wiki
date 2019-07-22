# AliOS Things OTA差分工具使用指南

<a name="mPYI6"></a>
# 简介
AliOS Things OTA不仅提供云端一体化的基础的整包升级能力，还能提供更高阶的差分升级能力（对bin文件差分算法压缩率低于35%）。差分升级是将老版本和新版本取差异部分进行增量升级，可以极大的减少下载包的流量，同时能节省存储升级固件的ROM存储空间。<br /> <br />此文档针使用AliOS Things OTA的芯片开发人员，版本基线为github：[https://github.com/alibaba/AliOS-Things.git](https://github.com/alibaba/AliOS-Things.git) 分支：rel_2.1.1。<br /> 
<a name="D7o0z"></a>
# 1. 差分升级基本原理
![image.png](https://img.alicdn.com/tfs/TB1xmMQa4D1gK0jSZFKXXcJrVXa-553-341.png)<br /> 
<a name="PWRB2"></a>
# 2. 本地制作和验证差分包(非AliOS Things用户)
1). 准备好老版本old.bin和新版本new.bin文件<br />2). 下载本地差分命令行工具ota_nbdiff和ota_nbpatch<br />3). 使用如下命令行工具ota_nbdiff制作生成差分包文件diff.bin，并将如下diff.bin文件上传到阿里云物联网平台，填下的版本号为新固件new.bin的版本号，其他升级流程跟阿里云物联网平台整包升级一致。<br />##### ota_nbdiff
old.bin new.bin diff.bin<br />4). 执行如下命令行工具ota_nbpatch将老版本old.bin文件和差分包文件diff.bin还原生成新版本文件new1.bin, 将还原的new1.bin和原始的new.bin文件进行比对一致说明生成差分包正确。<br />##### ota_nbpatch
old.bin new1.bin diff.bin<br /> 
<a name="laRhB"></a>
# 3. 本地制作和验证差分包(AliOS Things用户)
1).下载AliOS Things源码后自带差分工具在目录下：build/cmd/linux32 or linux64<br />##### git clone [https://github.com/alibaba/AliOS-Things.git
-b rel_2.1.1](https://github.com/alibaba/AliOS-Things.git%20-b%20rel_2.1.1)<br />2). 执行如下命令安装 aos-cube 工具（临时测试用，正式版本待发布）：<br />##### sudo pip2 install -i [http://10.101.74.181/](http://10.101.74.181/) --trusted-host 10.101.74.181 -U
aos-cube==0.3.4b3<br />3). 编译AliOS Things源代码产生老版本old.bin和新版本new.bin文件<br />##### aos make
linkkitapp@mk3060 –c config<br />##### aos make<br />4). 使用如下AliOS Things aos-cube命令行工具制作差分包文件，默认会在当前目录生成diff.bin差分包文件，并将如下diff.bin文件上传到阿里云物联网平台，填下的版本号为新固件new.bin的版本号，其他升级流程跟阿里云物联网平台整包升级一致。<br />##### aos ota diff old.bin
new.bin<br />5). 执行如下命令行将差分包文件diff.bin和老版本old.bin文件还原生成新版本文件new1.bin, 将还原的new1.bin和原始的new.bin文件进行比对一致说明生成差分包正确（**这里是模拟板子运行，注意还原校验的原始固件不能超过8M**）。<br />##### aos ota patch old.bin
new1.bin diff.bin