# TIM输出比较示例程序（PWM驱动LED呼吸灯&PWM驱动舵机&PWM驱动直流电机）

## 输出比较相关库函数

### OC初始化（掌握）
```c
// 配置输出比较模块，输出比较单元有四个，对应也有四个函数
// 第二个参数是结构体，就是输出比较的一些参数
void TIM_OC1Init(TIM_TypeDef* TIMx, TIM_OCInitTypeDef* TIM_OCInitStruct);
void TIM_OC2Init(TIM_TypeDef* TIMx, TIM_OCInitTypeDef* TIM_OCInitStruct);
void TIM_OC3Init(TIM_TypeDef* TIMx, TIM_OCInitTypeDef* TIM_OCInitStruct);
void TIM_OC4Init(TIM_TypeDef* TIMx, TIM_OCInitTypeDef* TIM_OCInitStruct);
 
// 给输出比较结构体赋一个默认值（防止结构体的值不确定导致一些奇怪的问题）
void TIM_OCStructInit(TIM_OCInitTypeDef* TIM_OCInitStruct);
```

### OC参数更改（TIM_SetComparex函数最重要）
```c
// 使用高级定时器输出PWM波形时使能主输出，否则PWM波形不能正常输出
void TIM_CtrlPWMOutputs(TIM_TypeDef* TIMx, FunctionalState NewState);
 
// 单独设置输出比较的输出极性（带N的是高级定时器中互补通道的配置）
// 在这里可以设置输出极性，在OC初始化函数中也可以用结构体设置输出极性，这里相当于将单独修改结构体中的某一参数封装到一个函数中
//在结构体初始化的那个函数里也可以设置极性，这两个地方设置极性的作用是一样的，只不过是用结构体是一起初始化的，在这里是单独函数进行修改的
//一般来说，结构体里的参数都会有一个单独的函数可进行更改
void TIM_OC1PolarityConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCPolarity);
void TIM_OC1NPolarityConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCNPolarity);
void TIM_OC2PolarityConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCPolarity);
void TIM_OC2NPolarityConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCNPolarity);
void TIM_OC3PolarityConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCPolarity);
void TIM_OC3NPolarityConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCNPolarity);
void TIM_OC4PolarityConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCPolarity);
 
// 单独修改输出使能参数
void TIM_CCxCmd(TIM_TypeDef* TIMx, uint16_t TIM_Channel, uint16_t TIM_CCx);
void TIM_CCxNCmd(TIM_TypeDef* TIMx, uint16_t TIM_Channel, uint16_t TIM_CCxN);
 
// 单独更改输出比较模式的函数
void TIM_SelectOCxM(TIM_TypeDef* TIMx, uint16_t TIM_Channel, uint16_t TIM_OCMode);
 
// 单独更改CCR寄存器值的函数
//在运行时，更改占空比，就需要用到这四个函数
void TIM_SetCompare1(TIM_TypeDef* TIMx, uint16_t Compare1);
void TIM_SetCompare2(TIM_TypeDef* TIMx, uint16_t Compare2);
void TIM_SetCompare3(TIM_TypeDef* TIMx, uint16_t Compare3);
void TIM_SetCompare4(TIM_TypeDef* TIMx, uint16_t Compare4);
```

### OC输出比较的一些小功能
```c
// 配置强制输出模式（运行中暂停输出波形且强制输出高/低电平）
// 强制输出高电平和设置100%占空比等效，强制输出低电平和设置0%占空比等效
void TIM_ForcedOC1Config(TIM_TypeDef* TIMx, uint16_t TIM_ForcedAction);
void TIM_ForcedOC2Config(TIM_TypeDef* TIMx, uint16_t TIM_ForcedAction);
void TIM_ForcedOC3Config(TIM_TypeDef* TIMx, uint16_t TIM_ForcedAction);
void TIM_ForcedOC4Config(TIM_TypeDef* TIMx, uint16_t TIM_ForcedAction);
 
// 配置CCR寄存器的预装功能（影子寄存器，就是写入的值不会立即生效而是在更新事件才会生效，可以避免一些小问题）
void TIM_OC1PreloadConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCPreload);
void TIM_OC2PreloadConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCPreload);
void TIM_OC3PreloadConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCPreload);
void TIM_OC4PreloadConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCPreload);
 
// 配置快速使能（手册中“单脉冲模式”一节有介绍）
void TIM_OC1FastConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCFast);
void TIM_OC2FastConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCFast);
void TIM_OC3FastConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCFast);
void TIM_OC4FastConfig(TIM_TypeDef* TIMx, uint16_t TIM_OCFast);
 
// 清除REF信号（手册中在“外部事件时清除REF信号”一节有介绍）
void TIM_ClearOC1Ref(TIM_TypeDef* TIMx, uint16_t TIM_OCClear);
void TIM_ClearOC2Ref(TIM_TypeDef* TIMx, uint16_t TIM_OCClear);
void TIM_ClearOC3Ref(TIM_TypeDef* TIMx, uint16_t TIM_OCClear);
void TIM_ClearOC4Ref(TIM_TypeDef* TIMx, uint16_t TIM_OCClear);
```

补充
```
//仅高级定时器使用
//在使用高级定时器输出PWM时。需要调用这个函数，使能输出。否则PWM将不能正常输出
void TIM_CtrlPWMOutputs(TIM_TypeDef* TIMx, FunctionalState NewState);
```

```
//TIM_OCMode 输出比较模式中的选择

TIM_OCMode_Timing//冻结模式
TIM_OCMode_Active//相等时置有效电平
TIM_OCMode_Inactive//相等时置无效电平
TIM_OCMode_Toggle//相等时电平翻转
TIM_OCMode_PWM1//PWM模式1，主要用
TIM_OCMode_PWM2//PWM模式2
TIM_ForcedAction_Active//强制输出模式，初始化时不使用
TIM_ForcedAction_InActive
TIM_Output_Compare_Polarity  输出比较的极性选择
TIM_OCPolarity_High  //高极性，就是极性不翻转，REF波形直接输出，或者说是有效电平是高电平，REF有效时，输出高电平
TIM_OCPolarity_Low //低极性，就是REF电平取反，或者说是有效电平为低电平
```

## PWM驱动LED呼吸灯

接线图如下：注意LED是正极接在PA0引脚，负极接在GND的驱动方法，这样就是高电平点亮，低电平熄灭，这是正极性的驱动方法，这样的话观察更直观一点，就是占空比越大LED越亮，占空比越小LED越暗

![](https://pic.xhcheats.cn/assets/2024/01/15/031915.png)

main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "PWM_LED.h"

uint8_t i;

int main(void)
{
	
	OLED_Init();	//初始化OLED
	pwm_init();
 
	while(1)
	{
		//不断调用PWM_SetCompare1函数，更改CCR的值，实现LED呼吸灯的效果
		for(i=0;i<=100;i++)
		{
			PWM_SetCompare1(i);//设置CCR寄存器的值
			Delay_ms(10);
		}
		for(i=0;i<=100;i++)
		{
			PWM_SetCompare1(100-i);
			Delay_ms(10);
		}
	}
}
```

PWM_LED.c
```c
#include "stm32f10x.h"                  // Device header
 
/*
pwm初始化函数基本步骤（参考笔记PWM基本结构图）
第一步，RCC开启时钟，把要用的TIM外设和GPIO外设的时钟打开
第二步，配置单元，包括时钟源选择和时基单元都配置好
第三步，配置输出比较单元，包括CCR值、输出比较模式、极性选择、输出使能这些参数，在库函数里也是用结构体统一来配置
第四步，配置GPIO，把PWM对应的GPIO口，初始化为复用推挽输出的配置，Pwm和GPIO的对应关系可以参考引脚定义表
第五步，运行控制，启动计数器，这样就能输出PWM了
*/
 
void pwm_init(void)
{
	//1.打开时钟，选择内部时钟
	//使用APB1的开启时钟函数，TIM2是APB1总线的外设
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2,ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);	//打开时钟
	
	//引脚重映射内容，将PA0引脚重映射到PA15，将下面GPIO改为PA15其它不动
//	RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO,ENABLE);//引脚重映射；引脚重映射（TIM2的CH1本来是挂载在PA0引脚的，现在我想在其他引脚使用TIM2的CH1通道
//	GPIO_PinRemapConfig(GPIO_PartialRemap1_TIM2,ENABLE);//参考手册AFIO。将PA0引脚重映射到PA15，第一个参数可以是GPIO_PartialRemap1_TIM2或GPIO_FullRemap_TIM2
//	GPIO_PinRemapConfig(GPIO_Remap_SWJ_JTAGDisable,ENABLE);//取消调试端口复用JTAG，PA15端口默认使用JTAG调试端口，需要关闭；SWJ就是SWD和JTAG两种调试方式；若想用PA15\PB3\PB4三个引脚做GPIO使用，先打开AFIO再将JTAG复用解除
 
	//2.初始化时基单元
	//选择时基单元的时钟,选择内部时钟;若不调用这个函数，系统上电也是默认是内部时钟
	TIM_InternalClockConfig(TIM2);
	
	//3.配置时基单元 
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;  //指定时钟分频
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; //计数器模式
	/*
	公式：
	PWM频率：Freq = CK_PSC / (PSC + 1) / (ARR + 1)
	PWM占空比：Duty = CCR / (ARR + 1)
	PWM分辨率：Reso = 1 / (ARR + 1)
	若PWM波形为频率为1KHz，占空比为50%，分辨率为1%
	CK_PSC=72MHz
	代入公式：
	Freq =1000Hz=72MHz / 720 / 100
	Duty = 50% = 50 / 100
	Reso = 1% = 1 / 100
	因此：PSC=719，ARR=99，ARR=50
	*/
	TIM_TimeBaseInitStructure.TIM_Period = 100 - 1;  //ARR 周期
	TIM_TimeBaseInitStructure.TIM_Prescaler = 720 - 1;  //PSC 预分频器
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;  //重复计数器的值
	TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStructure);
	TIM_ClearFlag(TIM2,TIM_FLAG_Update);
	
	//4.初始化输出比较单元（通道）
	TIM_OCInitTypeDef TIM_OCInitStructure;
	TIM_OCStructInit(&TIM_OCInitStructure);//给结构体赋初始值；若不想把所有成员都列一遍赋值，就可以先用这个函数赋一个初始值，再更改你想改的值
	TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1;//设置输出比较的模式
	TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;//设置输出比较的极性
	TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;//设置输出使能（输出状态）
	TIM_OCInitStructure.TIM_Pulse = 50;//设置CCR，Pulse直译是脉冲
	TIM_OC1Init(TIM2, &TIM_OCInitStructure);//使用PA0口对应是第一个输出比较通道；在TIM2的OC1通道上就可以输出PWM波形了
	
	//5.初始化GPIO
	GPIO_InitTypeDef GPIO_InitStructure;		//结构体变量名GPIO_InitStructure
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;	//复用推挽输出；PWM波形通过引脚输出，使用定时器来控制引脚，输出数据寄存器将被断开，输出控制权将转移给片上外设（这里片上外设引脚连接的就是TIM2的CH1通道）
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;	
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; //默认50mhz输出
	GPIO_Init(GPIOA,&GPIO_InitStructure);		//使用的是地址传递		
	
	//6.启动定时器
	TIM_Cmd(TIM2,ENABLE);//PWM波形就能通过PA0输出了
}

//让LED呈现呼吸灯的效果，那就是不断更改CCR的值就行了
//在运行过程更改CCR，使用函数TIM_SetCompare1封装用来单独更改通道1的CCR值
 
//TIM_SetCompare1封装
void PWM_SetCompare1(uint16_t Compare1)
{
	TIM_SetCompare1(TIM2,Compare1);
}
```

## PWM驱动舵机

![](https://pic.xhcheats.cn/assets/2024/01/15/031958.png)

驱动舵机的关键就是输出一个下面一样的PWM波形，只要波形能够按照如下规定，准确的输出，那驱动舵机就非常简单了

![](https://pic.xhcheats.cn/assets/2024/01/15/032016.png)

main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "serve.h"
#include "key.h"

uint8_t keynum; //按键键码
float angle;//角度变量
int main(void)
{
	
	OLED_Init();	//初始化OLED
	serve_init();
	key_init();
	
	OLED_ShowString(1, 1 ,"angle:");
	
	//serve_setangle(120); //舵机设置角度
	//PWM_SetCompare2(500); //对应舵机0度的位置
	//建立一个舵机模块，封装函数。调用函数就能变为对应的角度，舵机设置角度，参数是0到180度
	
	while(1)
	{
		keynum = key_getnum();
		if(keynum == 1)
		{
			angle += 30;
			if(angle > 180)
			{
			angle = 0;
			}
		}
		serve_setangle(angle); //舵机设置角度
		OLED_ShowNum(1,7,angle,3);//一行七列显示angle变量长度为3
	}
}
```

pwm_led.c
```c
#include "stm32f10x.h"                  // Device header
 
/*
pwm初始化函数基本步骤（参考笔记PWM基本结构图）
第一步，RCC开启时钟，把要用的TIM外设和GPIO外设的时钟打开
第二步，配置单元，包括时钟源选择和时基单元都配置好
第三步，配置输出比较单元，包括CCR值、输出比较模式、极性选择、输出使能这些参数，在库函数里也是用结构体统一来配置
第四步，配置GPIO，把PWM对应的GPIO口，初始化为复用推挽输出的配置，Pwm和GPIO的对应关系可以参考引脚定义表
第五步，运行控制，启动计数器，这样就能输出PWM了
*/
 
//驱动舵机用的是PA1口的通道2
void pwm_init(void)
{
	//1.打开时钟，选择内部时钟
	//使用APB1的开启时钟函数，TIM2是APB1总线的外设
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2,ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);	//打开时钟
	
	//2.初始化时基单元
	//选择时基单元的时钟,选择内部时钟;若不调用这个函数，系统上电也是默认是内部时钟
	TIM_InternalClockConfig(TIM2);
	
	//3.配置时基单元 
	/*
	**********************************************************
	公式：
	PWM频率：  Freq = CK_PSC / (PSC + 1) / (ARR + 1)
	PWM占空比：Duty = CCR / (ARR + 1)
	PWM分辨率：Reso = 1 / (ARR + 1)
	************************************************************
	若PWM波形为频率为1KHz，占空比为50%，分辨率为1%
	舵机要求的周期是20ms，频率就是1/20ms=50hz；舵机要求高电平时间是0.5ms-2.5ms，也就是占空比
	ARR设置为20k对应20ms（计数器加一次就是1us）
	CCR设置500就是0.5ms，设置2500就是2.5ms
	*/
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;  //指定时钟分频
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; //计数器模式
	TIM_TimeBaseInitStructure.TIM_Period = 20000 - 1;  //ARR 周期
	TIM_TimeBaseInitStructure.TIM_Prescaler = 72 - 1;  //PSC 预分频器
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;  //重复计数器的值
	TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStructure);
	TIM_ClearFlag(TIM2,TIM_FLAG_Update);
	
	//4.初始化输出比较单元（通道）
	TIM_OCInitTypeDef TIM_OCInitStructure;
	TIM_OCStructInit(&TIM_OCInitStructure);//给结构体赋初始值；若不想把所有成员都列一遍赋值，就可以先用这个函数赋一个初始值，再更改你想改的值
	TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1;//设置输出比较的模式
	TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;//设置输出比较的极性
	TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;//设置输出使能（输出状态）
	TIM_OCInitStructure.TIM_Pulse = 50;//设置CCR，Pulse直译是脉冲
	TIM_OC2Init(TIM2, &TIM_OCInitStructure);//OC2是通道2；通道和引脚是对应的；对于同一个定时器的不同通道输出的PWM的特点如后：因为不同通道共用一个计数器，所以它们的频率必须是一样的，它们的占空比由各自的CCR决定的；由于计数器的更新，所有PWM同时跳变，所以它们的相位是同步的
	
	//5.初始化GPIO
	GPIO_InitTypeDef GPIO_InitStructure;		//结构体变量名GPIO_InitStructure
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;	//复用推挽输出；PWM波形通过引脚输出，使用定时器来控制引脚，输出数据寄存器将被断开，输出控制权将转移给片上外设（这里片上外设引脚连接的就是TIM2的CH1通道）
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1;	
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; //默认50mhz输出
	GPIO_Init(GPIOA,&GPIO_InitStructure);		//使用的是地址传递		
	
	//6.启动定时器
	TIM_Cmd(TIM2,ENABLE);//PWM波形就能通过PA0输出了
}

//TIM_SetCompare2封装，使用通道2
void PWM_SetCompare2(uint16_t Compare)
{
	TIM_SetCompare2(TIM2,Compare);
}
```

serve.c
```c
#include "stm32f10x.h"                  // Device header
#include "PWM_LED.h"  //继承pwm的功能

//舵机初始化函数
void serve_init(void)
{
	pwm_init();//将pwm底层初始化
}
/*
0度 对应 CCR 500
180          2500
对angle进行缩放。0-180是180范围，500-2500是2000范围，所以angle / 180*2000 + 500偏移，就得到目标比例了完成0-180到500-2500的映射了
*/
 
void serve_setangle(float angle) //舵机设置角度
{
	PWM_SetCompare2(angle / 180 * 2000 + 500);//线性映射
}
```

## PWM驱动直流电机

![](https://pic.xhcheats.cn/assets/2024/01/15/032102.png)

VM是电机电源，接在STLINK的5v引脚

VCC逻辑电源接在面包板3.3v正极

A01和AO2是电机输出端接电机的两根线，接线不分正反，对调两根线，电机的旋转方向就会反过来

STBY是待机控制脚，不需要待机，直接接逻辑电源正3.3v

控制引脚 AIN1和AIN2是方向控制，任意接两个GPIO就可以

控制引脚 PWMA是速度控制，需接PWM的输出脚，PA2对应的是TIM2的通道3

加大PWM频率，当PWM频率足够大时，超出人耳的范围，人耳就听不到了，人耳听到的范围是20Hz到20KHz。可以减小PSC来加大频率且不会影响占空比

main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "moter.h"
#include "key.h"

uint8_t keynum;//按键键码
int8_t speed;//有符号的速度变量

int main(void)
{
	OLED_Init();	//初始化OLED
	moter_init();
	key_init();
	
	OLED_ShowString(1,1,"speed:");
	
	while(1)
	{	
		keynum = key_getnum();
		if(keynum == 1)
		{
			speed += 20;
			if(speed > 100)
			{
				speed = -100;//speed从-100到100变化
			}	
		}
		moter_setspeed(speed);//实现按键控制速度
		OLED_ShowSignedNum(1,7,speed,3);
	}
}
```

PWM_LED.c
```c
#include "stm32f10x.h"                  // Device header
/*
pwm初始化函数基本步骤（参考笔记PWM基本结构图）
第一步，RCC开启时钟，把要用的TIM外设和GPIO外设的时钟打开
第二步，配置单元，包括时钟源选择和时基单元都配置好
第三步，配置输出比较单元，包括CCR值、输出比较模式、极性选择、输出使能这些参数，在库函数里也是用结构体统一来配置
第四步，配置GPIO，把PWM对应的GPIO口，初始化为复用推挽输出的配置，Pwm和GPIO的对应关系可以参考引脚定义表
第五步，运行控制，启动计数器，这样就能输出PWM了
*/

//1.电机接在TIM2的通道3上。修改：GPIO_Pin_2。TIM_OC3Init。PWM_SetCompare3
//2.对于直流电机也建立一个hardware模块

void pwm_init(void)
{
	//1.打开时钟，选择内部时钟
	//使用APB1的开启时钟函数，TIM2是APB1总线的外设
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2,ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);	//打开时钟
	
	//2.初始化时基单元
	//选择时基单元的时钟,选择内部时钟;若不调用这个函数，系统上电也是默认是内部时钟
	TIM_InternalClockConfig(TIM2);
	
	//3.配置时基单元 
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;  //指定时钟分频
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; //计数器模式
	/*
	公式：
	PWM频率：Freq = CK_PSC / (PSC + 1) / (ARR + 1)
	PWM占空比：Duty = CCR / (ARR + 1)
	PWM分辨率：Reso = 1 / (ARR + 1)
	若PWM波形为频率为1KHz，占空比为50%，分辨率为1%
	CK_PSC=72MHz
	代入公式：
	Freq =1000Hz=72MHz / 720 / 100
	Duty = 50% = 50 / 100
	Reso = 1% = 1 / 100
	因此：PSC=719，ARR=99，ARR=50
	*/
	TIM_TimeBaseInitStructure.TIM_Period = 100 - 1;  //ARR 周期
	TIM_TimeBaseInitStructure.TIM_Prescaler = 36 - 1;  //PSC 预分频器,现在为20KHz
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;  //重复计数器的值
	TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStructure);
	TIM_ClearFlag(TIM2,TIM_FLAG_Update);
	
	//4.初始化输出比较单元（通道）
	TIM_OCInitTypeDef TIM_OCInitStructure;
	TIM_OCStructInit(&TIM_OCInitStructure);//给结构体赋初始值；若不想把所有成员都列一遍赋值，就可以先用这个函数赋一个初始值，再更改你想改的值
	TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1;//设置输出比较的模式
	TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;//设置输出比较的极性
	TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;//设置输出使能（输出状态）
	TIM_OCInitStructure.TIM_Pulse = 50;//设置CCR，Pulse直译是脉冲
	TIM_OC3Init(TIM2, &TIM_OCInitStructure);//TIM2通道3
	
	//5.初始化GPIO
	GPIO_InitTypeDef GPIO_InitStructure;		//结构体变量名GPIO_InitStructure
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;	//复用推挽输出；PWM波形通过引脚输出，使用定时器来控制引脚，输出数据寄存器将被断开，输出控制权将转移给片上外设（这里片上外设引脚连接的就是TIM2的CH1通道）
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2;	
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; //默认50mhz输出
	GPIO_Init(GPIOA,&GPIO_InitStructure);		//使用的是地址传递		
	
	//6.启动定时器
	TIM_Cmd(TIM2,ENABLE);//PWM波形就能通过PA0输出了
}

//TIM_SetCompare1封装
void PWM_SetCompare3(uint16_t Compare)
{
	TIM_SetCompare3(TIM2,Compare);
}
```

moter.c
```c
#include "stm32f10x.h"                  // Device header
#include "PWM_LED.h"   //继承PWM模块
void moter_init(void) //初始化函数
{
	pwm_init();//调用底层的PWM_init，初始化pwm
	
	//需要额外初始化方向控制的两个脚，即初始化GPIO引脚
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);	//打开时钟
	//配置端口模式
	GPIO_InitTypeDef GPIO_InitStructA;		//结构体变量名GPIO_InitStructA
	GPIO_InitStructA.GPIO_Mode = GPIO_Mode_Out_PP;	//推挽输出
	GPIO_InitStructA.GPIO_Pin = GPIO_Pin_4 |GPIO_Pin_5;	//或运算，选择两个引脚
	GPIO_InitStructA.GPIO_Speed = GPIO_Speed_50MHz; //默认50mhz输出
	GPIO_Init(GPIOA,&GPIO_InitStructA);		//使用的是地址传递			
}

//设置速度的函数
void moter_setspeed(int8_t speed)
{
	//针对正转和翻转，用if来分别处理
	if(speed >= 0)//正转的逻辑
	{
		//首先将方向控制脚设置为一个高电平，一个低电平.哪个为高哪个为底无所谓
		GPIO_SetBits(GPIOA,GPIO_Pin_4);
		GPIO_ResetBits(GPIOA,GPIO_Pin_5);
		//速度
		PWM_SetCompare3(speed);
	}
	else//speed就是负数，代表反转
	{
		//首先是正反转，将set和reset反过来就能反转了 
		GPIO_ResetBits(GPIOA,GPIO_Pin_4);
		GPIO_SetBits(GPIOA,GPIO_Pin_5);
		PWM_SetCompare3(-speed);//此时speed为负数，必须为正数，在speed前加负号
	}
}
```