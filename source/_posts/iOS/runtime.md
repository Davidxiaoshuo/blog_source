---
title: iOS Runtime 上手 -
date: 2021-06-19 19:15:37
categories: iOS
tags: swift, runtime
---

### 文章结构

> 1. 什么是 Runtime
> 2. Runtime 中的专有知识解释
> 3. 消息框架「消息机制工作流程」
> 4. 消息转发
> 5. 消息发送 & 转发总结

## 什么是 Runtime

`Objective-C` 是一门动态语言，因此 `OC` 会将更多的操作决策从编译时和链接时推迟到运行时进行。如，在运行时才会检查变量的数据类型，在运行时才会查找要调用的具体函数。这是使得 `OC` 变得异常灵活。`OC` 这一切的基础就是基于 `Runtime` 来提供支持的。Apple 官方提供出一个 `Runtime`库，来对外暴露接口。目前存在两个版本：[Runtime Versions and Platforms](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtVersionsPlatforms.html#//apple_ref/doc/uid/TP40008048-CH106-SW1)

## Runtime 中的专有知识解释

### `ojbc_msgSend`

在 `bjective-C` 中，消息直到运行时才会绑定到方法实现。

```objective-c
[receiver message]；
```

调用消息传递函数。

```c
objc_msgSend(receiver, selector)

objc_msgSend(receiver, selector, arg1, arg2, ...)
```

### `Class` 「类」

在 `objc/runtime.h` 中，`Class「类」`定义为指向 `objc_class` 的结构体的指针。`objc_class` 结构体定义如下：

```C
/// An opaque type that represents an Objective-C class.
typedef struct objc_class *Class;

struct objc_class {
    Class _Nonnull isa;                                          // objc_class 结构体的实例指针

#if !__OBJC2__
    Class _Nullable super_class;                                 // 指向父类的指针
    const char * _Nonnull name;                                  // 类的名字
    long version;                                                // 类的版本信息，默认为 0
    long info;                                                   // 类的信息，供运行期使用的一些位标识
    long instance_size;                                          // 该类的实例变量大小
    struct objc_ivar_list * _Nullable ivars;                     // 该类的实例变量列表
    struct objc_method_list * _Nullable * _Nullable methodLists; // 方法定义的列表
    struct objc_cache * _Nonnull cache;                          // 方法缓存
    struct objc_protocol_list * _Nullable protocols;             // 遵守的协议列表
#endif

};
```
> Note： `objc_class` 结构体存放的数据称为**元数据「meta data」**

> objc_class 结构体 的第一个成员变量是 isa 指针，isa 指针 保存的是所属类的结构体的实例的指针，这里保存的就是 objc_class 结构体的实例指针，而实例换个名字就是 对象。换句话说，Class（类） 的本质其实就是一个对象，我们称之为 类对象。

### `Object` 「对象」

在 `objc/objc.h.h` 中，`Object「对象」`被定义为 `objc_object` 结构体。结构体定义如下：

```C
/// Represents an instance of a class.
struct objc_object {
    Class isa  OBJC_ISA_AVAILABILITY;
};

/// A pointer to an instance of a class.
typedef struct objc_object *id;
```
> 这里的 id 被定义为一个指向 objc_object 结构体 的指针。从中可以看出 objc_object 结构体 只包含一个 Class 类型的 isa 指针。一个 Object「对象」唯一保存的就是它所属 Class「类」 的地址。 当我们对一个对象，进行方法调用时，比如 `[receiver selector];`，它会通过 `objc_object` 结构体的 `isa` 指针 去找对应的 `objc_class` 结构体，然后在 `objc_class` 结构体 的 `methodLists` 中找到我们调用的方法，然后执行。

类和对象结构的这些元素如图:

![](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Art/messaging1.gif)


### Meta Class 「元类」

```
objc_object:isa -> objc_class -> objc_class:isa -> ???`
```

对象「`objc_object` 结构体」的 `isa` 指针指向的是对应的类对象`objc_class`结构体。`objc_class` 结构体的 `isa` 指针实际上指向的的是类对象自身的 `Meta Class`「元类」

Meta Class「元类」就是一个类对象所属的类。一个对象所属的类叫做类对象，而一个类对象所属的类就叫做元类。

### 实例对象、类、元三者的关系

![](https://raw.githubusercontent.com/WiInputMethod/interview/master/img/ios-runtime-class.png)

**isa** ：

- 水平方向上，每一级中的 实例对象 的 isa 指针 指向了对应的 类对象，而 类对象 的 isa 指针 指向了对应的 元类。而所有元类的 isa 指针 最终指向了 NSObject 元类，因此 NSObject 元类 也被称为 根源类。
- 垂直方向上， 元类 的 isa 指针 和 父类元类 的 isa 指针 都指向了 根元类。而 根源类 的 isa 指针 又指向了自己

**superclass**

- 类对象 的  父类指针 指向了 父类的类对象，父类的类对象 又指向了 根类的类对象，根类的类对象 最终指向了 nil。
- 元类 的 父类指针 指向了 父类对象的元类。父类对象的元类 的 父类指针指向了 根类对象的元类，也就是 根元类。而 根元类 的 父亲指针 指向了 根类对象，最终指向了 nil。


### Method「方法」

在 `objc/runtime.h` 中， `objc_method` 结构体定义如下：

```C
/// An opaque type that represents a method in a class definition.
typedef struct objc_method *Method;

struct objc_method {
    SEL method_name             OBJC2_UNAVAILABLE; // 方法名
    char *method_types          OBJC2_UNAVAILABLE; // 方法类型
    IMP method_imp              OBJC2_UNAVAILABLE; // 方法实现
}                               OBJC2_UNAVAILABLE;
```

**SEL & IMP**
```C
typedef struct objc_selector *SEL;

struct objc_selector {
    char *name;                       OBJC2_UNAVAILABLE;
    char *types;                      OBJC2_UNAVAILABLE;
};
```
IMP 可以理解为函数指针，指向了最终的实现。
SEL 与 IMP 的关系非常类似于 HashTable 中 key 与 value 的关系。OC 中不支持函数重载的原因就是因为一个类的方法列表中不能存在两个相同的 SEL 。但是多个方法却可以在不同的类中有一个相同的 SEL，不同类的实例对象执行相同的 SEL 时，会在各自的方法列表中去根据 SEL 去寻找自己对应的IMP。这使得OC可以支持函数重写。

**method_types**

方法类型 method_types 是个字符串，用来存储方法的参数类型和返回值类型。


## 消息转发「消息机制工作流程」

当一个对象 sender 调用代码[receiver message];的时候，实际上是调用了runtime的objc_msgSend函数，所以OC的方法调用并不像C函数一样能按照地址直接取用，而是经过了一系列的过程。这样的机制使得 runtime 可以在接收到消息后对消息进行特殊处理，这才使OC的一些特性譬如：给 nil 发送消息不崩溃，给类动态添加方法和消息转发等成为可能。也正因为每一次调用方法的时候实际上是调用了一些 runtime 的消息处理函数，OC的方法调用相对于C来说会相对较慢，但 OC 也通过引入 cache 机制来很大程度上的克服了这个缺点。

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/runtime_messaging.jpg)

### 动态消息解析

![](https://raw.githubusercontent.com/WiInputMethod/interview/master/img/ios-runtime-method-resolve.png)

1. 通过 resolveInstanceMethod 得知方法是否为动态添加，YES则通过 class_addMethod 动态添加方法，处理消息，否则进入下一步。dynamic 属性就与这个过程有关，当一个属性声明为 dynamic 时 就是告诉编译器：开发者一定会添加 setter/getter 的实现，而编译时不用自动生成。
2. 这步会进入 forwardingTargetForSelector 用于指定哪个对象来响应消息。如果返回nil 则进入第三步。这种方式把消息原封不动地转发给目标对象，有着比较高的效率。如果不能自己的类里面找到替代方法，可以重载这个方法，然后把消息转给其他的对象。
3. 这步调用 methodSignatureForSelector 进行方法签名，这可以将函数的参数类型和返回值封装。如果返回 nil 说明消息无法处理并报错 unrecognized selector sent to instance，如果返回 methodSignature，则进入 forwardInvocation ，在这里可以修改实现方法，修改响应对象等，如果方法调用成功，则结束。如果依然不能正确响应消息，则报错 unrecognized selector sent to instance.

> 可以利用 2、3 中的步骤实现对接受消息对象的转移，可以实现“多重继承”的效果。

### 参考资料
- https://juejin.cn/post/6844903878794706957
- https://hit-alibaba.github.io/interview/iOS/ObjC-Basic/Runtime.html
- https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html#//apple_ref/doc/uid/TP40008048-CH104-SW2

- https://github.com/opensource-apple/objc4/tree/master/runtime

