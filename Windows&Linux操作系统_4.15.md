# Windows & Linux
将Windows上的C++ 代码移植到Linux上，需要掌握以下几方面知识：

### Linux系统基础
- **系统操作**：熟悉Linux的基本命令，如文件操作（cp、mv、rm等）、目录切换（cd）、权限管理（chmod、chown等）。了解Linux的文件系统结构，包括根目录、系统目录（/bin、/etc等）、用户目录（/home）等。
- **软件安装与管理**：掌握使用包管理工具（如apt、yum等）安装、卸载和更新软件包。

### GCC编译工具链
- **GCC基础**：了解GCC（GNU Compiler Collection）的基本用法，包括如何使用GCC编译C++ 源文件，如何指定编译选项，如优化选项（-O1、-O2等）、调试选项（-g）等。
- **Makefile编写**：学会编写Makefile来管理项目的编译过程。掌握Makefile中的基本规则，如目标、依赖关系和命令，以及如何定义变量、使用函数等，以便能够自动化编译过程。

### C++ 语言特性及跨平台差异
- **标准C++ 特性**：确保对C++ 语言的标准特性有深入理解，包括数据类型、控制结构、函数、类、模板、STL等。因为在不同平台上，标准C++ 的行为应该是一致的，所以要保证代码中使用的是标准的C++ 特性，而不是依赖于特定平台的扩展。
- **平台相关差异**：了解Windows和Linux平台在C++ 编程方面的一些差异。例如，Windows上的文件路径分隔符是反斜杠（\），而Linux上是正斜杠（/）；Windows上的线程创建函数与Linux上的不同，需要使用不同的头文件和函数来实现线程功能。

### 库和依赖管理
- **库的使用**：如果代码中使用了第三方库，需要了解在Linux上如何安装和使用这些库。不同的库在Linux上可能有不同的安装方式，有些库可以通过包管理工具安装，而有些则需要从源代码编译安装。
- **动态链接与静态链接**：理解动态链接库（.so文件）和静态链接库（.a文件）在Linux上的区别和使用方法。知道如何在编译时指定链接库的路径和名称，以及如何处理库的依赖关系。

### 调试与测试
- **调试工具**：熟悉Linux上的调试工具，如GDB（GNU Debugger）。掌握如何使用GDB来调试C++ 程序，包括设置断点、查看变量值、单步执行等操作，以便能够快速定位和解决移植过程中出现的问题。
- **测试方法**：了解如何在Linux上进行软件测试，包括单元测试、集成测试等。可以使用一些开源的测试框架，如Google Test等，来编写和运行测试用例，确保移植后的代码功能正确。

### 文本编辑器与开发环境
- **文本编辑器**：掌握至少一种Linux上常用的文本编辑器，如Vim、Emacs或Nano等，以便能够方便地编辑C++ 源代码。了解这些编辑器的基本操作命令，如插入、删除、查找、替换等。
- **集成开发环境（IDE）**：了解一些在Linux上常用的C++ 开发IDE，如Eclipse CDT、CLion等。掌握如何在这些IDE中创建项目、配置编译选项、调试程序等基本操作，提高开发效率。

# 库
将Windows上的C++代码移植到Linux上时，库和依赖管理是关键环节。

### 系统库
#### 确定系统库差异
Windows和Linux使用不同的系统库。例如，Windows有Windows API，而Linux有POSIX标准库。检查代码中是否有使用Windows特定的系统库，若有，需替换为Linux等效库。
比如，在Windows上使用`CreateFile`来进行文件操作，在Linux上可以用`open`函数。
```cpp
// Windows 代码示例
#include <windows.h>
HANDLE hFile = CreateFile("test.txt", GENERIC_READ, 0, NULL, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, NULL);

// Linux 等效代码示例
#include <fcntl.h>
#include <unistd.h>
int fd = open("test.txt", O_RDONLY);
```

#### 安装系统库
在Linux上，许多系统库可通过包管理器来安装。例如，Ubuntu系统使用`apt`，CentOS系统使用`yum`。
若要安装开发工具包，在Ubuntu上可运行以下命令：
```bash
sudo apt-get update
sudo apt-get install build-essential
```

### 第三方库
#### 查找可用的Linux版本
很多流行的第三方库都有适用于Linux的版本。先去官方网站或者开源代码托管平台（如GitHub）查找这些库的Linux版本。
例如，Boost库是一个广泛使用的C++库集合，你可以从其官方网站下载源码并在Linux上编译安装。

#### 使用包管理器安装
有些第三方库能通过系统的包管理器直接安装。以OpenCV为例，在Ubuntu上可执行：
```bash
sudo apt-get install libopencv-dev
```

#### 从源码编译安装
若包管理器中没有所需的库，或者需要特定版本的库，就需要从源码编译安装。步骤如下：
1. **下载源码**：从官方网站或者代码托管平台获取库的源码压缩包。
2. **解压源码**：使用`tar`命令解压压缩包，例如：
```bash
tar -zxvf library_name.tar.gz
```
3. **配置编译选项**：进入解压后的目录，运行`./configure`脚本（如果有），该脚本会检查系统环境并生成Makefile。
```bash
cd library_name
./configure
```
4. **编译和安装**：使用`make`命令编译库，再用`make install`命令安装库。
```bash
make
sudo make install
```

### 静态链接库和动态链接库
#### 静态链接库（`.a`文件）
静态链接库会被链接到可执行文件中，使得可执行文件包含所有必要的代码。在Linux上编译时，使用`-static`选项来进行静态链接。
```bash
g++ main.cpp -o main -L/path/to/library -lstatic_library_name -static
```

#### 动态链接库（`.so`文件）
动态链接库在程序运行时才会被加载。编译时，使用`-L`选项指定库的搜索路径，用`-l`选项指定库名。
```bash
g++ main.cpp -o main -L/path/to/library -ldynamic_library_name
```
运行程序前，需要确保系统能够找到动态链接库。可以通过设置`LD_LIBRARY_PATH`环境变量来实现：
```bash
export LD_LIBRARY_PATH=/path/to/library:$LD_LIBRARY_PATH
./main
```

### 依赖管理工具
#### Conan
Conan是一个开源的C/C++包管理器，可帮助你管理项目的依赖。使用步骤如下：
1. **安装Conan**：通过Python的`pip`安装Conan。
```bash
pip install conan
```
2. **创建`conanfile.txt`**：在项目根目录创建`conanfile.txt`文件，列出项目的依赖。
```plaintext
[requires]
library_name/version

[generators]
cmake
```
3. **安装依赖**：在项目根目录运行以下命令安装依赖。
```bash
conan install .
```

#### vcpkg
vcpkg是Microsoft开发的跨平台C++包管理器。使用步骤如下：
1. **安装vcpkg**：从GitHub克隆vcpkg仓库并运行安装脚本。
```bash
git clone https://github.com/Microsoft/vcpkg.git
cd vcpkg
./bootstrap-vcpkg.sh
```
2. **安装依赖**：使用`vcpkg install`命令安装所需的库。
```bash
./vcpkg install library_name
```

# 移植可行性分析
## Wine
Wine（Wine Is Not an Emulator）是一个开源的兼容层，它允许 Windows 应用程序在 Linux 和其他类 Unix 操作系统上运行。Wine 实现了如下功能：
- API 翻译：Windows 程序依赖于 Windows API 来调用操作系统的功能，例如文件操作、图形处理、内存管理等。Wine 通过实现一个兼容的 Windows API，把这些 API 调用翻译成对应的 Linux API 调用，从而使得 Windows 程序可以在 Linux 环境中运行。
- 动态链接库（DLL）替换：Windows 程序通常依赖于各种 Windows 动态链接库（DLL）。Wine 提供了一组与 Windows DLL 相对应的开源实现，以满足 Windows 程序对这些DLL 的需求。这些开源实现的 DLL 可以替代原生的 Windows DLL。
- EXE 和 PE 格式支持：Windows 程序的可执行文件（如 .exe 和 .dll ）使用的是 PE（Portable Executable）格式。Wine 包含了一个 PE 加载器，能够读取和执行这些文件格式，并在 Linux 系统上加载和运行 Windows 程序。
- 图形界面支持：Wine 实现了对 Windows 图形接口（如 GDI 和 DirectX ）的支持，使得 Windows 程序可以在 Linux 上正常显示图形界面。Wine 通过使用 X Window System（X11）或其他图形系统，将 Windows 的图形调用转换成 Linux 能够理解的图形调用。
- 注册表支持：Wine实现了 Windows 注册表的功能，这使得 Windows 程序可以读取和写入注册表设置。Wine 在 Linux 文件系统中创建一个虚拟的 Windows 注册表文件，供 Windows 程序使用。
- 文件系统映射：Wine 将 Windows 文件系统路径映射到 Linux 的文件系统路径。例如，C:\ drive 通常被映射到 Linux 上的一个目录（如 ~/.wine/drive_c ）。
通过这些技术， Wine 能够在 Linux 上提供一个 Windows 兼容的运行环境，使得大多数 Windows 应用程序可以在 Linux 上运行，而无需修改程序代码。

但由于 Windows 是闭源操作系统，加上 Windows 和 Linux 操作系统架构之间的差异，导致某些 Windows API 实现在 Linux 下表现和 Windows 下不同，导致一些兼容问题。经过多年的发展，兼容性问题已经得到很大程度的解决，主流的 Windows 应用程序和许多大型游戏都能在 Linux 下完美运行。

由于驱动更加底层，所以是无法通过 Wine 使用 Windows 驱动的。
