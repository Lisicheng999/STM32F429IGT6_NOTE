# core_cm4.h
 `core_cm4.h` 是 **CMSIS（Cortex Microcontroller Software Interface Standard）** 中的核心文件之一，专门用于支持 Cortex-M4 内核的功能。它包含定义、函数和数据结构，方便开发者高效地使用 Cortex-M4 的硬件特性。

---

### **文件内容与功能概述**

#### 1. **处理器和内核相关定义**
   - 提供与 Cortex-M4 内核相关的寄存器和功能接口。
   - 包括：
     - 核心寄存器（如 `NVIC`、`SCB`、`SysTick` 等）。
     - 寄存器偏移量和访问宏。

   示例：
   ```c
   #define SCB_BASE        (0xE000ED00UL) /*!< System Control Block Base Address */
   #define NVIC_BASE       (0xE000E100UL) /*!< Nested Vectored Interrupt Controller Base Address */
   #define SysTick_BASE    (0xE000E010UL) /*!< SysTick Base Address */
   ```

#### 2. **数据类型定义**
   - 提供标准化的数据类型，确保跨平台兼容。
   - 示例：
     ```c
     typedef volatile uint32_t __I;  /*!< Defines 'read only' permissions */
     typedef volatile uint32_t __O;  /*!< Defines 'write only' permissions */
     typedef volatile uint32_t __IO; /*!< Defines 'read / write' permissions */
     ```

#### 3. **内核功能接口**
   - 定义了访问和操作内核功能的 API，比如设置中断优先级、触发中断等。

   示例：
   ```c
   __STATIC_INLINE void NVIC_SetPriority(IRQn_Type IRQn, uint32_t priority) {
       if (IRQn < 0) {
           SCB->SHP[((uint32_t)(IRQn) & 0xF) - 4] = ((priority << (8 - __NVIC_PRIO_BITS)) & 0xFF);
       } else {
           NVIC->IP[(uint32_t)(IRQn)] = ((priority << (8 - __NVIC_PRIO_BITS)) & 0xFF);
       }
   }
   ```

#### 4. **中断管理**
   - 提供了 NVIC（Nested Vectored Interrupt Controller）相关的函数，用于：
     - 设置中断优先级。
     - 使能或禁止中断。
     - 清除中断挂起标志。

   示例：
   ```c
   void NVIC_EnableIRQ(IRQn_Type IRQn);
   void NVIC_DisableIRQ(IRQn_Type IRQn);
   uint32_t NVIC_GetPendingIRQ(IRQn_Type IRQn);
   void NVIC_ClearPendingIRQ(IRQn_Type IRQn);
   ```

#### 5. **系统滴答定时器（SysTick）**
   - 提供系统计时器的初始化和操作函数，用于实现延时或时间片轮转。

   示例：
   ```c
   void SysTick_Config(uint32_t ticks) {
       if ((ticks - 1) > SysTick_LOAD_RELOAD_Msk) return; /* Reload value impossible */
       SysTick->LOAD = (uint32_t)(ticks - 1);             /* Set reload register */
       NVIC_SetPriority(SysTick_IRQn, (1UL << __NVIC_PRIO_BITS) - 1UL); /* Set Priority for Systick Interrupt */
       SysTick->VAL = 0UL;                                /* Load the SysTick Counter Value */
       SysTick->CTRL = SysTick_CTRL_CLKSOURCE_Msk |
                       SysTick_CTRL_TICKINT_Msk |
                       SysTick_CTRL_ENABLE_Msk;          /* Enable SysTick Timer */
   }
   ```

#### 6. **性能优化指令支持**
   - 提供访问 Cortex-M4 特有的硬件功能，比如协处理器、MPU（Memory Protection Unit）等。
   - 包括硬件加速的数学计算指令和字节序转换。

   示例：
   ```c
   __STATIC_INLINE uint32_t __REV(uint32_t value) {
       __ASM volatile ("rev %0, %1" : "=r" (value) : "r" (value));
       return value;
   }
   ```

---

### **文件结构示例**

1. **核心寄存器定义**
   ```c
   typedef struct {
       __IOM uint32_t ISER[8];  /*!< Interrupt Set Enable Register */
       uint32_t RESERVED0[24];
       __IOM uint32_t ICER[8];  /*!< Interrupt Clear Enable Register */
   } NVIC_Type;

   #define NVIC ((NVIC_Type *) NVIC_BASE)
   ```

2. **系统初始化函数**
   - 包括 `SystemInit()`，通常在启动代码中调用，用于设置系统时钟等初始化任务。

   示例：
   ```c
   void SystemInit(void) {
       SCB->VTOR = FLASH_BASE | 0x0; /* Vector Table Relocation */
   }
   ```

3. **宏定义**
   - 提供常用操作的简化宏。

   示例：
   ```c
   #define __enable_irq()  __ASM volatile ("cpsie i" : : : "memory")
   #define __disable_irq() __ASM volatile ("cpsid i" : : : "memory")
   ```

---
