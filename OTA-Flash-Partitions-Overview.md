[中文](OTA-Flash-Partitions-Overview.zh) | EN
1. Introduction
AliOS Things maintains a flash partition table due to its functional requirements. This flash partition table includes the bootloader area, the application area, the OTA TMP area, and the parameters area, as shown in the figure below:



​
​

2. Flash Partition
The next steps describe how to divide the flash space.

get the mcu flash size.
get the mcu bootloader infomation, includes: boot type (dual bank or single bank) and boot address (dual bank bootloader has two boot address);
according step 1 & step 2 to divide the flash:
    1) bootloader boot type is single bank.

​
​

    2) bootloader boot type is dual bank.

​
​

Note:

From the perspective of security, users are suggested to use mcu which support dual bank upgrade mode to support image rollback;
In addition to the above defined flash partition, a lot of chips or modules have some configuration files that need to be written to flash fixed address, so make sure that the above partition can't be overwritten by these configuration files.
Once above partitions are defined, they can't be changed easily, or data loss will be caused; if the customer needs to add a custom partition, make sure insert it at the end of the partition table and not in the middle.
3. Example
Take esp8266 as an example, with a flash size of 2Mbytes, and it supports 1024 * 1024 dual bank upgrade mode:

1. get Flash size:
Flash size = 2M bytes; address range: 0x000000 ~ 0x200000

2. Bootloader information:
support dual bank upgrade mode;
bootloader boot address1 = 0x1000;
bootloader boot address2 = 0x101000;
Application max size = 0x101000 - 0x1000 = 0x100000;

3. Dividing parameters area:
As shown by the document, the address : 0x1FC000 ~ 0x200000 is the configuration file storage area, so parameters1 ~paramters4 can only be partitioned from 0x1FC000 to a low address, that is:

Parameters1: start address: 0x1F6000 size: 0x1000 bytes;

Parameters2: start address: 0x1F7000 size: 0x2000 bytes;

Parameters3: start address: 0x1F9000 size: 0x1000 bytes;

Parameters4: start address: 0x1FA000 size: 0x1000 bytes;

4. Dividing application area and OTA TMP area:
According to step 3, the range of OTA TMP region can only be parameters1 ~ boot address2 : 0x1F6000 - 0x101000 = 0xF5000; Due to dual bank upgrade, the size of application area should be equal to the OTA TMP area;

By the above 4 steps to complete the division of the partion talbe and the flash partition table can find at : aos/board/esp8266/board.c；

