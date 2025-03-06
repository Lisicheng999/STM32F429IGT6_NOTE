# About x86 Assembly
## 1. 以一个例子开头
```x86asm
        .file   "main.c"
        .text
        .p2align 4
        .globl  cal
        .type   cal, @function
cal:
.LFB0:
        .cfi_startproc
        endbr64
        movl    (%rsi), %eax
        addl    (%rdi), %eax
        movl    %eax, (%rdi)
        subl    %eax, (%rsi)
        ret
        .cfi_endproc
.LFE0:P
        .size   cal, .-cal
        .section        .text.startup,"ax",@progbits
        .p2align 4
        .globl  main
        .type   main, @function
main:
.LFB1:
        .cfi_startproc
        endbr64
        xorl    %eax, %eax
        ret
        .cfi_endproc
.LFE1:
        .size   main, .-main
        .ident  "GCC: (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0"
        .section        .note.GNU-stack,"",@progbits
        .section        .note.gnu.property,"a"
        .align 8
        .long   1f - 0f
        .long   4f - 1f
        .long   5
0:
        .string "GNU"
1:
        .align 8
        .long   0xc0000002
        .long   3f - 2f
2:
        .long   0x3
3:
        .align 8
4:
```
- DeepSeek对代码的解读<br> 

以下是提供的汇编代码的逐行解读及对应的功能分析：

---

### **1. 文件与段定义**
```x86asm
.file   "main.c"          ; 源文件名（编译自 main.c）
.text                     ; 代码段开始
```

---

### **2. `cal` 函数定义**
```x86asm
.p2align 4                ; 代码按 16 字节对齐（优化执行效率）
.globl  cal               ; 声明全局符号 cal
.type   cal, @function    ; 定义 cal 为函数类型
cal:
.LFB0:                    ; 函数开始标签
    .cfi_startproc        ; 调用帧信息开始（用于调试/异常处理）
    endbr64               ; Intel CET 安全指令（防代码重用攻击）
    movl    (%rsi), %eax  ; 将 rsi 指向的内存值加载到 eax（参数2的值）
    addl    (%rdi), %eax  ; 将 rdi 指向的内存值加到 eax（参数1的值 + 参数2的值）
    movl    %eax, (%rdi)  ; 将 eax 的结果写入 rdi 指向的内存（更新参数1的值）
    subl    %eax, (%rsi)  ; 从 rsi 指向的内存值中减去 eax（参数2 = 原参数2 - 新参数1）
    ret                   ; 函数返回
    .cfi_endproc          ; 调用帧信息结束
.LFE0:                    ; 函数结束标签
.size   cal, .-cal        ; 记录函数大小（计算为 cal 到当前地址的偏移）
```

#### **函数功能总结**：
- **输入**：两个指针参数 `rdi`（参数1）和 `rsi`（参数2），指向两个整数。
- **操作**：
  1. 将参数2的值加到参数1。
  2. 用新的参数1值更新原参数1的内存。
  3. 从参数2的原始值中减去新的参数1值，更新参数2的内存。
- **数学等效**：
  ```c
  void cal(int *a, int *b) {
      *a = *a + *b;  // 更新 a 为两数之和
      *b = *b - *a;  // 更新 b 为原 b 减去新 a（即原 b - (原 a + 原 b) = -原 a）
  }
  ```
  **最终结果**：`*a = 原a + 原b`，`*b = -原a`。

---

### **3. `main` 函数定义**
```x86asm
.section .text.startup,"ax",@progbits  ; 启动代码段（程序入口）
.p2align 4
.globl  main
.type   main, @function
main:
.LFB1:
    .cfi_startproc
    endbr64               ; 安全指令
    xorl    %eax, %eax    ; 将 eax 清零（返回值 0）
    ret                   ; 返回
    .cfi_endproc
.LFE1:
.size   main, .-main
```

#### **函数功能总结**：
- **操作**：直接返回 0。
- **等效 C 代码**：
  ```c
  int main() {
      return 0;
  }
  ```

---

### **4. 编译器与平台信息**
```x86asm
.ident  "GCC: (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0"  ; 编译器版本
.section .note.GNU-stack,"",@progbits                 ; 标记栈不可执行（安全特性）
.section .note.gnu.property,"a"                       ; GNU 属性段
.align 8
.long   1f - 0f
.long   4f - 1f
.long   5
0:
    .string "GNU"
1:
    .align 8
    .long   0xc0000002    ; 程序属性（如 CET 启用）
    .long   3f - 2f
2:
    .long   0x3           ; 属性大小
3:
    .align 8
4:
```

#### **关键信息**：
- **编译器**：GCC 11.4.0，Ubuntu 22.04 环境。
- **安全特性**：
  - `.note.GNU-stack`：标记栈不可执行（NX 保护）。
  - `.note.gnu.property`：包含控制流强制（CET）等安全属性。

---

### **关键点总结**
1. **`cal` 函数的行为**：
   - 接收两个整数指针，修改它们的值为 `*a + *b` 和 `*b - (*a + *b)`。
   - 例如，若 `a=1`, `b=2`，调用后 `a=3`, `b=-1`。

2. **`main` 函数**：
   - 无实际操作，直接返回 0，可能为占位或简化示例。

3. **汇编代码特点**：
   - 使用 x86-64 调用约定（参数通过 `rdi` 和 `rsi` 传递）。
   - 包含现代安全特性（如 `endbr64` 和 NX 保护）。

4. **调试与优化**：
   - `.p2align 4` 确保代码对齐，提升执行效率。
   - `endbr64` 和 CET 属性防止恶意控制流劫持。

---

### **完整 C 代码还原**
```c
// cal 函数
void cal(int *a, int *b) {
    *a = *a + *b;
    *b = *b - *a; // 等效于 *b = -(*a - *b)
}

// main 函数
int main() {
    return 0;
}
```

---

### **注意事项**
- **参数顺序**：x86-64 调用约定中，第一个参数通过 `rdi` 传递，第二个通过 `rsi`。
- **安全性**：`endbr64` 是 Intel CET 的一部分，仅在支持 CET 的 CPU 上生效。


## 2. 关于-O2优化后，main函数汇编代码中不包含cal函数的问题
已经观察到在-O0（无优化）的情况下，cal函数的调用确实出现在汇编代码中，但在默认的-O2优化级别下却不见了。

-O0是默认关闭所有优化，保持代码的原始结构，便于调试。而-O2则启用大量优化，包括删除未使用的代码、内联函数、常量传播等。代码中可能有一些因素导致编译器认为调用cal函数是没有必要的，因此被优化掉了。

C代码示例中，main函数调用了cal函数，而cal函数修改了全局变量num_1和num_2的值。但是在main函数中，这些全局变量在调用cal之后并没有被使用。例如，main函数可能在调用cal之后直接返回，而没有对num_1和num_2进行任何操作，比如打印它们或者返回它们的值。这种情况下，编译器可能会认为调用cal函数是无效的，因为其结果没有被使用，因此将其优化掉。

如果全局变量在调用cal之后没有被使用，那么根据C语言的“as-if”规则，编译器可以自由地消除这些无效的操作，因为程序的可见行为并没有改变。也就是说，只要程序的可观察行为（如输出、文件I/O等）不变，编译器可以优化掉不影响这些行为的代码。

另一个可能性是cal函数本身被编译器识别为无效代码。例如，如果cal函数没有副作用（即不修改全局变量或进行I/O操作），那么调用它可能被视为无操作，从而被删除。但根据之前的代码示例，cal函数确实修改了全局变量，所以这可能不适用。

此外，检查汇编代码发现main函数在调用cal之后直接返回0，而没有对num_1和num_2进行任何后续操作。因此，编译器可能会认为修改这些全局变量是不必要的，因为它们的值未被使用，从而将整个调用cal的代码删除。

还有一种可能是内联优化。在-O2级别下，cal函数可能被内联到main函数中，但由于某些原因，内联后的代码被进一步优化掉了。不过，调用cal的代码完全不存在，所以更可能的是整个调用被删除而非内联。

总结可能的原因：

1. **无用代码消除（Dead Code Elimination）**：如果cal函数修改的全局变量在后续代码中未被使用，编译器认为这些修改是无效的，因此删除调用。

2. **常量传播和优化**：如果num_1和num_2在编译时可以被确定为常量，或者它们的值在调用cal后未被使用，编译器可能直接优化掉相关操作。

3. **函数副作用分析**：如果编译器确定cal函数没有可观察的副作用（如不进行I/O，不修改volatile变量等），可能会删除调用。


了解编译器优化的具体机制，如“as-if”规则和优化传递，以更好地理解为什么某些代码会被删除。

在启用 `-O2` 优化级别时，GCC 会进行更激进的代码优化。如果 `main` 函数中调用 `cal` 的代码被优化掉，通常是因为编译器通过静态分析发现 **该函数调用对程序的最终行为没有影响**（即属于“无用代码”）。以下是具体原因和解决方案：

---

### **原因分析**
#### 1. **无用代码消除（Dead Code Elimination）**
   - 如果 `cal` 函数修改的全局变量（`num_1` 和 `num_2`）在后续代码中未被使用，编译器会认为这些修改是冗余的，直接删除相关操作。
   - **示例场景**：
     ```c
     int num_1, num_2;

     void cal(int *a, int *b) {
         *a = *a + *b;
         *b = *b - *a;
     }

     int main() {
         num_1 = 1;
         num_2 = 2;
         cal(&num_1, &num_2); // 修改全局变量
         return 0;            // 但未使用 num_1 和 num_2
     }
     ```
     - 在 `-O2` 优化下，`cal` 的调用会被删除，因为 `num_1` 和 `num_2` 的修改对程序输出无影响。

#### 2. **常量传播（Constant Propagation）**
   - 如果编译器能推断出 `cal` 的参数或返回值是常量，可能直接替换为计算结果。
   - **示例场景**：
     ```c
     void cal(int *a, int *b) {
         *a = 1; // 直接赋值为常量
     }

     int main() {
         int x = 0;
         cal(&x, NULL);
         return x; // 编译器直接替换为 return 1
     }
     ```
     - `cal` 的调用会被替换为 `x = 1`，甚至内联删除。

#### 3. **函数内联（Function Inlining）**
   - 如果 `cal` 函数体较小，可能被内联到 `main` 中，但若内联后发现结果未被使用，仍会被删除。
   - **示例**：
     ```c
     static inline void cal(int *a, int *b) {
         *a = *a + *b;
     }
     ```

---

### **解决方案**
若需保留 `cal` 的调用，需确保其操作对程序行为有实际影响：

#### 1. **使用全局变量**
   ```c
   int num_1, num_2;

   int main() {
       num_1 = 1;
       num_2 = 2;
       cal(&num_1, &num_2);
       printf("%d %d\n", num_1, num_2); // 强制使用全局变量
       return 0;
   }
   ```
   - 通过 `printf` 输出全局变量，编译器无法删除 `cal` 的调用。
