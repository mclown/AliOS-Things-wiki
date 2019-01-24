# 一、电源管理框架简介
电源管理框架的目的在于节约CPU的功耗。传统上，当操作系统处于空闲状态时，比如所有用户任务和系统任务处于阻塞状态，将执行idle task。idle
task的通常做法是一个while(1)空循环，从汇编视角看是不断执行跳转指令，也就是说当操作系统空闲时，CPU将处于空转状态。使能电源管理框架后，当系统进入idle
task后，将设置CPU进入低功耗状态，从而节省CPU的功耗。

![](https://github.com/liano1987/picture/blob/master/pwrmgmt.png)

AliOS Things电源管理框架具有如下特点：

* （1）使用对应用透明

    应用配置电源管理框架并添加初始化代码后，整个框架的运行对应用透明，用户无需为了支持电源管理框架而修改应用代码。

* （2）支持多级低功耗状态

    在某些MCU上，根据不同的节电程度和唤醒时间分为多级睡眠，电源管理框架提供了对该特性的支持，在进入低功耗状态时将根据睡眠时间和节电程度选择最佳睡眠等级。

* （3）支持tickless机制

    当MCU决定进入低功耗状态时，将关闭系统tick中断，并在醒来的时候恢复系统tick中断并补偿睡眠过程中丢失的tick数。这种策略通过减少系统时钟中断来最大程度降低系统空闲时的功耗。

* （4）支持精简的低功耗模式

    当MCU进入低功耗后，不关闭系统tick中断，系统tick也能唤醒系统。它的优点是实现简单，但当系统长时间空闲时，由于系统时钟频繁唤醒系统，不利于节能。

**在某基于nrf52832 MCU的开发板上测试电源管理框架的运行效果如下**

在普通运行模式下nrf52832
MCU的平均运行电流在4mA左右，在添加电源管理模块后MCU的电流测试如下表所示：

| **测试项**                                          | **平均电流** | **说明**                             |
|-----------------------------------------------------|--------------|--------------------------------------|
| 低功耗状态                                          | 2.06uA       |                                      |
| BLE广播态功耗（开启低功耗模块,广播intervel 100ms）  | 120uA        | 电压3v，发送负载21字节，TX功率0dBm。 |
| BLE广播态功耗（开启低功耗模块,广播intervel 1000ms） | 14.7uA       | 电压3v，发送负载21字节，TX功率0dBm。 |
| BLE广播态功耗（开启低功耗模块,广播intervel 2000ms） | 8.1uA        | 电压3v，发送负载21字节，TX功率0dBm。 |

从测试结果可以看出，在对功耗敏感的系统上，比如依靠电池供电的系统，非常有必要使用电源管理框架，它可显著降低系统功耗，增加系统待机时间。

# 二、应用配置（为应用添加低功耗支持）
应用若要使用电源管理框架，需进行如下配置：

* （1）应用目录的.mk文件中添加对电源管理模块的依赖，示例：

    `GLOBAL_DEFINES += RHINO_CONFIG_CPU_PWR_MGMT=1`

    `$(NAME)_COMPONENTS := rhino/pwrmgmt`

* （2）在应用初始化函数中（比如`application_start(int argc, char
\*argv[])`）调用电源管理模块初始化函数。

    `cpu_pwrmgmt_init();`

# 三、示例应用（app/example/pwr_test）
目前AliOS Things
2.0版本在developerkit和PCA10040平台上对电源管理框架进行了适配，可用如下命令编译示例应用并下载到develoerkit上运行：

`aos make pwr_test\@developerkit`

`aos upload pwr_test\@developerkit`

示例应用创建了2个任务demo1和demo2。demo1的主要逻辑是一个while循环：count1增1，同时打印count1和g_idle_count[0]的值，然后睡眠1秒。demo2的主要逻辑也是一个while循环：count2增1，同时打印count2的值，然后睡眠2秒。

其中g_idle_count[0]是一个全局变量，idle任务在执行时会累加该值。

若没有开启低功耗模块，那么当demo1和demo2处于睡眠状态时，idle任务持续执行，g_idle_count[0]不断增加。输出示例如下：

count1 = 0, idle = 0

count2 = 0

count1 = 1, idle = 2347298

count1 = 2, idle = 4693421

count2 = 1

count1 = 3, idle = 7036926

count1 = 4, idle = 9383049

count2 = 2

count1 = 5, idle = 11726554

count1 = 6, idle = 14072465
注：如果前后两次打印g_idle_count[0]没有增加，说明系统中有任务一直在运行导致idle任务没有运行。通常是串口采用轮询输入引起的，可修改为中断输入或去掉对CLI模块的依赖（应用.mk文件中$(NAME)_COMPONENTS行去掉CLI）。

开启低功耗时，当demo1和demo2处于睡眠状态时，idle任务执行g_idle_count[0]增1后，调用`cpu_pwr_down()`进入低功耗状态。因此系统每次进入空闲状态，g_idle_count[0]只增加1。输出示例如下：

count1 = 0, idle = 0

count2 = 0

count1 = 1, idle = 1

count2 = 1

count1 = 2, idle = 3

count1 = 3, idle = 4

count2 = 2

count1 = 4, idle = 6

count1 = 5, idle = 7

说明：如果条件允许，直接测试功耗，比如测试MCU的电流，效果更直观。

# 四、电源管理框架的适配
由于电源管理框架的运行依赖于硬件能力，因此在适配时首先要分析目标硬件是否有能力支持，然后要基于硬件能力为电源管理框架提供相关驱动。

## 4.1 硬件要求
要想支持电源管理框架，目标MCU需要支持如下特性：

（1）至少支持一种低功耗模式。在该低功耗模式下，RAM和寄存器的值能够被维持。

（2）在低功耗模式下，存在可用的定时器，且该定时器能唤醒系统。在tickless机制下，该定时器用于计算低功耗时间，以补偿系统时钟。

## 4.2 适配接口
为了支持电源管理模块需完成如下接口适配：

| **适配接口**       | **功能说明**                                                                                                       |
|--------------------|--------------------------------------------------------------------------------------------------------------------|
| `board_cpu_pwr_init` | 初始化CPU的电源管理能力，比如注册CPU电源状态设置函数，注册CPU电源管理能力，注册唤醒延迟时间，注册唤醒/计时定时器。 |
| `cpu_cstate_set_t`   | 设置CPU的低功耗状态                                                                                                |
| `systick_suspend`    | 挂起系统时钟，系统时钟在低功耗状态下停止运行                                                                       |
| `systick_resume`     | 恢复系统时钟                                                                                                       |
| `one_shot_timer_t`   | 低功耗下运行的唤醒/计时定时器。在低功耗下的计时，用于退出低功耗状态时补偿系统时钟。                                |

注：可参考developerkit和PCA10040平台上的适配示例（pwrmgmt_hal目录）。

在适配过程中用户可以调用如下接口：

| **可用接口**                   | **功能概述**                         |
|--------------------------------|--------------------------------------|
| `cpu_pwr_node_init_static`       | 初始化CPU节点                        |
| `cpu_pwr_node_record`            | 注册CPU节点                          |
| `cpu_pwr_c_state_capability_set` | 设置CPU支持的低功耗模式              |
| `cpu_pwr_c_state_latency_save`   | 设置某个指定低功耗状态的唤醒延迟时间 |
| `tickless_one_shot_timer_save`   | 注册支持tickless机制的定时器         |
| `cpu_pwr_c_method_set`           | 注册CPU状态设置函数                  |

##4.3 接口具体说明
* （1）board_cpu_pwr_init示例调用说明：
调用`cpu_pwr_node_init_static/cpu_pwr_node_record`配置并记录CPU核信息。
调用`cpu_pwr_c_method_set`设置CPU的状态设置函数；
调用`cpu_pwr_c_state_capability_set`设置CPU支持的电源状态，其中C0表示运行态，如果只支持一种低功耗，那么再配置C1即可。
调用`cpu_pwr_c_state_latency_save`设置退出Cx的延时，比如退出C1低功耗状态的延时；
调用`tickless_one_shot_timer_save`注册低功耗下的时钟；
调用`tickless_c_states_add`配置Cx支持tickless策略。

如果适配的平台为单核且只支持1种低功耗状态，通常可直接使用示例代码。当然，最好检查一下tickless_one_shot_timer_save中注册的低功耗定时器是否是你定义的。
* （2）cpu_cstate_set_t
设置CPU低功耗状态的函数指针。
每条分支表示进入该状态需要调用的函数，比如：
`case CPU_CSTATE_C0:
    break;
case CPU_CSTATE_C1:
    __WFI();
    break;`

说明：C0为运行态，在单核下，通常由中断从低功耗唤醒，不需要调用函数，因此这里直接break即可；C1映射CPU的一种低功耗状态，通常需要调用函数才能进入，比如Cortex M4下调用WFI指令进入。

该函数指针对应的函数为：
static pwr_status_t board_cpu_c_state_set(uint32_t cpuCState, int master)


# 五、从FreeRTOS tickless切换为AliOS Things电源管理框架
FreeRTOS tickless机制主要在函数vPortSuppressTicksAndSleep中实现。其主要流程如下：
![](https://github.com/liano1987/picture/blob/master/freertos%20tickless.png)
AliOS Things电源管理框架也实现了tickless机制，但在实现上采用了更好的架构：把通用部分放在框架中处理，比如时钟补偿；为唤醒时钟定义one_shot_timer_t结构体，对低功耗时钟进行独立封装。对AliOS Things的用户来说，无需在一个函数中处理所有事情，只需为电源管理框架提供平台相关的HAL层函数即可。AliOS Things电源管理框架的主要流程为：
![](https://github.com/liano1987/picture/blob/master/alios%20pwrmgmt.png)
上图上半部分为低功耗框架部分，执行通用代码，下半部分为hal层函数为框架提供平台相关功能。
与低功耗时钟相关的封装在one_shot_timer_t结构内的函数中。one_shot_seconds_max函数返回低功耗定时器最大定时时间，相当于返回FreeRTOS下的xMaximumPossibleSuppressedTicks。one_shot_start为启动低功耗定时器，该函数的入参为预期睡眠时间，即把FreeRTOS下配置并启动低功耗时钟的相关代码封装在这里。one_shot_stop函数停止低功耗时钟，并返回低功耗经历的时间，即把FreeRTOS下停止低功耗时钟，计算低功耗下经历时间的代码封装在这个函数中。
systick_suspend/ systick_resume为挂起/恢复系统时钟，即把FreeRTOS下系统时钟挂起/恢复的代码段封装在这里。
cpu_cstate_set_t指向的函数（board_cpu_c_state_set）用于设置CPU状态，即把FreeRTOS下进入低功耗的函数调用封装在这里。
