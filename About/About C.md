# About C Keyword

## In C Programming
In C programming, keywords are reserved words that have specific meanings and purposes defined by the language standard. They cannot be used as identifiers (e.g., variable names or function names) because they are part of the language’s syntax and semantics. Below are the major types of keywords in C, grouped by their functionality, based on the C11 standard (the most recent widely-used standard as of my knowledge base). I’ll also explain their roles briefly.

### Total Keywords in C11
C11 defines **44 keywords**. These can be categorized into several logical groups based on their purpose:

---

### 1. **Data Type Keywords**
These keywords define the type of data a variable can hold.
- `char` - Defines a character type (usually 8 bits).
- `int` - Defines an integer type.
- `float` - Defines a single-precision floating-point type.
- `double` - Defines a double-precision floating-point type.
- `void` - Indicates no type (used for functions with no return value or generic pointers).
- `short` - Modifier for `int` to reduce size (e.g., `short int`).
- `long` - Modifier for `int` or `double` to increase size (e.g., `long int`, `long double`).
- `signed` - Specifies a signed integer type (default for most types).
- `unsigned` - Specifies an unsigned integer type.
- `_Bool` - Defines a boolean type (0 or 1, introduced in C99).
- `_Complex` - Defines a complex number type (introduced in C99).
- `_Imaginary` - Defines an imaginary number type (introduced in C99, optional).

---

### 2. **Storage Class Keywords**
These keywords specify the storage duration and linkage of variables or functions.
- `auto` - Specifies automatic storage duration (default for local variables, rarely used explicitly).
- `extern` - Declares a variable or function with external linkage (defined elsewhere).
- `static` - Specifies static storage duration (retains value between calls) or internal linkage (for functions/files).
- `register` - Suggests storing a variable in a CPU register for faster access (hint to compiler, not guaranteed).
- `_Thread_local` - Specifies thread-local storage (introduced in C11).

---

### 3. **Control Flow Keywords**
These keywords manage the flow of execution in a program.
- `if` - Starts a conditional statement.
- `else` - Specifies an alternative block for an `if` statement.
- `switch` - Starts a multi-way branch statement.
- `case` - Labels a branch in a `switch` statement.
- `default` - Specifies the default branch in a `switch` statement.
- `for` - Defines a loop with initialization, condition, and increment.
- `while` - Defines a loop based on a condition.
- `do` - Starts a do-while loop (condition checked after execution).
- `break` - Exits the innermost loop or `switch` statement.
- `continue` - Skips to the next iteration of a loop.
- `goto` - Jumps to a labeled statement in the same function.
- `return` - Exits a function and optionally returns a value.

---

### 4. **Qualifier Keywords**
These modify how variables are accessed or treated by the compiler.
- `const` - Declares a variable as read-only (cannot be modified after initialization).
- `volatile` - Indicates that a variable’s value may change unexpectedly (disables certain optimizations).
- `restrict` - Hints that a pointer is the only way to access the object it points to (introduced in C99, for optimization).

---

### 5. **Structure and Union Keywords**
These define composite data types.
- `struct` - Defines a structure (a collection of variables).
- `union` - Defines a union (a type that can hold different types in the same memory location).
- `enum` - Defines an enumeration (a set of named integer constants).

---

### 6. **Function and Type-Related Keywords**
These assist in function definitions or type management.
- `sizeof` - Returns the size of a type or variable in bytes.
- `typedef` - Creates an alias for an existing type.
- `_Alignas` - Specifies the alignment of an object (introduced in C11).
- `_Alignof` - Queries the alignment requirement of a type (introduced in C11).
- `_Generic` - Enables type-generic programming (introduced in C11).

---

### 7. **Atomic and Synchronization Keywords (C11)**
These are for concurrency and atomic operations.
- `_Atomic` - Declares a variable or type as atomic (introduced in C11).
- `_Noreturn` - Indicates that a function does not return (introduced in C11).

---

### 8. **Miscellaneous Keywords**
These don’t fit neatly into other categories but are still significant.
- `inline` - Suggests inlining a function for performance (introduced in C99).

---

### Notes on Evolution of Keywords
- **C89 (ANSI C)**: Had 32 keywords.
- **C99**: Added 5 new keywords (`_Bool`, `_Complex`, `_Imaginary`, `inline`, `restrict`).
- **C11**: Added 7 more (`_Alignas`, `_Alignof`, `_Atomic`, `_Generic`, `_Noreturn`, `_Static_assert`, `_Thread_local`), bringing the total to 44.

Not all compilers fully support every keyword (e.g., `_Imaginary` is optional in C99/C11).

---

### Practical Example
Here’s a snippet showing some keywords in action:
```c
volatile int sensor_value;           // Variable might change unexpectedly
static const float PI = 3.14159;     // Constant value, static duration
struct Point { int x, y; };          // Structure definition
typedef unsigned long ulong;         // Type alias
for (int i = 0; i < 10; i++) {       // Loop control
    if (i % 2 == 0) continue;       // Skip even numbers
}
```

---

### How Keywords Affect Compiler/CPU
- **Compiler**: Keywords guide how the compiler generates machine code (e.g., `volatile` prevents optimization, `const` enforces read-only checks).
- **CPU**: Keywords indirectly affect CPU behavior by dictating memory access patterns (e.g., `volatile` ensures fresh reads), register usage (`register`), or instruction flow (`break`, `return`).

If you’d like a deeper dive into any specific keyword (e.g., how `volatile` works with STM32F429 registers), let me know!

## `__attribute__`

`__attribute__` it’s NOT A KEYWORD in the standard C programming language. Let me explain why this is the case and provide some clarity.

### What Are C Keywords?
In C, keywords are specific words defined by the ISO/IEC standards (like C89, C99, or C11) that have special meaning to the compiler. These words—such as `int`, `for`, `return`, or `if`—are reserved, meaning you can’t use them as variable names or identifiers in your code. The full list of keywords depends on the version of the C standard you’re using, but it’s always a fixed, universal set recognized by all standard-compliant C compilers.

### Why `__attribute__` Isn’t a Standard Keyword
Unlike these standard keywords, `__attribute__` is **not part of the C language as defined by the standard**. Instead, it’s a **compiler-specific extension**, primarily introduced by the GNU Compiler Collection (GCC) and supported by other compilers like Clang that adopt GCC’s features. Here’s why it doesn’t qualify as a standard C keyword:

- **Compiler-Specific Feature**: `__attribute__` is an addition provided by certain compilers, not a requirement of the C standard. This means it’s not guaranteed to work across all C compilers.
- **Non-Portable**: Code using `__attribute__` might not compile or behave the same way if you switch to a compiler that doesn’t support GCC extensions, like Microsoft’s MSVC.
- **Purpose**: It’s used to give the compiler extra instructions about functions, variables, or types—things like alignment, optimization hints, or behavior specifications. For example:
  ```c
  void my_function() __attribute__((noreturn));
  ```
  This tells GCC that `my_function` never returns, which can help the compiler optimize the code.

### Standard C vs. Extensions
Since `__attribute__` isn’t in the official C standard, it wasn’t included in a list of C keywords. The focus was on the universal keywords that every C compiler must recognize. However, there are cases where the C standard offers alternatives. For instance, C11 introduced the `_Noreturn` keyword to indicate a function doesn’t return, which overlaps with `__attribute__((noreturn))`. Not all uses of `__attribute__` have standard equivalents, though, which is why it remains popular in GCC environments.

### Practical Implications
If you’re using GCC or a compatible compiler, `__attribute__` can be a powerful tool for fine-tuning your code. But because it’s not standard, you should be cautious about relying on it if you need your code to be portable across different compilers. In such cases, you might use preprocessor directives (e.g., `#ifdef`) to handle compiler-specific features more safely.

In short, I omitted `__attribute__` because it’s not a keyword in standard C—it’s an extension specific to certain compilers. If you’re curious about how it works in GCC or have a specific use case in mind, feel free to ask for more details!

## Operators
以下是C语言运算符的分类、说明及示例，按照优先级从高到低排列：

---

### **1. 基本运算符**
#### **1.1 括号与成员访问**
- `()`：函数调用或强制优先级  
  ```c
  int sum = add(a, b); // 函数调用
  int result = (a + b) * c; // 强制先计算a+b
  ```
- `[]`：数组下标访问  
  ```c
  int arr[5] = {1, 2, 3};
  int x = arr[0]; // x = 1
  ```
- `.`：结构体/联合体成员访问  
  ```c
  struct Point { int x; } p;
  p.x = 10;
  ```
- `->`：通过指针访问结构体/联合体成员  
  ```c
  struct Point *ptr = &p;
  ptr->x = 20;
  ```

---

### **2. 单目运算符（右结合）**
- `++`、`--`：自增/自减  
  ```c
  int i = 5;
  int a = i++; // a=5, i=6（后缀）
  int b = ++i; // b=7, i=7（前缀）
  ```
- `&`：取地址  
  ```c
  int num = 10;
  int *p = &num; // p指向num的地址
  ```
- `*`：解引用指针  
  ```c
  int value = *p; // value = 10
  ```
- `+`、`-`：正负号  
  ```c
  int x = -5;
  int y = +10;
  ```
- `~`：按位取反  
  ```c
  unsigned char a = 0b00001111; // ~a = 0b11110000
  ```
- `!`：逻辑非  
  ```c
  int flag = 0;
  if (!flag) { /* 执行 */ }
  ```
- `sizeof`：获取数据类型或对象的大小  
  ```c
  size_t size = sizeof(int); // 通常为4字节
  ```

---

### **3. 算术运算符**
- `*`、`/`、`%`：乘、除、取模  
  ```c
  int a = 10 * 3; // 30
  int b = 10 / 3; // 3（整数除法）
  int c = 10 % 3; // 1
  ```
- `+`、`-`：加、减  
  ```c
  int d = 10 + 5; // 15
  int e = 10 - 5; // 5
  ```

---

### **4. 移位运算符**
- `<<`：左移  
  ```c
  int x = 1 << 3; // x = 8（二进制0001 → 1000）
  ```
- `>>`：右移（算术右移保留符号位）  
  ```c
  int y = -8 >> 2; // 结果依赖编译器，通常为-2
  ```

---

### **5. 关系运算符**
- `<`、`<=`、`>`、`>=`：大小比较  
  ```c
  if (a > b) { /* ... */ }
  ```
- `==`、`!=`：等于/不等于  
  ```c
  if (a == 0) { /* ... */ }
  ```

---

### **6. 位运算符**
- `&`：按位与  
  ```c
  int mask = 0b1010 & 0b1100; // 结果0b1000
  ```
- `^`：按位异或  
  ```c
  int xor = 0b1010 ^ 0b1100; // 结果0b0110
  ```
- `|`：按位或  
  ```c
  int or = 0b1010 | 0b1100; // 结果0b1110
  ```

---

### **7. 逻辑运算符**
- `&&`：逻辑与（短路求值）  
  ```c
  if (a != NULL && a->value == 10) { /* ... */ }
  ```
- `||`：逻辑或（短路求值）  
  ```c
  if (a == 0 || b == 0) { /* ... */ }
  ```

---

### **8. 条件运算符（三目运算符）**
- `?:`：条件判断  
  ```c
  int max = (a > b) ? a : b;
  ```

---

### **9. 赋值运算符**
- `=`：赋值  
  ```c
  int x = 10;
  ```
- 复合赋值：`+=`、`-=`、`*=`、`/=`、`%=`、`<<=`、`>>=`、`&=`、`^=`、`|=`  
  ```c
  a += 5; // 等价于 a = a + 5
  ```

---

### **10. 逗号运算符**
- `,`：按顺序执行，返回最后一个表达式的值  
  ```c
  int a = (x=5, y=10, x+y); // a=15
  ```

---

### **优先级与结合性速查表**
| 优先级 | 运算符                     | 结合性   |
|--------|----------------------------|----------|
| 最高   | `()` `[]` `.` `->` `++`（后缀） `--`（后缀） | 左结合   |
|        | `++`（前缀） `--`（前缀） `&` `*` `+`（正） `-`（负） `~` `!` `sizeof` | 右结合   |
|        | `*` `/` `%`                | 左结合   |
|        | `+` `-`                    | 左结合   |
|        | `<<` `>>`                  | 左结合   |
|        | `<` `<=` `>` `>=`          | 左结合   |
|        | `==` `!=`                  | 左结合   |
|        | `&`                        | 左结合   |
|        | `^`                        | 左结合   |
|        | `\|`                       | 左结合   |
|        | `&&`                       | 左结合   |
|        | `\|\|`                     | 左结合   |
|        | `?:`                       | 右结合   |
|        | `=` `+=` `-=` 等复合赋值   | 右结合   |
| 最低   | `,`                        | 左结合   |

---

### **常见注意事项**
1. **优先级陷阱**：`a & 3 == 2` 等价于 `a & (3 == 2)`，应使用括号明确意图：`(a & 3) == 2`。
2. **逻辑与位运算符混淆**：`&&` 是逻辑与，`&` 是按位与。
3. **自增/自减副作用**：避免在同一个表达式中多次修改同一变量（如 `i = i++` 是未定义行为）。
4. **短路求值**：利用 `&&` 和 `||` 避免不必要的计算（如空指针检查）。

通过掌握这些运算符及其优先级，可以编写高效且准确的C代码。