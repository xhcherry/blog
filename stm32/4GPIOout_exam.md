# GPIO输出示例

## LED闪烁

![](https://pic.xhcheats.cn/assets/2023/12/24/180324.png)
```C
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
 
int main (void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);	//开启时钟
	
	GPIO_InitTypeDef GPIO_InitStructure;	//定义结构体
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA,&GPIO_InitStructure);	//GPIO配置初始化
	
	//GPIO_SetBits(GPIOA,GPIO_Pin_0);
	
	
	
	while(1)
	{
		GPIO_WriteBit(GPIOA,GPIO_Pin_0,Bit_RESET);
		Delay_ms(500);
		GPIO_WriteBit(GPIOA,GPIO_Pin_0,Bit_SET);
		Delay_ms(500);
		
		GPIO_ResetBits(GPIOA,GPIO_Pin_0);
		Delay_ms(500);
		GPIO_SetBits(GPIOA,GPIO_Pin_0);
		Delay_ms(500);
		
		GPIO_WriteBit(GPIOA,GPIO_Pin_0,(BitAction)0);
		Delay_ms(500);
		GPIO_WriteBit(GPIOA,GPIO_Pin_0,(BitAction)1);
		Delay_ms(500);
			 
	}
}
```

## LED流水灯

![](https://pic.xhcheats.cn/assets/2023/12/24/180427.png)
```C
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);//开启时钟
	
	GPIO_InitTypeDef GPIO_InitStruct;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_Out_PP;
//	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2 | GPIO_Pin_3 | GPIO_Pin_4 | GPIO_Pin_5 | GPIO_Pin_6 | GPIO_Pin_7;
	// |或运算  按键右转  前三个引脚或后为0x0111  ...
	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_All;
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;
	
	GPIO_Init(GPIOA,&GPIO_InitStruct);
	
	while(1)
	{
		GPIO_Write(GPIOA,~0x0001);// 0000 0000 0000 0001   取反前~
		Delay_ms(500);
		GPIO_Write(GPIOA,~0x0002);// 0000 0000 0000 0010
		Delay_ms(500);
		GPIO_Write(GPIOA,~0x0004);// 0000 0000 0000 0100
		Delay_ms(500);
		GPIO_Write(GPIOA,~0x0008);// 0000 0000 0000 1000
		Delay_ms(500);
		GPIO_Write(GPIOA,~0x0010);// 0000 0000 0001 0000
		Delay_ms(500);
		GPIO_Write(GPIOA,~0x0020);// 0000 0000 0010 0000
		Delay_ms(500);
		GPIO_Write(GPIOA,~0x0040);// 0000 0000 0100 0000
		Delay_ms(500);
		GPIO_Write(GPIOA,~0x0080);// 0000 0000 1000 0000
		Delay_ms(500);
	}
}
```

## 蜂鸣器

![](https://pic.xhcheats.cn/assets/2023/12/24/180449.png)
```C
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
int main(void)
{
    //RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB,ENABLE);
    
    GPIO_InitTypeDef GPIO_Initstruct;
    GPIO_Initstruct.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_Initstruct.GPIO_Pin = GPIO_Pin_12;
    GPIO_Initstruct.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOB,&GPIO_Initstruct);
    
    
    
    while(1)
    {
//        GPIO_ResetBits(GPIOB,GPIO_Pin_12);
//        Delay_ms(100);
//        GPIO_SetBits(GPIOB,GPIO_Pin_12);
//        Delay_ms(100);
//        GPIO_ResetBits(GPIOB,GPIO_Pin_12);
//        Delay_ms(100);
//        GPIO_SetBits(GPIOB,GPIO_Pin_12);
//        Delay_ms(700);
    }
}
```