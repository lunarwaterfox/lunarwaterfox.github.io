---

layout: default
author: lunarwaterfox
title: CMake Language
categories: cmake

---

# 关于 CMake Language
> [CMake Language](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html)


# 文件结构
```console
$ tree Hello
Hello
├── Hello
│   ├── CMakeLists.txt
│   ├── cmake
│   │   └── utils.cmake
│   └── main.cpp
└── build
```

# message

在 Hello/cmake/utils.cmake 写入

```cmake
message("Hello World!")
```

message 是标准输出语句

```console
$ cmake -P Hello/cmake/utils.cmake
Hello World!
```

cmake -P 表示仅执行 cmake 脚本，而不构建项目

---

message 支持以下几种极别
> [Reference: message](https://cmake.org/cmake/help/latest/command/message.html)

| mode | 使用方式 |
| :- | :- |
| (none) | 重要信息 |
| STATUS | 次要信息 |
| WARNING | Warning 信息，不影响生成 |
| AUTHOR_WARNING | 同 WARNING，但仅 dev 状态显示 |
| SEND_ERROR | Error 信息，不影响生成，跳过构建 |
| FATAL_ERROR | Error 信息，停止生成 |

# set

用 set 设置变量

```cmake
set(NAME "World")
message("Hello ${NAME}!")
```

```console
$ cmake -P Hello/cmake/utils.cmake
Hello World!
```

复合变量

```cmake
set(NAME "World")
set(MSG "Hello ${NAME}!")
message("${MSG}")
```

```console
$ cmake -P Hello/cmake/utils.cmake
Hello World!
```

通过编译命令 -D 指定的变量

```cmake
message("Hello ${NAME}!")
```

```console
$ cmake -DNAME=World -P Hello/cmake/utils.cmake
Hello World!
```

# list
可以用 set 来设置 list

```cmake
set(NAMES John Susan Michelle)  # 等价
set(NAMES John;Susan;Michelle)  # 等价
set(NAMES John Susan;Michelle)  # 等价
```

展示 NAMES 的第二个元素

```cmake
set(NAMES John Susan Michelle)
list(GET NAMES 2 NAME)
message("Hello ${NAME}!")
```

```console
$ cmake -P Hello/cmake/utils.cmake
Hello Michelle!
```

list 的更多用法参考
> [Reference: list](https://cmake.org/cmake/help/latest/command/list.html#sort)

# if

```cmake
if(NAME STREQUAL "con1")
    message("condition 1")
elseif(NAME STREQUAL "con2")
    message("condition 2")
else()
    message("condition 3")
endif()
```

```console
$ cmake -DNAME=con1 -P Hello/cmake/utils.cmake
condition 1
$ cmake -DNAME=con2 -P Hello/cmake/utils.cmake
condition 2
$ cmake -P Hello/cmake/utils.cmake
condition 3
```

条件判断的更多用法参考
> [Reference: if](https://cmake.org/cmake/help/latest/command/if.html)


# while

```cmake
while(${X} GREATER 0)
    message("X=${X}")
    math(EXPR X "${X} - 1")
endwhile()
```

```console
$ cmake -DX=3 -P Hello/cmake/utils.cmake
X=3
X=2
X=1
```

条件判断同 if

# foreach

常规循环

```cmake
foreach(X RANGE 0 10 3)
    message("X=${X}")
endforeach()
```

```console
$ cmake -P Hello/cmake/utils.cmake
X=0
X=3
X=6
X=9
```

遍历 list

```cmake
set(A 0;1)
set(B 2 3)
set(C "4 5")
set(D 6;7 8)
set(E "")

foreach(X IN LISTS A B C D E)
    message("X=${X}")
endforeach()
```

```console
$ cmake -P Hello/cmake/utils.cmake
X=0
X=1
X=2
X=3
X=4 5
X=6
X=7
X=8
```


