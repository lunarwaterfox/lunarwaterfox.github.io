---

layout: default
author: lunarwaterfox
title: CMake Modules (1) - FetchContent
categories: cmake

---

# 关于 FetchContent 
> [CMake-Modules: FetchContent](https://cmake.org/cmake/help/latest/module/FetchContent.html)

FetchContent 是 cmake 最新的引用外部依赖的模块，取代了之前的 ExternalProject，从 3.11 开始支持，使用时需设置最低版本号 3.11

```cmake
cmake_minimum_required(VERSION 3.11)
include(FetchContent)
```

另：FetchContent 与 ExternalProject，最大的不同是 FetchContent 是在 configure time 获取外部工程，而 ExternalProject 是在 build time 获取

# Demo

```console
$ tree Hello
Hello
├── Hello
│   └── CMakeLists.txt
└── build
```

CMakeLists.txt 内容

```cmake
cmake_minimum_required(VERSION 3.11)

include(FetchContent)

FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG        release-1.8.0
)

FetchContent_GetProperties(googletest)

if(NOT googletest_POPULATED)
    FetchContent_Populate(googletest)
    add_subdirectory(${googletest_SOURCE_DIR} ${googletest_BINARY_DIR})
endif()
```

编译

```console
$ cd build
$ cmake ../Hello
$ cmake --build .
```

# FetchContent_Declare

```cmake
FetchContent_Declare(<name> <contentOptions>...)
```

FetchContent_Declare 支持的几种类型

```cmake
FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG        release-1.8.0
)

FetchContent_Declare(
    myCompanyIcons
    URL      https://intranet.mycompany.com/assets/iconset_1.12.tar.gz
    URL_HASH 5588a7b18261c20068beabfb4f530b87
)

FetchContent_Declare(
    myCompanyCertificates
    SVN_REPOSITORY svn+ssh://svn.mycompany.com/srv/svn/trunk/certs
    SVN_REVISION   -r12345
)
```

FetchContent_Declare 只是用于设置参数，相关的参数可以使用后续的 FetchContent_GetProperties 取到

# FetchContent_GetProperties

```cmake
FetchContent_GetProperties( <name>
    [SOURCE_DIR <srcDirVar>]
    [BINARY_DIR <binDirVar>]
    [POPULATED <doneVar>]
)
```

FetchContent_GetProperties 会把相关参数写入全局变量里

| name_SOURCE_DIR | 源码路径 |
| name_BINARY_DIR | 生成路径 |
| name_POPULATED | 是否已完成 Populate |

其中 name 是 FetchContent_Declare 时设置的名称

# FetchContent_Populate

```cmake
FetchContent_Populate( <name> )
```
开始获取
