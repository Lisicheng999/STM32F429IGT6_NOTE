# STM32F429XX.s source code analysis

## 定义栈段
Stack_Size		EQU     0x400

                AREA    STACK, NOINIT, READWRITE, ALIGN=3
Stack_Mem       SPACE   Stack_Size
__initial_sp

### 栈顶和栈底
__initial_sp：指向 Stack_Mem 的栈顶，用于初始化 SP。
栈底和栈顶：
栈底：Stack_Mem 的起始地址。
栈顶：Stack_Mem 的结束地址，即 Stack_Mem + Stack_Size。

## 定义堆，堆基址，堆顶
Heap_Size      EQU     0x200

                AREA    HEAP, NOINIT, READWRITE, ALIGN=3
__heap_base
Heap_Mem        SPACE   Heap_Size
__heap_limit

## 对齐声明，指定指令集
                PRESERVE8
                THUMB

## 定义中断向量表
; Vector Table Mapped to Address 0 at Reset
                AREA    RESET, DATA, READONLY
                EXPORT  __Vectors
                EXPORT  __Vectors_End
                EXPORT  __Vectors_Size

__Vectors       DCD     __initial_sp               ; Top of Stack
                DCD     Reset_Handler              ; Reset Handler
                DCD     NMI_Handler                ; NMI Handler
                DCD     HardFault_Handler          ; Hard Fault Handler
                DCD     MemManage_Handler          ; MPU Fault Handler
                DCD     BusFault_Handler           ; Bus Fault Handler
                DCD     UsageFault_Handler         ; Usage Fault Handler
                DCD     0                          ; Reserved
                DCD     0                          ; Reserved
                DCD     0                          ; Reserved
                DCD     0                          ; Reserved
                DCD     SVC_Handler                ; SVCall Handler
                DCD     DebugMon_Handler           ; Debug Monitor Handler
                DCD     0                          ; Reserved
                DCD     PendSV_Handler             ; PendSV Handler
                DCD     SysTick_Handler            ; SysTick Handler

                ; External Interrupts
                DCD     WWDG_IRQHandler                   ; Window WatchDog                                        
                DCD     PVD_IRQHandler                    ; PVD through EXTI Line detection                        
                DCD     TAMP_STAMP_IRQHandler             ; Tamper and TimeStamps through the EXTI line            
                DCD     RTC_WKUP_IRQHandler               ; RTC Wakeup through the EXTI line                       
                DCD     FLASH_IRQHandler                  ; FLASH                                           
                DCD     RCC_IRQHandler                    ; RCC                                             
                DCD     EXTI0_IRQHandler                  ; EXTI Line0                                             
                DCD     EXTI1_IRQHandler                  ; EXTI Line1                                             
                DCD     EXTI2_IRQHandler                  ; EXTI Line2                                             
                DCD     EXTI3_IRQHandler                  ; EXTI Line3                                             
                DCD     EXTI4_IRQHandler                  ; EXTI Line4                                             
                DCD     DMA1_Stream0_IRQHandler           ; DMA1 Stream 0                                   
                DCD     DMA1_Stream1_IRQHandler           ; DMA1 Stream 1                                   
                DCD     DMA1_Stream2_IRQHandler           ; DMA1 Stream 2                                   
                DCD     DMA1_Stream3_IRQHandler           ; DMA1 Stream 3                                   
                DCD     DMA1_Stream4_IRQHandler           ; DMA1 Stream 4                                   
                DCD     DMA1_Stream5_IRQHandler           ; DMA1 Stream 5                                   
                DCD     DMA1_Stream6_IRQHandler           ; DMA1 Stream 6                                   
                DCD     ADC_IRQHandler                    ; ADC1, ADC2 and ADC3s                            
                DCD     CAN1_TX_IRQHandler                ; CAN1 TX                                                
                DCD     CAN1_RX0_IRQHandler               ; CAN1 RX0                                               
                DCD     CAN1_RX1_IRQHandler               ; CAN1 RX1                                               
                DCD     CAN1_SCE_IRQHandler               ; CAN1 SCE                                               
                DCD     EXTI9_5_IRQHandler                ; External Line[9:5]s                                    
                DCD     TIM1_BRK_TIM9_IRQHandler          ; TIM1 Break and TIM9                   
                DCD     TIM1_UP_TIM10_IRQHandler          ; TIM1 Update and TIM10                 
                DCD     TIM1_TRG_COM_TIM11_IRQHandler     ; TIM1 Trigger and Commutation and TIM11
                DCD     TIM1_CC_IRQHandler                ; TIM1 Capture Compare                                   
                DCD     TIM2_IRQHandler                   ; TIM2                                            
                DCD     TIM3_IRQHandler                   ; TIM3                                            
                DCD     TIM4_IRQHandler                   ; TIM4                                            
                DCD     I2C1_EV_IRQHandler                ; I2C1 Event                                             
                DCD     I2C1_ER_IRQHandler                ; I2C1 Error                                             
                DCD     I2C2_EV_IRQHandler                ; I2C2 Event                                             
                DCD     I2C2_ER_IRQHandler                ; I2C2 Error                                               
                DCD     SPI1_IRQHandler                   ; SPI1                                            
                DCD     SPI2_IRQHandler                   ; SPI2                                            
                DCD     USART1_IRQHandler                 ; USART1                                          
                DCD     USART2_IRQHandler                 ; USART2                                          
                DCD     USART3_IRQHandler                 ; USART3                                          
                DCD     EXTI15_10_IRQHandler              ; External Line[15:10]s                                  
                DCD     RTC_Alarm_IRQHandler              ; RTC Alarm (A and B) through EXTI Line                  
                DCD     OTG_FS_WKUP_IRQHandler            ; USB OTG FS Wakeup through EXTI line                        
                DCD     TIM8_BRK_TIM12_IRQHandler         ; TIM8 Break and TIM12                  
                DCD     TIM8_UP_TIM13_IRQHandler          ; TIM8 Update and TIM13                 
                DCD     TIM8_TRG_COM_TIM14_IRQHandler     ; TIM8 Trigger and Commutation and TIM14
                DCD     TIM8_CC_IRQHandler                ; TIM8 Capture Compare                                   
                DCD     DMA1_Stream7_IRQHandler           ; DMA1 Stream7                                           
                DCD     FMC_IRQHandler                    ; FMC                                             
                DCD     SDIO_IRQHandler                   ; SDIO                                            
                DCD     TIM5_IRQHandler                   ; TIM5                                            
                DCD     SPI3_IRQHandler                   ; SPI3                                            
                DCD     UART4_IRQHandler                  ; UART4                                           
                DCD     UART5_IRQHandler                  ; UART5                                           
                DCD     TIM6_DAC_IRQHandler               ; TIM6 and DAC1&2 underrun errors                   
                DCD     TIM7_IRQHandler                   ; TIM7                   
                DCD     DMA2_Stream0_IRQHandler           ; DMA2 Stream 0                                   
                DCD     DMA2_Stream1_IRQHandler           ; DMA2 Stream 1                                   
                DCD     DMA2_Stream2_IRQHandler           ; DMA2 Stream 2                                   
                DCD     DMA2_Stream3_IRQHandler           ; DMA2 Stream 3                                   
                DCD     DMA2_Stream4_IRQHandler           ; DMA2 Stream 4                                   
                DCD     ETH_IRQHandler                    ; Ethernet                                        
                DCD     ETH_WKUP_IRQHandler               ; Ethernet Wakeup through EXTI line                      
                DCD     CAN2_TX_IRQHandler                ; CAN2 TX                                                
                DCD     CAN2_RX0_IRQHandler               ; CAN2 RX0                                               
                DCD     CAN2_RX1_IRQHandler               ; CAN2 RX1                                               
                DCD     CAN2_SCE_IRQHandler               ; CAN2 SCE                                               
                DCD     OTG_FS_IRQHandler                 ; USB OTG FS                                      
                DCD     DMA2_Stream5_IRQHandler           ; DMA2 Stream 5                                   
                DCD     DMA2_Stream6_IRQHandler           ; DMA2 Stream 6                                   
                DCD     DMA2_Stream7_IRQHandler           ; DMA2 Stream 7                                   
                DCD     USART6_IRQHandler                 ; USART6                                           
                DCD     I2C3_EV_IRQHandler                ; I2C3 event                                             
                DCD     I2C3_ER_IRQHandler                ; I2C3 error                                             
                DCD     OTG_HS_EP1_OUT_IRQHandler         ; USB OTG HS End Point 1 Out                      
                DCD     OTG_HS_EP1_IN_IRQHandler          ; USB OTG HS End Point 1 In                       
                DCD     OTG_HS_WKUP_IRQHandler            ; USB OTG HS Wakeup through EXTI                         
                DCD     OTG_HS_IRQHandler                 ; USB OTG HS                                      
                DCD     DCMI_IRQHandler                   ; DCMI  
                DCD     0                          ; Reserved				                              
                DCD     HASH_RNG_IRQHandler               ; Hash and Rng
                DCD     FPU_IRQHandler                    ; FPU
                DCD     UART7_IRQHandler                  ; UART7
                DCD     UART8_IRQHandler                  ; UART8
                DCD     SPI4_IRQHandler                   ; SPI4
                DCD     SPI5_IRQHandler                   ; SPI5
                DCD     SPI6_IRQHandler                   ; SPI6
                DCD     SAI1_IRQHandler                   ; SAI1
                DCD     LTDC_IRQHandler                   ; LTDC
                DCD     LTDC_ER_IRQHandler                ; LTDC error
                DCD     DMA2D_IRQHandler                  ; DMA2D
                                         
__Vectors_End

__Vectors_Size  EQU  __Vectors_End - __Vectors

## 复位处理函数
Reset_Handler    PROC
                 EXPORT  Reset_Handler             [WEAK]
        IMPORT  SystemInit
        IMPORT  __main

                 LDR     R0, =SystemInit
                 BLX     R0
                 LDR     R0, =__main
                 BX      R0
                 ENDP
                 
1. Reset_Handler PROC
PROC（Procedure）指示接下来的部分是一个过程（函数）。在这里，Reset_Handler 被定义为一个过程。
它表示复位处理程序的开始。
2. EXPORT Reset_Handler [WEAK]
EXPORT 是用于将 Reset_Handler 函数导出，使其在其他模块中可见。
[WEAK] 关键字表示该函数是 弱符号。这意味着，如果有其他地方定义了 Reset_Handler，链接器会优先使用其他定义，而不是这个弱定义。弱符号通常用于提供默认的实现，当用户没有定义具体实现时，使用默认实现。
3. IMPORT SystemInit
IMPORT 关键字声明 SystemInit 函数来自外部模块，表示 SystemInit 函数将在链接时被引用。
SystemInit 通常是一个在启动时用于初始化硬件（如时钟、外设等）的函数，通常由系统库提供。
4. IMPORT __main
IMPORT 关键字声明 __main 符号来自外部模块，表示 __main 是程序的入口点，链接时会被解析为实际的 main() 函数。
在某些启动文件或库中，__main 是程序的主要入口点，它会最终调用用户编写的 main() 函数。
5. LDR R0, =SystemInit
LDR（Load Register）指令将 SystemInit 函数的地址加载到寄存器 R0 中。
=SystemInit 是汇编的一个伪指令，表示将 SystemInit 函数的地址加载到寄存器 R0。这意味着 R0 将存储 SystemInit 的内存地址。
6. BLX R0
BLX（Branch with Link and Exchange）指令将控制流跳转到 R0 中存储的地址，并保存当前地址到链接寄存器（LR）中，以便之后返回。
这里它将跳转到 SystemInit 函数执行硬件初始化。
7. LDR R0, =__main
这条指令将 __main 函数的地址加载到寄存器 R0 中。
__main 通常是程序的主入口点，负责调用用户定义的 main() 函数。
8. BX R0
BX（Branch and Exchange）指令将控制流跳转到 R0 中存储的地址。
这条指令会将控制流跳转到 __main，并开始执行程序的主逻辑，即调用 main() 函数。
9. ENDP
ENDP（End Procedure）指示过程定义的结束，标志着 Reset_Handler 函数的结束

## GPIO

### 1. STM32F429 的 GPIO 概述
在 STM32F429 上，并非所有引脚都可以作为 GPIO 使用，但大部分引脚都具有 GPIO 功能，并且可以复用为特定的外设功能（例如 UART、SPI、I2C 等）。以下是主要特性：

GPIO 通用引脚组：

STM32F429 芯片包含多个 GPIO 端口（GPIOA 到 GPIOK，视具体型号而定），每个端口包含最多 16 个引脚（PIN0 ~ PIN15）。
每个引脚可以配置为输入、输出、复用功能或模拟功能。
复用功能：

GPIO 引脚可以通过寄存器配置为不同的复用功能，例如 USART、SPI、ADC 等。
非 GPIO 引脚：

某些引脚可能仅用于电源、时钟输入（如 VDD、VSS、OSC_IN）或调试功能（如 SWDIO、SWCLK），这些引脚无法配置为 GPIO。

### 2. GPIO 引脚控制方式
STM32 的 GPIO 引脚通过寄存器可以访问和修改其状态，包括设置输出值、读取输入状态、配置引脚模式等。寄存器映射如下：

（1）主要 GPIO 寄存器
寄存器名称	作用	每个端口的偏移
GPIOx_MODER	配置引脚模式（输入/输出/复用/模拟）	0x00
GPIOx_OTYPER	配置引脚输出类型（推挽/开漏）	0x04
GPIOx_OSPEEDR	配置引脚输出速度	0x08
GPIOx_PUPDR	配置引脚上下拉	0x0C
GPIOx_IDR	读取引脚输入数据	0x10
GPIOx_ODR	设置引脚输出数据	0x14
GPIOx_BSRR	单独置位或清零某引脚	0x18
GPIOx_AFRL	配置引脚复用功能（引脚 0~7）	0x20
GPIOx_AFRH	配置引脚复用功能（引脚 8~15）	0x24
（2）通过寄存器控制引脚
设置 GPIO 输出值： 通过 GPIOx_ODR 或 GPIOx_BSRR，可以直接修改输出值：

c
复制
编辑
GPIOA->ODR |= (1 << 5);  // 设置 GPIOA 第 5 引脚为高电平
GPIOA->ODR &= ~(1 << 5); // 设置 GPIOA 第 5 引脚为低电平
或者使用 GPIOx_BSRR：

c
复制
编辑
GPIOA->BSRR = (1 << 5);       // 设置 GPIOA 第 5 引脚为高电平
GPIOA->BSRR = (1 << (5 + 16)); // 设置 GPIOA 第 5 引脚为低电平
读取 GPIO 输入值： 使用 GPIOx_IDR 读取引脚的输入状态：

c
复制
编辑
uint8_t pin_state = (GPIOA->IDR & (1 << 5)) ? 1 : 0; // 读取 GPIOA 第 5 引脚状态
配置 GPIO 模式： 使用 GPIOx_MODER 配置引脚模式：

c
复制
编辑
GPIOA->MODER &= ~(3 << (5 * 2)); // 清除 GPIOA 第 5 引脚模式
GPIOA->MODER |= (1 << (5 * 2));  // 设置为输出模式

### 3. 通过寄存器访问和修改 GPIO 引脚状态
STM32 的 GPIO 引脚完全可以通过寄存器直接访问和修改状态，包括：

模式设置：通过 MODER 配置引脚为输入、输出、复用或模拟模式。
值设置：通过 ODR 或 BSRR 设置引脚输出值。
状态读取：通过 IDR 读取引脚的输入状态。
这些寄存器通过直接操作硬件地址映射实现快速访问，因此非常高效。

### 4. 示例代码：点亮一个 LED
假设 GPIOA 的 PIN5 连接到 LED，通过代码点亮 LED：

```c
#include "stm32f4xx.h" // 包含寄存器定义

int main(void) {
    // 使能 GPIOA 时钟
    RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;

    // 配置 GPIOA PIN5 为输出模式
    GPIOA->MODER &= ~(3 << (5 * 2)); // 清除模式位
    GPIOA->MODER |= (1 << (5 * 2));  // 设置为输出模式

    // 设置 PIN5 为高电平（点亮 LED）
    GPIOA->BSRR = (1 << 5);

    while (1) {
        // 保持 LED 点亮
    }
}
```
实际在自己开发上操作，打开PH10, 11 12端口，连接到底板LED, 用GPIOH
---
---
在 STM32 微控制器中，`GPIOA` 中的 `A` 表示一个 **GPIO 端口** 的名称，是硬件结构中的一个分组概念。STM32 的 GPIO 引脚被分成多个端口，每个端口以字母命名，如 `GPIOA`、`GPIOB`、`GPIOC`，直到 `GPIOK`（具体支持的端口数量取决于芯片型号）。


### **GPIO 端口的含义**
1. **GPIO 端口**：
   - STM32 的 GPIO 引脚是以 **端口（Port）+ 引脚（Pin）** 的形式组织的。
   - 每个端口最多有 16 个引脚（编号从 0 到 15），比如 `GPIOA` 的引脚是 `PA0` 到 `PA15`。
   - 所有 GPIO 端口共享相同的控制寄存器结构（如 `GPIOx_MODER`, `GPIOx_ODR` 等），只是在实际操作中通过不同端口的基地址区分。

2. **端口名称与芯片结构**：
   - 不同型号的 STM32 微控制器支持的端口数量不同。例如：
     - STM32F103 系列可能支持 `GPIOA` 到 `GPIOG`。
     - STM32F429 系列支持更多端口（`GPIOA` 到 `GPIOK`）。
   - 未使用的端口（如芯片上不存在的 GPIO）无法访问，也不会启用时钟。

---

### **示例：`GPIOA` 的引脚编号**
假设我们使用 `STM32F429` 微控制器，其 `GPIOA` 的引脚包括：
- `PA0`, `PA1`, `PA2`, ..., `PA15`

每个引脚可以分别配置为输入、输出或复用功能等。例如：
- `PA0` 可以用于外部中断输入。
- `PA5` 可能连接到一个 LED 或外设功能（如 SPI 时钟）。

---

### **为什么用端口分组？**
1. **方便管理**：
   - 每个端口最多包含 16 个引脚，方便分组和配置。
   - 相同端口的引脚共享同一组寄存器（如模式寄存器、输出数据寄存器等），减少了寄存器数量。

2. **灵活复用**：
   - 每个端口的引脚可以配置为不同功能。例如：
     - `GPIOA` 的部分引脚可以用于 SPI 通信。
     - 其他引脚可以配置为普通的 GPIO 输入/输出。

---

### **总结**
- **`GPIOA` 的 `A` 是端口的名称**，它只是芯片中多个 GPIO 分组中的一个。
- STM32 中的 GPIO 按端口分组（`GPIOA`, `GPIOB`, `GPIOC`, ...），每个端口最多 16 个引脚（`PIN0` ~ `PIN15`）。
- **通过寄存器地址映射和启用对应的时钟，可以控制每个端口及其引脚的功能和状态。**