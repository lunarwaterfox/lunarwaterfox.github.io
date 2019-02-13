---

layout: default
author: lunarwaterfox
title: CMake Modules (2) - GoogleTest
categories: cmake

---

# 关于 GoogleTest 
> [CMake-Modules: GoogleTest](https://cmake.org/cmake/help/latest/module/GoogleTest.html)

GoogleTest 可以将 gtest 的测试结果，集成到 ctest 中，cmake 最低支持版本 3.10

```cmake
cmake_minimum_required(VERSION 3.10)
include(GoogleTest)
```

# Demo

```console
$ tree Hello
Hello
├── Hello
│   ├── CMakeLists.txt
│   └── test.cpp
└── build
```

CMakeLists.txt 写入

```cmake
# 最低版本设置成 3.11 是为了使用 FetchContent，单独使用 GoogleTest 时可设置为 3.10
cmake_minimum_required(VERSION 3.11)

# 打开 ctest 开关
enable_testing()

# 添加 FetchContent 依赖
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

# test 项目
project(Hello)
add_executable(test_hello test.cpp)
target_link_libraries(test_hello gtest_main)

# 集成 gtest
include(GoogleTest)
gtest_discover_tests(test_hello)

```

test.cpp 写一个简单的 test 

```cpp
#include <gtest/gtest.h>

TEST(Hello, passed) {
    ASSERT_TRUE(true);
}

TEST(Hello, failed) {
    ASSERT_TRUE(false);
}

```

编译

```console
$ cd build
$ cmake ../Hello
$ cmake --build .
```

测试

```console
$ ctest
Test project /Users/cmake/Desktop/hello/build
    Start 1: Hello.passed
1/2 Test #1: Hello.passed .....................   Passed    0.00 sec
    Start 2: Hello.failed
2/2 Test #2: Hello.failed .....................***Failed    0.00 sec

50% tests passed, 1 tests failed out of 2

Total Test time (real) =   0.01 sec

The following tests FAILED:
          2 - Hello.failed (Failed)
Errors while running CTest
```
<!-- 

```cmake
gtest_discover_tests(target
                     [EXTRA_ARGS arg1...]
                     [WORKING_DIRECTORY dir]
                     [TEST_PREFIX prefix]
                     [TEST_SUFFIX suffix]
                     [NO_PRETTY_TYPES] [NO_PRETTY_VALUES]
                     [PROPERTIES name1 value1...]
                     [TEST_LIST var]
                     [DISCOVERY_TIMEOUT seconds]
)
```
-->
