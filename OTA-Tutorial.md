[中文](OTA-Tutorial.zh) | EN

1. Introduction
user target audience: AliOS Things user or developer;
demo app: otaapp
demo board: mk3060
developement environment: linux
code path: https://github.com/alibaba/AliOS-Things branch: rel_2.0.0
cloud platform: https://iot.console.aliyun.com/product
This article introduces the features of AliOS Things OTA; It mainly includes features as below: 

support the whole image upgrade;
support diff image upgrade;
support security upgrade;
note:

if you encounter problems during use, please refer to the troubleshooting steps in Chapter 4 and 5 and the frequently asked questions' answers of Q&A;

2. OTA software framework & workflow
1. OTA software framework
​
​



2. workflow
​
​



① download the code and select the OTA demo app and development board;

② log in the cloud account to get the four elements(pk,ps,dn,ds) and open the firmware upgrade service;

③ if want to enable security upgrade function, obtain the public key from cloud;

④ compile and make different version of firmware, one is low verison another is high version;

⑤ burn the lower version image to the target board, run the demo app then input: ota_app pk dn ds ps;

⑥ upload the high verion image to the cloud when the device goes online;

⑦ trigger cloud ota function, the device will receive the image and verifies integrity of the image;

⑧ the device side completes the image check and enters the system upgrade;

⑨ device reboot and report it's image version to cloud;

3. Operation guide
3.1. Download the code and select the OTA demo app and target board.
Download the AliOS Things code from github and select the OTA demo app: "otapp". The otaapp is designed to test the OTA function. After selection, you need to compile the corresponding firmware according to your target board.

3.2. Log in to the cloud account to get the PK PS DN DS and open firmware OTA service.
The user logs in to the Alibaba IOT cloud platform at https://iot.console.aliyun.com, and obtains the PK, DN, DS, PS; the device gets "PK", "DN", "DS", "PS" can connect to the cloud; If you use the OTA service for the first time, you need to "Maintenance" and open the OTA function.

Note: PK: product key; PS: product secret; DN: device name; DS: device secret;



​
​



After creating the product, click Device --> Add Device and add a device under the created product, as shown below:

​
​



After the device is added, the platform will pop up a device certificate with PK, DN,DS, as shown below:

​
​



PS can get from: Devices ----> Product -----> View, as show below:

​
​



3.3. Get the public key of the secure download from the cloud.
If you want to enable the OTA security upgrade function, please follow the following process: Log in to the IOT cloud ------>IOT Platform----->Maintenance----->Firmware Upgrade----- ->Security Upgrade ------> Product Name ------> Secure upgrade, click the button to select "Activated", and then click "Copy" to get the cloud public key, as shown below:



​
​



User needs to overwrites the obtained public key into the "aos/middleware/uagent/uota/src/verify/ota_public_key_config.h" file, as shown below:

​
​

3.4. Complile two different verisions of image according to the target board.
The selected development board is "mk3060 (board/mk3060)", so type "aos make otaapp@mk3060" in the terminal and wait for the compilation to complete, then compile two different versions of image separately. The version number of the firmware can be obtained from the information printed by the terminal at compile time, such as: "app_version: app-1.0.0-xxxxxx", please remember the version number, . To manually change the version number, you can change app-1.0.0-$(CURRENT_TIME) in the file aos/middleware/common/common.mk.

3.5. Burn low version firmware to target board to run demo app
After completing step 4, go to "aos/out/otaapp@mk3060/binary/" to find the corresponding image "otaapp@mk3060.bin" as shown below:

​
​

Burn the compiled low version "app-1.0.0-xxxxxx" firmware "otaapp@mk3060.bin" file to the target board, restart the board, type the command: "netmgr connect ssid passwd", the "ssid" and "passwd" in the command are the wifi account and password. After the network connection is successful, you will see information such as the IP address, as shown below:



​
​

From the log information, you can see the device name and other information of the device. If the setting is incorrect, the device will not connect successfully. From the log information, you can verify whether the device's PK, DN, DS, etc. are correct.

3.6. After the device is online, upload a high version image to the cloud.
Modify "aos/middleware/common/common.mk" file to change the version number "app-1.0.0-$(CURRENT_TIME)" to "app-2.0.0-$(CURRENT_TME)", then repeat step 4 to generate a image with a higher version: 2.0.0-xxxxxx, as shown below:

​
​



After the compilation is complete, upload otaapp@mk3060.ota.md5.bin to the cloud, pay attention to the correct firmware version, the cloud has rules to check the files such as suffixes, characters, etc.; IOT cloud platform login -----> IoT platform - ------>Maintenance-------->Firmware Upgrade------->New Firmware; the following box will pop up, according to the title with an asterisk in the frame, fill in the content and upload the firmware. the signature algorithm in the figure below is used to ensure the integrity of the firmware, as well as security features, currently supports two method: "md5" and "sha256", we recommend using sha256.



​
​



After the firmware is uploaded, click "Validate Firmware". The following dialog box will pop up, prompting user to select the update method: "Full" or "Differential". The "Full" upgrade mode will push the complete image to the device from the cloud. The "Differential" upgrade mode will push differential increment packagethe to the device. 

When selecting the "Full" upgrade mode, as shown below:

​
​



When selecting the differential upgrade mode, as shown below:

​
​

Note: mk3060 slice size = 64Kb.

3.7. Cloud push image to device side, and do integrity check.
After the firmware download is completed, the device will check the image integrity. If the user selects the security upgrade function, the device will also verifies the image RSA to ensure that the received image is safe and reliable. For details, see the following log information: 



​
​

From the above figure, you can see the sha256 checksum, include MD5 and other authentication information of the firmware itself. If there any check fails, the system will stop upgrading.

3.8. The device side completes the image check and enters the system upgrade.
When the downloaded firmware verification is successful, the system will save the upgrade status and automatically restart, then enter the bootloader to complete the differential image recovery or image copy, complete the firmware upgrade.

3.9. After the device is upgraded successfully, the new version is reported to the cloud.
After the device is successfully upgraded, the system restarts and reports the current firmware version to cloud. Then cloud verifies whether the reported version by device is the same as the version of the cloud. If the versiun is the same, ota succeeds, otherwise ota fails. After the upgrade succeeds, the cloud displays. As shown below:

4.Troubleshooting steps
Cloud troubleshooting: pk ps dn ds, firmware version, upgrade method, etc. ensure correct;
Troubleshoot the connection channel: check whether device is online, and eliminate the connection channel problem;
Download channel troubleshooting: You can run otaapp demo on linux host to see if you can download files from the cloud normally.
Troubleshoot the firmware verification and write flash error of the device: find out the problem by analyzing the log and if you need help, provide a complete log of the device.
5. Q&A
Q:Version mismatch causes upgrade failure(ota cancel, ota version don't match dev version!)
A: The firmware version that has been uploaded in the cloud must be same as the version format of the firmware that has been burned on the device side, and the cloud image version is larger than the device side version. For example, the version of the device: APP-1.0.0-xxxxxx, the version of the cloud: APP-2.0.0-xxxxxx. Manually change the version place: middleware\common\common.mk.

Q:Which is the OTA bin file for uploading the cloud?
A:The OTA bin file used for cloud upgrade may be different from the firmware for end-side offline burning, such as: mk3080 is out/xxxx@mk3080/binary/ota_all.bin and mk3060 is out/xxx@mk3060/binary/ xxxx@mk3060.ota .md5.bin

Q:After OTA successful, the network information (wifi ssid and password) is lost?
A:The reason is that the storage area of the network information is erased during the OTA upgrade process. Solution: Check the partition table of board.c to ensure that each partition does not overlap.

Q: The mk3080 imge can't run when ota bin file compiled in linux environment.
A:This is a known problem. The tool that generates the OTA bin file provided by the original chip company works fine under Windows but the tool under Linux cannot generate the normal OTA bin file format.