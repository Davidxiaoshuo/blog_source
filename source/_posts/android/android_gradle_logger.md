---
title: Android Studio Gradle's Logger
tags: android
---

**前言:** 我们在通过 Android Studio 进行 android 开发的时候，难免需要在进行 Gradle 编译构建的时候需要输出一些信息来辅助我们进行配置调试，或是需要输出一些必要信息来提醒其他开发者，此时，我们就要了解下 gradle 的 log 打印相关辅助工具类了。

> **补充：Gradle -> 构建系统工具，它的 DSL 基于 Groovy 实现**



### 日志级别

|   级别    |   用途   |
| :-------: | :------: |
|   ERROR   | 错误信息 |
|   QUIET   | 重要信息 |
|  WARNING  | 警告信息 |
| LIFECYCLE | 进度信息 |
|   INFO    | 内容信息 |
|   DEBUG   | 调试信息 |



```java
package org.gradle.api.logging;

/**
 * The log levels supported by Gradle.
 */
public enum LogLevel {
    DEBUG,
    INFO,
    LIFECYCLE,
    WARN,
    QUIET,
    ERROR
}
```



### 日志开关选项

|   开关选项    |      输出的日志级别       |
| :-----------: | :-----------------------: |
|    无选项     |    LIFECYCLE及更高级别    |
| -q 或 --quiet |      QUIRT及更高级别      |
| -i 或 --info  |      INFO及更高级别       |
| -d 或 --debug | DEBUG及更高级别(全部日志) |

```console
// 输出 INFO及更高级别的日志
gradle -i task
```

***提醒： Android Studio 默认情况开启的是` LIFECYCLE及更高级别`, 所以此时在通过 Android Studio 进行默认 Build 操作时，此时在 Build Output 控制台打印出来的是这个级别的，Info，Debug Log 并不会输出***



### 在 gradle 中尝试打印Log

```groovy
logger.error('this is log of gradle, level ---> error')
logger.quiet('this is log of gradle, level ---> quiet')
logger.warn('this is log of gradle, level ---> warn')
logger.lifecycle('this is log of gradle, level ---> lifecycle')
logger.info('this is log of gradle, level ---> info')
logger.debug('this is log of gradle, level ---> debug')
```

在验证时，通过 `-d` 得到以下输出，验证 Log 级别

```verilog
10:01:42.029 [ERROR] [org.gradle.api.Project] this is log of gradle, level ---> error
10:01:42.029 [QUIET] [org.gradle.api.Project] this is log of gradle, level ---> quiet
10:01:42.029 [WARN] [org.gradle.api.Project] this is log of gradle, level ---> warn
10:01:42.029 [LIFECYCLE] [org.gradle.api.Project] this is log of gradle, level ---> lifecycle
10:01:42.029 [INFO] [org.gradle.api.Project] this is log of gradle, level ---> info
10:01:42.029 [DEBUG] [org.gradle.api.Project] this is log of gradle, level ---> debug
```



> **`printf` 在 gradle 中也是可以再 Build Output 控制台中输出 Log 的**

```verilog
10:07:24.545 [QUIET] [system.out] this is log of gradle, event ---> printf
```

