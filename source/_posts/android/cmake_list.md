---
title: android studio cmake 编译常规设置
tags: android
---

# android studio cmake 编译常规设置

### add_executable 指令

语法：`add_executable(executable_file_name [source])`将一组源文件 source 生成一个可执行文件。 source  可以是多个源文件，也可以是对应定义的变量  如：`add_executable(hello main.c)`

### cmake_minimun_required(VERSION 3.4.1)

用来指定 CMake 最低版本为3.4.1，如果没指定，执行 cmake 命令时可能会出错

### add_subdirectory 指令

语法：`add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])`

这个指令用于向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存放的位置。`EXCLUDE_FROM_ALL`参数含义是将这个目录从编译过程中排除。

另外，也可以通过 SET 指令重新定义` EXECUTABLE_OUTPUT_PATH` 和 ` LIBRARY_OUTPUT_PATH`  变量来指定最终的目标二进制的位置 (指最终生成的 hello 或者最终的共享库，不包含编译生成的中间文件)

```
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
```

### add\_library 指令

语法：`add_library(libname [SHARED | STATIC | MODULE] [EXCLUDE_FROM_ALL] [source])`

将一组源文件 source 编译出一个库文件，并保存为 libname.so \(lib 前缀是生成文件时 CMake自动添加上去的\)。其中有三种库文件类型，**不写的话，默认为 STATIC**:

* SHARED: 表示动态库，可以在\(Java\)代码中使用`System.loadLibrary(name)`动态调用；
* STATIC: 表示静态库，集成到代码中会在编译时调用；
* MODULE: 只有在使用 dyId 的系统有效，如果不支持 dyId，则被当作 SHARED 对待；
* EXCLUDE\_FROM\_ALL:  表示这个库不被默认构建，除非其他组件依赖或手工构建

```
#将compress.c 编译成 libcompress.so 的共享库
add_library(compress SHARED compress.c)
```

- add\_library 命令也可以用来导入第三方的库:`add_library(libname [SHARED | STATIC | MODULE | UNKNOWN] IMPORTED)` 如，导入 libjpeg.so

```cmake
add_library(libjpeg SHARED IMPORTED)
```

导入库后，当需要使用 target\_link\_libraries 链接库时，可以直接使用该库

### find_library 指令

语法：`find_library(name1 path1 path2 …)`VAR 变量表示找到的库全路径，包含库文件名 。例如：

```
find_library(libX  X11 /usr/lib)

find_library(log-lib log)
#路径为空，应该是查找系统环境变量路径
```

### set_target_properties 指令

语法:` set_target_properties(target1 target2 … PROPERTIES prop1 value1 prop2 value2 …)`这条指令可以用来设置输出的名称（设置构建同名的动态库和静态库，或者指定要导入的库文件的路径），对于动态库，还可以用来指定动态库版本和 API 版本。如: `set_target_properties(hello_static PROPERTIES OUTPUT_NAME “hello”)` 设置同名的 hello 动态库和静态库：

```
set_target_properties(hello PROPERTIES CLEAN_DIRECT_OUTPUT 1)

set_target_properties(hello_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)
```

指定要导入的库文件的路径

```
add_library(jpeg SHARED IMPORTED)
#注意要先 add_library，再 set_target_properties

set_target_properties(jpeg PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/libs/${ANDROID_ABI}/libjpeg.so)
```

设置动态库 hello 版本和 API 版本：`set_target_properties(hello PROPERTIES VERSION 1.2 SOVERSION 1)`和它对应的指令：`get\_target_property(VAR target property)`。如上面的例子，获取输出的库的名字

```
get_target_property(OUTPUT_VALUE hello_static OUTPUT_NAME)

message(STATUS "this is the hello_static OUTPUT_NAME:"${OUTPUT_VALUE})
```

### include_directories 指令

语法：`include_directories([AFTER | BEFORE] [SYSTEM] dir1 dir2…)`

这个指令可以用来向工程添加多个特定的头文件搜索路径，路径之间用空格分割，如果路径中包含了空格，可以使用双引号将它括起来，默认的行为是追加到当前的头文件搜索路径的后面。

### target_link_libraries 指令

语法：`target_link_libraries(target library library2…)`

这个指令可以用来为 target 添加需要的链接的共享库，同样也可以用于为自己编写的共享库添加共享库链接。如：

```
#指定 compress 工程需要用到 libjpeg 库和 log 库
target_link_libraries(compress libjpeg ${log-lib})
```

同样，link_directories(directory1 directory2 …) 可以添加非标准的共享库搜索路径。还有其他 file、list、install 、find_ 指令和控制指令等就不介绍了，详细可以查看手册。

###  CMake 的常用变量

- 变量引用方式

使用 ${} 进行变量的引用。不过在 IF 等语句中，可以直接使用变量名而不用通过 ${} 取值

- 自定义变量的方式

  主要有隐式定义和显式定义两种。隐式定义，如 PROJECT 指令会隐式定义`_BINARY_DIR` 和`_SOURCE_DIR`而对于显式定义就是通过 SET 指令来定义。如：set(HELLO_SRC main.c)

- CMake 常用变量

	- `CMAKE_BINARY_DIR, PROJECT_BINARY_DIR,  _BINARY_DIR`这三个变量指代的内容都是一样的，如果是 in-source 编译，指的是工程顶层目录，如果是 out-of-source 编译，指的是工程编译发生的目录。
	- `CMAKE_SOURCE_DIR, PROJECT_SOURCE_DIR,   _SOURCE_DIR`这三个变量指代的内容也是一样的，不论哪种编译方式，都是工程顶层目录。
	- `CMAKE_CURRENT_SOURCE_DIR`当前处理的 CMakeLists.txt 所在的路径
	- `CMAKE_CURRENT_BINARY_DIR`如果是 in-source 编译，它跟            CMAKE_CURRENT_SOURCE_DIR 一致，如果是 `out-of-source` 编译，指的是 target 编译目录。使用 ADD_SUBDIRECTORY(src bin)可以修改这个变量的值；而使用 `SET(EXECUTABLE_OUTPUT_PATH &lt; 新路径&gt;)` 并不会对这个变量造成影响，它仅仅修改了最终目标文件存放的路径。
	- `CMAKE_CURRENT_LIST_FILE` 输出调用这个变量的 CMakeLists.txt 的完整路径
	- `CMAKE_CURRENT_LIST_LINE` 输出这个变量所在的行
    - `CMAKE_MODULE_PATH` 这个变量用来定义自己的 CMake 模块所在的路径。如果你的工程比较复杂，有可能会自己编写一些 cmake 模块，这些 cmake 模块是随你的工程发布的，为了让 cmake 在处理 CMakeLists.txt 时找到这些模块，你需要通过 SET 指令，将自己的 cmake 模块路径设置一下。比如：` SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)` 这时候你就可以通过 INCLUDE 指令来调用自己的模块了。
	- `EXECUTABLE_OUTPUT_PATH` 和 `LIBRARY_OUTPUT_PATH`分别用来重新定义最终结果的存放目录，前面我们已经提到了这两个变量。
	- `PROJECT_NAME`返回通过 PROJECT 指令定义的项目名称。

### Android CMake 的使用

#### CMakeList.txt 的编写

再回归到 Android NDK 开发中 CMake 的使用，先看一个系统生成的 NDK 项目的 CMakeLists.txt 的配置：(去掉原有的注释)

```
# 设置编译 native library 需要最小的 cmake 版本
cmake_minimum_required(VERSION 3.4.1)
# 将指定的源文件编译为名为 libnative-lib.so 的动态库
add_library
(native-lib SHARED src/main/cpp/native-lib.cpp)
# 查找本地 log 库
find_library
(log-lib log)
# 将预构建的库添加到自己的原生库
target_link_libraries
(native-lib ${log-lib})
```

复杂一点的 CMakeLists，这是一个本地使用 libjpeg.so 来做图片压缩的项目

```
cmake_minimum_required(VERSION 3.4.1)
#设置生成的so动态库最后输出的路径
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI})
#指定要引用的libjpeg.so的头文件目录
set(LIBJPEG_INCLUDE_DIR src/main/cpp/include)
include_directories(${LIBJPEG_INCLUDE_DIR})

#导入libjpeg动态库 SHARED；静态库为STATIC
add_library(jpeg SHARED IMPORTED)

#对应so目录，注意要先 add_library，再 set_target_properties）
set_target_properties(jpeg PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/libs/${ANDROID_ABI}/libjpeg.so)

add_library(compress SHARED src/main/cpp/compress.c)

find_library(graphics jnigraphics)
find_library(log-lib log)

#添加链接上面个所 find 和 add 的 library
target_link_libraries(compress jpeg ${log-lib} ${graphics})

```

#### 配置 Gradle

简单的配置如下，至于 cppFlags 或 cFlags 的参数有点复杂，一般设置为空或不设置也是可以的，这里就不过多介绍了

```
android {
compileSdkVersion 25
buildToolsVersion "25.0.3"

defaultConfig {
    minSdkVersion 15
    targetSdkVersion 25
    versionCode 1
    versionName "1.0"
    externalNativeBuild {
        cmake {
            // Passes optional arguments to CMake.
            arguments 
            "-DANDROID_ARM_NEON=TRUE",
            "-DANDROID_TOOLCHAIN=clang"
            // Sets optional flags for the C compiler.
            cFlags "-D_EXAMPLE_C_FLAG1","-D_EXAMPLE_C_FLAG2"
            // Sets a flag to enable format macro constants for the C++ compiler.
            cppFlags "-D__STDC_FORMAT_MACROS"
            //生成.so库的目标平台
            abiFilters 'x86','x86_64','armeabi','armeabi-v7a','arm64-v8a'
        }
    }
}
//配置 CMakeLists.txt 路径
externalNativeBuild {
     cmake {
        path "CMakeLists.txt"
    }
}

```



