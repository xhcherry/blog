# 示例程序（定时器定时中断&定时器外部时钟）

知识点get：

滤波器工作原理：可以滤掉信号的抖动干扰。在一个固定的时钟频率f下进行采样，如果连续N个采样点都为相同的电平，那就代表输入信号稳定了，就把这个采样值输出出去；如果这N个采样值不全都相同，那就说明信号有抖动，这时就保持上一次的输出，或者直接输出低电平也行，这样就能保证输出信号在一定程度上的滤波；这里的采样频率f和采样点数N都是滤波器的参数，频率越低，采样点数越多，滤波效果越好，不过相应的信号延迟就越大；采样频率f由内部时钟直接而来，也可以是由内部时钟加一个时钟分频而来，这个分频多少是由参数ClockDivision决定的，这个参数其实跟时基单元关系并不大，它的可选值可以选择1分频（也就是不分频），2分频和4分频。

## 定时中断和时钟源选择相关库函数使用

定时器相关的库函数非常多，本节仅对将要使用的库函数和 亿些使用细节 进行说明（即使这样也还是很多）。

定时中断基本结构如下，便于理解下面的库函数及程序流程。

![](https://pic.xhcheats.cn/assets/2024/01/15/025453.png)

定时器初始化步骤如下，对应定时中断结构图

第一步，RCC开启时钟，定时器的基准时钟和整个外设的工作时钟就都打开了
第二步，选择时基单元的时钟源，对于定时中断就选择内部时钟源
第三步，配置时基单元，包括预分频器、自动重装载器、计数模式等，参数用结构体配置
第四步，配置输出中断控制，允许更新中断输出到NVIC
第五步，配置NVIC，在NVIC中打开定时器中断通道并分配一个优先级
第六步，运行控制，使能计数器，当定时器使能后，计数器就开始计数了，当计数器更新时，触发中断
最后再写一个中断函数，中断函数每隔一段时间就能自动执行一次

### 定时器TIM的库函数

基本配置、时基单元、中断输出控制、NVIC、运行控制函数
```c
// 恢复定时器缺省配置
void TIM_DeInit(TIM_TypeDef* TIMx);
 
// 时基单元初始化
void TIM_TimeBaseInit(TIM_TypeDef* TIMx, TIM_TimeBaseInitTypeDef* TIM_TimeBaseInitStruct);
 
// 把时基单元初始化函数所用的结构体变量赋一个默认值
void TIM_TimeBaseStructInit(TIM_TimeBaseInitTypeDef* TIM_TimeBaseInitStruct);
 
// 使能计数器（对应定时中断结构图中的“运行控制”功能） 
void TIM_Cmd(TIM_TypeDef* TIMx, FunctionalState NewState);
 
// 使能中断输出信号（对应定时中断结构图中的“中断输出控制”功能）
void TIM_ITConfig(TIM_TypeDef* TIMx, uint16_t TIM_IT, FunctionalState NewState);
```

时钟源选择函数

```c
//选择内部时钟
void TIM_InternalClockConfig(TIM_TypeDef* TIMx);
 
//选择ITRx其它定时器时钟
void TIM_ITRxExternalClockConfig(TIM_TypeDef* TIMx, uint16_t TIM_InputTriggerSource);
 
//选择TIx捕获通道时钟，对于外部引脚的波形一般都会有极性选择和滤波器
void TIM_TIxExternalClockConfig(TIM_TypeDef* TIMx, uint16_t TIM_TIxExternalCLKSource,
                                uint16_t TIM_ICPolarity, uint16_t ICFilter);
 
//选择ETR通过外部时钟模式1输入的时钟
void TIM_ETRClockMode1Config(TIM_TypeDef* TIMx, uint16_t TIM_ExtTRGPrescaler, uint16_t TIM_ExtTRGPolarity,
                             uint16_t ExtTRGFilter);
 
//选择ETR通过外部时钟模式2输入的时钟，如果不需触发输入功能本函数可与上面函数互换
void TIM_ETRClockMode2Config(TIM_TypeDef* TIMx, uint16_t TIM_ExtTRGPrescaler, 
                             uint16_t TIM_ExtTRGPolarity, uint16_t ExtTRGFilter);
 
//单独用来配置ETR引脚的预分频器、极性、滤波器这些参数
void TIM_ETRConfig(TIM_TypeDef* TIMx, uint16_t TIM_ExtTRGPrescaler, uint16_t TIM_ExtTRGPolarity,
                   uint16_t ExtTRGFilter);
```

### 参数（PSC、ARR等）更改函数（在程序运行过程中修改）

以下，单独的函数可以方便地更改PSC\ARR等参数
```c
// 预分频值设置，TIM_PSCReloadMode为是否应用输入缓冲功能配置
void TIM_PrescalerConfig(TIM_TypeDef* TIMx, uint16_t Prescaler, uint16_t TIM_PSCReloadMode);
 
// 改变计数器的计数模式
void TIM_CounterModeConfig(TIM_TypeDef* TIMx, uint16_t TIM_CounterMode);
 
// 自动重装寄存器预装功能配置（计数器有无预装功能）
void TIM_ARRPreloadConfig(TIM_TypeDef* TIMx, FunctionalState NewState);
 
// 手动给计数器写入一个值
void TIM_SetCounter(TIM_TypeDef* TIMx, uint16_t Counter);
 
// 手动给自动重装寄存器写入一个值
void TIM_SetAutoreload(TIM_TypeDef* TIMx, uint16_t Autoreload);
 
// 获取当前计数器的值
uint16_t TIM_GetCounter(TIM_TypeDef* TIMx);
 
// 获取当前的预分频器的值
uint16_t TIM_GetPrescaler(TIM_TypeDef* TIMx);
 
// 获取定时中断的标志位和清除标志位，使用方法与EXTI相同
FlagStatus TIM_GetFlagStatus(TIM_TypeDef* TIMx, uint16_t TIM_FLAG);
void TIM_ClearFlag(TIM_TypeDef* TIMx, uint16_t TIM_FLAG);
ITStatus TIM_GetITStatus(TIM_TypeDef* TIMx, uint16_t TIM_IT);
void TIM_ClearITPendingBit(TIM_TypeDef* TIMx, uint16_t TIM_IT);
```

### 使用定时器库函数的一些细节

选择内部时钟函数：定时器上电后默认选择内部时钟，如果要选择内部时钟，这一句可以省略。
```c
TIM_InternalClockConfig(TIM_TypeDef* TIMx);
```

时基单元初始化函数TIN_TimeBaseInit：在配置结构体变量时，会遇到以下几个细节问题
```
1.TIM_TimeBaseInitStructure.TIM_ClockDivision （采样）时钟分频频率选择
  在定时器的外部信号输入引脚一般都有一个滤波器来消除信号的抖动干扰，它的工作原理是：在一个固定的时钟频率f下进行采样，如果连续N个采样点都是相同的电平，就代表输入信号稳定了，就将采样值输出到下一级电路；如果N个采样点不全都相同，就说明信号有抖动，这时保持上一次的输出，或直接输出低电平。 这样就能保证输出信号在一定程度上的滤波。这里的采样频率f 和采样点数N，f和N是滤波器的参数，频率越低，采样点数越多，滤波效果就越好，不过相应的信号延迟就越大。
  采样频率f的来源可以是内部时钟直接提供，也可以是内部时钟加一个时钟分频而来。 分频是多少，就由参数TIM_ClockDivision决定。可见 TIM_ClockDivision与时基单元的关系并不大，它的可选值可以选择1分频，2分频和4分频。

2.TIM_TimeBaseInitStructure.TIM_CounterMode  计数器模式
TIM_CounterMode_Up  （向上计数）
TIM_CounterMode_CenterAligned1  （中央对齐计数）
3.TIM_TimeBaseInitStructure.TIM_Period     周期，ARR自动重装器的值
4.TIM_TimeBaseInitStructure.TIM_Prescaler  PSC预分频器的值
以上2-3-4参数就是时基单元里面每个关键寄存器的参数，在配置结构体变量时，并没有能直接操作计数器CNT的参数。如果需要，可以采用SetCounter和GetCounter两个函数来操作计数器。
5.TIM_TimeBaseInitStructure.TIM_RepetitionCounter 重复计数寄存器，通过这个参数可以设置重复计数寄存器。但是通用定时器中没有这一个寄存器，故可以直接设置为0。

6.定时时间的计算

决定定时时间的参数，是TIM_Period和TIM_Prescaler
  参考公式：
计数器溢出频率：CK_CNT_OV = CK_CNT / (ARR + 1) = CK_PSC / (PSC + 1) / (ARR + 1)

* 定时1s也就是定时频率为1Hz，定时频率=72M/ (PSC + 1) / (ARR + 1) = 1s =1Hz，那就可以PSC给7200，ARR给10000（1MHz等于10^6Hz）,然后两个参数再减1。

* 注意PSC（TIM_Prescaler）和ARR（TIM_Period）的取值都要在0~65535之间。

* PSC和ARR的取值不是唯一的。可以预分频给少点，自动重装给多点，这样就是以一个比较高的频率计比较多的数；也可以预分配给多点，自动重装给少点，这样就是以一个比较低的频率计比较少的数；两种方法都可以达到目标的定时时间。

* 在这里预分频是对72M进行7200分频，得到的就是10k的计数频率，在10k的频率下，计10000个数，就是1s的时间。

7.在TIM_TimeBaseInit函数的最后，会立刻生成一个更新事件，来重新装载预分频器和重复计数器的值。预分频器有缓冲寄存器，我们写入的PSC和ARR只有在更新事件时才会起作用。但是更新事件和更新中断是同时发生的，更新中断会置更新中断标志位，手动生成一个更新事件，就相当于在初始化时立刻进入更新函数执行一次，在开启中断之前手动清除一次更新中断标志位，就可以避免刚初始化完成就进入中断函数的问题。
```

使能中断函数TIM_ITConfig
```c
TIM_ITConfig(TIM2,TIM_IT_Update,ENABLE); //开启更新中断到NVIC的通道
```

启动定时器
```
TIM_Cmd(TIM2,ENABLE);//当产生更新时，就会触发中断
```

外部时钟配置函数TIM_ETRClockMode2Config

1.TIM_ExtTRGPrescaler外部时钟预分频器：可以选择外部时钟分频关闭（1分频）、2分频、4分频、8分频。

2.TIM_ExtTRGPolarity 外部触发的极性：TIM_ExtTRGPolarity_Inverted为反向极性，即低电平和下降沿有效；TIM_ExtTRGPolarity_NonInverted为不反向，即高电平和上升沿有效。

3.ExtTRGFilter 外部输入滤波器：工作原理与内部时钟的滤波器相似，它的值可以是0x00到0x0F之间的一个值，其决定了采样的f和N，具体的对应关系在手册中有对应表：

![](https://pic.xhcheats.cn/assets/2024/01/15/025852.png)

4.GPIO配置：因为是使用外部接口输入时钟，故在使用该函数之前还需要配置GPIO端口。对于定时器，手册中给的推荐配置是浮空输入。但是浮空输入会导致引脚的输入电平极易受干扰，所以输入信号的功率不小时一般选择上拉或下拉输入。当外部的输入信号功率很小，内部的上拉/下拉电阻（较大）可能会影响到这个输入信号，这时就需要用浮空输入，防止影响外部输入的电平。

![](https://pic.xhcheats.cn/assets/2024/01/15/025917.png)

## 定时器定时中断实例

本次实验要完成的现象是：定义一个 uint16_t 的 Num 变量，使其每秒+1。器件连接图和程序源码如下所示：

![](https://pic.xhcheats.cn/assets/2024/01/15/025954.png)

Timer.c
```c
#include "stm32f10x.h"                  // Device header
 
/*
定时器初始化
对应定时中断结构图
 
第一步，RCC开启时钟，定时器的基准时钟和整个外设的工作时钟就都打开了
第二步，选择时基单元的时钟源，对于定时中断就选择内部时钟源
第三步，配置时基单元，包括预分频器、自动重装载器、计数模式等，参数用结构体配置
第四步，配置输出中断控制，允许更新中断输出到NVIC
第五步，配置NVIC，在NVIC中打开定时器中断通道并分配一个优先级
第六步，运行控制，使能计数器，当定时器使能后，计数器就开始计数了，当计数器更新时，触发中断
最后再写一个中断函数，中断函数每隔一段时间就能自动执行一次
 
*/
void Timer_Init(void)  //定时中断初始化代码
{
	//初始化tim2，也就是通用定时器
	//使用APB1的开启时钟函数，TIM2是APB1总线的外设
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2,ENABLE);
	
	//选择时基单元的时钟,选择内部时钟;若不调用这个函数，系统上电也是默认是内部时钟
	TIM_InternalClockConfig(TIM2);
	
	//配置时基单元 
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;  //指定时钟分频
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; //计数器模式
	/*定时1s也就是定时频率为1Hz，定时频率=72M/ (PSC + 1) / (ARR + 1) = 1s =1Hz，
	那就可以PSC给7200，ARR给10000（1MHz等于10^6Hz），然后两个参数再减1
	在这里预分频是对72M进行7200分频，得到的就是10k的计数频率，
	在10k的频率下，计10000个数，就是1s的时间*/
	TIM_TimeBaseInitStructure.TIM_Period = 10000 - 1;  //ARR
	TIM_TimeBaseInitStructure.TIM_Prescaler = 7200 - 1;  //PSC
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;  //重复计数器的值
	TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStructure);
	
	/*在TIM_TimeBaseInit函数的最后，会立刻生成一个更新事件，来重新装载预分频器和重复计数器的值
	预分频器有缓冲寄存器，我们写入的PSC和ARR只有在更新事件时才会起作用
	为了让写入的值立刻起作用，故在函数的最后手动生成了一个更新事件
	但是更新事件和更新中断是同时发生的，更新中断会置更新中断标志位，手动生成一个更新事件，就相当于在初始化时立刻进入更新函数执行一次
	在开启中断之前手动清除一次更新中断标志位，就可以避免刚初始化完成就进入中断函数的问题*/
	TIM_ClearFlag(TIM2,TIM_FLAG_Update);
	
	//使能中断
	TIM_ITConfig(TIM2,TIM_IT_Update,ENABLE); //开启更新中断到NVIC的通道
	
	//NVIC中断
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);//NVIC优先级分组
	NVIC_InitTypeDef NVIC_InitTyStructure;
	NVIC_InitTyStructure.NVIC_IRQChannel = TIM2_IRQn;
	NVIC_InitTyStructure.NVIC_IRQChannelCmd = ENABLE;
	NVIC_InitTyStructure.NVIC_IRQChannelPreemptionPriority = 2;//抢占优先级
	NVIC_InitTyStructure.NVIC_IRQChannelSubPriority = 1;//响应优先级
	NVIC_Init(&NVIC_InitTyStructure); 
	
	//启动定时器
	TIM_Cmd(TIM2,ENABLE);//当产生更新时，就会触发中断
}
/*
中断函数模版
void TIM2_IRQHandler(void) //当定时器产生更新中断时，这个函数就会自动被执行
{
	//检查中断标志位
	if(TIM_GetITStatus(TIM2,TIM_IT_Update) == SET)
	{
	//执行相应的用户代码
		Num ++;
		TIM_ClearITPendingBit(TIM2,TIM_IT_Update);//清除标志位
	}
}
*/
```

main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "Timer.h"
 
uint16_t Num;
 
int main(void)
{
	
	OLED_Init();	//初始化OLED
	Timer_Init();    //初始化定时器
	
	OLED_ShowString(1,1,"Num:");
 
	while(1)
	{
		OLED_ShowNum(1,5,Num,5);
		OLED_ShowNum(2,5,TIM_GetCounter(TIM2),5);//CNT计数器值的变化情况（变化范围是ARR从0一直到自动重装值（10000-1））
	}
 
}
 
 
//定时器2中断函数放在使用中断的main.c文件中；在startup文件中
void TIM2_IRQHandler(void) //当定时器产生更新中断时，这个函数就会自动被执行
{
	//检查中断标志位
	if(TIM_GetITStatus(TIM2,TIM_IT_Update) == SET)
	{
	//执行相应的用户代码
		Num ++;//定时器每秒自动加一个Num全局变量
		TIM_ClearITPendingBit(TIM2,TIM_IT_Update);//清除标志位
	}
 
}
```

## 定时器外部时钟选择

可以在引脚定义图里找TIMx的ETR引脚是哪个

在上一个定时中断实例程序基础上进行更改；基本任务仍然是定时中断，时钟部分就不使用内部时钟了

本次实验要完成的现象是：用光敏传感器手动模拟一个外部时钟，定义一个 uint16_t 的 Num 变量，当外部时钟触发10次（预分频之后的脉冲）后Num + 1。器件连接图和程序源码如下所示：

![](https://pic.xhcheats.cn/assets/2024/01/15/030039.png)

Timer.c
```c
#include "stm32f10x.h"                  // Device header
/*
定时器初始化
对应定时中断结构图
第一步，RCC开启时钟，定时器的基准时钟和整个外设的工作时钟就都打开了
第二步，选择时基单元的时钟源，对于定时中断就选择内部时钟源
第三步，配置时基单元，包括预分频器、自动重装载器、计数模式等，参数用结构体配置
第四步，配置输出中断控制，允许更新中断输出到NVIC
第五步，配置NVIC，在NVIC中打开定时器中断通道并分配一个优先级
第六步，运行控制，使能计数器，当定时器使能后，计数器就开始计数了，当计数器更新时，触发中断
最后再写一个中断函数，中断函数每隔一段时间就能自动执行一次

*/
void Timer_Init(void)  //定时中断初始化代码
{
	//初始化tim2，也就是通用定时器
	//使用APB1的开启时钟函数，TIM2是APB1总线的外设
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2,ENABLE);
	
	//外部模式2需要用到gpio,进行GPIOA的时钟配置
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;	
	GPIO_Init(GPIOA,&GPIO_InitStructure);
	
	//通过ETR引脚的外部时钟模式2配置
	TIM_ETRClockMode2Config(TIM2,TIM_ExtTRGPSC_OFF,TIM_ExtTRGPolarity_NonInverted,0x00);
	
	//配置时基单元 
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;  //指定时钟分频
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; //计数器模式
	/*定时1s也就是定时频率为1Hz，定时频率=72M/ (PSC + 1) / (ARR + 1) = 1s =1Hz，
	那就可以PSC给7200，ARR给10000（1MHz等于10^6Hz），然后两个参数再减1
	在这里预分频是对72M进行7200分频，得到的就是10k的计数频率，
	在10k的频率下，计10000个数，就是1s的时间*/
	TIM_TimeBaseInitStructure.TIM_Period = 10 - 1;  //ARR
	TIM_TimeBaseInitStructure.TIM_Prescaler = 1 - 1;  //PSC
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;  //重复计数器的值
	TIM_TimeBaseInit(TIM2,&TIM_TimeBaseInitStructure);
	
	/*在TIM_TimeBaseInit函数的最后，会立刻生成一个更新事件，来重新装载预分频器和重复计数器的值
	预分频器有缓冲寄存器，我们写入的PSC和ARR只有在更新事件时才会起作用
	为了让写入的值立刻起作用，故在函数的最后手动生成了一个更新事件
	但是更新事件和更新中断是同时发生的，更新中断会置更新中断标志位，手动生成一个更新事件，就相当于在初始化时立刻进入更新函数执行一次
	在开启中断之前手动清除一次更新中断标志位，就可以避免刚初始化完成就进入中断函数的问题*/
	TIM_ClearFlag(TIM2,TIM_FLAG_Update);
	
	//使能中断
	TIM_ITConfig(TIM2,TIM_IT_Update,ENABLE); //开启更新中断到NVIC的通道
	
	//NVIC中断
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);//NVIC优先级分组
	NVIC_InitTypeDef NVIC_InitTyStructure;
	NVIC_InitTyStructure.NVIC_IRQChannel = TIM2_IRQn;
	NVIC_InitTyStructure.NVIC_IRQChannelCmd = ENABLE;
	NVIC_InitTyStructure.NVIC_IRQChannelPreemptionPriority = 2;//抢占优先级
	NVIC_InitTyStructure.NVIC_IRQChannelSubPriority = 1;//响应优先级
	NVIC_Init(&NVIC_InitTyStructure); 

	//启动定时器
	TIM_Cmd(TIM2,ENABLE);//当产生更新时，就会触发中断
}

//函数封装
uint16_t Timer_getcounter(void)
{
	return TIM_GetCounter(TIM2);
}

/*
中断函数模版
void TIM2_IRQHandler(void) //当定时器产生更新中断时，这个函数就会自动被执行
{
	//检查中断标志位
	if(TIM_GetITStatus(TIM2,TIM_IT_Update) == SET)
	{
	//执行相应的用户代码
		Num ++;
		TIM_ClearITPendingBit(TIM2,TIM_IT_Update);//清除标志位
	}
}
*/
```

main.c
```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "Timer.h"

uint16_t Num;

int main(void)
{
	
	OLED_Init();	//初始化OLED
	Timer_Init();    //初始化定时器
	
	OLED_ShowString(1,1,"Num:");
	OLED_ShowString(2,1,"CNT:");
	
	while(1)
	{
		OLED_ShowNum(1,5,Num,5);
		OLED_ShowNum(2,5,TIM_GetCounter(TIM2),5);//CNT计数器值的变化情况（变化范围是ARR从0一直到自动重装值（10000-1））
	}

}

//定时器2中断函数放在使用中断的main.c文件中；在startup文件中
void TIM2_IRQHandler(void) //当定时器产生更新中断时，这个函数就会自动被执行
{
	//检查中断标志位
	if(TIM_GetITStatus(TIM2,TIM_IT_Update) == SET)
	{
	//执行相应的用户代码
		Num ++;//定时器每秒自动加一个Num全局变量
		TIM_ClearITPendingBit(TIM2,TIM_IT_Update);//清除标志位
	}

}
```