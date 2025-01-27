# 关于CMake

## 项目示例
project tree
```css
my_project/
├── CMakeLists.txt
├── src/
│   ├── main.c
│   ├── utils.c
│   └── another_module.c
├── include/
│   ├── utils.h
│   └── another_module.h
└── build/
```
### CMakeLists.txt
针对上一个同级标题的示例
```cmake
cmake_minimum_required(VERSION 3.10)

# Set project name
project(STM32F429IGTx_Test)

# Set C standard
set(CMAKE_C_STANDARD 11)

# Define global macros
add_definitions(-DGLOBAL_TEST_MACRO=1)

# Set debugging flags
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -g")  # Add debugging information
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -O0") # Disable optimization for Debug builds

# Set header file directory
include_directories(${PROJECT_SOURCE_DIR}/include)

# Set source file directory
file(GLOB SOURCES "src/*.c")  # Automatically find all .c files in the src directory

# Add executable file
add_executable(main ${SOURCES})

# Set the output directory for executables
set_target_properties(main PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Set the target output file name
set_target_properties(main PROPERTIES OUTPUT_NAME "main")

```