# cmake最小版本号

```cmake
cmake_minimum_required(VERSION xxx)
```

# 指定项目名称

```cmake
project(demo)
```

# set的五种用法

## 将源文件放到变量中

```cmake
#set(变量名 源文件名)
set(SRC add.cpp div.cpp mult.cpp main.cpp sub.cpp)
```

## 指定编译版本

```cmake
#宏定义CMAKE_CXX_STANDARD 11 、CMAKE_CXX_STANDARD 14、CMAKE_CXX_STANDARD 17
set(CMAKE_CXX_STANDARD 11)
```

## 指定输出路径

```cmake
#使用绝对路径CMakeLists.txt拷贝到其他机器上也能使用
set(EXECUTABLE_OUTPUT_PATH  /home/ny/code/project)
```

## 指定编译选项

```cmake
#-O2表示优化 -Wall表示打印警告信息
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -Wall")

#生成可调试版本
set(CMAKE_BUILD_TYPE Debug)
```

## 使用clangd插件

```cmake
#生成compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```

# 指定源文件目录

```ABAP
$ tree
.
├── build
├── CMakeLists.txt
├── include
│   └── head.h
└── src
    ├── add.cpp
    ├── div.cpp
    ├── main.cpp
    ├── mult.cpp
    └── sub.cpp
```

当源文件很多时，使用set命令不方便，此时可以指定源文件目录。

```cmake
#PROJECT_SOURCE_DIR是指CMakeLists.txt文件所在的目录
#src指源文件的目录名字
#SRC为保存文件的变量名
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC)
```

```cmake
#CMAKE_CURRENT_SOURCE_DIR始终指向包含当前被处理的CMakeLists.txt文件的目录。
file(GLOB SRC ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
```

# 指定头文件目录

```cmake
include_directories(${PROJECT_SOURCE_DIR}/include)
```

# 链接接线程库

```cmake
# 查找线程库  
find_package(Threads REQUIRED)  
  
# 添加可执行文件  
add_executable(my_program main.cpp)  
  
# 链接线程库到可执行文件  
target_link_libraries(my_program PRIVATE Threads::Threads)
```

# 输出可执行程序

```cmake
add_executable(app ${SRC})
```

# ----------------------

# 制作库文件

```cmake
#静态库名字:lib + 库名字 + .a
#动态库名字:lib + 库名字 + .so
#add_library(库名字 static 源文件)

#指定库文件的输出路径
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#实例
#指定cmake最小版本
cmake_minimum_required(VERSION 3.0)
#工程文件名
project(CALC)
#包含的头文件
include_directories(${PROJECT_SOURCE_DIR}/include)
#源文件放到SRC变量
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC)
#指定库路径
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
#生成静态库
add_library(calc STATIC ${SRC})
#生成动态库
add_library(calc SHARED ${SRC})
```

# 使用库文件

```ABAP
$ tree 
.
├── build
├── CMakeLists.txt
├── include
│   └── head.h
├── lib
│   └── libcalc.a     # 制作出的静态库的名字
└── src
    └── main.cpp
```

## 静态库的使用

```cmake
#当使用的库不是系统库而是第三方库时，需要包含静态库路径 link_directories(库的路径)
#链接静态库 link_libraries(库名1 库名2 ...)
link_libraries(calc)
link_directories(${PROJECT_SOURCE_DIR}/lib)

#实例
cmake_minimum_required(VERSION 3.0)
project(CALC)
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC)
include_directories(${PROJECT_SOURCE_DIR}/include)
link_directories(${PROJECT_SOURCE_DIR}/lib)
link_libraries(calc)
add_executable(app ${SRC})
```

## 动态库的使用

```cmake
#target 指需要加载动态库的文件
#动态库在运行时加载，所以一般放在最后
#target_link_libraries(target 库1 库2 ...)

#实例
cmake_minimum_required(VERSION 3.0)
project(CALC)
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC)
include_directories(${PROJECT_SOURCE_DIR}/include)
link_directories(${PROJECT_SOURCE_DIR}/lib)
add_executable(app ${SRC})
target_link_libraries(app calc)
```

# 防止git跟踪

在 VSCode 中运行 CMakeLists.txt 后，若 Git 仓库中多出大量文件，通常是因为 CMake 生成了构建文件（如 build/ 目录、CMakeCache.txt、CMakeFiles/ 等），而 Git 默认会跟踪这些文件。以下是解决方法：

### **1. 使用独立的构建目录（推荐）**

CMake 支持 **Out-of-Source 构建**，即在项目根目录外单独创建构建目录（如 `build/`），避免污染源代码目录：

1. **创建构建目录**：

   ```bash
   bash复制代码
   
   mkdir build && cd build
   ```

2. **在构建目录中运行 CMake**：

   ```bash
   bash复制代码
   
   cmake ..
   ```

3. **编译项目**：

   ```bash
   bash复制代码
   
   cmake --build .
   ```

这样，所有生成的文件会集中在 `build/` 目录中，与源代码隔离。

------

### **2. 配置 `.gitignore` 忽略构建文件**

在项目根目录的 `.gitignore` 文件中添加以下内容，忽略常见 CMake 生成文件：

```gitignore
# CMake 生成文件
build/
CMakeCache.txt
CMakeFiles/
CMakeScripts/
Testing/
Makefile
cmake_install.cmake
*.cmake
*.user
*.sln
*.vcxproj
*.vcxproj.filters
*.suo
*.pdb
*.ilk
*.exp
*.lib
*.dll
*.exe
*.o
*.a
*.so
```
