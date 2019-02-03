---

layout: default
author: lunarwaterfox
title: CMake Hello World
categories: cmake

---

# 关于 CMake 
> [https://cmake.org/](https://cmake.org/)

# Hello World!

建立一个最简单的 Hello World 工程

```console
$ tree Hello
Hello
├── Hello
│   ├── CMakeLists.txt
│   └── main.cpp
└── build
```

```cmake
# CMakeLists.txt 是 cmake 工程的配置文件

cmake_minimum_required(VERSION 3.12)
project(hello)

add_executable(hello main.cpp)
```

```cpp
// main.cpp 一个简单的 Hello World

#include <iostream>

int main(int argc, char *argv[]) {
    std::cout << "Hello World!" << std::endl;

    return 0;
}
```
---

cmake 编译、运行

```console
$ cd build
$ cmake ../Hello
$ cmake --build .
$ ./hello
Hello World!
```

# 构建器
可以使用 -G 指定构建器，比如指定 Xcode 构建

```console
$ cmake -G
CMake Error: No generator specified for -G

Generators
  Unix Makefiles               = Generates standard UNIX makefiles.
  Ninja                        = Generates build.ninja files.
  Xcode                        = Generate Xcode project files.
  ...

$ cmake -G Xcode ../Hello
```

其中：

- 只能支持一种构建模式的构建器称为 single-configuration，比如 makefile，ninja
- 可以支持多种构建模式的构建器称为 multi-configuration，比如 Xcode，Visual studio

# 构建模式
cmake  支持多种构建模式，其默认列表为

- Debug
- Release
- MinSizeRel
- RelWithDebInfo

single-configuration 需要在生成配置时，就指定好构建模式

```console
$ cmake ../Hello -DCMAKE_BUILD_TYPE=Release
$ cmake --build .
```

multi-configuration 可以在构建阶段切换

```console
$ cmake ../Hello -GXcode
$ cmake --build . --config Release
```

# 相关链接
- [CMake Language]({% post_url 2019-01-29-cmake-language %})


