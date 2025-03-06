# All In One

## prvSetupHardware()

### **1. 函数声明**
```c
static void prvSetupHardware( void )
```
- `static`：表示该函数的作用域仅限于当前文件，无法被其他文件调用。
- `prvSetupHardware`：函数名，表明其用途是设置硬件。

---

### **2. 时钟初始化**
#### **2.1 复位时钟配置**
```c
RCC_DeInit();
```
- 调用 `RCC_DeInit()` 将复位时钟控制寄存器（RCC）到默认状态，确保时钟配置从已知状态开始。

#### **2.2 启用外部高速时钟 (HSE)**
```c
RCC_HSEConfig( RCC_HSE_ON );
while( RCC_GetFlagStatus( RCC_FLAG_HSERDY ) == RESET )
{
}
```
- `RCC_HSEConfig(RCC_HSE_ON)`：启用外部高速振荡器（HSE）。
- `RCC_GetFlagStatus(RCC_FLAG_HSERDY)`：等待 HSE 稳定并准备好使用。如果未准备好，则进入循环等待。

#### **2.3 配置 Flash 等待周期**
```c
*( ( unsigned long * ) 0x40022000 ) = 0x02;
```
- 直接操作 Flash 存储器的访问控制寄存器（地址 `0x40022000`），设置 2 个等待周期。这是为了适应更高的时钟频率（如 72 MHz），防止 Flash 访问速度不足导致错误。

#### **2.4 配置 AHB/APB 总线时钟**
```c
RCC_HCLKConfig( RCC_SYSCLK_Div1 );
RCC_PCLK2Config( RCC_HCLK_Div1 );
RCC_PCLK1Config( RCC_HCLK_Div2 );
```
- `RCC_HCLKConfig(RCC_SYSCLK_Div1)`：将 AHB 总线时钟（HCLK）设置为与系统时钟（SYSCLK）相同。
- `RCC_PCLK2Config(RCC_HCLK_Div1)`：将 APB2 总线时钟（PCLK2）设置为与 HCLK 相同。
- `RCC_PCLK1Config(RCC_HCLK_Div2)`：将 APB1 总线时钟（PCLK1）设置为 HCLK 的一半。

#### **2.5 配置 PLL（锁相环）**
```c
RCC_PLLConfig( RCC_PLLSource_HSE_Div1, RCC_PLLMul_9 );
RCC_PLLCmd( ENABLE );
while( RCC_GetFlagStatus( RCC_FLAG_PLLRDY ) == RESET )
{
}
```
- `RCC_PLLConfig(RCC_PLLSource_HSE_Div1, RCC_PLLMul_9)`：配置 PLL 使用 HSE 作为输入源，并将其倍频 9 倍（8 MHz × 9 = 72 MHz）。
- `RCC_PLLCmd(ENABLE)`：启用 PLL。
- `RCC_GetFlagStatus(RCC_FLAG_PLLRDY)`：等待 PLL 稳定。

#### **2.6 切换系统时钟源为 PLL**
```c
RCC_SYSCLKConfig( RCC_SYSCLKSource_PLLCLK );
while( RCC_GetSYSCLKSource() != 0x08 )
{
}
```
- `RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK)`：将系统时钟源切换为 PLL 输出。
- `RCC_GetSYSCLKSource()`：等待确认系统时钟源已切换为 PLL（返回值 `0x08` 表示 PLL 已成为系统时钟源）。

---

### **3. 外设时钟使能**
#### **3.1 使能 GPIO 和 AFIO 时钟**
```c
RCC_APB2PeriphClockCmd( RCC_APB2Periph_GPIOA | RCC_APB2Periph_GPIOB | RCC_APB2Periph_GPIOC
                        | RCC_APB2Periph_GPIOD | RCC_APB2Periph_GPIOE | RCC_APB2Periph_AFIO, ENABLE );
```
- 启用 GPIOA、GPIOB、GPIOC、GPIOD、GPIOE 和 AFIO（复用功能 I/O）的时钟，以便后续可以使用这些外设。

#### **3.2 使能 SPI2 时钟**
```c
RCC_APB1PeriphClockCmd( RCC_APB1Periph_SPI2, ENABLE );
```
- 启用 SPI2 的时钟，用于串行通信。

---

### **4. 中断向量表配置**
```c
NVIC_SetVectorTable( NVIC_VectTab_FLASH, 0x0 );
```
- 设置中断向量表的基地址为 Flash 的起始地址（`0x08000000`）。

---

### **5. 中断优先级分组**
```c
NVIC_PriorityGroupConfig( NVIC_PriorityGroup_4 );
```
- 配置中断优先级分组为 `NVIC_PriorityGroup_4`，即抢占优先级占 4 位，子优先级占 0 位。

---

### **6. SysTick 时钟源配置**
```c
SysTick_CLKSourceConfig( SysTick_CLKSource_HCLK );
```
- 配置 SysTick 定时器的时钟源为 HCLK（AHB 总线时钟）。

---

### **7. 初始化外设测试**
```c
vParTestInitialise();
```
- 调用 `vParTestInitialise()` 函数，可能是用于初始化某些外设或测试功能。

---

## __HAL_RCC_TIM1_CLK_ENABLE()

---

### **1. 宏定义**
```c
#define __HAL_RCC_TIM1_CLK_ENABLE()     do { \
```
- 使用 `#define` 定义了一个宏 `__HAL_RCC_TIM1_CLK_ENABLE()`。
- 宏的内容被包裹在一个 `do { ... } while(0U)` 结构中：
  - 这是嵌入式编程中常见的技巧，用于确保宏在任何上下文中都能安全地展开为一个完整的语句块。
  - 即使宏在 `if` 或 `else` 等条件语句中使用，也不会因为缺少大括号而导致语法错误。

---

### **2. 变量声明**
```c
__IO uint32_t tmpreg = 0x00U;
```
- 声明了一个 `__IO` 类型的变量 `tmpreg`，并初始化为 `0x00U`。
  - `__IO` 是一个宏，通常定义为 `volatile`，表示该变量可能会被硬件直接修改，编译器不应对其进行优化。
  - `tmpreg` 的作用是临时存储寄存器的值，用于后续操作。

---

### **3. 启用 TIM1 时钟**
```c
SET_BIT(RCC->APB2ENR, RCC_APB2ENR_TIM1EN);
```
- 调用 `SET_BIT` 宏，将 `RCC->APB2ENR` 寄存器中的 `TIM1EN` 位设置为 1。
  - `RCC->APB2ENR`：指向 RCC（复位和时钟控制）外设的 APB2 总线时钟使能寄存器。
  - `RCC_APB2ENR_TIM1EN`：表示 TIM1 的时钟使能位。
  - 通过设置该位，启用了 TIM1 的时钟。

---

### **4. 延迟以确保时钟稳定**
```c
tmpreg = READ_BIT(RCC->APB2ENR, RCC_APB2ENR_TIM1EN);
UNUSED(tmpreg);
```
- **读取寄存器值**：
  ```c
  tmpreg = READ_BIT(RCC->APB2ENR, RCC_APB2ENR_TIM1EN);
  ```
  - 调用 `READ_BIT` 宏，读取 `RCC->APB2ENR` 寄存器中 `TIM1EN` 位的值，并将其存储到 `tmpreg` 中。
  - 这一步的目的是确保写入操作已经生效，并且硬件已经更新了寄存器的状态。

- **标记未使用的变量**：
  ```c
  UNUSED(tmpreg);
  ```
  - `UNUSED` 是一个宏，通常用于告诉编译器某个变量虽然被声明但并未实际使用，避免编译器发出未使用变量的警告。
  - 在这里，`tmpreg` 的读取操作主要是为了引入一个短暂的延迟，确保时钟配置完成。

---

### **5. 结束宏定义**
```c
} while(0U)
```
- `while(0U)` 确保整个宏展开后是一个完整的语句块。
- `0U` 表示无符号整数常量 0，符合 C 标准的写法。

---

### **总结**
这段代码的核心功能是启用 TIM1 的时钟，具体步骤如下：
1. **启用时钟**：通过设置 `RCC->APB2ENR` 寄存器中的 `TIM1EN` 位，启用 TIM1 的时钟。
2. **读取确认**：读取寄存器中的 `TIM1EN` 位，确保写入操作生效。
3. **延迟处理**：通过读取寄存器引入一个短暂的延迟，确保时钟信号稳定。
4. **避免警告**：使用 `UNUSED` 宏标记未使用的变量，避免编译器警告。

这种设计模式在嵌入式开发中非常常见，尤其是在 STM32 HAL 库中，用于确保硬件资源的正确配置和稳定性。

## HAL_RCC_GetClockConfig()

---

### **1. 函数声明**
```c
void HAL_RCC_GetClockConfig(RCC_ClkInitTypeDef *RCC_ClkInitStruct, uint32_t *pFLatency)
```
- **参数**：
  - `RCC_ClkInitStruct`：指向一个 `RCC_ClkInitTypeDef` 结构体的指针，用于存储当前的时钟配置。
  - `pFLatency`：指向一个 `uint32_t` 类型的变量，用于存储 Flash 的等待状态（Latency）。

- **功能**：
  - 该函数从硬件寄存器中读取当前的时钟配置，并将结果填充到 `RCC_ClkInitStruct` 和 `pFLatency` 中。

---

### **2. 设置所有可能的时钟类型**
```c
RCC_ClkInitStruct->ClockType = RCC_CLOCKTYPE_SYSCLK | RCC_CLOCKTYPE_HCLK | RCC_CLOCKTYPE_PCLK1 | RCC_CLOCKTYPE_PCLK2;
```
- **作用**：
  - 将 `RCC_ClkInitStruct->ClockType` 设置为包含所有可能的时钟类型。
  - 这一步是为了标记需要读取的所有时钟信号类型，包括：
    - `RCC_CLOCKTYPE_SYSCLK`：系统时钟（SYSCLK）。
    - `RCC_CLOCKTYPE_HCLK`：AHB 总线时钟（HCLK）。
    - `RCC_CLOCKTYPE_PCLK1`：APB1 总线时钟（PCLK1）。
    - `RCC_CLOCKTYPE_PCLK2`：APB2 总线时钟（PCLK2）。

- **注意**：
  - 实际上，这里设置的 `ClockType` 只是一个标志位掩码，表示后续会读取这些时钟类型的配置。

---

### **3. 获取 SYSCLK 配置**
```c
RCC_ClkInitStruct->SYSCLKSource = (uint32_t)(RCC->CFGR & RCC_CFGR_SW);
```
- **作用**：
  - 从 `RCC->CFGR` 寄存器中读取当前的系统时钟源（SYSCLK Source）。
  - `RCC_CFGR_SW` 是一个掩码，用于提取 `CFGR` 寄存器中与系统时钟源相关的位。

- **背景**：
  - `RCC->CFGR` 是 STM32 的时钟配置寄存器，其中 `SW` 字段表示当前的系统时钟源：
    - `0x00`：HSI（高速内部振荡器）。
    - `0x01`：HSE（高速外部振荡器）。
    - `0x02`：PLL（锁相环输出）。

---

### **4. 获取 HCLK 配置**
```c
RCC_ClkInitStruct->AHBCLKDivider = (uint32_t)(RCC->CFGR & RCC_CFGR_HPRE);
```
- **作用**：
  - 从 `RCC->CFGR` 寄存器中读取 AHB 总线时钟（HCLK）的分频系数。
  - `RCC_CFGR_HPRE` 是一个掩码，用于提取 `CFGR` 寄存器中与 AHB 分频相关的位。

- **背景**：
  - `HPRE` 字段决定了 HCLK 相对于 SYSCLK 的分频比例。例如：
    - `0x00`：不分频（HCLK = SYSCLK）。
    - `0x08`：分频 2（HCLK = SYSCLK / 2）。
    - 其他值表示更高的分频比例。

---

### **5. 获取 APB1 配置**
```c
RCC_ClkInitStruct->APB1CLKDivider = (uint32_t)(RCC->CFGR & RCC_CFGR_PPRE1);
```
- **作用**：
  - 从 `RCC->CFGR` 寄存器中读取 APB1 总线时钟（PCLK1）的分频系数。
  - `RCC_CFGR_PPRE1` 是一个掩码，用于提取 `CFGR` 寄存器中与 APB1 分频相关的位。

- **背景**：
  - `PPRE1` 字段决定了 PCLK1 相对于 HCLK 的分频比例。例如：
    - `0x00`：不分频（PCLK1 = HCLK）。
    - `0x04`：分频 2（PCLK1 = HCLK / 2）。
    - 其他值表示更高的分频比例。

---

### **6. 获取 APB2 配置**
```c
RCC_ClkInitStruct->APB2CLKDivider = (uint32_t)((RCC->CFGR & RCC_CFGR_PPRE2) >> 3U);
```
- **作用**：
  - 从 `RCC->CFGR` 寄存器中读取 APB2 总线时钟（PCLK2）的分频系数。
  - `RCC_CFGR_PPRE2` 是一个掩码，用于提取 `CFGR` 寄存器中与 APB2 分频相关的位。
  - `>> 3U`：将 `PPRE2` 字段右移 3 位，以便对齐到最低有效位。

- **背景**：
  - `PPRE2` 字段决定了 PCLK2 相对于 HCLK 的分频比例，与 `PPRE1` 类似。

---

### **7. 获取 Flash 等待状态（Latency）**
```c
*pFLatency = (uint32_t)(FLASH->ACR & FLASH_ACR_LATENCY);
```
- **作用**：
  - 从 `FLASH->ACR` 寄存器中读取 Flash 的等待状态（Latency）。
  - `FLASH_ACR_LATENCY` 是一个掩码，用于提取 `ACR` 寄存器中与等待状态相关的位。

- **背景**：
  - Flash 等待状态是为了确保在高频率下访问 Flash 存储器时不会发生数据错误。
  - 不同的系统时钟频率需要不同的等待状态。例如：
    - 0 等待状态：适用于低频率（如 ≤ 24 MHz）。
    - 1 等待状态：适用于中等频率（如 ≤ 48 MHz）。
    - 2 等待状态：适用于高频率（如 ≤ 72 MHz）。

---

### **8. 总结**
该函数的主要功能是从硬件寄存器中读取当前的时钟配置，并将其存储到 `RCC_ClkInitStruct` 和 `pFLatency` 中。具体步骤如下：

1. **设置时钟类型**：标记需要读取的所有时钟信号类型。
2. **读取系统时钟源（SYSCLK）**：从 `RCC->CFGR` 中提取当前的系统时钟源。
3. **读取 AHB 总线时钟（HCLK）**：从 `RCC->CFGR` 中提取 HCLK 的分频系数。
4. **读取 APB1 总线时钟（PCLK1）**：从 `RCC->CFGR` 中提取 PCLK1 的分频系数。
5. **读取 APB2 总线时钟（PCLK2）**：从 `RCC->CFGR` 中提取 PCLK2 的分频系数。
6. **读取 Flash 等待状态（Latency）**：从 `FLASH->ACR` 中提取 Flash 的等待状态。

---

### **9. 使用场景**
该函数通常用于以下场景：
- **调试**：检查当前的时钟配置是否符合预期。
- **动态调整**：在运行时获取当前的时钟配置，以便根据需求进行调整。
- **恢复默认配置**：在某些情况下，可能需要保存或恢复系统的时钟配置。

---

### **10. 示例代码**
以下是一个使用该函数的示例：

```c
RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
uint32_t pFLatency = 0;

// 获取当前的时钟配置
HAL_RCC_GetClockConfig(&RCC_ClkInitStruct, &pFLatency);

// 打印结果
printf("SYSCLK Source: %lu\n", RCC_ClkInitStruct.SYSCLKSource);
printf("HCLK Divider: %lu\n", RCC_ClkInitStruct.AHBCLKDivider);
printf("PCLK1 Divider: %lu\n", RCC_ClkInitStruct.APB1CLKDivider);
printf("PCLK2 Divider: %lu\n", RCC_ClkInitStruct.APB2CLKDivider);
printf("Flash Latency: %lu\n", pFLatency);
```

## RCC_TypeDef
---

### 1. **代码结构分析**
```c
typedef struct
{
  vu32 CR;
  vu32 CFGR;
  vu32 CIR;
  vu32 APB2RSTR;
  vu32 APB1RSTR;
  vu32 AHBENR;
  vu32 APB2ENR;
  vu32 APB1ENR;
  vu32 BDCR;
  vu32 CSR;
} RCC_TypeDef;
```

#### **1.1 `typedef` 的作用**
- `typedef` 是 C 语言中的关键字，用于为已有的数据类型定义一个新的别名。
- 在这里，`typedef` 将一个匿名结构体类型定义为 `RCC_TypeDef`，使得后续可以直接使用 `RCC_TypeDef` 来声明变量，而无需每次都写 `struct`。

#### **1.2 结构体成员**
- 结构体中包含多个成员，每个成员的类型是 `vu32`。
- 这些成员的名称（如 `CR`, `CFGR`, `CIR` 等）通常与硬件寄存器相关，具体含义需要结合上下文（例如微控制器的参考手册）来理解。

#### **1.3 `vu32` 的含义**
- `vu32` 很可能是一个自定义的数据类型，通常通过宏定义或类型定义实现。
- 根据常见的嵌入式开发习惯，`vu32` 可能表示 "volatile unsigned int"，即：
  ```c
  typedef volatile unsigned int vu32;
  ```
  - **`volatile`**: 表示该变量可能会被程序外部（如硬件中断）修改，编译器在优化时不会假设其值不变。
  - **`unsigned int`**: 表示无符号整数，通常是 32 位宽（具体宽度取决于目标平台）。

---

### 2. **代码用途**
这段代码通常出现在嵌入式系统开发中，特别是基于 ARM Cortex-M 系列微控制器的项目中（如 STM32）。以下是其可能的用途：

#### **2.1 硬件寄存器映射**
- `RCC_TypeDef` 结构体用于描述微控制器中复位和时钟控制（Reset and Clock Control, RCC）模块的寄存器布局。
- 每个成员对应一个具体的硬件寄存器，例如：
  - `CR`: 时钟控制寄存器（Clock Control Register）。
  - `CFGR`: 时钟配置寄存器（Clock Configuration Register）。
  - `AHBENR`: AHB 外设时钟使能寄存器（AHB Peripheral Clock Enable Register）。
  - 其他成员类似，分别控制不同的功能模块。

#### **2.2 内存映射访问**
- 在嵌入式系统中，外设寄存器通常通过内存映射的方式访问。
- 使用这种结构体定义后，可以通过指针将寄存器地址映射到结构体变量上。例如：
  ```c
  #define RCC_BASE_ADDR 0x40021000  // 假设 RCC 寄存器基地址
  #define RCC ((RCC_TypeDef *) RCC_BASE_ADDR)

  // 访问寄存器
  RCC->CR = 0x00000001;  // 修改 CR 寄存器的值
  ```

---

## RCC_DeInit()
---

### **1. 函数功能概述**
`RCC_DeInit` 函数的主要目的是将 RCC 模块恢复到默认状态。具体来说：
- 禁用所有外设的复位和时钟。
- 配置系统时钟源为内部高速振荡器（HSI）。
- 清除各种配置位和中断标志。

这种操作通常在系统启动时执行，以确保硬件处于已知的初始状态。

---

### **2. 代码逐行解读**

#### **2.1 禁用 APB2 和 APB1 外设复位**
```c
RCC->APB2RSTR = 0x00000000;
RCC->APB1RSTR = 0x00000000;
```
- `APB2RSTR` 和 `APB1RSTR` 是 APB2 和 APB1 总线上的外设复位寄存器。
- 将这些寄存器清零，表示禁用对 APB2 和 APB1 上所有外设的复位信号。

---

#### **2.2 启用 FLITF 和 SRAM 时钟**
```c
RCC->AHBENR = 0x00000014;
```
- `AHBENR` 是 AHB 总线上的外设时钟使能寄存器。
- `0x00000014` 的二进制形式为 `0000 0000 0000 0000 0001 0100`，表示启用了以下两个时钟：
  - **FLITF**: Flash 接口时钟（用于访问 Flash 存储器）。
  - **SRAM**: 内部 SRAM 时钟。

---

#### **2.3 禁用 APB2 和 APB1 外设时钟**
```c
RCC->APB2ENR = 0x00000000;
RCC->APB1ENR = 0x00000000;
```
- `APB2ENR` 和 `APB1ENR` 是 APB2 和 APB1 总线上的外设时钟使能寄存器。
- 将这些寄存器清零，表示禁用 APB2 和 APB1 上所有外设的时钟。

---

#### **2.4 启用 HSI（内部高速振荡器）**
```c
RCC->CR |= (u32)0x00000001;
```
- `CR` 是时钟控制寄存器。
- `0x00000001` 的最低位是 **HSION** 位，设置该位会启用内部高速振荡器（HSI）。
- HSI 是微控制器的默认时钟源之一，通常在系统启动时使用。

---

#### **2.5 重置系统时钟配置**
```c
RCC->CFGR &= 0xF8FF0000;
```
- `CFGR` 是时钟配置寄存器。-0
- `0xF8FF0000` 是一个掩码，用于清除以下位：
  - **SW[1:0]**: 系统时钟切换位。
  - **HPRE[3:0]**: AHB 预分频器配置。
  - **PPRE1[2:0]** 和 **PPRE2[2:0]**: APB1 和 APB2 预分频器配置。
  - **ADCPRE[1:0]**: ADC 预分频器配置。
  - **MCO[2:0]**: 微控制器时钟输出配置。
- 这些位被清零后，系统时钟配置恢复到默认状态。

---

#### **2.6 重置 HSE、CSS 和 PLL 配置**
```c
RCC->CR &= 0xFEF6FFFF;
```
- `0xFEF6FFFF` 是一个掩码，用于清除以下位：
  - **HSEON**: 外部高速振荡器（HSE）使能位。
  - **CSSON**: 时钟安全系统（CSS）使能位。
  - **PLLON**: 锁相环（PLL）使能位。
- 这些位被清零后，HSE、CSS 和 PLL 被禁用。

---

#### **2.7 重置 HSE 旁路模式**
```c
RCC->CR &= 0xFFFBFFFF;
```
- `0xFFFBFFFF` 是一个掩码，用于清除 **HSEBYP** 位。
- **HSEBYP**: 外部高速振荡器旁路模式位。
- 清除该位后，HSE 振荡器工作在正常模式，而不是旁路模式。

---

#### **2.8 重置 PLL 配置**
```c
RCC->CFGR &= 0xFF80FFFF;
```
- `0xFF80FFFF` 是一个掩码，用于清除以下位：
  - **PLLSRC**: PLL 时钟源选择位。
  - **PLLXTPRE**: HSE 分频器用于 PLL 输入。
  - **PLLMUL[3:0]**: PLL 倍频系数。
  - **USBPRE**: USB 预分频器。
- 这些位被清零后，PLL 配置恢复到默认状态。

---

#### **2.9 禁用所有中断**
```c
RCC->CIR = 0x00000000;
```
- `CIR` 是时钟中断寄存器。
- 将该寄存器清零，表示禁用所有与 RCC 相关的中断。

---

## SRAM
STM32微控制器中的SRAM（Static Random Access Memory，静态随机存取存储器）扮演着几个关键角色，其作用如下：

1. 临时数据存储：SRAM用于存储临时数据和变量，这些数据在程序执行过程中需要快速访问和修改。
2. 高速缓存：SRAM可以作为CPU的缓存，提高数据访问速度，因为SRAM的访问速度通常比闪存（Flash）快得多。
3. 中断服务例程：当微控制器响应中断时，中断服务例程（ISR）可能会使用SRAM来存储局部变量，这些变量只在中断处理期间使用。
4. 堆栈：SRAM用于存储程序的调用堆栈，即在函数调用过程中保存返回地址和局部变量。
5. 数据缓冲：在数据传输过程中，SRAM可以作为缓冲区，暂存从外部设备接收的数据或待发送的数据。
6. 配置数据：某些STM32微控制器使用SRAM来存储配置数据，这些数据在系统启动时加载，可能影响系统的运行模式或行为。
7. 实时性能：由于SRAM的读写速度快，它对于需要实时性能的应用至关重要，比如工业控制系统或实时操作系统。
8. 易失性存储：SRAM是易失性的，意味着在断电后数据会丢失。这使得SRAM适合存储那些在系统运行期间需要频繁修改的数据。
9. 低功耗：与动态随机存取存储器（DRAM）相比，SRAM在保持数据时不需要刷新周期，因此功耗较低。
10. 系统初始化：在系统启动过程中，SRAM可能用于存储引导加载程序（Bootloader）的临时数据，直到程序从Flash加载到SRAM并开始执行。

SRAM的区域从地址 0x2000 0000 开始，并可能扩展到 0x3FFF FFFF。但是，实际的结束地址取决于内部SRAM的有效量。例如，对于具有64KB SRAM的MCU，最终地址为0x2000 FFFF。尝试访问此区域之外的位置将导致总线故障异常

## core_cm4.h
cm4核心寄存器定义: 有CMSIS核心的所有结构和符号

#define __O volatile

此行定义__O为volatile,表示只写属性。

#define __IO volatile

此行定义__IO为volatile,表示读写属性。

所以,这段代码主要完成了:

1. 如果__NVIC_PRIO_BITS未定义,则定义中断优先级位数为4。否则忽略。

2. 根据编译器选择定义只读属性__I为volatile或volatile const。

3. 定义只写属性__O为volatile。

4. 定义读写属性__IO为volatile。

5. 这四个属性主要用于定义外设寄存器的访问权限,以确保编译器不会对访问的代码作优化,影响读取的准确性。

也就是说,这段代码为寄存器的访问屏蔽了编译器的优化,在编译过程中让编译器明确区分:

这是一个只读寄存器,值可能会被其他因素改变,读取时总是获取最新值。

这是一个只写寄存器,每次写入的值必须被外设接收。

这个寄存器是可读写的,读取与写入都必须准确地映射到外设。这样可以最大限度地确保我们的程序以预期的方式使用这些寄存器,也不会因为编译器的优化而导致意外的结果。

## NVIC_Type
NVIC_Type结构体包含以下成员:

__IO uint32_t ISER[8]; 中断使能置位寄存器,用于使能中断,数组8个成员对应NVIC的8个中断组。

__IO uint32_t ICER[8]; 中断清除使能寄存器,用于禁止中断,数组8个成员对应NVIC的8个中断组。

__IO uint32_t ISPR[8]; 中断待处理置位寄存器,用于置位某中断的待处理标志,数组8个成员对应NVIC的8个中断组。

__IO uint32_t ICPR[8]; 中断待处理清除寄存器,用于清除某中断的待处理标志,数组8个成员对应NVIC的8个中断组。

__IO uint32_t IABR[8]; 中断激活寄存器,指示哪些中断被激活,数组8个成员对应NVIC的8个中断组。

__IO uint8_t IP[240]; 中断优先级寄存器,设置优先级,共240个成员,每个成员1字节,对应MCU中的240个中断优先级设置。

__O uint32_t STIR; 软件触发中断寄存器,用于软件触发指定的中断。

所以,这个结构体包含了NVIC所有的控制与状态寄存器,通过这些寄存器,我们可以完成:

1. 中断使能与失能设置。

2. 中断待处理标志的置位与清除。

3. 检测哪些中断被激活。

4. 设置各个中断的优先级。

5. 软件触发某个指定的中断。

简单来说,这个结构体高度抽象和集成地代表了NVIC及其所有的功能与控制寄存器,使用时直接通过相应的成员来操作寄存器。

这个NVIC_Type结构体代表的不是某个具体的存储区域,而是概念性地将NVIC所有的寄存器集成在一个结构体中,以方便我们管理和访问这些寄存器。
结构体中的每个成员,如__IO uint32_t ISER[8]都代表NVIC中的一个实际的32位寄存器。
这些寄存器的地址在MCU的外设地址映射中已经固定,结构体将它们逻辑上集成在一起,方便我们按功能管理和访问。
所以,当我们需要操作NVIC使能某个中断时,只需要像下面这样使用ISER成员:

NVIC->ISER[2]|=(1<< 5); // 使能中断组2中的第6个中 这行代码通过NVIC结构体操作寄存器,而ISER[2]则映射到NVIC使能寄存器组2实际的物理地址。

在MDK或IAR等IDE中,这些寄存器的具体地址将在外设地址映射Memap窗口中显示。

例如,ISER[2]可能映射到0xE000E200这样的实际地址,这个地址上的32位寄存器包含对应的位用于使能第6个中断。

所以,总结来说:
1. NVIC_Type 结构体逻辑上将NVIC的所有寄存器集成在一起,方便管理和访问,但本身不代表实际的存储区域。
2. 结构体中的每个成员代表NVIC中的一个实际物理寄存器,映射到固定的地址。
3. 我们通过结构体操作这些寄存器,然后编译器会将其映射到实际的物理地址上。
4. 这些寄存器的具体地址将在MCU的外设地址映射中指定,我们可以在IDE的Memap窗口中查看。
5. 所以结构体更像是一个逻辑上的抽象,将NVIC的寄存器方便地集成在一起,在程序中按功能管理和访问。