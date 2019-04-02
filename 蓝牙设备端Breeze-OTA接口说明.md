<a name="8814759b"></a>

## 1.简介

Breeze协议提供基于蓝牙链路连接阿里云的安全通道和服务, breeze OTA是在其基础上实现的BLE升级功能。用户移植需要对接的接口和数据结构有:上层应用接口，底层驱动对接接口和bootloader对接数据结构。
<a name="ad0f07d2"></a>

## 2.上层应用对接API

**接口名称：**

```c
int ota_breeze_service_init(ota_breeze_service_manage_t* ota_manage);
```

**参数：**

| 名称       | 类型                         | 描述                                          |
| ---------- | ---------------------------- | --------------------------------------------- |
| ota_manage | ota_breeze_service_manage_t* | 初始化ota信息，包括版本号，消息接收回调函数等 |

**类型介绍：**

```
typedef struct _ota_ble_service_dat{
    unsigned char is_ota_enable; /*0:disable ota; 1:enable ota*/
    ota_breeze_version_t verison;/*os version:used to version verify*/
    ota_breeze_get_data_t get_dat_cb;/* breeze to ota send message callback function*/
}ota_breeze_service_manage_t;
```

**返回值：**<br />成功：0；失败：-1.<br />详细内容参考：`ota_ble/inc/ota_breeze_export.h`
<a name="26db0f0c"></a>

## 3.底层驱动对接API

**接口名称：**

```
uint32_t hal_flash_erase_sector_size(void);
```

**参数：**<br />无<br />**返回值：**<br />返回芯片flash最小擦写单元大小；

**接口名称：**

```
int32_t hal_flash_erase(hal_partition_t pno, uint32_t off_set, uint32_t size);
```

**参数：**

| 名称    | 类型            | 描述                   |
| ------- | --------------- | ---------------------- |
| pno     | hal_partition_t | flash 分区ID           |
| off_set | uint32_t        | 相对分区起始地址偏移量 |
| size    | uint32_t        | 擦写大小               |

**返回值：**<br />成功：0；失败：-1.

**接口名称：**

```
int32_t hal_flash_read(hal_partition_t pno, uint32_t* poff, void* buf, uint32_t buf_size);
```

**参数：**

| 名称     | 类型            | 描述                   |
| -------- | --------------- | ---------------------- |
| pno      | hal_partition_t | flash 分区ID           |
| poff     | uint32_t*       | 相对分区起始地址偏移量 |
| buf      | void*           | 读取的数据数组         |
| buf_size | uint32_t        | 读取的数据长度         |

**返回值：**<br />成功：已经读取的数据长度；失败：-1.

**接口名称：**

```
int32_t hal_flash_write(hal_partition_t pno, uint32_t* poff, const void* buf, uint32_t buf_size);
```

**参数：**

| 名称     | 类型            | 描述                   |
| -------- | --------------- | ---------------------- |
| pno      | hal_partition_t | flash 分区ID           |
| poff     | uint32_t*       | 相对分区起始地址偏移量 |
| buf      | const void*     | 写入数据数组           |
| buf_size | uint32_t        | 写入数据长度           |

**返回值：**<br />成功：已经写入的数据长度；失败：-1.

**接口名称：**

```
hal_logic_partition_t *hal_flash_get_info(hal_partition_t pno);
```

**参数：**

| 名称 | 类型            | 描述         |
| ---- | --------------- | ------------ |
| pno  | hal_partition_t | flash 分区ID |

**返回值：**<br />成功：flash分区信息：分区起始地址，长度等；失败：NULL.<br />详细内容参考：`include/aos/hal/flash.h`
<a name="c77d880e"></a>

## 4. Bootloader对接数据结构

升级完成后，系统会将固件信息等保存flash参数区，AliOS Things 保存在参数分区1；Bootloader 需要读取保存的参数，完成固件的copy和替代，实现升级；

```
typedef struct
{
    unsigned int settings_crc32;   /*settings crc32 value*/
    unsigned int ota_flag;         /*flag ota status*/
    unsigned int src_addr;         /*image storage address*/
    unsigned int dest_addr;        /*iamge run address*/
    unsigned int image_size;       /*image size*/
    unsigned int image_crc32;      /*image crc32*/
    unsigned int image_has_copy_len;   /*remember the copy len for bootloader off line*/
    unsigned char reserved_buf1[RESERVED_SIZE1]; /*reserved*/
    /*backup image include store addr, recovery bank addr, image size and image crc32*/
    unsigned int  backup_store_addr;
    unsigned int  backup_dest_addr;
    unsigned int  backup_image_size;
    unsigned int  backup_image_crc32;
    unsigned int  backup_has_copy_len;           /*remember the copy len for off line*/
    unsigned char reserved_buf2[RESERVED_SIZE2]; /*reserved*/
    /*down_loader image parameters bootloader don't need to care*/
    unsigned int break_point_offset;                   /*breakpoint save when ota processing.*/
    unsigned char version_store_buf[VERSION_BUF_SIZE]; /*version save*/
    unsigned short image_info_crc16;                   /*image info crc value*/
    unsigned char bin_type;                            /*Image type*/
    unsigned char reserved_buf3[RESERVED_SIZE3];
} ota_settings_t;
```

