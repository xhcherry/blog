# EXTI中断示例程序（对射式红外传感器&旋转编码器计次）

## 旋转编码器简介
旋转编码器：用来测量位置、速度或旋转方向的装置，当其旋转轴旋转时，其输出端可以输出与旋转速度和方向对应的方波信号，读取方波信号的频率和相位信息即可得知旋转轴的速度和方向

类型：机械触点式/霍尔传感器式/光栅式

1.下面的是一种最简单的编码器样式，这里使用的也是对射式红外传感器来测速的，为了测速还需配合一个光栅编码盘（银色圆圈），当这个编码盘转动时，红外传感器的红外光就会出现遮挡、透过、遮挡、透过这样的现象，对应模块输出的电平就是高低电平交替的方波，方波的个数代表了转过的角度，方波的频率表示转速，我们就可以用外部中断来捕获这个方波的边沿，以此来判断位置和速度，不过这个模块只有一路输出，正转反转输出波形没法区分，所以这种测试方法只能测位置和速度，不能测量旋转方向，为了进一步测量方向，我们就可以用后面的几种编码器。

![](https://pic.xhcheats.cn/assets/2024/01/10/005342.png)
![](https://pic.xhcheats.cn/assets/2024/01/10/005544.png)

2.如下是我们接下来将要用过的旋转编码器，左边是外观，右边是内部拆解的结构；可以看到内部是用金属触电进行通断的，所以它是一种机械触电式编码器，左右是两部分开关触电；中间银色圆形金素片为一个按键，这个旋转编码器的轴是可以按下去的，这种编码器一般是用来进行调节的，比如音响调节音量，因为它是触电接触的形式，所以不适合电机这种高速旋转的地方，另外三种都是非接触的形式，可以用于电机测速（电机测速在电机驱动的应用中还是很常见的）

![](https://pic.xhcheats.cn/assets/2024/01/10/005629.png)

下面为详细讲解旋转编码器的硬件部分：

金属触电

![](https://pic.xhcheats.cn/assets/2024/01/10/005716.png)

内侧的两根细的触电都是和中间的引脚c连接的，外侧触电一个连接A，一个连接B。

![](https://pic.xhcheats.cn/assets/2024/01/10/005728.png)

圆形金属片（按键）的两根线，就在上面引出来了；按键的轴按下，上面两根线短路，松手，上面两根线断开，就是个普通的按键

![](https://pic.xhcheats.cn/assets/2024/01/10/005803.png)

这个旋转编码器的轴是可以按下去的；轴的外侧是白色的编码盘，它也是一系列光栅一样的东西，只不过这是金属触电，在旋转时，依次接通和断开两边的触电；这个金属盘的位置是经过设计的，它能让两侧触电的通断产生一个90度的相位差，最终配合一下外部电路，这个编码器的两个输出就会输出如下这样的正交波形，带正交波形输出的编码器是可以用来测方向的（这就是单相输出和两相正交输出的区别），当然还有的编码器不是输出正交波形，而是一个引脚输出方波信号代表转速，另一个输出高低电平代表旋转方向，这种不是正交输出的编码器也是可以测方向的。 

![](https://pic.xhcheats.cn/assets/2024/01/10/005822.png)

当正转时，A相引脚输出一个方波波形，B相引脚输出一个和它相位相差90的波形（正交波形），如下。

![](https://pic.xhcheats.cn/assets/2024/01/10/005903.png)

当反向旋转时，A相引脚还是方波信号，B相引脚会提前90度，如下。

![](https://pic.xhcheats.cn/assets/2024/01/10/005927.png)

3.霍尔传感器形式编码器，这种是直接附在电机后面的编码器，中间是一个圆形磁铁，边上有两个位置错开的活儿传感器，当磁铁旋转时，通过霍尔传感器就可以输出正交的方波信号，如下。

![](https://pic.xhcheats.cn/assets/2024/01/10/005945.png)

4.这是独立的编码器元件，它的输入轴转动时，输出就会波形，这个也是可以测速和测方向的，具体用法再看相应的手册。如下。

![](https://pic.xhcheats.cn/assets/2024/01/10/010001.png)

## 旋转编码器的硬件电路

![](https://pic.xhcheats.cn/assets/2024/01/10/010022.png)

下面为模块电路细节介绍：

这里是编码器内部的两个触电，旋转轴旋转时，这两个触电以相位相差90度的方式交替导通，因为这只是个开关信号，所以要配合外围电路才能输出高低电平

![](https://pic.xhcheats.cn/assets/2024/01/10/010047.png)

左边接了一个10k的上拉电阻，默认没旋转的情况下，这个点被上拉为高电平，再通过R3这个电阻输出到A端口的就也是高电平，当旋转时，内部触电导通，那C端口处就直接被拉低到GND，再通过R3输出，A端口就是低电平了，之后这个R3是一个输出限流电阻（是为了防止模块引脚电流过大的）；C1是输出滤波电容，可以防止一些输出信号抖动。剩下的右边电路和左边是雷同的。

![](https://pic.xhcheats.cn/assets/2024/01/10/010126.png)

使用这个模块时的接线如下，下面的A相输出和B相输出接到STM32的两个引脚上（主要引脚的尾数不能一样），中间的C引脚就是GND，我们暂时不用

![](https://pic.xhcheats.cn/assets/2024/01/10/010143.png)

## 接线图

对射式红外传感器

![](https://pic.xhcheats.cn/assets/2024/01/10/010207.png)

旋转编码器

![](https://pic.xhcheats.cn/assets/2024/01/10/010229.png)

## 程序-对射式红外传感器
main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "countsensor.h"
 
 
int main(void)
{
	countsensor_init();//初始化红外对射式模块计次
	OLED_Init();	//初始化OLED
	
	OLED_ShowString(1,1,"Count:");//第一行第三列开始显示字符串
 
	while(1)
	{
		OLED_ShowNum(1,7,countsersor_get(),5);//显示countsersor_get的返回值，长度为5
	}
 
}
```

countsensor.c
```c
#include "stm32f10x.h"                  // Device header
 
uint16_t countsensor_count; //这个数字来统计中断触发的次数
 
 
//初始化函数，将模块要用的资源配置好
void countsensor_init(void)
{
	//第一步，时钟配置
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB,ENABLE); //开启RCC时钟
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO,ENABLE);	//开启AFIO时钟
	//EXTI和NVIC两个外设的时钟是一直开的 ，NVIC内核外设都是不需要开启时钟
 
	//第二步，配置GPIO
	//首先定义结构体
	GPIO_InitTypeDef GPIO_initstruct;  //结构体名字GPIO_initstruct
	//将结构体成员引出来
	//对于EXTI来说，模式为浮空输入|上拉输入|下拉输入；不知该写什么模式，可以看参考手册中的外设GPIO配置
	GPIO_initstruct.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_initstruct.GPIO_Pin = GPIO_Pin_14;
	GPIO_initstruct.GPIO_Speed = GPIO_Speed_50MHz;
	//最后初始化GPIO
	GPIO_Init(GPIOB,&GPIO_initstruct);	//传地址
	
	//第三步，配置AFIO外设中断引脚选择
	//AFIO的库函数是和GPIO在一个文件里，可以查看Library文件中的gpio.h查看函数
	GPIO_EXTILineConfig(GPIO_PortSourceGPIOB,GPIO_PinSource14);//代表连接PB14号口的第14个中断线路
	
	//第四步，配置EXTI,这样PB14的电平信号就能够通过EXTI通向下一级的NVIC了
	EXTI_InitTypeDef EXTI_InitStructure;//结构体类型名EXTI_InitTypeDef，变量名EXTI_InitStructure
	EXTI_InitStructure.EXTI_Line = EXTI_Line14;
	EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
	EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;//因为上面是GPIO_Mode_IPU设置为高电平，所以触发中断是下降
	EXTI_InitStructure.EXTI_LineCmd = ENABLE;
	EXTI_Init(&EXTI_InitStructure);
	
	//第五步，配置NVIC，NVIC是内核外设，所以它的库函数在misc.h
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2); //分组方式，整个芯片只能用一种。如放在模块中进行分组，要确保每个模块分组都选的是同一个；或者将这个代码放在主函数的最开始
	
	NVIC_InitTypeDef NVIC_InitStructure;
	NVIC_InitStructure.NVIC_IRQChannel = EXTI15_10_IRQn;
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;//因为我们这个程序只有一个，所以中断优先级的配置也是非常随意的
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 2;
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
	NVIC_Init(&NVIC_InitStructure);	
}
 
uint16_t countsersor_get(void)
{
	return countsensor_count;
 
}
 
 
//中断函数，都是无参无返回值的，名字固定在startup文件向量表中
//中断函数不需声明，它是自动执行的
void EXTI15_10_IRQHandler(void)
{
	/*一般都是先进行一个中断标志位的判断，确保是我们想要的中断源触发的函数，因为这个函数EXTI10到
	EXTI15都能进来，所以要先判断一下是不是我们想要的EXTI14进来的*/
	if(EXTI_GetFlagStatus(EXTI_Line14) == SET)
	{
		countsensor_count++;
	//每次中断函数结束后，都应该清除一下中断标志位
		EXTI_ClearITPendingBit(EXTI_Line14);
	}
	
}
//写完模块之后最好编译一下，要不然代码提示可能显示不出我们新写的函数
```

countsensor.h
```c
#ifndef __COUNTSENSOR_H
#define __COUNTSENSOR_H
 
void countsensor_init(void);
uint16_t countsersor_get(void);
 
#endif
```

## 程序-旋转编码器计次

main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "encoder.h"
 
 
int16_t num;
 
 
int main(void)
{
	OLED_Init();	//初始化OLED
	encoder_init();
	
	OLED_ShowString(1,1,"num:");//第一行第三列开始显示字符串hello word！
	while(1)
	{
		num += encoder_get();
		OLED_ShowSignedNum(1,5,num,5);
	}
}
```

encoder.c
```c
#include "stm32f10x.h"                  // Device header


int16_t encoder_count;

void encoder_init(void)
{
 
	//第一步，时钟配置
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB,ENABLE); //开启RCC时钟
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO,ENABLE);	//开启AFIO时钟
	//EXTI和NVIC两个外设的时钟是一直开的 ，NVIC内核外设都是不需要开启时钟
 
	//第二步，配置GPIO
	//首先定义结构体
	GPIO_InitTypeDef GPIO_initstruct;  //结构体名字GPIO_initstruct
	//将结构体成员引出来
	//对于EXTI来说，模式为浮空输入|上拉输入|下拉输入；不知该写什么模式，可以看参考手册中的外设GPIO配置
	GPIO_initstruct.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_initstruct.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1;
	GPIO_initstruct.GPIO_Speed = GPIO_Speed_50MHz;
	//最后初始化GPIO
	GPIO_Init(GPIOB,&GPIO_initstruct);	//传地址
	
	//第三步，配置AFIO外设中断引脚选择
	//AFIO的库函数是和GPIO在一个文件里，可以查看Library文件中的gpio.h查看函数
	GPIO_EXTILineConfig(GPIO_PortSourceGPIOB,GPIO_PinSource0);
	GPIO_EXTILineConfig(GPIO_PortSourceGPIOB,GPIO_PinSource1);
	//第四步，配置EXTI,这样PB14的电平信号就能够通过EXTI通向下一级的NVIC了
	EXTI_InitTypeDef EXTI_InitStructure;//结构体类型名EXTI_InitTypeDef，变量名EXTI_InitStructure
	EXTI_InitStructure.EXTI_Line = EXTI_Line0 | EXTI_Line1;
	EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
	EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;//因为上面是GPIO_Mode_IPU设置为高电平，所以触发中断是下降
	EXTI_InitStructure.EXTI_LineCmd = ENABLE;
	EXTI_Init(&EXTI_InitStructure);
	
	//第五步，配置NVIC，NVIC是内核外设，所以它的库函数在misc.h
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2); //分组方式，整个芯片只能用一种。如放在模块中进行分组，要确保每个模块分组都选的是同一个；或者将这个代码放在主函数的最开始
	NVIC_InitTypeDef NVIC_InitStructure;
	
	NVIC_InitStructure.NVIC_IRQChannel = EXTI0_IRQn;
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
	NVIC_Init(&NVIC_InitStructure);	
	
	NVIC_InitStructure.NVIC_IRQChannel = EXTI1_IRQn;
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 2;
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
	NVIC_Init(&NVIC_InitStructure);	
}

int16_t encoder_get(void)
{
	int16_t temp;
	temp = encoder_count;
	encoder_count = 0;
	return temp;
}

void EXTI0_IRQHandler(void)
{
	if(EXTI_GetITStatus(EXTI_Line0) == SET)
	{
		if(GPIO_ReadInputDataBit(GPIOB,GPIO_Pin_1) == 0)
		{
			encoder_count--;
		}
	
		EXTI_ClearITPendingBit(EXTI_Line0);
	}
		
}

void EXTI1_IRQHandler(void)
{
	if(EXTI_GetITStatus(EXTI_Line1) == SET)
	{
		if(GPIO_ReadInputDataBit(GPIOB,GPIO_Pin_0) == 0)
		{
			encoder_count++;
		}
	
		EXTI_ClearITPendingBit(EXTI_Line1);
	}
}
```

encoder.h
```c
#ifndef __ENCODER_H
#define __ENCODER_H

void encoder_init(void);
int16_t encoder_get(void);

#endif
```