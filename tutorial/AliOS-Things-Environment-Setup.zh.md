[EN](AliOS-Things-Environment-Setup) | 中文

# 目录
- [1 系统环境配置](#1-系统环境配置)
- [2 硬件环境准备](#2-硬件环境准备)

# 1 系统环境配置
|Local Tool\OS|Linux|Windows|Mac|
|------|--------|-------|-------|
|AliOS Studio|[AliOS Studio](AliOS-Things-Studio)|[AliOS Studio](AliOS-Things-Studio)|[AliOS Studio](AliOS-Things-Studio)|
|Keil|X|[Create Keil Project](https://dev.iot.aliyun.com/doc/detail/aliosthings?spm=a2c56.pc_iot_community_doc_center.0.0.432652065IZNUi#keilautogen.html)|X|
|IAR|X|[Create IAR Project](https://dev.iot.aliyun.com/doc/detail/aliosthings?spm=a2c56.pc_iot_community_doc_center.0.0.432652065IZNUi#iarautogen.html)|X|
|Docker|[one-key Linux Docker](AliOS-Things-Docker-Environment-Setup#Linux环境开发)|[one-key Windows Docker](AliOS-Things-Docker-Environment-Setup#Windows环境开发)|[one-key Mac Docker](AliOS-Things-Docker-Environment-Setup#Mac环境开发)|
|Native|[use my native Linux](AliOS-Things-Linux-Environment-Setup)|[use my native Windows](AliOS-Things-Windows-Environment-Setup)|[use my native Mac](AliOS-Things-MAC-Environment-Setup)|
   
# 2 硬件环境设置
## MXCHIP MK3060 WiFi Module
- [Hardware Setup](MK3060-Hardware-Setup)

## ESP32 DevKitC 
- [Hardware Information](http://esp-idf.readthedocs.io/en/latest/get-started/get-started-devkitc.html)
- Image download example<br>
python platform/mcu/esp32/esptool_py/esptool/esptool.py --chip esp32 --port /dev/ttyUSBx --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size 4MB  0x10000 /path/to/image
