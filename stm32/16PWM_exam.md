# PWM输入捕获示例程序（输入捕获模式测频率&PWMI模式测频率和占空比）

## 接线图和电路分析

两个代码的接线图都一样

![](https://pic.xhcheats.cn/assets/2024/01/16/020007.png)

测量信号的输入引脚是PA6，信号从PA6进来，待测的PWM信号也是STM32自己生成的，输出引脚是PA0。

需要配置电路连接图示如下：

![](https://pic.xhcheats.cn/assets/2024/01/16/020035.png)

步骤就是：

第一步，RCC开启时钟，把GPIO和TIM的时钟打开

第二步，GPIO初始化，把GPIO配置成输入模式（一般选择上拉输入或浮空输入模式）

第三步，配置时基单元，让CNT计数器在内部时钟的驱动下自增运行，和之前代码一样

第四步，配置输入捕获单元，包括滤波器、极性、直连通道、交叉通道、分频器这些参数，用一个结构体就可以统一进行配置了

第五步，选择从模式的触发源，触发源选择为TI1FP1，这里调用一个库函数给一个参数就行了

第六步，选择触发之后执行的操作，执行Reset操作，这里调用一个库函数就行了

最后，当这些电路都配置好之后，调用TIM_Cmd函数，开启定时器。这样所有的电路就能配合起来了，按照我们的要求工作了。当我们需要读取最新一个周期的频率时，直接读取CCR寄存器，然后按照fc/N，计算一下就行了，这就是整个程序的思路

## 输入捕获相关库函数

输入捕获初始化配置
```c
// 输入捕获IC初始化函数
// 输入捕获和输出比较都有四个通道，输出比较每个通道单独占一个函数，输入捕获每个通道共用一个函数（在结构体里会额外有一个参数可以用来选择具体是配置哪个通道，因为可能有交叉通道所以函数合在一起比较方便）
void TIM_ICInit(TIM_TypeDef* TIMx, TIM_ICInitTypeDef* TIM_ICInitStruct);

// 可以给输入捕获结构体赋一个初始值
void TIM_ICStructInit(TIM_ICInitTypeDef* TIM_ICInitStruct);

// 这个函数和TIM_ICInit类似都是用于初始化输入捕获单元的，但是TIM_ICInit函数只是单一地配置一个通道而TIM_PWMIConfig可以快速配置两个通道的PWMI模式（自动将另一个通道配置为相反的模式）
void TIM_PWMIConfig(TIM_TypeDef* TIMx, TIM_ICInitTypeDef* TIM_ICInitStruct);

// 单独写入时基单元的PSC，第三个参数与预装载有关
void TIM_PrescalerConfig(TIM_TypeDef* TIMx, uint16_t Prescaler, uint16_t TIM_PSCReloadMode);

// 单独配置四个通道的预分频器，结构体中也可以配置，效果相同
void TIM_SetIC1Prescaler(TIM_TypeDef* TIMx, uint16_t TIM_ICPSC);
void TIM_SetIC2Prescaler(TIM_TypeDef* TIMx, uint16_t TIM_ICPSC);
void TIM_SetIC3Prescaler(TIM_TypeDef* TIMx, uint16_t TIM_ICPSC);
void TIM_SetIC4Prescaler(TIM_TypeDef* TIMx, uint16_t TIM_ICPSC);

// 分别读取四个通道的CCR的值（与输出比较中的TIM_SetComparex函数对应，读写的都是CCR寄存器；输出比较模式下CCR是只写，要用SetCompare写入；输入捕获模式下CCR是只读，要用GetCapture读出）
uint16_t TIM_GetCapture1(TIM_TypeDef* TIMx);
uint16_t TIM_GetCapture2(TIM_TypeDef* TIMx);
uint16_t TIM_GetCapture3(TIM_TypeDef* TIMx);
uint16_t TIM_GetCapture4(TIM_TypeDef* TIMx);
```

主从触发模式配置

```c
// 选择输入（从模式）触发源TRGI
void TIM_SelectInputTrigger(TIM_TypeDef* TIMx, uint16_t TIM_InputTriggerSource);

// 选择输出（主模式）触发源TRGO，选择主模式触发的触发源
void TIM_SelectOutputTrigger(TIM_TypeDef* TIMx, uint16_t TIM_TRGOSource);

// 选择从模式需要执行的操作
void TIM_SelectSlaveMode(TIM_TypeDef* TIMx, uint16_t TIM_SlaveMode);
```
注意滤波器和分频器的区别：虽然它俩都是计次，但是滤波器计次并不会改变信号的原有频率，一般滤波器的采样频率都会远高于信号频率，所以滤波器只会滤除高频噪声使信号更平滑，1KHz滤波之后仍然是1KHz，信号频率不会变化；而分频器就是对信号本身进行计次，会改变频率，1KHz，2分频之后就是500Hz，4分频就是250Hz。

## 输入捕获模式测频率

main.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "PWM_LED.h"
#include "IC.h"

int main(void)
{
	
	OLED_Init();	//初始化OLED
	pwm_init();
	IC_init();//初始化整个电路 
	
	OLED_ShowString(1,1,"Freq:00000Hz");

	//PA0口输出1khz频率，50%占空比的待测信号;PWM模块将待测信号输出给PA0，PA0然后通过导线输入到PA6（PA6是TIM3的通道1，通道1通过输入捕获模块测量得到频率，然后在主循环里不断刷新显示频率）
	PWM_setPSC(720-1); //频率=72M/(psc+1)/(arr+1)   //频率=72M/720/100 =1khz
	PWM_SetCompare1(50);//占空比=ccr/（ARR+1）      //占空比=50/100 = 50%

	while(1)
	{
		OLED_ShowNum(1,6,IC_GetFreq(),5);//不断刷新显示频率
	}
}
```

IC.c

```c
#include "stm32f10x.h"                  // Device header

void IC_init(void)
{
	//1.打开时钟，选择内部时钟
	//使用APB1的开启时钟函数，TIM3是APB1总线的外设
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM3,ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);//打开时钟,PA6的通道1
 
	//2.初始化GPIO
	GPIO_InitTypeDef GPIO_InitStructure;//结构体变量名GPIO_InitStructure
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;//上拉输入
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;	
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;//默认50mhz输出
	GPIO_Init(GPIOA,&GPIO_InitStructure);//使用的是地址传递
	
	//3.1.初始化时基单元
	//选择时基单元的时钟,选择内部时钟;若不调用这个函数，系统上电也是默认是内部时钟
	TIM_InternalClockConfig(TIM3);
	
	//3.2.配置时基单元参数 
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;  //指定时钟分频
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; //计数器模式，向上计数
				/*公式：	
				PWM频率：Freq = CK_PSC / (PSC + 1) / (ARR + 1)
				PWM占空比：Duty = CCR / (ARR + 1)
				PWM分辨率：Reso = 1 / (ARR + 1)           */
	TIM_TimeBaseInitStructure.TIM_Period = 65536 - 1; //ARR周期，最好要设置大一些防止计数溢出，16位的计数器可以满量程计数是65535
	TIM_TimeBaseInitStructure.TIM_Prescaler = 72 - 1; //PSC预分频器，标准频率就是72M/72=1MHz；这个值决定了测周法的标准频率fc，72M/预分频就是计数器自增的频率就是计数标准频率；需要根据信号频率的分步范围来调整
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;  //重复计数器的值
	TIM_TimeBaseInit(TIM3,&TIM_TimeBaseInitStructure);
	
	//4.初始化输入捕获单元
	TIM_ICInitTypeDef TIM_ICInitStructure;
	TIM_ICInitStructure.TIM_Channel = TIM_Channel_1;//选择通道，使用TIM3的通道1
	TIM_ICInitStructure.TIM_ICFilter = 0xF;//配置输入捕获的滤波器，数越大，滤波效果越好，每个数值对应的采样频率和采样次数在参考手册里有，若信号有毛刺和噪声就可以增大滤波器参数可以有效避免干扰
	TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising;//对应边沿检测、极性选择部分，可以选择上升沿触发/下降沿触发/上升沿和下降沿都触发
	TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1;//分频器，触发信号分频器，不分频就是每次触发都有效，2分频就是每隔一次有效一次，以此类推
	TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_DirectTI;//选择触发信号从哪个引脚输入，对应配置数据选择器的。可以选择直连通道/交叉通道/TRC引脚
	TIM_ICInit(TIM3,&TIM_ICInitStructure);
	
	//5.配置触发源选择，配置TRGI的触发源为TI1FP1
	TIM_SelectInputTrigger(TIM3, TIM_TS_TI1FP1);//触发源选择TI1FP1
	
	
	//6.配置从模式，为Reset
	TIM_SelectSlaveMode(TIM3,TIM_SlaveMode_Reset);//从模式选择Reset
	
	//7.启动定时器，调用TIM_Cmd
	TIM_Cmd(TIM3,ENABLE);//CNT就会在内部时钟的驱动下不断自增，即使没有信号过来，它也会不断自增；有信号来的时候，CNT就会在从模式的作用下自动清零并不会影响测量
	/*
	初始化之后，整个电路就能全自动测量了，当我们想查看频率时，需要读取CCR进行计算，所以需要在下面写一个函数
	*/
}

uint32_t IC_GetFreq(void)
{
//使用测周法的公式，fc=72M/（psc+1），目前psc=72-1，所以fc=1MHz
	return 1000000 / (TIM_GetCapture1(TIM3) + 1);	//返回的是最新一个周期的频率值（单位是HZ） = 1MHz（1000000） / N(就是读取CCR的值）
}
```

PWM.c
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

//PWM控制呼吸灯的代码逻辑是初始化TIM2的通道1，产生一个PWM波形，输出引脚是PA0，然后通过PWM_SetCompare1可以调节CCR1寄存器的值从而控制PWM的占空比，PWM的频率是固定写好在初始化程序里了，运行时候调节不太方便
//在最后再加一个函数，用来便捷地调节PWM频率，PSC和ARR都可以调节频率，但是调节ARR会影响占空比，通过PSC调节频率不会影响占空比，所以计划是固定ARR值，通过调节PSC来改变PWM频率
//一般可以根据分辨率的要求先确定ARR，PSC决定频率，CCR决定占空比

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

//在运行过程更改CCR，使用函数TIM_SetCompare1封装用来单独更改通道1的CCR值，进而改变占空比
void PWM_SetCompare1(uint16_t Compare1)//TIM_SetCompare1封装
{
	TIM_SetCompare1(TIM2,Compare1);
}

//封装此函数，在初始化之后单独修改PSC,进而改变频率
void PWM_setPSC(uint16_t prescaler)
{
//调用库函数里单独写入PSC的函数，在tim.h中找，这个函数还有一个重装模式的参数所以叫TIM_PrescalerConfig
	TIM_PrescalerConfig(TIM2,prescaler,TIM_PSCReloadMode_Immediate);//写入PSC,第二个参数是写入PSC的值，直接将外层函数的prescaler参数传进去，第三个参数是重装模式（还是影子寄存器、预装载这个问题，就是写入的值是立刻生效还是在更新事件生效；立刻生效可能会在值改变时产生切断波形的现象会出现不完整的周期，更新事件生效就是会有一个缓存器，延迟参数的写入时间，等一个周期结束了，在更新事件时，再统一改变参数，保证每个周期的完整）
}
```

## PWMI模式测频率和占空比

main.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
#include "PWM_LED.h"
#include "IC.h"
int main(void)
{
	OLED_Init();	//初始化OLED
	pwm_init();
	IC_init();//初始化整个电路 
	
	OLED_ShowString(1,1,"Freq:00000Hz");
	OLED_ShowString(2,1,"Duty:00%");
	
	//PA0口输出1khz频率，50%占空比的待测信号;PWM模块将待测信号输出给PA0，PA0然后通过导线输入到PA6（PA6是TIM3的通道1，通道1通过输入捕获模块测量得到频率，然后在主循环里不断刷新显示频率）
	PWM_setPSC(7200-1); //频率=72M/(psc+1)/(arr+1)   //频率=72M/720/100 =1khz
	PWM_SetCompare1(80);//占空比=ccr/（ARR+1）      //占空比=50/100 = 50%
 
	while(1)
	{
		OLED_ShowNum(1,6,IC_GetFreq(),5);//不断刷新显示频率
		OLED_ShowNum(2,6,IC_GetDuty(),2);//不断刷新显示占空比
	}
}
```

IC.c

```c
#include "stm32f10x.h"                  // Device header
//PWMI模式，方法1：修改上一个程序的4.初始化输入捕获单元
//方法2：使用TIM_PWMIConfig函数
void IC_init(void)
{
	//1.打开时钟，选择内部时钟
	//使用APB1的开启时钟函数，TIM3是APB1总线的外设
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM3,ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);//打开时钟,PA6的通道1
 
	//2.初始化GPIO
	GPIO_InitTypeDef GPIO_InitStructure;//结构体变量名GPIO_InitStructure
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;//上拉输入
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;	
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;//默认50mhz输出
	GPIO_Init(GPIOA,&GPIO_InitStructure);//使用的是地址传递
	
	//3.1.初始化时基单元
	//选择时基单元的时钟,选择内部时钟;若不调用这个函数，系统上电也是默认是内部时钟
	TIM_InternalClockConfig(TIM3);
	
	//3.2.配置时基单元参数 
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;  //指定时钟分频
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; //计数器模式，向上计数
				/*公式：	
				PWM频率：Freq = CK_PSC / (PSC + 1) / (ARR + 1)
				PWM占空比：Duty = CCR / (ARR + 1)
				PWM分辨率：Reso = 1 / (ARR + 1)  
				目前我们给的标准频率时1mhz，计数器最大只能计到65535，所以所测量的最低频率是1m/65535=15hz，如果信号频率再低，计数器就要溢出了所以最低频率就是15hz左右，如果想再降低一些最低频率的限制可以把psc再加大点这样标准频率就更低所支持测量的最低频率也就更低；最大频率没有界限，1MHZ，信号频率接近1mhz时误差已经非常大了，
				*/
	TIM_TimeBaseInitStructure.TIM_Period = 65536 - 1; //ARR周期，最好要设置大一些防止计数溢出，16位的计数器可以满量程计数是65535
	TIM_TimeBaseInitStructure.TIM_Prescaler = 72 - 1; //PSC预分频器，标准频率就是72M/72=1MHz；这个值决定了测周法的标准频率fc，72M/预分频就是计数器自增的频率就是计数标准频率；需要根据信号频率的分步范围来调整
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;  //重复计数器的值
	TIM_TimeBaseInit(TIM3,&TIM_TimeBaseInitStructure);
	
	//4.初始化输入捕获单元，PWMI模式需配置成两个通道同时捕获同一个引脚的模式（一个简单的想法是：将通道初始化部分复制一份，结构体定义不需复制，通道1是直连模式上升沿触发通道2也延用这个配置）
	TIM_ICInitTypeDef TIM_ICInitStructure;
	TIM_ICInitStructure.TIM_Channel = TIM_Channel_1;//选择通道，使用TIM3的通道1
	TIM_ICInitStructure.TIM_ICFilter = 0xF;//配置输入捕获的滤波器，数越大，滤波效果越好，每个数值对应的采样频率和采样次数在参考手册里有，若信号有毛刺和噪声就可以增大滤波器参数可以有效避免干扰
	TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising;//对应边沿检测、极性选择部分，可以选择上升沿触发/下降沿触发/上升沿和下降沿都触发
	TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1;//分频器，触发信号分频器，不分频就是每次触发都有效，2分频就是每隔一次有效一次，以此类推
	TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_DirectTI;//选择触发信号从哪个引脚输入，对应配置数据选择器的。可以选择直连通道/交叉通道/TRC引脚
	TIM_ICInit(TIM3,&TIM_ICInitStructure);
	
	//方法1，：将通道初始化部分复制一份，结构体定义不需复制
//	TIM_ICInitStructure.TIM_Channel = TIM_Channel_2;//改为通道2
//	TIM_ICInitStructure.TIM_ICFilter = 0xF;
//	TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Falling;//改为下降沿触发
//	TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1;
//	TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_IndirectTI;//交叉输入
//	TIM_ICInit(TIM3,&TIM_ICInitStructure);
	
	//方法2：使用TIM_PWMIConfig函数,可快捷地把电路配置成PWMI模式的标准结构，这个函数只支持通道1和2不支持通道3和4，和方法1的效果是一样的，只需传入一个通道的参数就行了，在函数里会自动把剩下的一个通道初始化成相反的配置（比如已经传入了通道1、直连、上升沿，那函数里就会顺带配置通道2、交叉、下降沿）
	TIM_PWMIConfig(TIM3,&TIM_ICInitStructure);
	
	//5.配置触发源选择，配置TRGI的触发源为TI1FP1
	TIM_SelectInputTrigger(TIM3, TIM_TS_TI1FP1);//触发源选择TI1FP1
	
	
	//6.配置从模式，为Reset
	TIM_SelectSlaveMode(TIM3,TIM_SlaveMode_Reset);//从模式选择Reset
	
	//7.启动定时器，调用TIM_Cmd
	TIM_Cmd(TIM3,ENABLE);//CNT就会在内部时钟的驱动下不断自增，即使没有信号过来，它也会不断自增；有信号来的时候，CNT就会在从模式的作用下自动清零并不会影响测量
	/*
	初始化之后，整个电路就能全自动测量了，当我们想查看频率时，需要读取CCR进行计算，所以需要在下面写一个函数
	*/
}

//获取频率的函数
uint32_t IC_GetFreq(void)
{
//使用测周法的公式，fc=72M/（psc+1），目前psc=72-1，所以fc=1MHz
	return 1000000 / (TIM_GetCapture1(TIM3) + 1);	//返回的是最新一个周期的频率值（单位是HZ） = 1MHz（1000000） / N(就是读取CCR的值）
}

//获取占空比的函数
uint32_t IC_GetDuty(void)
{
//高电平的计数值存在CCR2里，整个周期的计数值存在CCR1里，用CCR2/CCR1就能得到占空比了
	 return (TIM_GetCapture2(TIM3) + 1) * 100 / (TIM_GetCapture1(TIM3) + 1);//显示整数的话，给它乘100，这样返回值的范围就是0-100，对应占空比0%-100%
	//经过实测，CCR总会少一个数，所以需要各加一个1补回来
}
```

PWM.c

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

//PWM控制呼吸灯的代码逻辑是初始化TIM2的通道1，产生一个PWM波形，输出引脚是PA0，然后通过PWM_SetCompare1可以调节CCR1寄存器的值从而控制PWM的占空比，PWM的频率是固定写好在初始化程序里了，运行时候调节不太方便
//在最后再加一个函数，用来便捷地调节PWM频率，PSC和ARR都可以调节频率，但是调节ARR会影响占空比，通过PSC调节频率不会影响占空比，所以计划是固定ARR值，通过调节PSC来改变PWM频率
//一般可以根据分辨率的要求先确定ARR，PSC决定频率，CCR决定占空比
 
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

//在运行过程更改CCR，使用函数TIM_SetCompare1封装用来单独更改通道1的CCR值，进而改变占空比
void PWM_SetCompare1(uint16_t Compare1)//TIM_SetCompare1封装
{
	TIM_SetCompare1(TIM2,Compare1);
}

//封装此函数，在初始化之后单独修改PSC,进而改变频率
void PWM_setPSC(uint16_t prescaler)
{
//调用库函数里单独写入PSC的函数，在tim.h中找，这个函数还有一个重装模式的参数所以叫TIM_PrescalerConfig
	TIM_PrescalerConfig(TIM2,prescaler,TIM_PSCReloadMode_Immediate);//写入PSC,第二个参数是写入PSC的值，直接将外层函数的prescaler参数传进去，第三个参数是重装模式（还是影子寄存器、预装载这个问题，就是写入的值是立刻生效还是在更新事件生效；立刻生效可能会在值改变时产生切断波形的现象会出现不完整的周期，更新事件生效就是会有一个缓存器，延迟参数的写入时间，等一个周期结束了，在更新事件时，再统一改变参数，保证每个周期的完整）
}
```