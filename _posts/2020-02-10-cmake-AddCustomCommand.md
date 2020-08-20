---

layout: default
author: lunarwaterfox
title: CMake Commands (1) - add_custom_command
categories: cmake

---


# 关于 add_custom_command

> [CMake-Command: add_custom_command](https://cmake.org/cmake/help/latest/command/add_custom_command.html)

有些时候，我们需要预处理一些语言之外的东西（比方资源文件，其他语言接口，检查依赖&#x2026;），这些东西并不能通过 add_executable 或是 add_library 来做到，这时就可以使用 add_custom_command 来完成。

```cmake
add_custom_command(OUTPUT output1 [output2 ...]
                   COMMAND command1 [ARGS] [args1...]
                   [DEPENDS [depends...]]
                   [WORKING_DIRECTORY dir]
                   [COMMENT comment])
```

这个命令或生成类似 makefile 中的依赖关系

```cmake
OUTPUT: DEPENDS
        COMMAND
```

也就是说，如果 如果 DEPENDS 的时间戳晚于 OUTPUT，就会使用命令 COMMAND 生成 OUTPUT


# 注意

add_custom_command 是在 OUTPUT 中的内容被使用时，才会被调用

比如

```cmake

add_custom_command(OUTPUT ${PROJECT_BINARY_DIR}/dep.cpp COMMAND touch dep.cpp)
add_executable(test ${PROJECT_BINARY_DIR}/dep.cpp)

```

当 ${PROJECT_BINARY_DIR}/dep.cpp 需要在 add_executable 中使用时，才会去寻找对应的 command，去执行。如果没有被使用到，则不会运行


# Demo

```console
$ tree Hello
Hello/
├── Hello
│   ├── CMakeLists.txt
│   ├── dep
│   │   └── list.csv
│   ├── script
│   │   ├── CMakeLists.txt
│   │   └── resource.cpp
│   └── test.cpp
└── build
```

CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.11)

project(hello)
set (CMAKE_CXX_STANDARD 11)

add_subdirectory(script)

add_custom_command(OUTPUT ${PROJECT_BINARY_DIR}/fruits_cn.cpp ${PROJECT_BINARY_DIR}/fruits_en.cpp
                   COMMAND language ${PROJECT_SOURCE_DIR}/dep/list.csv
                   DEPENDS ./dep/list.csv ./script/resource.cpp
                   COMMENT "按语言生成函数")

set(FRUITS ${PROJECT_BINARY_DIR}/fruits_en.cpp)   #我们跟据不同的语言环境使用不同的头文件，而这个头文件用 add_custom_command 生成

if(LANG STREQUAL "cn")
    set(FRUITS ${PROJECT_BINARY_DIR}/fruits_cn.cpp)
endif()

add_executable(test test.cpp ${FRUITS})

```

test.cpp

```cpp
#include <iostream>
#include <string>

std::string getFruit();  // 这个函数用代码生成

int main(int argc, char *argv[]) {
    std::cout << getFruit() << std::endl;

    return 0;
}

```

dep/list.csv

```csv
苹果,Apple
```

script/CMakeLists.txt

```cmake
add_executable(language resource.cpp)
```

script/resource.cpp

```cpp
#include <fstream>
#include <iostream>
#include <sstream>

using namespace std;

void printHeader(ofstream& of) {
    of << "#include <string>" << endl;

    of << "std::string getFruit() {" << endl;
}

void printFooter(ofstream& of) {

    of << "}" << endl;
}

int main(int argc, char *argv[]) {
    ifstream sourceFile(argv[1]);
    ofstream cnFile("fruits_cn.cpp");
    ofstream enFile("fruits_en.cpp");
    string cnStr;
    string enStr;

    getline(sourceFile, cnStr, ',');
    getline(sourceFile, enStr, ',');


    printHeader(cnFile);
    printHeader(enFile);

    cnFile << "    return \"" << cnStr << "\";" << endl;
    enFile << "    return \"" << enStr << "\";" << endl;

    printFooter(cnFile);
    printFooter(enFile);

    return 0;
}

```

使用不同的语言环境编译

```console
$ cmake -DLANG=en Hello  #使用英文头文件
$ cmake -DLANG=cn Hello  #使用中文头文件
```

这样可以达到跟据语言作预处理的目的