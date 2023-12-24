# GPIO输入示例

知识点：

上拉输入：若GPIO引脚配置为上拉输入模式，在默认情况下（GPIO引脚无输入），读取的GPIO引脚数据为1，即高电平。

下拉输入：若GPIO引脚配置为下拉输入模式，在默认情况下（GPIO引脚无输入），读取的GPIO引脚数据为0，即低电平。

##  按键控制LED

![](https://pic.xhcheats.cn/assets/2023/12/24/233751.png)

![](https://pic.xhcheats.cn/assets/2023/12/24/233810.png)

按键电路，该方式需设置为上拉模式。

驱动.c文件用来存放驱动程序的主体代码；驱动.h用来存放驱动程序可以对外提供的函数或变量声明。

.h文件要添加一个防止头文件重复包含的代码，格式固定，如下

```c
#ifndef __LED_H    //如果没有定义LED这个字符串
#define __LED_H    //那么就定义这个字符串
 
//函数和变量声明放在这里
 
#endif             //是和ifndef组成的括号
 
//空行结尾
```

```c
//如下函数用于读取输入模式某个位，返回值就是输入数据寄存器的某一位的值

uint8_t GPIO_ReadInputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)

//如下函数用于读取输入模式整个输入数据寄存器的值

uint8_t GPIO_ReadInputData(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)

//如下函数用于输出模式下，用来看一下自己输出的是什么

uint8_t GPIO_ReadOutputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)

//如下函数，少了个bit，用来读取整个输出寄存器

uint8_t GPIO_ReadOutputData(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)
```

程序代码如下
main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "LED.h"	//包含led的头文件
#include "key.h"

uint8_t keynum;	//全局变量，用来存键码的返回值，与局部变量作用域不同
//局部变量只能在本函数使用，全局变量每个函数都可使用
//在函数里优先使用自己的局部变量，如果没有才会使用外部的全局变量

int main(void)
{
	led_init();	//完成led的初始化，默认低电平
	key_init();	//初始化按键
	
	while(1)
	{
		keynum = key_getnum(); //不断读取键码值，放在keynum变量里
		if(keynum == 1)	//按键1按下
		{
			led1_turn();	//电平翻转，led状态取反，需用到GPIO_readoutput函数			
		}
		if(keynum == 2)
		{
			led2_turn();
		}
		
//		led1_on();
//		led2_off();
//		Delay_ms(500);
//		led1_off();
//		led2_on(); 
//		Delay_ms(500);
	}
}
```

lcd.c
```c
#include "stm32f10x.h"                  // Device header

void led_init(void)//初始化led
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);	//打开时钟
	
	//赋值结构体
	GPIO_InitTypeDef GPIO_InitStructA;		//结构体变量名GPIO_InitStructA
	GPIO_InitStructA.GPIO_Mode = GPIO_Mode_Out_PP;	//推挽输出
	GPIO_InitStructA.GPIO_Pin = GPIO_Pin_1 |GPIO_Pin_2;	//按位或来选择多个引脚
	GPIO_InitStructA.GPIO_Speed = GPIO_Speed_50MHz; //默认50mhz输出
	GPIO_Init(GPIOA,&GPIO_InitStructA);		//使用的是地址传递，将指定的GPIO外设初始化好			
	
	GPIO_SetBits(GPIOA,GPIO_Pin_1 | GPIO_Pin_2); //这样后，初始化led是高电平是熄灭的
}

void led1_on(void)  //点亮led1，就是pa1口
{
	GPIO_ResetBits(GPIOA,GPIO_Pin_1); //低电平点亮
}

void led1_off(void) //熄灭led1，就是pa1口
{
	GPIO_SetBits(GPIOA,GPIO_Pin_1);		//高电平熄灭
}

void led1_turn(void)	//led1状态取反，电平翻转
	//GPIO_ReadOutputDataBit这个函数，来读取端口输出的是什么
{
	if(GPIO_ReadOutputDataBit(GPIOA,GPIO_Pin_1) == 0)
	{
		GPIO_SetBits(GPIOA,GPIO_Pin_1); //状态取反，0变1
	}
	else
	{
		GPIO_ResetBits(GPIOA,GPIO_Pin_1);//状态取反，1变0
	}
}

//下方雷同
void led2_on(void)
{
	GPIO_ResetBits(GPIOA,GPIO_Pin_2);
}

void led2_off(void)
{
	GPIO_SetBits(GPIOA,GPIO_Pin_2);
}

void led2_turn(void)
{
	if(GPIO_ReadOutputDataBit(GPIOA,GPIO_Pin_2) == 0)
	{
		GPIO_SetBits(GPIOA,GPIO_Pin_2);
	}
	else
	{
		GPIO_ResetBits(GPIOA,GPIO_Pin_2);
	}
}
```

lcd.h
```c
#ifndef __LED_H
#define __LED_H

void led_init(void); //对模块外部声明，这个函数是可以被外部调用的函数
void led1_on(void);
void led1_off(void);
void led2_on(void);
void led2_off(void);
void led1_turn(void);
void led2_turn(void);

#endif
```

key.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

void key_init(void)		//按键初始化函数
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB,ENABLE);	//开启时钟
	
	//配置端口模式
	GPIO_InitTypeDef GPIO_InitStructB; //结构体变量名GPIO_InitStructB
	GPIO_InitStructB.GPIO_Mode = GPIO_Mode_IPU; //需要上拉输入，按键未按时默认高电平
	GPIO_InitStructB.GPIO_Pin = GPIO_Pin_1 |GPIO_Pin_11;
	GPIO_InitStructB.GPIO_Speed = GPIO_Speed_50MHz;//在输入模式下，这个参数其实无用，无影响
	GPIO_Init(GPIOB,&GPIO_InitStructB);
	
}

uint8_t key_getnum(void) //读取按键值的函数
{
	uint8_t keynum = 0; //按键键码，没有按下，就返回0，局部变量
	
	if(GPIO_ReadInputDataBit(GPIOB,GPIO_Pin_1) == 0) //读取GPIO端口，返回值就是输入数据寄存器的某一位值，等于0代表按键按下
	{
		Delay_ms(20); //按键按下消抖20ms
		while(GPIO_ReadInputDataBit(GPIOB,GPIO_Pin_1) == 0); // 松手后动作，直到松手
		Delay_ms(20);	//按键松手消抖
		keynum = 1;		//键码为1.传递出去
	}
	if(GPIO_ReadInputDataBit(GPIOB,GPIO_Pin_11) == 0)
	{
		Delay_ms(20);
		while(GPIO_ReadInputDataBit(GPIOB,GPIO_Pin_11) == 0);
		Delay_ms(20);
		keynum = 2;
	}
	
	return keynum;
}
```

key.h
```c
#ifndef __KEY_H
#define __KEY_H

void key_init(void);
uint8_t key_getnum(void);

#endif
```

## 光敏传感器控制蜂鸣器

![](https://pic.xhcheats.cn/assets/2023/12/24/234117.png)

main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "buzzer.h"
#include "lightsenoer.h"

int main(void)
{
	buzzer_init(); //初始化蜂鸣器
	lightsenoer_init(); //初始化光敏传感器
	while(1)
	{
		if(lightsenoer_get() == 1) //光线暗，模块本身不亮指示灯
		{
			buzzer_off(); //关闭蜂鸣器
		}
		else
		{
			buzzer_on(); //否者，打开蜂鸣器
		}	
	}
}
```

buzzer.c
```c
#include "stm32f10x.h"                  // Device header

void buzzer_init(void)		//蜂鸣器初始化
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB,ENABLE); //开启时钟
	GPIO_InitTypeDef GPIO_InitStruct; //定义结构体变量
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_Out_PP;//推挽输出
	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_12;	//pa12端口
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB,&GPIO_InitStruct);//传递地址
	GPIO_SetBits(GPIOB,GPIO_Pin_12);//初始化为高电平，蜂鸣器不响
}

void buzzer_on(void)
{
	GPIO_ResetBits(GPIOB,GPIO_Pin_12);
}
void buzzer_off(void)
{
	GPIO_SetBits(GPIOB,GPIO_Pin_12);
}

void buzzer_turn(void)
{
	if(GPIO_ReadOutputDataBit(GPIOB,GPIO_Pin_12) == 0)
	{
		GPIO_SetBits(GPIOB,GPIO_Pin_12);
	}
	else
	{
		GPIO_ResetBits(GPIOB,GPIO_Pin_12);
	}
}
```

buzzer.h
```c
#ifndef __BUZZER_H
#define __BUZZER_H

void buzzer_init(void);
void buzzer_on(void);
void buzzer_off(void);
void buzzer_turn(void);

#endif
```

lightsenoer.c

```c
#include "stm32f10x.h"                  // Device header

void lightsenoer_init(void)	//光敏传感器初始化函数
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB,ENABLE);
	
	GPIO_InitTypeDef GPIO_InitStruct;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_IPU; //上拉输入，默认高电平状态；若始终接在端口上，也可以选择浮空输入；只要保证引脚不会悬空即可
	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_13; //pb13端口
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB,&GPIO_InitStruct);
}

uint8_t lightsenoer_get(void)  //返回端口值函数
{
	return GPIO_ReadInputDataBit(GPIOB,GPIO_Pin_13);
}
```

lightsenoer.h
```c
#ifndef __LIGHTSENOER_H
#define __LIGHTSENOER_H

void lightsenoer_init(void);
uint8_t lightsenoer_get(void);

#endif
```