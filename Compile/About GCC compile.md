# x86_64 gcc/g++/assembly
## 1. .bss 段 
```
.bss 段是用来声明和预留未初始化的全局或静态变量空间的一个段。这个段中的数据在程序开始执行前不会被初始化,并且默认会被设置为0。.bss 段与 .data 段不同,.data 段用于存放已初始化的全局或静态变量。
```

## 2. 典型函数序言prologue
```assembly
pushq %rbp
movq %rsp, %rbp

```

## 3. linux    全局变量定义
```assembly
    .global x
    .align  4
    .type   x, @object
    .size   x, 4
x:
    .zero 4
```

## 4. windows 全局变量定义

## 5. 反汇编
ndisasm -b 16 example.bin
ndisasm -b 16 example.bin > disasm.txt  重定向到指定文件  >> 追加
ndisasm -b 16 example.bin -o 0x7c00 > disasm.asm    -b 指定为16位代码(实模式), -o 指定起始位置


## 5. 什么是VGA, QVGA, SVGA, XGA
VGA的英文全称是Video Graphic Array,即显示绘图阵列
VGA 一般指640*480的分辨率,VGA摄像头就是30万像素的摄像头。
SQCIF=12*96
QCIF=176*144
CIF=352*288
QVGA=320*240
VGA=640*480
SVGA=800*600  Super Video Graphics Array
WSVGA=1024*600
XGA=1024*768  Extended Graphics Array
XVGA=1280*960
UXGA=1600*1200  Ultra Video Graphics Array

## 6. gcc 参数

### gcc 常用参数
-h 或 --section-headers: 显示目标文件中的节头信息。
-x 或 --headers: 显示所有可用的头信息，包括节头、程序头和段头。
-d 或 --disassemble: 从可执行段反汇编代码。
-D: 反汇编所有段。
-s 或 --full-contents: 显示节的内容。
-S 或 --source: 尽可能地反汇编并混合源代码。
-C 或 --demangle: 将低级符号名解码（demangle）为用户级名字。
-f 或 --file-headers: 显示文件头信息。
-t 或 --syms: 显示符号表条目。
-r 或 --reloc: 显示可重定位条目。
-R 或 --dynamic-reloc: 显示动态重定位条目。
-l 或 --line-numbers: 在反汇编输出中显示行号。
-e 或 --debugging: 显示调试信息。
-g: 仅显示调试信息。
-w: 或 --wide: 使得输出宽度不受限制，适用于宽行。
-z: 或 --disassemble-zeroes: 反汇编时不要跳过零字节。

在编译器(如 GCC 或 Clang)中,`-O0`、`-O1`、`-O2` 和 `-O3` 是用于控制编译器优化级别的选项。这些选项会影响编译器在编译程序时对代码执行的优化程度。以下是各个优化级别的区别:
### `-O0`(无优化)
- `-O0` 是默认的优化级别,表示没有优化。
- 编译器不会尝试优化代码,而是尽可能快地编译代码。
- 这通常意味着编译时间最短,但生成的代码执行效率可能不是最高的。
- 在调试程序时通常使用 `-O0`,因为它保留了源代码的结构,使得调试更加直观。
### `-O1`(一级优化)
- `-O1` 启用了一级优化,它试图优化代码的大小和执行时间,但不会进行过于激进的优化。
- 这个级别包括了基本的优化,如常量传播、死代码消除和简单的表达式简化。
- `-O1` 旨在减少代码大小和提高执行效率,同时保持编译时间相对较短。
### `-O2`(二级优化)
- `-O2` 是更高级别的优化,它包括了 `-O1` 的所有优化,并增加了更多的优化。
- `-O2` 通常包括循环优化、更多的内联函数扩展和更激进的死代码消除。
- 这个级别的优化可能会增加编译时间,但通常能够显著提高程序的执行效率。
- `-O2` 是一个比较平衡的选择,用于生产环境,因为它在编译时间和执行效率之间提供了一个较好的折中。
### `-O3`(三级优化)
- `-O3` 是最高级别的优化,它包括了 `-O2` 的所有优化,并进一步增加了更多的优化。
- `-O3` 可能会执行更多的循环转换、内联扩展、指令重排和向量化等。
- 这个级别的优化可能会显著增加编译时间,并且有时生成的代码执行效率提升并不明显,因为某些优化可能会增加代码大小,从而影响缓存性能。
- `-O3` 在某些情况下可能会产生不稳定的结果,比如数值精度问题。
### 注意事项
- 优化级别越高,编译器对代码的改动可能越大,这有时会导致调试困难,因为优化的代码可能不再与源代码一一对应。
- 高级别的优化可能会引入一些微妙的错误,尤其是当程序依赖于未定义行为时。
- 不同的编译器可能在不同的优化级别上实现不同的优化策略。
在实际使用中,通常需要根据具体的需求来选择合适的优化级别。对于大多数情况,`-O2` 是一个不错的选择,因为它在编译时间和执行效率之间提供了良好的平衡。而对于需要极致性能的场景,可以尝试 `-O3`,但需要仔细测试以确认优化没有引入新的问题。


-O0: 不优化(默认级别)。编译器会尽可能快地编译程序,不进行任何优化。
-O1: 启用一级优化。它包括基本的优化,如内联小函数和执行简单的寄存器优化。
-O2: 优化程序大小。这个选项会尽量减小最终生成的可执行文件的大小,但性能提升可能不如 -O2 或 -O3。
-Ofast: 启用所有 -O3 优化,并可能违反语言标准以提升性能。这通常包括一些可能违反严格数值稳定性的优化。

在编译器(如 GCC 或 Clang)中,`-g0` 选项用于控制生成的调试信息量。以下是 `-g0` 选项的具体含义:
### `-g0` 选项
`-g0` 选项表示不生成任何调试信息。当你使用 `-g0` 编译程序时,生成的可执行文件将不包含任何用于调试的信息,例如变量名、函数名、行号等。
这种情况下,如果你尝试使用调试器(如 GDB)来调试程序,你将无法看到源代码的详细信息,也无法设置断点或查看变量的值。这是因为没有调试信息,调试器无法将执行中的程序与源代码关联起来。
### 使用场景
`-g0` 通常在以下场景中使用:
- 当你想要发布最终版本的程序,并且不希望任何人能够调试它时。
- 当你想要减小可执行文件的大小,因为调试信息可能会显著增加文件大小。
- 当你进行性能测试,并且不希望调试信息的生成影响编译时间或程序的运行性能。
### 与其他调试选项的比较
- `-g`: 生成默认的调试信息,通常包括行号、变量和类型信息,但不包括所有的优化信息。
- `-g1`: 生成最小的调试信息,通常只包括行号。
- `-g2`: 生成默认的调试信息加上一些额外的信息,如宏定义。
- `-g3`: 生成最多的调试信息,包括类型和变量信息,以及额外的优化信息。
一般来说,除了 `-g0` 以外的 `-g` 选项都提供了不同程度的调试信息,而 `-g` 或 `-g2` 是开发过程中常用的选项,因为它们提供了足够的调试信息,同时不会对编译时间或程序大小产生太大影响。

### -c
编译源代码但不进行链接

### -static
-static 选项用于指示编译器生成一个完全静态链接的可执行文件。这意味着在生成的可执行文件中,将包含程序运行所需的所有库代码,而不是在运行时动态链接这些库

### -Wall 
告诉编译器激活所有警告。Wall 代表 “all warnings”,它会使得编译器在编译代码时报告尽可能多的潜在问题,比如类型不匹配、未使用的变量、格式字符串问题等。这有助于发现代码中的错误或不规范的写法,从而提高代码的质量和可维护性。

### -v  --verbose
这个选项会让 GCC 输出编译过程中使用的每个工具的名称和命令行选项,以及搜索头文件和库文件的路径

#### Multilib 支持：

Windows MinGW（x86_64-w64-mingw32）：禁用了多架构支持 (--disable-multilib)，因为它只针对 x86_64 架构。
Linux GCC（x86_64-linux-gnu）：通常启用了多架构支持，允许同时支持多个目标架构（如 i686、x86_64 等）。

#### 字符集本地化
Windows MinGW：禁用了本地化支持（--disable-nls），因为 Windows 环境下的 GCC 不需要处理多语言和本地化相关的内容。
Linux GCC：通常会启用本地化支持（NLS，Native Language Support），因为 Linux 系统通常需要处理多语言环境和区域设置。


### -dM
`gcc -dM -E - < /dev/null`
参数说明
`-dM`: 只输出预定义的宏。
`-E`: 运行预处理器，不进行编译。
`- < /dev/null`: 表示从空文件输入，避免处理具体代码文件，只显示默认定义的宏。

#### 查看特定文件的宏
如果有一个源文件 test.c, 使用`gcc -dM -E test.c`

## 7. readelf 
gcc -g 生成调试信息
readelf -h file 查看elf文件头信息

readelf -S file 查看段表
--wide 展开显示
readelf -S --wide file
readelf -l 查看程序头部表或段
readelf -a file 查看所有信息


## 8. objdump 反汇编
### objdump -d file 查看汇编代码
### objdump -h file 查看文件头信息
-h只是把ELF文件中的关键的段显示出来,而省略了其他的辅助性的段,例如符号表、字符串表、段名字符串表、重定位表等

.text  代码段 :程序源代码编译后的机器指令被放在这个段里,里面保存了机器代码；

.data 数据段 :全局变量和局部静态变量数据被放在这个段里,如上图所示,说明a.o本身没有全局变量和局部静态变量size=0, 但是CONTENTS表示这个段在文件中是存在的；

.bss(block Started by Symbol) : 未初始化的全局变量和局部静态变量被放在这个段里,未初始化的全局变量和局部静态变量都为0,这个段存在的意义在于 只是为未初始化的全局变量和局部静态变量预留位置而已,它本身没有内容也不占据文件的空间,如上所所示,第2个bss, size为0, 也没有CONTENTS表示不存在；

.note.GNU-stack堆栈提示段 :虽然有CONTENTS但是它的长度是0.

objdump -h main.o

### 使用-d或-s, -d将所有包含指令.text段进行反编译,如果要对其他的section进行反编译使用-D

### objdump -x 显示所有的头的信息包括符号表和重定向入口
 显示可用的头信息,包括符号表和重定向的入口,等价于 -a -f -h -p -r -t

### objdump -f 显示文件头的信息
### objdump -r main.o查看重定向表

## 9. strip
移除调试符号:当你想要减小可执行文件的大小,或者不想让其他人容易地逆向工程你的程序时,可以使用 strip。
strip executable
strip 是一个命令行工具,用于从可执行文件、对象文件和其他文件中移除符号信息。这些符号信息通常包括调试符号、符号表、重定位信息和一些其他元数据,它们对于程序执行来说不是必需的,但是对于调试和开发过程很重要

strip -g executable 保留全局符号
strip -s executable 移除所有符号
strip -R .comment executable  删除特定的节如.comment


## 10. ELF结构、gcc编译与链接

### section (节)
在编译程序时,生成的可执行文件或对象文件通常包含多个节(section),每个节都有其特定的用途。以下是一些常见的节以及它们的作用:

.text:这是包含程序指令的部分,即程序的机器代码。这是执行程序所必需的部分。
.data:这个节包含初始化了的全局和静态变量。这些变量在程序启动时就已经有确定的值。
.bss:这个节包含未初始化的全局和静态变量。它们在程序启动时通常会被设置为0或者某个默认值。
.rodata:这个节包含只读数据,比如字符串字面量和常量。
以下节可能不是绝对必需的,但通常在可执行文件中找到:

.symtab:符号表,它包含了程序中定义和引用的函数和全局变量的信息。这对于调试非常有用,但对于程序的正常运行不是必需的。
.strtab:字符串表,它包含了符号表中的符号名称。与 .symtab 一样,这对调试很重要,但不是运行必需的。
.rel.text 或 .rela.text:这些节包含与 .text 节相关的重定位信息,用于在链接过程中修正指令中的地址引用。
.rel.data 或 .rela.data:这些节包含与 .data 和 .bss 节相关的重定位信息。
对于程序正常运行来说,以下节是必须保留的:

.text:包含程序的指令。
.data:包含初始化的数据。
.bss:包含未初始化的数据。
其他节,如符号表、字符串表和重定位信息,对于程序的执行不是必需的,但在调试和开发过程中非常有用。当你准备发布一个程序时,通常可以使用 strip 命令移除这些节来减小可执行文件的大小并保护你的代码不被轻易逆向工程。然而,即使在移除了这些节之后,程序仍然可以正常运行,因为它们不包含程序执行所需的指令或数据。

### 关于链接
链接过程会根据编译器的不同以及编译目标文件需要运行的平台的不同而有所差异。以下是一些关键点:
链接的过程:
链接是程序编译过程中的一个步骤,它发生在编译器将源代码转换为目标代码(通常是机器代码)之后。链接的主要目的是将多个目标文件(.o或.obj文件)以及库文件(.a或.lib文件)合并成一个可执行文件(.exe文件在Windows上,或无后缀的文件在Unix-like系统上)。
链接的具体任务包括:
合并段(Segments):
将所有输入的目标文件中的相同段(如.text段包含代码,.data段包含已初始化的全局变量,.bss段包含未初始化的全局变量)合并成单一的段。
解析符号引用:
确保所有符号引用都有对应的定义。例如,如果一个目标文件中的函数调用另一个文件中定义的函数,链接器需要确保这两个文件在链接过程中被正确地连接起来。
重定位:
当目标文件被合并时,它们中的地址引用可能需要被调整,因为它们原本是基于各自文件中的地址。链接器会更新这些地址,确保它们指向正确的位置。
分配内存地址:
链接器为合并后的程序中的每个段分配运行时的内存地址。
处理外部库:
如果程序使用了外部库,链接器会从库中提取需要的模块,并将它们合并到最终的程序中。
编译器和平台的影响:
编译器差异:
不同的编译器可能使用不同的目标文件格式(例如,GNU编译器使用ELF格式,而Microsoft编译器使用COFF格式)。
编译器特定的优化选项和链接器选项可能会影响链接过程。
平台差异:
不同操作系统可能要求不同的链接行为。例如,Windows和Linux在处理动态链接库(DLLs和SOs)的方式上有所不同。
不同架构的处理器(如x86、ARM、MIPS)可能有不同的调用约定和内存布局要求,这会影响链接器如何处理目标文件。
链接器差异:
不同的链接器(如GNU ld、Gold、lld、Microsoft的link.exe)可能有不同的功能和选项,这会影响链接过程和最终生成的可执行文件。
总之,链接是一个复杂的步骤,它确保了编译后的代码能够正确地组合在一起,形成一个可以在特定平台上运行的完整程序。编译器和平台的不同特性会影响链接器的行为和最终生成的可执行文件的格式。


### 关于extern和static
在 C 语言中,如果你在|函数外部|定义了一个变量,并且没有使用 static 关键字,那么这个变量默认具有外部链接(external linkage),这意味着它可以被其他文件中的函数访问,只要这些文件通过 extern 关键字声明了这个变量。

### 链接过程
使用 gcc 编译并链接一个 C 或 C++ 程序时,默认情况下,gcc 会链接以下类型的文件:

C 运行时启动文件 (crt1.o, crti.o, crtn.o): 这些文件包含了程序启动和终止所需的代码。它们负责设置和清理程序的运行时环境,并调用 main 函数。
C 标准库 (libc): 这是 GNU C 库(glibc),它提供了程序运行所需的基本函数,比如 printf, malloc, exit 等。
动态链接器 (ld-linux.so): 如果程序是动态链接的,那么会链接动态链接器,它负责在程序运行时加载所需的共享库。
以下是 gcc 在默认情况下链接的主要组件:
crt1.o:C 运行时启动文件,负责设置程序的初始执行环境。
crti.o:包含运行时初始化代码。
crtn.o:包含运行时终止代码。
libc.so:C 标准库的动态链接版本。
ld-linux.so:动态链接器。
当编译一个简单的程序并使用 gcc 链接时,以下命令:

gcc -o program program.c
等同于以下更详细的命令:

gcc -o program program.c crt1.o crti.o crtn.o -lc -lgcc --as-needed -lgcc_s --no-as-needed -ldl -lpthread -lrt -lm
这里:

-lc:链接 C 标准库。
-lgcc 和 -lgcc_s:链接 GCC 低级运行时支持库。
-ldl:链接 dl 库,用于动态加载共享库。
-lpthread:链接线程库,如果你的程序使用了线程。
-lrt:链接实时扩展库,某些系统调用需要。
-lm:链接数学库,如果你的程序使用了数学函数。

上述命令中的一些选项和库可能因操作系统、GCC 版本或编译器配置的不同而有所不同。gcc 会根据你的程序的需要自动包含这些库。如果你不需要某些库,可以通过编译选项来排除它们。
#### crt1.o crti.o crtbegin.o crtend.o crtn.o
crt1.o, crti.o, crtbegin.o, crtend.o, crtn.o 等目标文件和daemon.o(由我们自己的C程序文件产生)链接成一个执行文件。
前面这5个目标文件的作用分别是启动、初始化、构造、析构和结束,它们 通常会被自动链接到应用程序中

### .a (archive) 静态库
Windows系统的静态链接库后缀为.lib,在Linux上后缀.a,可能是archive的意思

### .so (shared object) 动态库
Windows系统的动态链接库(Dynamic Link Library, DLL)在Linux系统上叫共享对象(Shared Object,SO)

### -fPIC(Position-Independent Code)  
功能:-fPIC 选项用于生成位置无关代码(Position-Independent Code)。位置无关代码可以在内存中的任意位置执行,而不需要在加载时进行重定位。
用途:主要用于生成动态库(共享库,.so 文件)的目标文件(.o 文件)。动态库可能被多个程序加载到内存中的不同位置,因此需要位置无关代码来确保在不同内存位置上都能正确执行。
特点:
灵活性:-fPIC 生成的目标文件可以用于构建动态库,也可以用于构建位置无关的可执行文件(PIE)。
实现机制:通过使用全局偏移表(GOT)和过程链接表(PLT)来访问全局变量和函数,从而实现代码的地址无关性。

### -fPIE（Position-Independent Executable）  用于编译阶段
功能：-fPIE 选项用于生成位置无关的可执行文件代码。与 -fPIC 类似，它也生成位置无关代码，但专门针对可执行文件（而不是动态库）。
用途：主要用于生成位置无关的可执行文件（PIE）的目标文件。PIE 允许可执行文件在内存中的任意位置加载，这对于现代操作系统中的地址空间布局随机化（ASLR）非常有用。
特点：
优化：-fPIE 生成的代码在假设它将被链接为可执行文件的情况下进行了一些优化，可能比 -fPIC 代码更小、更快，因为它减少了对 GOT 和 PLT 的依赖。
限制：-fPIE 生成的目标文件不能用于生成动态库。

### -pie（Position-Independent Executable）  用于链接阶段
功能：-pie 选项是链接选项，用于生成位置无关的可执行文件（PIE）。它告诉链接器将使用 -fPIE 或 -fPIC 编译生成的目标文件链接成一个位置无关的可执行文件。
用途：用于构建可以在任意内存地址加载和执行的可执行文件。PIE 是现代操作系统中安全机制（如 ASLR）的一部分，用于防止攻击者预测程序在内存中的布局。
特点：
要求：所有用于生成 PIE 的目标文件必须是用 -fPIE 或 -fPIC 编译的。
安全性：生成的 PIE 文件可以显著提高程序的安全性，因为它们可以在每次运行时加载到不同的内存地址。

### gcc -v
配置参数
支持的编程语言:
启用了以下语言:

C
Ada
C++
Go
BRIG
D
Fortran
Objective-C
Objective-C++
Modula-2 (m2)
基本选项:

--prefix=/usr: 安装路径为 /usr。
--enable-shared: 启用共享库。
--enable-nls: 启用本地化支持。
--enable-bootstrap: 启用引导构建（GCC 编译自己）。
--enable-threads=posix: 启用 POSIX 线程。
--enable-libstdcxx-debug: 启用 libstdc++ 调试支持。
--enable-libstdcxx-time=yes: 启用 libstdc++ 时间支持。
--with-default-libstdcxx-abi=new: 设置 libstdc++ 的默认 ABI 为新版本。
--enable-gnu-unique-object: 启用 GNU 独特对象。
--disable-vtable-verify: 禁用虚拟表验证。
--enable-plugin: 启用插件支持。
--enable-default-pie: 启用默认的 PIE（位置独立执行文件）。
--with-system-zlib: 使用系统级的 zlib。
--enable-objc-gc=auto: 启用自动的 Objective-C 垃圾回收。
--enable-multiarch: 启用多架构支持。
--disable-werror: 禁用将警告当作错误处理。
--enable-cet: 启用控制流执行保护（Control Flow Enforcement）。
--with-arch-32=i686: 为 32 位架构设置 i686。
--with-abi=m64: 使用 64 位 ABI。
--with-multilib-list=m32,m64,mx32: 支持多种架构（32位、64位、x32）。
--enable-multilib: 启用多库支持（多架构支持）。
--with-tune=generic: 为一般目的优化。
--enable-offload-targets=nvptx-none,amdgcn-amdhsa: 启用 NVIDIA 和 AMD GPU 的 offload 目标。
链接选项:

--enable-linker-build-id: 启用生成链接器构建标识。
--enable-link-serialization=2: 启用链接序列化，版本为 2。
调试与检查:

--enable-checking=release: 启用发布版本的检查。
--with-build-config=bootstrap-lto-lean: 使用引导 LTO（链接时优化）精简配置。Link-Time Optimization
编译目标:

--target=x86_64-linux-gnu: 编译目标为 x86_64-linux-gnu（64位 Linux）。
--host=x86_64-linux-gnu: 主机平台为 x86_64-linux-gnu。
特殊功能
Offload 支持：启用了 nvptx-none 和 amdgcn-amdhsa 的 offload 目标，支持将部分代码卸载到 NVIDIA 和 AMD 的 GPU 上运行。
多架构支持：启用了多架构支持（32位、64位、x32），使得该 GCC 版本能在多种架构环境下工作。

#### windows: gcc -v
COLLECT_LTO_WRAPPER=D:/ProgramData/w64devkit/bin/../libexec/gcc/x86_64-w64-mingw32/14.1.0/lto-wrapper.exe
这是 LTO（Link Time Optimization）相关的工具路径。LTO 是一种优化技术，在链接阶段对程序进行进一步优化。此参数指示 GCC 使用 lto-wrapper.exe 来处理 LTO 相关的工作。

Target: x86_64-w64-mingw32
这是 GCC 编译器的目标平台，表示该编译器是为 64 位 Windows 系统上的 MinGW 环境构建的，目标架构为 x86_64，并且使用 MinGW 提供的 Windows 环境。

--prefix=/w64devkit: 编译器将被安装到 /w64devkit 路径下。
--with-sysroot=/w64devkit/x86_64-w64-mingw32: 指定系统根目录，这里用于指定 MinGW 的目标系统根目录。
--with-native-system-header-dir=/include: 头文件的默认目录。
--target=x86_64-w64-mingw32: 目标架构和操作系统，这里是针对 64 位 Windows 的 MinGW。
--host=x86_64-w64-mingw32: 主机平台，用于设置编译时使用的主机系统。
--enable-static: 启用静态链接库。
--disable-shared: 禁用共享库（动态库）支持。
--with-pic: 生成位置无关代码（Position Independent Code），通常用于生成共享库。
--enable-languages=c,c++: 启用 C 和 C++ 语言的支持。
--enable-libgomp: 启用 GOMP（GNU OpenMP）库的支持，用于并行编程。
--enable-threads=posix: 启用 POSIX 线程模型。
--enable-version-specific-runtime-libs: 启用与 GCC 版本相关的运行时库。
--disable-dependency-tracking: 禁用依赖关系跟踪（可以提高编译速度，但可能影响依赖更新时的正确性）。
--disable-lto: 禁用链接时优化（LTO），这可能影响生成的代码的优化。
--disable-multilib: 禁用多架构支持（该配置适用于目标架构 x86_64，而不是 32 位架构）。
--disable-nls: 禁用本地化支持，通常是为了减小编译器的体积。
--disable-win32-registry: 禁用 Windows 注册表的相关支持。
--enable-mingw-wildcard: 启用 MinGW 样式的通配符支持。
CFLAGS=-Os CXXFLAGS=-Os LDFLAGS=-s: 这些是编译时的默认选项，-Os 表示优化代码大小，-s 表示去除符号表和调试信息，减小生成文件的大小。

#### Windows MinGW（x86_64-w64-mingw32）：

静态链接（--enable-static）：MinGW 配置通常会更偏向于静态链接，因为在 Windows 上，开发者往往需要将所有依赖项打包到最终的可执行文件中，以便程序能在不依赖外部 DLL 的情况下运行。
--disable-shared：禁用共享库的支持，因此编译出的程序将是完全静态的。
Linux GCC（x86_64-linux-gnu）：

共享库支持：Linux 系统的 GCC 默认启用了动态链接，支持共享库（如 .so 文件）。这通常用于更有效的内存使用和共享代码。
LTO（链接时优化）：在 Linux GCC 中，LTO 可能默认启用或者通过 -flto 显式启用，允许进行跨文件的优化。


### 链接时

#### liblto_plugin.so 用于链接时优化
路径: /usr/lib/gcc/x86_64-linux-gnu/11/liblto_plugin.so
作用: 这是 GCC 使用的 LTO (Link-Time Optimization) 插件。LTO 是一种优化技术，在链接阶段对程序进行优化，而不仅仅是在编译阶段。
LTO 的工作原理: 在编译过程中，源代码被编译成中间表示（IR），而不是直接生成目标文件。这些中间表示文件在链接阶段被合并和优化，LTO 插件通过在链接阶段进行优化来提升程序的性能。
作用总结: liblto_plugin.so 插件实现了这种链接时优化的过程，它使得 GCC 能够执行跨文件的优化，例如内联函数的扩展、去除冗余代码等。

#### ld-linux-x86-64.so.2  用于动态链接库的加载。
路径: /lib64/ld-linux-x86-64.so.2
作用: 这是 动态链接器（Dynamic Linker），也被称为 运行时链接器。它是 Linux 系统中启动动态链接程序的核心组件。
动态链接过程: 当你执行一个动态链接的程序时，操作系统会通过这个动态链接器来加载程序所依赖的共享库（如 libc.so、libm.so 等）。它会定位并加载这些共享库，并将它们链接到程序的运行时地址空间，确保程序在执行时能够调用所需的函数。
作用总结: ld-linux-x86-64.so.2 是加载和链接动态共享库的工具，确保程序运行时能正确找到并加载其依赖的库。

### ABI (Application Binary Interface) 
OS/ABI: UNIX - System V
OS/ABI 表示该可执行文件的操作系统和应用二进制接口（ABI）标准。
UNIX - System V 是一种操作系统接口标准，指的是符合 UNIX 系统 V（System V）规范的操作系统。
System V 是 UNIX 操作系统的一种版本，许多 UNIX-like 系统（包括 Linux）遵循它的标准。
这个标志表明，该可执行文件遵循 UNIX 系统 V 标准的 ABI，通常意味着它是为 Linux 或其他类 Unix 操作系统构建的。
ABI (Application Binary Interface) 是操作系统提供的应用程序与操作系统之间的接口规范。它定义了函数调用、数据类型大小、字节对齐、系统调用接口等。
在 Linux 中，常见的 ABI 是 System V ABI（如 x86_64 ABI），它定义了如何在程序中传递参数、调用系统调用等。
总结: 这表示该文件符合 UNIX 系统 V 标准的 ABI，通常是针对 Linux 或其他遵循类似标准的类 Unix 操作系统。

### 全局宏
gcc -Dmacro_name=1 -o main main.c header.c ...

# 草稿
默认链接内容(gcc -o ab a.c b.c -v):
/usr/lib/gcc/x86_64-linux-gnu/11/liblto_plugin.so
/lib64/ld-linux-x86-64.so.2
等
Scrt1.o
crti.o
crtbeginS.o
crtendS.o
crtn.o

ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Position-Independent Executable file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x1040
  Start of program headers:          64 (bytes into file)
  Start of section headers:          14000 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         13
  Size of section headers:           64 (bytes)
  Number of section headers:         29
  Section header string table index: 28



ab:     file format elf64-x86-64


Disassembly of section .init:

0000000000001000 <_init>:
    1000:       f3 0f 1e fa             endbr64
    1004:       48 83 ec 08             sub    $0x8,%rsp
    1008:       48 8b 05 d9 2f 00 00    mov    0x2fd9(%rip),%rax        # 3fe8 <__gmon_start__@Base>
    100f:       48 85 c0                test   %rax,%rax
    1012:       74 02                   je     1016 <_init+0x16>
    1014:       ff d0                   call   *%rax
    1016:       48 83 c4 08             add    $0x8,%rsp
    101a:       c3                      ret

Disassembly of section .plt:

0000000000001020 <.plt>:
    1020:       ff 35 a2 2f 00 00       push   0x2fa2(%rip)        # 3fc8 <_GLOBAL_OFFSET_TABLE_+0x8>
    1026:       f2 ff 25 a3 2f 00 00    bnd jmp *0x2fa3(%rip)        # 3fd0 <_GLOBAL_OFFSET_TABLE_+0x10>
    102d:       0f 1f 00                nopl   (%rax)

Disassembly of section .plt.got:

0000000000001030 <__cxa_finalize@plt>:
    1030:       f3 0f 1e fa             endbr64
    1034:       f2 ff 25 bd 2f 00 00    bnd jmp *0x2fbd(%rip)        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    103b:       0f 1f 44 00 00          nopl   0x0(%rax,%rax,1)

Disassembly of section .text:

0000000000001040 <_start>:
    1040:       f3 0f 1e fa             endbr64
    1044:       31 ed                   xor    %ebp,%ebp
    1046:       49 89 d1                mov    %rdx,%r9
    1049:       5e                      pop    %rsi
    104a:       48 89 e2                mov    %rsp,%rdx
    104d:       48 83 e4 f0             and    $0xfffffffffffffff0,%rsp
    1051:       50                      push   %rax
    1052:       54                      push   %rsp
    1053:       45 31 c0                xor    %r8d,%r8d
    1056:       31 c9                   xor    %ecx,%ecx
    1058:       48 8d 3d ca 00 00 00    lea    0xca(%rip),%rdi        # 1129 <main>
    105f:       ff 15 73 2f 00 00       call   *0x2f73(%rip)        # 3fd8 <__libc_start_main@GLIBC_2.34>
    1065:       f4                      hlt
    1066:       66 2e 0f 1f 84 00 00    cs nopw 0x0(%rax,%rax,1)
    106d:       00 00 00

0000000000001070 <deregister_tm_clones>:
    1070:       48 8d 3d a1 2f 00 00    lea    0x2fa1(%rip),%rdi        # 4018 <__TMC_END__>
    1077:       48 8d 05 9a 2f 00 00    lea    0x2f9a(%rip),%rax        # 4018 <__TMC_END__>
    107e:       48 39 f8                cmp    %rdi,%rax
    1081:       74 15                   je     1098 <deregister_tm_clones+0x28>
    1083:       48 8b 05 56 2f 00 00    mov    0x2f56(%rip),%rax        # 3fe0 <_ITM_deregisterTMCloneTable@Base>
    108a:       48 85 c0                test   %rax,%rax
    108d:       74 09                   je     1098 <deregister_tm_clones+0x28>
    108f:       ff e0                   jmp    *%rax
    1091:       0f 1f 80 00 00 00 00    nopl   0x0(%rax)
    1098:       c3                      ret
    1099:       0f 1f 80 00 00 00 00    nopl   0x0(%rax)

00000000000010a0 <register_tm_clones>:
    10a0:       48 8d 3d 71 2f 00 00    lea    0x2f71(%rip),%rdi        # 4018 <__TMC_END__>
    10a7:       48 8d 35 6a 2f 00 00    lea    0x2f6a(%rip),%rsi        # 4018 <__TMC_END__>
    10ae:       48 29 fe                sub    %rdi,%rsi
    10b1:       48 89 f0                mov    %rsi,%rax
    10b4:       48 c1 ee 3f             shr    $0x3f,%rsi
    10b8:       48 c1 f8 03             sar    $0x3,%rax
    10bc:       48 01 c6                add    %rax,%rsi
    10bf:       48 d1 fe                sar    %rsi
    10c2:       74 14                   je     10d8 <register_tm_clones+0x38>
    10c4:       48 8b 05 25 2f 00 00    mov    0x2f25(%rip),%rax        # 3ff0 <_ITM_registerTMCloneTable@Base>
    10cb:       48 85 c0                test   %rax,%rax
    10ce:       74 08                   je     10d8 <register_tm_clones+0x38>
    10d0:       ff e0                   jmp    *%rax
    10d2:       66 0f 1f 44 00 00       nopw   0x0(%rax,%rax,1)
    10d8:       c3                      ret
    10d9:       0f 1f 80 00 00 00 00    nopl   0x0(%rax)

00000000000010e0 <__do_global_dtors_aux>:
    10e0:       f3 0f 1e fa             endbr64
    10e4:       80 3d 29 2f 00 00 00    cmpb   $0x0,0x2f29(%rip)        # 4014 <completed.0>
    10eb:       75 2b                   jne    1118 <__do_global_dtors_aux+0x38>
    10ed:       55                      push   %rbp
    10ee:       48 83 3d 02 2f 00 00    cmpq   $0x0,0x2f02(%rip)        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    10f5:       00
    10f6:       48 89 e5                mov    %rsp,%rbp
    10f9:       74 0c                   je     1107 <__do_global_dtors_aux+0x27>
    10fb:       48 8b 3d 06 2f 00 00    mov    0x2f06(%rip),%rdi        # 4008 <__dso_handle>
    1102:       e8 29 ff ff ff          call   1030 <__cxa_finalize@plt>
    1107:       e8 64 ff ff ff          call   1070 <deregister_tm_clones>
    110c:       c6 05 01 2f 00 00 01    movb   $0x1,0x2f01(%rip)        # 4014 <completed.0>
    1113:       5d                      pop    %rbp
    1114:       c3                      ret
    1115:       0f 1f 00                nopl   (%rax)
    1118:       c3                      ret
    1119:       0f 1f 80 00 00 00 00    nopl   0x0(%rax)

0000000000001120 <frame_dummy>:
    1120:       f3 0f 1e fa             endbr64
    1124:       e9 77 ff ff ff          jmp    10a0 <register_tm_clones>

0000000000001129 <main>:
    1129:       f3 0f 1e fa             endbr64
    112d:       55                      push   %rbp
    112e:       48 89 e5                mov    %rsp,%rbp
    1131:       8b 05 d9 2e 00 00       mov    0x2ed9(%rip),%eax        # 4010 <x>
    1137:       89 c7                   mov    %eax,%edi
    1139:       e8 07 00 00 00          call   1145 <func1>
    113e:       b8 00 00 00 00          mov    $0x0,%eax
    1143:       5d                      pop    %rbp
    1144:       c3                      ret

0000000000001145 <func1>:
    1145:       f3 0f 1e fa             endbr64
    1149:       55                      push   %rbp
    114a:       48 89 e5                mov    %rsp,%rbp
    114d:       89 7d fc                mov    %edi,-0x4(%rbp)
    1150:       83 45 fc 07             addl   $0x7,-0x4(%rbp)
    1154:       90                      nop
    1155:       5d                      pop    %rbp
    1156:       c3                      ret

Disassembly of section .fini:

0000000000001158 <_fini>:
    1158:       f3 0f 1e fa             endbr64
    115c:       48 83 ec 08             sub    $0x8,%rsp
    1160:       48 83 c4 08             add    $0x8,%rsp
    1164:       c3                      ret