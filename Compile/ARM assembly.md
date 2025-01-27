# ARM Architecture Assembler

ARM Cortex-M arch
编译器: ARMCC/ARMCLANG
汇编器: armasm
风格: 如AREA

而x86_64 linux
compiler: gcc
assembler: as
style like: .section .global 
GCC 的 as 汇编器默认支持 AT&T 格式，因为这是传统 Unix 系统的风格。
可以通过 -masm=intel 参数切换到 Intel 风格。


ARM 处理器支持 ARM 指令集 和 Thumb 指令集。
Cortex-M 系列默认使用精简的 Thumb 指令集（Thumb-2），指令长度较短（16 位和 32 位混合），适合嵌入式设备。

适用于 ARM 的 Keil MDK 工具链（使用 ARMCC/ARMASM 汇编器）。
AREA、SPACE 等是 ARM 汇编伪指令，由 ARM 工具链特有的汇编器支持。

ARM 的栈通常是向下增长的 如PUSH, SP 减小
ARM 默认为小端模式

## 语法特点
label label:
my_label
    MOV R0, #0  ; label后面可以不跟随冒号

而x86 AT&T风格 的label后面需要跟随冒号
## General Instrctions
常见段名称和功能
一些常见的段名称和它们的惯例功能：

STACK：表示用于栈空间的段。
HEAP：表示用于堆空间的段。
CODE：表示存放程序代码的段。
DATA：表示存放初始化数据的段。
BSS：表示存放未初始化数据的段。

`这些名字仅仅是约定惯例，汇编器和链接器不会对这些名字赋予特殊的意义`

### AREA
理解: 描述一个段，面向汇编和链接过程
AREA    aka .section
AREA    section_name,   [attributes]
AREA    段名        ,   指定段的属性
AREA    STACK, NOINIT, READWRITE, ALIGN=3

#### 段名（STACK）
STACK 是该段的名称。
这个名称是标识符，供汇编器、链接器、调试工具使用。
命名惯例：
栈段通常命名为 STACK。
不同用途的段可以取不同名称，如 DATA（数据段）、CODE（代码段）、HEAP（堆段）等。

#### 属性 NOINIT
作用：指定段为 未初始化段。

在程序加载时，段的内容不需要初始化（不会被清零或填充）。
适用于栈、堆等动态分配内存，因为它们在程序运行时会被覆盖。
常见属性比较：

属性	含义
NOINIT	未初始化段，不进行加载时的初始化。
DATA	已初始化数据段，加载时有默认值。
CODE	代码段，用于存储可执行代码。

#### 属性 READWRITE
作用：指定段为可读写。
含义：
栈需要频繁读写，因此必须设置为 READWRITE。
如果是只读段（如常量），会用 READONLY 属性。

#### 属性 ALIGN=3
作用：指定段的对齐方式。

含义：

ALIGN=3 表示对齐到 2^3=8 字节边界
ARM Cortex-M 架构要求堆栈必须以 8 字节对齐，以满足指针和数据访问的效率要求。


### SPACE
SPACE   aka .skip   伪指令 用于分配未初始化的内存空间
Stack_Mem   SPACE   Stack_Size

### PRESERVE8
含义：
指示汇编器和生成的代码确保栈指针（SP）始终保持 8 字节对齐。
作用：
符合 ARM ABI（应用二进制接口）的要求，确保栈操作对齐，以避免潜在的数据访问错误或性能下降。
特别是在处理 64 位数据类型（如 double）时，对齐是至关重要的。

### THUMB
含义：
告诉汇编器生成的代码使用 Thumb 指令集，而不是 ARM 指令集。
Thumb 指令集以 16 位为主（部分指令为 32 位），比 ARM 指令集更紧凑。
作用：
提高代码密度，减少存储需求。
ARM Cortex-M 系列（如 STM32F429）仅支持 Thumb 指令集，因此必须设置此选项。

### DCD
DCD：定义常量数据（Define Constant Data），这里表示存储 __initial_sp 的地址

### EXPORT
EXPORT __Vectors, __Vectors_End, __Vectors_Size

将标签 __Vectors、__Vectors_End 和 __Vectors_Size 导出，使它们在其他文件或链接器中可见

### PROC
PROC（Procedure）指示接下来的部分是一个过程（函数）。在这里，Reset_Handler 被定义为一个过程

#### PROC vs .type function_name, @function
PROC 是 ARM 汇编中的伪指令，用来标识一个过程的开始。它告诉汇编器，这个标签后面的代码是一个函数的实现。通常与 ENDP 配对，后者标识过程的结束。
PROC 主要用于标记过程的范围，并为函数提供一个合适的起始位置，供汇编器、链接器和调试器处理。

```assembly
PROC MyFunction
    ; Function body
ENDP
```
PROC 只是为了结构化代码，使得汇编文件更容易理解和维护。它本身并不会影响生成的机器码或二进制输出

.type 主要用于告诉汇编器和链接器某个符号的类型。在这里，@function 表示这个符号是一个函数。
它并不会直接影响代码的逻辑，只是提供附加的元数据，帮助调试器和链接器识别符号的类型。它通常与调试相关，帮助符号表中的符号识别。

```assembly
.type MyFunction, @function
MyFunction:
    ; Function body
```

PROC 是 ARM 汇编中的结构性指令，表示一个函数的开始，它有助于组织代码，但不会影响最终生成的机器代码。
.type 是 AT&T 汇编中的伪指令，用于提供符号的类型信息，通常是为了调试和符号表生成，不直接影响代码的行为或结构

调试信息：

PROC 本身不携带调试信息，更多是汇编代码层面的结构化组织。
.type 主要用于调试信息的生成，帮助调试工具或符号表管理工具识别符号的属性，如是否为函数、变量等


### [weak]
EXPORT Reset_Handler [WEAK]
EXPORT 是用于将 Reset_Handler 函数导出，使其在其他模块中可见。
[WEAK] 关键字表示该函数是 弱符号。这意味着，如果有其他地方定义了 Reset_Handler，链接器会优先使用其他定义，而不是这个弱定义。弱符号通常用于提供默认的实现，当用户没有定义具体实现时，使用默认实现

### IMPORT
IMPORT 关键字声明 SystemInit 函数来自外部模块，表示 SystemInit 函数将在链接时被引用

### LDR
LDR R0, =SystemInit
LDR（Load Register）指令将 SystemInit 函数的地址加载到寄存器 R0 中。
=SystemInit 是汇编的一个伪指令，表示将 SystemInit 函数的地址加载到寄存器 R0。这意味着 R0 将存储 SystemInit 的内存地址

### BLX
BLX R0
BLX（Branch with Link and Exchange）指令将控制流跳转到 R0 中存储的地址，并保存当前地址到链接寄存器（LR）中，以便之后返回。
这里它将跳转到 SystemInit 函数执行硬件初始化