---
tags:
  - cmake
  - cpp
  - 目录
---
>cmake命令不分大小写
## 变量说明
变量使用
```cmake
${VAR}
```
的方式取出
>变量要分大小写
## 参数说明
参数名要用空格或者分号分开，如：
```cmake
add_executable(demo main.cpp main.cc main.cxx main.ixx)
#也可以
add_executable(demo;main.cpp;main.cc;main.cxx;main.ixx)
```
参数名中有空格的画用引号括起来，如：
```cmake
add_executable(cmake "my src.cpp")
```
> c语言是C,c++是CXX\
> 参数名要分大小写
## cmake最低版本要求
```cmake
cmake_minimum_required(VERSION 3.15)
```
## 添加目标
### 构建一个可执行文件
```cmake
add_executable(demo src/main.cpp)
```
### 构建静态或动态库文件
```cmake
add_library(lib SHARED src/lib.cpp) #生成动态库文件
add_library(lib STATIC src/lib.cpp) #生成静态库文件
```
## 打印
可以通过这条命令打印上面设置的项目名
```cmake
message(${PROJECT_NAME})
```
不同模式
```cmake
message("hello")
# 输出：hello
message(STATUS "hello")
# 输出：-- hello
message(SEND_ERROR "hello")
# 产生错误，跳过构建过程
message(FATAL "hello")
# 产生错误，中止运行
```
## set
```cmake
set(变量名 值)
```
例如

```cmake
set(num 1)
message(${num})
# 输出：1
```
### 使用set()设置生成目录
```cmake
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BIANRY_DIR}/bin)
```
## 内置变量
| 变量名                        | 解释 |
| :-----:                       | :----: |
| C                             | c语言 |
| CXX                           | c++ |
| PROJECT_NAME                  | 项目名 |
| PROJECT_SOURCE_DIR            | 项目目录 |
| <PROJECT_NAME>_SOURCE_DIR     | 项目目录 |
| PROJECT_BIANRY_DIR            | 二进制文件目录 |
| <PROJECT_NAME>_BIANRY_DIR     | 二进制文件目录 |
| PROJECT_VERSION               | 项目版本号 |
| <PROJECT_NAME>_VERSION        | 项目版本号 |
| PROJECT_VERSION_MAJOR         | 主版本号 |
| <PROJECT_NAME>_VERSION_MAJOR  | 主版本号 |
| PROJECT_VERSION_MINOR         | 次版本号 |
| <PROJECT_NAME>_VERSION_MINOR  | 次版本号 |
## 设置语言标准
```cmake
set(CMAKE_CXX_STANDARD 23)#设置c++标准为c++23
```
## option
定义一个开关
```cmake
option(IS_ "is" ON)#这里设置了默认为ON,不设置默认为OFF
```
## 逻辑
```cmake
if()
#语句
elseif()
#语句
else()
#语句
endif()
```
## 头文件包含目录
#### include_directories
```cmake
添加所有目标的头文件包含目录
include_directories(目录1 目录2 ...)
```
#### target_include_directories
```cmake
添加目标的头文件包含目录
target_include_directories(<target> PUBLIC 目录1)
```
>一般用这个

## 打开compile_commands文件输出
```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```