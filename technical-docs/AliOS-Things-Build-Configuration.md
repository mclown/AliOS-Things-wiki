EN | [中文](AliOS-Things-Build-Configuration.zh)

# AliOS Things 2.1 Build and Configuration

## Build

AliOS Things 2.1 supports to configure a project with "menuconfig", the build steps are: configure and build:

```
# Customize project configs and build
$ aos make menuconfig
$ aos make

# Generate minimal project configs with "appname, boardname" and build
$ aos make appname@boardname -c config && aos make

# Clean build output, but don't clean project configs
$ aos make clean

# Clean build output and project configs
$ aos make distclean
```

## Add Config file for new component

While adding a new component to system, the config file "Config.in" is required for it as format:
```
config PREFIX_COMPNAME    # component config option
    bool "Prompt Info"    # type of config option，and prompt for it
    help
        ...               # help message

if PREFIX_COMPNAME
config COMPNAME_CONFIG_XXX
    ...                   # more configs for component
endif
```

Convention of config option name:
* The name must be unique
* All letters must be CAPITAL 
* Any "-" muse be replaced with "_" (Keep Compatible with C Macros)

### Create Config.in for "arch"
Sample for arch Config.in (eg. armv7m):
```
config AOS_ARCH_ARMV7M            # arch component config option
    bool                          # type of the option, no prompt for it
    help
      arch for armv7m             # help message

if AOS_ARCH_ARMV7M
# Configurations for arch armv7m  # add more config options for arch if needed
endif
```

Convention of arch config option:
* Use prefix "AOS_ARCH_" + "NAME" of arch component (omit "arch_" in NAME)
* Follow other convention of config option name

### Create Config.in for "mcu"
Sample for mcu Config.in (eg. aamcu_demo):
```
config AOS_MCU_AAMCU_DEMO            # mcu component config option
    bool                             # type of the option, no prompt for it
    select AOS_ARCH_ARMV7M           # depends on arch component
    select ...                       # depndds on other component
    help
      driver & sdk for platform/mcu aamcu_demo     # help message

if AOS_MCU_AAMCU_DEMO
# Configurations for mcu aamcu_demo  # add more config options for mcu if needed
endif
```

Convention of mcu config option:
* Use prefix "AOS_MCU_" + "NAME" of mcu component (omit "mcu_" in NAME)
* Follow other convention of config option name

### Create Config.in for "board"
Sample for board Config.in (eg. aaboard_demo):

```
config AOS_BOARD_AABOARD_DEMO    # board component config option
    bool "AABOARD_DEMO"          # type of the option, and prompt for it
    select AOS_MCU_AAMCU_DEMO    # depends on mcu component
    select ...                   # depndds on other component
    help
      ...                        # help message
    
if AOS_BOARD_AABOARD_DEMO
# Configurations for board aaboard_demo

# "BSP SUPPORT FEATURE"          # hardware support
config BSP_SUPPORT_UART
    bool
    default y
...
endif
```

Convention of board config option:
* Use prefix "AOS_BOARD_" + "NAME" of board component (omit "board_" in NAME)
* Follow other convention of config option name

### Add new board to menuconfig

After added Config.in file for board, it's required to add the board into "menuconfig" as below (eg. aaboard_demo):

```
source "board/aaboard_demo/Config.in"           # source board Config.in file
if AOS_BOARD_AABOARD_DEMO                       # if the board is enabled:
    config AOS_BUILD_BOARD                      
        default "aaboard_demo"                  # set AOS_BUILD_BOARD = board dir name

    source "platform/mcu/aamcu_demo/Config.in"  # source mcu Config.in file
    source "platform/arch/arm/armv7m/Config.in" # source arch Config.in file
endif
```

Note：The value of "AOS_BUILD_BOARD" must be same with the "board" from command line（aos make app@board -cconfig).

### Create Config.in for "app"
Sample for app Config.in (eg. helloworld):

```
config AOS_APP_HELLOWORLD              # app component config option
    bool "HelloWorld"                  # type of the option, and prompt for it
    select AOS_COMP_OSAL_AOS           # depends on other component
    help
        Hello World                    # help message

if AOS_APP_HELLOWORLD
# Configurations for app helloworld    # add more config options if needed
endif
```

Convention of app config option:
* Use prefix "AOS_APP_" + "NAME" of board component (omit "app_" in NAME)
* Follow other convention of config option name

### Add new app to menuconfig
It's similiar to add new board:

* If the app resides in "app/example", add it to "app/exampe/Config.in"
* If the app resides in "app/profile", add it to "app/profile/Config.in

For example:

```
source "app/example/helloworld/Config.in"   # source app Config.in
if AOS_APP_HELLOWORLD                       # if the app is enabled:
    config AOS_BUILD_APP
        default "helloworld"                # set AOS_BUILD_APP = app dir name
endif
```

Note：The value of "AOS_BUILD_APP" must be same with the "app" from command line（aos make app@board -cconfig).

## Support defconfig files

It's allowed to store user defined configs as defconfig files and reuse them from new builds. Just copy your configs to "build/configs" and name it with `<config_name>_defconfig`: 

```
# List all defconfig files
$ aos make list-defconfig

# Load the defconfig file
$ aos make <configname>_defconfig
```

