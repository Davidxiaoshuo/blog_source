---
title: C++ 中 基础类型转换成 char *
tags: c++
---

# C++ 中 基础类型转换成 char*

有时候我们在项目中难免会用到，将基础数据类型转换成char*，这样会方面一些业务上的开展。前一阵子做基于mips 平台下 阿里云Log Service时候就用到了，由于考虑要封装sdk的轻量性，所以没有考虑依赖其他的系统库。

### 以下demo 代码依赖如下系统库，

```c++
#include <stdio.h>
#include <string>
```

### 基础数据类型的相互转换其实有多种方式。以下多是通过 `printf` 方式来完成的。

### int 转 char *

```c++
char* int_to_char_ptr(int src) {
    char result[30] = "";
    sprintf(result, "%d", src);
    char* ret_value = result;
    return ret_value;
}
```

### double 转 char*

```c++
char* double_to_char_ptr(double src) {
    char result[30] = "";
    // 如果需要保留小数x位数, eg: %.xlf
    sprintf(result, "%lf", src);
    char* ret_value = result;
    return ret_value;
}
```

### float 转 char*

```c++
char* float_to_char_ptr(float src) {
    char result[30] = "";
    // 如果需要保留小数x位数, eg: %.xf
    sprintf(result, "%f", src);
    char* ret_value = result;
    return ret_value;
}
```

### size_t 转 char*

```c++
char* size_t_to_char_ptr(size_t value) {
    char result[30] = "";
    sprintf(result, "%zu", value);
    char * ret_value = result;
    return ret_value;
}
```

### string 转 char*

```c++
char* str_to_char_array(std::string value) {
    return const_cast<char*>(value.c_str());
}
```

### char* 转 string

```c++
std::string char_ptr_to_str(const char* value) {
    std::string res;
    res = value;
    return res;
}
```


