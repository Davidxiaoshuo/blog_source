# Android JNI 层 Log 输出

### 概述：

android 中在进行一些 C++ 底层库开发的时候难免需要一些 log 来辅助我们的开发调试，或是打印一些重要信息给到 Lib 库的使用者。在 android 中，通过 C++ 层的 `print`  Or `std::cout` 是无法在logcat 中正常显示 log 信息的。android NDK 专门提供了相关的 [Logging](https://developer.android.com/ndk/reference/group/logging) 工具。



### __android_log_print 主要使用的函数

```c++
int __android_log_print(
  int prio,	//优先级
  const char *tag,	//标签
  const char *fmt,  // format log string
  ...
)
```



### cmake 编译， 在 `CMakeLists.txt` 中配置需要依赖的 Log 系统库

```
#查找要依赖的系统库
find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

#指定要生成的动态库所依赖的库（系统库，外部第三方库）
target_link_libraries( # Specifies the target library.
                       # Links the target library to the log library
                       # included in the NDK.
                       
                       ${log-lib} )
```



### 通过对 __android_log_print 稍加修改，来使 log 输出变得更加简单

> **首先要引入所需的头文件** `#include <android/log.h>`

```c++
#include <android/log.h>

#define TAG "ProjectName" // 这个是自定义的LOG的标识   

#define LOGD(...) __android_log_print(ANDROID_LOG_DEBUG,TAG ,__VA_ARGS__) // 定义LOGD类型，在 Release 模式下禁用   

#define LOGI(...) __android_log_print(ANDROID_LOG_INFO,TAG ,__VA_ARGS__) // 定义LOGI类型   

#define LOGW(...) __android_log_print(ANDROID_LOG_WARN,TAG ,__VA_ARGS__) // 定义LOGW类型   

#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR,TAG ,__VA_ARGS__) // 定义LOGE类型   

#define LOGF(...) __android_log_print(ANDROID_LOG_FATAL,TAG ,__VA_ARGS__) // 定义LOGF类型 
```



### 使用

```c++
char* log_example_str = "hello logger";
LOGD("This is a log, the content is %s", log_example_str);
LOGI("This is a log, the content is %s", log_example_str);
LOGW("This is a log, the content is %s", log_example_str);
LOGE("This is a log, the content is %s", log_example_str);
LOGF("This is a log, the content is %s", log_example_str);
```

