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
## SCB_Type

SCB是System Control Block的缩写，属于ARM Cortex-M内核的一部分，用于系统级别的配置和控制。这些寄存器通常处理异常、中断、系统复位、以及处理器特性等信息。结构体中的每个成员对应一 个特定的寄存器，它们的偏移地址相对于SCB的基地址。

1. CPUID：这是CPUID基址寄存器，只读。它存储处理器的版本、架构等信息。读取这个寄存器可以确定处理器的型号和特性。

2. ICSR：中断控制和状态寄存器，可读写。这个寄存器用于控制NMI、PendSV等系统异常，以及查看当前中断的状态，比如是否有挂起的异常。

3. VTOR：向量表偏移寄存器，设置向量表的起始地址。这在重定位中断向量表时非常重要，比如将向量表放在Flash或RAM的不同位置。

4. AIRCR：应用中断和复位控制寄存器。这个寄存器用于控制系统的复位，设置中断优先级分组，以及端序（大端或小端模式）。

5. SCR：系统控制寄存器，用于管理低功耗模式，比如Sleep和Deep Sleep，以及控制是否在异常返回时进入线程模式或处理模式。

6. CCR：配置控制寄存器，用于配置处理器的行为，例如使能除法零陷阱、对齐检查等。

7. SHP数组：系统处理程序优先级寄存器。这些寄存器设置系统异常（如SysTick、PendSV、中断）的优先级。每个寄存器对应4个优先级字段，可能每个占8位，所以有12个字节，覆盖12个异常。

8. SHCSR：系统处理程序控制和状态寄存器，用于使能或禁用特定的系统异常，并查看它们的状态，比如是否激活或挂起。

9. CFSR：可配置故障状态寄存器，包含MemManage、BusFault和UsageFault的状态信息。这三个错误的具体原因会记录在这里。

10. HFSR：硬故障状态寄存器，当发生无法被更优先异常处理的错误时，记录原因，比如调试事件或向量表读取错误。

11. DFSR：调试故障状态寄存器，记录调试事件的原因，例如断点或观察点触发。

12. MMFAR和BFAR：分别记录内存管理错误和总线错误的地址，帮助调试时确定问题发生的位置。

13. AFSR：辅助故障状态寄存器，由芯片厂商定义，可能提供额外的错误信息。

14. PFR、DFR、ADR、MMFR、ISAR系列寄存器：这些是处理器特性寄存器，只读，用于识别处理器的功能，如支持的调试特性、内存模型、指令集等。

15. CPACR：协处理器访问控制寄存器，用于启用或禁用浮点单元（FPU）等协处理器的访问权限。



---

### **SCB_Type 结构体解析**
**SCB (System Control Block)** 是 ARM Cortex-M 内核的核心系统控制模块，用于管理异常、中断、复位、处理器特性及调试功能。各寄存器功能及偏移如下：

---

#### **1. 基本控制与状态寄存器**
| 寄存器       | 偏移  | 类型   | 功能描述                                                                 |
|--------------|-------|--------|--------------------------------------------------------------------------|
| **CPUID**    | 0x000 | 只读   | 存储处理器 ID、架构版本等信息（如 Cortex-M4 的版本号）。                |
| **ICSR**     | 0x004 | 读写   | 中断控制与状态：挂起/清除 NMI、PendSV、SysTick，查看当前中断状态。      |
| **VTOR**     | 0x008 | 读写   | 向量表偏移地址：设置中断向量表的位置（需对齐，如 512 字节）。           |
| **AIRCR**    | 0x00C | 读写   | 应用中断与复位控制：系统复位、中断优先级分组、端序设置（需写钥匙 `0x5FA`）。 |
| **SCR**      | 0x010 | 读写   | 系统控制：低功耗模式（Sleep/Deep Sleep）、异常返回行为控制。            |
| **CCR**      | 0x014 | 读写   | 配置控制：使能对齐检查、除法零陷阱等处理器行为。                        |

---

#### **2. 异常与中断管理**
| 寄存器       | 偏移  | 类型   | 功能描述                                                                 |
|--------------|-------|--------|--------------------------------------------------------------------------|
| **SHP[12]**  | 0x018 | 读写   | 系统异常优先级：设置 SysTick、PendSV 等系统异常的优先级（每字节一个优先级）。 |
| **SHCSR**    | 0x024 | 读写   | 系统异常控制与状态：使能/禁用 MemManage、BusFault 等异常，查看激活状态。|

---

#### **3. 故障诊断寄存器**
| 寄存器       | 偏移  | 类型   | 功能描述                                                                 |
|--------------|-------|--------|--------------------------------------------------------------------------|
| **CFSR**     | 0x028 | 读写   | 可配置故障状态：包含 MemManage、BusFault、UsageFault 的具体错误原因。   |
| **HFSR**     | 0x02C | 读写   | 硬故障状态：记录无法处理的严重错误（如向量表读取失败）。                |
| **DFSR**     | 0x030 | 读写   | 调试故障状态：记录断点、观察点等调试事件。                              |
| **MMFAR**    | 0x034 | 读写   | MemManage 故障地址：触发内存保护错误的访问地址。                        |
| **BFAR**     | 0x038 | 读写   | BusFault 故障地址：触发总线错误的访问地址。                             |
| **AFSR**     | 0x03C | 读写   | 辅助故障状态：芯片厂商自定义的额外错误信息。                            |

---

#### **4. 处理器特性识别**
| 寄存器       | 偏移  | 类型   | 功能描述                                                                 |
|--------------|-------|--------|--------------------------------------------------------------------------|
| **PFR[2]**   | 0x040 | 只读   | 处理器特性：支持的指令集（Thumb、ARM）、调试模式等。                    |
| **DFR**      | 0x048 | 只读   | 调试特性：支持的调试功能（如断点数量）。                                 |
| **ADR**      | 0x04C | 只读   | 辅助特性：保留供未来扩展。                                               |
| **MMFR[4]**  | 0x050 | 只读   | 内存模型特性：内存保护/管理单元（MPU/MMU）支持信息。                     |
| **ISAR[5]**  | 0x060 | 只读   | 指令集属性：支持的指令集扩展（如除法指令、SIMD）。                       |

---

#### **5. 协处理器控制**
| 寄存器       | 偏移  | 类型   | 功能描述                                                                 |
|--------------|-------|--------|--------------------------------------------------------------------------|
| **CPACR**    | 0x088 | 读写   | 协处理器访问控制：启用浮点单元（FPU）等协处理器的访问权限（如 Cortex-M4 的 FPU 使能）。 |

---

#### **保留空间**
- **RESERVED0[5]**：填充地址 0x054–0x084，确保 `CPACR` 对齐到 0x088。

---

### **关键编程注意事项**
1. **寄存器访问**：通过内存映射访问（如 `SCB->VTOR = 0x20000000`）。
2. **特权级**：多数寄存器需在特权模式（非用户模式）下访问。
3. **写保护**：某些寄存器（如 AIRCR）需写入特定钥匙值（如 `0x5FA << 16`）。
4. **对齐要求**：如 VTOR 需对齐到向量表大小（如 512 字节对齐）。

---

### **示例应用场景**
1. **重定位向量表**：
   ```c
   SCB->VTOR = 0x08004000; // 将向量表设置在 Flash 的 0x08004000
   ```
2. **触发系统复位**：
   ```c
   SCB->AIRCR = (0x5FA << 16) | (1 << 2); // 写入钥匙值并请求复位
   ```
3. **启用 FPU（Cortex-M4）**：
   ```c
   SCB->CPACR |= (0xF << 20); // 允许 CP10/CP11（FPU）的完全访问
   ```

---

## __NVIC_SetPriorityGrouping(uint32_t PriorityGroup)
```c
__STATIC_INLINE void __NVIC_SetPriorityGrouping(uint32_t PriorityGroup) 
{
  uint32_t reg_value;
  // 限制分组值在0-7范围（实际Cortex-M仅支持0-7分组）
  uint32_t PriorityGroupTmp = (PriorityGroup & (uint32_t)0x07UL); 

  // 读取AIRCR寄存器当前值
  reg_value = SCB->AIRCR;

  // 清除关键位：写保护密钥(VECTKEY)和优先级分组位(PRIGROUP)
  reg_value &= ~((uint32_t)(SCB_AIRCR_VECTKEY_Msk | SCB_AIRCR_PRIGROUP_Msk));

  // 组合新值：写入密钥0x5FA + 新优先级分组
  reg_value = (reg_value | 
              ((uint32_t)0x5FAUL << SCB_AIRCR_VECTKEY_Pos) | 
              (PriorityGroupTmp << SCB_AIRCR_PRIGROUP_Pos));

  // 写入修改后的值到AIRCR寄存器
  SCB->AIRCR = reg_value;
}
```