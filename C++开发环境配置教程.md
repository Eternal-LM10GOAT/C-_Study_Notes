# C++开发环境配置教程

## Ubuntu下环境配置

**vscode**属于编辑器，用于进行代码编辑；**clang和gcc**之类的属于编译器，用于将C++程序翻译为二进制代码；

**clangd**则是一个语言服务器，专注于为 IDE 或编辑器提供智能化的代码分析服务，支持代码补全、语法高亮、错

误提示、跳转到定义、代码重构等功能；**LLDB和GDB**属于调试器；**CMake** 是一个 **跨平台的构建系统生成器**，用

于自动化软件项目的构建过程。

1、ubuntu上安装**编译器clang和语言服务器clangd以及调试器lldb**

```
sudo apt update
sudo apt clang
sudo apt clangd
sudo apt lldb
```

2、在vscode上安装clangd插件

3、在vscode上安装 “C/C++ Clang Command Adapter” 插件，为 VSCode 提供基于 Clang 的代码补全和诊断功能

4、在vscode上安装codelldb插件，**CodeLLDB 插件是一个基于 LLDB（Low Level Debugger）的调试工具**

5、安装自动化项目构建工具cmake和ninja（安装cmake和make也可以，但是ninja速度比较快）

```
sudo apt install cmake
sudo apt install ninja-build
```

6、安装代码格式化插件clang-format

```
sudo apt install clang-format
```

7、配置格式化代码clang-format

```
生成默认配置文件：
在任意目录运行以下命令，生成基于 LLVM 风格的默认配置文件：

clang-format -style=llvm -dump-config > ~/.clang-format

~/.clang-format 是全局配置文件，适用于所有项目。
如果需要特定项目使用不同风格，可在项目根目录创建 .clang-format 文件。
```

8、常用clang-format配置

```
# .clang-format 文件内容
BasedOnStyle: LLVM
IndentWidth: 4
UseTab: Never
BreakBeforeBraces: Allman
AllowShortIfStatementsOnASingleLine: false
IndentCaseLabels: true
ColumnLimit: 120
AccessModifierOffset: -4
NamespaceIndentation: All
FixNamespaceComments: true

# 空格相关
SpaceBeforeParens: ControlStatements
SpaceInEmptyParentheses: false
SpacesInCStyleCastParentheses: false
SpacesInContainerLiterals: false
SpaceAfterCStyleCast: false
SpaceBeforeCpp11BracedList: false

# 括号对齐
AlignConsecutiveMacros: true
AlignConsecutiveDeclarations: true
AlignConsecutiveAssignments: true
AlignOperands: Align
AlignTrailingComments: true
AlignEscapedNewlines: Right

# 指针/引用对齐
PointerAlignment: Right
ReferenceAlignment: Right

# 构造函数初始化列表
ConstructorInitializerIndentWidth: 4
ConstructorInitializerAllOnOneLineOrOnePerLine: false

# 其他
SortIncludes: false
SortUsingDeclarations: false
Cpp11BracedListStyle: true
```

安装Code snippets可以配置快速生成指定代码（选装）

安装Doxygen插件生成注释
