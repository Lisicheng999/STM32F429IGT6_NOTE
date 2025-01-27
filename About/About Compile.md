# STM32F429IGTX开发板工程日志

## 核心板CP2102串口

连接RX, TX口后，软件未识别usb bridge, 下载安装CP2102驱动解决

## 编译时指定宏

在编译 C 或 C++ 代码时，使用 GCC 或 ARMCC（Keil 或 Arm Compiler）都可以通过命令行选项定义宏。这种方式通常用于条件编译或提供配置参数。

1. GCC 指定宏
在 GCC 中，可以通过 -D 选项指定宏。其格式为：

bash
复制
编辑
gcc -D<宏名> file.c -o file

## 编译选项
### C/C++
--c99 -c --cpu Cortex-M4.fp.sp -D__EVAL -D__MICROLIB -g -O3 --apcs=interwork --split_sections -I ../Core/Inc -I ../Drivers/STM32F4xx_HAL_Driver/Inc -I ../Drivers/STM32F4xx_HAL_Driver/Inc/Legacy -I ../Middlewares/Third_Party/FreeRTOS/Source/include -I ../Middlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS_V2 -I ../Middlewares/Third_Party/FreeRTOS/Source/portable/RVDS/ARM_CM4F -I ../Drivers/CMSIS/Device/ST/STM32F4xx/Include -I ../Drivers/CMSIS/Include
-I./RTE/_FreeRTOS_Test
-ID:/Local/Arm/Packs/ARM/CMSIS/6.1.0/CMSIS/Core/Include
-D__UVISION_VERSION="541" -DSTM32F429xx -D_RTE_ -DUSE_HAL_DRIVER -DSTM32F429xx
-o FreeRTOS_Test\*.o --omf_browse FreeRTOS_Test\*.crf --depend FreeRTOS_Test\*.d

### ASM
--cpu Cortex-M4.fp.sp --pd "__EVAL SETA 1" -g --apcs=interwork --pd "__MICROLIB SETA 1" -I ../Core/Inc 

### Linker
--cpu Cortex-M4.fp.sp *.o 
--library_type=microlib --strict --scatter "FreeRTOS_Test\FreeRTOS_Test.sct" 

### 不适用Microlibc
--c99 -c --cpu Cortex-M4.fp.sp -D__EVAL -g -O3 --apcs=interwork --split_sections -I ../Core/Inc -I ../Drivers/STM32F4xx_HAL_Driver/Inc -I ../Drivers/STM32F4xx_HAL_Driver/Inc/Legacy -I ../Middlewares/Third_Party/FreeRTOS/Source/include -I ../Middlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS_V2 -I ../Middlewares/Third_Party/FreeRTOS/Source/portable/RVDS/ARM_CM4F -I ../Drivers/CMSIS/Device/ST/STM32F4xx/Include -I ../Drivers/CMSIS/Include
-I./RTE/_FreeRTOS_Test
-ID:/Local/Arm/Packs/ARM/CMSIS/6.1.0/CMSIS/Core/Include
-D__UVISION_VERSION="541" -DSTM32F429xx -D_RTE_ -DUSE_HAL_DRIVER -DSTM32F429xx
-o FreeRTOS_Test\*.o --omf_browse FreeRTOS_Test\*.crf --depend FreeRTOS_Test\*.d


## 对照
这段代码展示了典型的嵌入式项目（基于 ARM Cortex-M4 微控制器）的编译和链接选项，分为 C/C++ 编译、汇编编译和链接过程。以下是详细分析：

---

### **1. C/C++ 编译部分**
```plaintext
--c99 -c --cpu Cortex-M4.fp.sp -D__EVAL -D__MICROLIB -g -O3 --apcs=interwork --split_sections \
-I ../Core/Inc \
-I ../Drivers/STM32F4xx_HAL_Driver/Inc \
-I ../Drivers/STM32F4xx_HAL_Driver/Inc/Legacy \
-I ../Middlewares/Third_Party/FreeRTOS/Source/include \
-I ../Middlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS_V2 \
-I ../Middlewares/Third_Party/FreeRTOS/Source/portable/RVDS/ARM_CM4F \
-I ../Drivers/CMSIS/Device/ST/STM32F4xx/Include \
-I ../Drivers/CMSIS/Include\
-I./RTE/_FreeRTOS_Test\
-ID:/Local/Arm/Packs/ARM/CMSIS/6.1.0/CMSIS/Core/Include\
-D__UVISION_VERSION="541" \
-DSTM32F429xx \
-D_RTE_ \
-DUSE_HAL_DRIVER \
-DSTM32F429xx \
-o FreeRTOS_Test\*.o \
--omf_browse FreeRTOS_Test\*.crf \
--depend FreeRTOS_Test\*.d
```

#### **关键选项解析**
1. **语言标准与目标：**
   - `--c99`: 使用 C99 标准。
   - `-c`: 仅编译，不链接。
   - `--cpu Cortex-M4.fp.sp`: 目标处理器为 Cortex-M4，支持硬件浮点（单精度 `fp.sp`）。

2. **宏定义：**
   - `-D__EVAL`: 定义宏 `__EVAL`，可能用于条件编译。
   - `-D__MICROLIB`: 使用 ARM 的 Microlib 小型标准库。
   - `-DSTM32F429xx`: 标识芯片型号。
   - `-DUSE_HAL_DRIVER`: 启用 STM32 HAL 驱动。
   - `-D_RTE_`: 用于标识实时操作系统环境（如 Keil RTX 或 FreeRTOS）。
   - `-D__UVISION_VERSION="541"`: 表明 Keil µVision IDE 的版本为 5.41。

3. **优化与调试：**
   - `-g`: 生成调试信息。
   - `-O3`: 启用最高级别的优化，注重执行效率。

4. **内存模型：**
   - `--apcs=interwork`: 使用 ARM/Thumb 混合调用约定。

5. **头文件路径：**
   - 通过多个 `-I` 选项指定包含路径，例如：
     - `../Core/Inc`: 核心代码的头文件。
     - `../Drivers/STM32F4xx_HAL_Driver/Inc`: STM32 HAL 驱动头文件。
     - `../Middlewares/Third_Party/FreeRTOS/Source/include`: FreeRTOS 的核心头文件。
     - `D:/Local/Arm/Packs/ARM/CMSIS/6.1.0/CMSIS/Core/Include`: ARM CMSIS 的核心文件。

6. **输出与依赖文件：**
   - `-o FreeRTOS_Test\*.o`: 输出对象文件。
   - `--omf_browse FreeRTOS_Test\*.crf`: 生成浏览文件，用于代码分析。
   - `--depend FreeRTOS_Test\*.d`: 生成依赖文件，用于记录头文件依赖关系。

7. **分段与链接支持：**
   - `--split_sections`: 按段划分代码，以便链接器优化未使用的部分。

---

### **2. 汇编部分**
```plaintext
--cpu Cortex-M4.fp.sp --pd "__EVAL SETA 1" -g --apcs=interwork --pd "__MICROLIB SETA 1" -I ../Core/Inc
```

#### **关键选项解析**
1. **目标处理器：**
   - `--cpu Cortex-M4.fp.sp`: 目标处理器为 Cortex-M4，启用单精度浮点支持。

2. **宏定义：**
   - `--pd "__EVAL SETA 1"`: 将汇编宏 `__EVAL` 设置为 1。
   - `--pd "__MICROLIB SETA 1"`: 将汇编宏 `__MICROLIB` 设置为 1。

3. **调试信息：**
   - `-g`: 生成调试信息。

4. **内存模型：**
   - `--apcs=interwork`: 使用 ARM/Thumb 混合调用约定。

5. **头文件路径：**
   - `-I ../Core/Inc`: 指定头文件路径，通常用于引用汇编相关的定义文件。

---

### **3. 链接部分**
```plaintext
--cpu Cortex-M4.fp.sp *.o 
--library_type=microlib --strict --scatter "FreeRTOS_Test\FreeRTOS_Test.sct"
```

#### **关键选项解析**
1. **目标处理器：**
   - `--cpu Cortex-M4.fp.sp`: 目标处理器为 Cortex-M4。

2. **输入文件：**
   - `*.o`: 链接所有生成的对象文件。

3. **库类型：**
   - `--library_type=microlib`: 使用 ARM Microlib，小型标准库（适合嵌入式系统）。

4. **严格检查：**
   - `--strict`: 启用严格模式，捕获未定义或潜在危险的行为。

5. **内存布局文件：**
   - `--scatter "FreeRTOS_Test\FreeRTOS_Test.sct"`:
     - 使用 scatter 文件指定内存分布。
     - Scatter 文件包含代码段和数据段的具体放置规则，通常与硬件内存分布相关。

---

### **组织结构**
- **代码文件目录：**
  - 核心代码路径：`../Core/Inc`。
  - HAL 驱动路径：`../Drivers/STM32F4xx_HAL_Driver/Inc`。
  - FreeRTOS 路径：
    - 核心：`../Middlewares/Third_Party/FreeRTOS/Source/include`。
    - CMSIS-RTOS V2 接口：`../Middlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS_V2`。
    - 硬件移植：`../Middlewares/Third_Party/FreeRTOS/Source/portable/RVDS/ARM_CM4F`。

- **依赖的外部文件：**
  - CMSIS 文件路径：`D:/Local/Arm/Packs/ARM/CMSIS/6.1.0/CMSIS/Core/Include`。
  - STM32 HAL 和 FreeRTOS 中间件。

- **关键宏定义：**
  - `STM32F429xx`：标识芯片型号。
  - `USE_HAL_DRIVER`：启用 HAL 驱动支持。

---

### **总结**
这段编译和链接选项展示了一个典型的基于 STM32F429 的嵌入式项目，使用了：
- **C99 标准**：确保现代 C 功能支持。
- **FreeRTOS**：提供实时操作系统功能。
- **HAL 驱动**：抽象底层硬件操作。
- **ARM Microlib**：小型库优化内存占用。
- **Scatter 文件**：明确内存分布。

这样的配置非常适合资源受限的嵌入式开发项目，同时支持高性能处理。

