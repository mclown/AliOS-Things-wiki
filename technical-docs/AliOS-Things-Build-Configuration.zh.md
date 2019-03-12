[EN](AliOS-Things-Build-Configuration) | 中文

# AliOS Things 2.1构建与配置

## 构建步骤

AliOS Things 2.1引入了全新的, 基于menuconfig的配置系统，构建过程更新为“先配置，再构建”两个步骤：

```
# 通过图形界面配置，然后构建
$ aos make menuconfig
$ aos make

# 配置时指定app、board，先生成最简配置，然后构建
$ aos make appname@boardname -c config && aos make

# 清除构建目录，不清除配置
$ aos make clean

# 清除构建目录和配置
$ aos make distclean
```

## 在配置系统中添加新组件

当添加新组件时，需要同时为组件开发配置文件“Config.in”。组件配置文件由组件配置选项和组件内配置两部分构成，约定格式为：
```
config PREFIX_COMPNAME    # 组件配置选项
    bool "Prompt Info"    # 配置选项类型，双引号定义该选项在menuconfig中显示名称
    help
        ...               # 配置选项帮助

if PREFIX_COMPNAME
config COMPNAME_CONFIG_XXX
    ...                   # 组件内配置
endif
```

配置选项命名基本要求：
* 必须唯一
* 全部大写
* 组件名包含“-”号要替换成下划线“_”(C语言宏定义限制)

### arch Config.in文件编写
arch Config.in添加规范如下（以armv7m为例）：
```
config AOS_ARCH_ARMV7M            # 定义组件配置选项
    bool                          # 配置选项类型
    help
      arch for armv7m             # 配置选项帮助

if AOS_ARCH_ARMV7M
# Configurations for arch armv7m  # 如有必要，定义更多组件内配置选项
endif
```

Arch组件配置选项命名规范：
* 使用前缀“AOS_ARCH_” + 组件NAME（省略NAME中包含“arch_”前缀）
* 遵守配置选项命名基本要求

### mcu Config.in文件编写
mcu Config.in添加规范如下（以aamcu_demo为例）：
```
config AOS_MCU_AAMCU_DEMO            # 定义组件配置选项
    bool                             # 配置选项类型
    select AOS_ARCH_ARMV7M           # 依赖特定arch
    select ...                       # 依赖其他组件
    help
      driver & sdk for platform/mcu aamcu_demo     # 配置选项帮助

if AOS_MCU_AAMCU_DEMO
# Configurations for mcu aamcu_demo  # 如有必要，定义更多组件内配置选项
endif
```

mcu组件配置选项命名规范：
* 使用前缀“AOS_MCU_” + 组件NAME（省略NAME中包含“mcu_”前缀）
* 同arch配置选项命名规范

### board Config.in文件编写
board Config.in添加规范如下（以aaboard_demo为例）：

```
config AOS_BOARD_AABOARD_DEMO    # 定义组件配置选项
    bool "AABOARD_DEMO"          # 配置选项类型，双引号定义该选项显示名称
    select AOS_MCU_AAMCU_DEMO    # 依赖特定mcu
    select ...                   # 依赖其他组件
    help
      ...                        # 配置选项帮助
    
if AOS_BOARD_AABOARD_DEMO
# Configurations for board aaboard_demo

# "BSP SUPPORT FEATURE"          # 硬件支持的能力
config BSP_SUPPORT_UART
    bool
    default y
...
endif
```

board组件配置选项命名规范：
* 使用前缀“AOS_BOARD_” + 组件NAME（省略NAME中包含“board_”前缀）
* 同arch配置选项命名规范

### 将board加入系统配置菜单
新增board及其配置文件Config.in之后，需要进一步更新“board/Config.in”，将新board加入系统配置菜单。仿照现有格式为新board添加配置项（以aaboard_demo为例）：

```
source "board/aaboard_demo/Config.in"           # 引用board配置文件
if AOS_BOARD_AABOARD_DEMO                       # 如果board组件被启用
    config AOS_BUILD_BOARD                      
        default "aaboard_demo"                  # 为AOS_BUILD_BOARD赋值，与board目录名一致

    source "platform/mcu/aamcu_demo/Config.in"  # 引用mcu配置文件
    source "platform/arch/arm/armv7m/Config.in" # 引用arch配置文件
endif
```

注意：“AOS_BUILD_BOARD”默认值必须与配置命令行（aos make app@board -cconfig）输入的board保持一致。

### app Config.in文件编写
app Config.in文件编写规范（以helloworld为例）：

```
config AOS_APP_HELLOWORLD              # 定义组件配置选项
    bool "HelloWorld"                  # 配置选项类型，双引号定义该选项显示名称
    select AOS_COMP_OSAL_AOS           # 依赖其他组件
    help
        Hello World                    # 配置选项帮助

if AOS_APP_HELLOWORLD
# Configurations for app helloworld    # 如有必要，定义更多组件内配置选项
endif
```

app组件配置选项命名规范：
* 使用前缀“AOS_APP_” + 组件NAME（省略NAME中包含“app_”前缀）
* 同arch配置选项命名规范

### 将app加入系统配置菜单
与新增Board类似：

* 如果新增example在“app/exampe”目录下，编辑“app/exampe/Config.in”
* 如果新增example在“app/profile”目录下，编辑“app/profile/Config.in”

例如：

```
source "app/example/helloworld/Config.in"   # 引用app配置文件
if AOS_APP_HELLOWORLD                       # 如果app组件被启用
    config AOS_BUILD_APP
        default "helloworld"                # 为AOS_BUILD_APP赋值，与app目录一致
endif
```

注意：
* AOS_BUILD_APP默认值必须与配置命令行（aos make app@board -cconfig）输入的app保持一致

## 支持默认配置文件
用户可以将常用的配置文件存储为默认配置，在构建时直接载入。当前支持的机制是把配置文件保存到“build/configs”目录，配置文件必须以“_defconfig”结尾：

```
# 列出所有配置文件
$ aos make list-defconfig

# 加载默认配置文件
$ aos make <configname>_defconfig
```

