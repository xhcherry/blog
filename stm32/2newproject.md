# 标准库新建工程

## STM32的开发方式

目前stm32的开发方式主要有基于寄存器的方式、基于标准库的方式（库函数的方式）、基于HAL库的方式

基于库函数的方式是使用ST官方提供的封装好的函数，通过调用这些函数来间接地配置寄存器

基于HAL库的方式可以用图形化界面快速配置STM32,但这种方式隐藏了底层逻辑

## 库函数文件夹

百度下载库函数包STM32F10x_StdPeriph_Lib_V3.6.0.zip

如果使用的是keil5 v6，则需要下载STM32Cube_FW_F1_V1.8.0.zip修正core_cm3.c提示的错误

具体是删除core_cm3.c文件，使用STM32Cube_FW_F1_V1.8.0\Drivers\CMSIS\Include的core_cm3.h替换旧的core_cm3.h，并添加cmsis_armclang.h，cmsis_version.h，cmsis_compiler.h到core_cm3.h的目录

新建工程选择stm32f103c8即可

工程目录创建Library，User，Start文件夹

将STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x\startup\arm目录下的文件复制到Start

STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x中的.h and .c 文件放到Start

将STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\CoreSupport中文件放到Start

将STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\STM32F10x_StdPeriph_Driver\src目录下所有放到Library

将STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\STM32F10x_StdPeriph_Driver\inc目录下所有放到Library

STM32F10x_StdPeriph_Lib_V3.6.0\Project\STM32F10x_StdPeriph_Template目录下main.c stm32f10x_conf.h stm32f10x_it.c stm32f10x_it.h放到User

回到keil将上述文件添加至工程组

点击魔术棒按钮，打开工程选项，在c/c++里，找到Include Paths栏，点击右边的三个点的按钮，然后再点击新建路径，然后再点三个点的按钮，把start，User，Library的路径添加进来

C/C++里的Define里添加USE_STDPERIPH_DRIVER

Debug里的Use使用STLink，Use的setting中选择reset and run

## 新建工程启动文件选择

对应STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x\startup\arm内的文件

![](https://pic.xhcheats.cn/assets/2023/12/23/030552.png)