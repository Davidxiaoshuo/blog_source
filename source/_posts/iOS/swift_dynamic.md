---
title: Swift 静态 vs 动态 分派
subtitle: Static vs Dynamic Dispatch in Swift
date: 2021-06-19 19:15:37
categories: iOS
tags: swift
---


### 前言

  在讨论关于 Swift 的静态分派和动态分派之前，需要先弄清基本概念。

1. `值类型` 和 `引用类型`: 值类型，存储在栈中；引用类型存储在堆中。在 Swift 中，`struct`、`enum` 属于值类型，存储在栈上，`class` 属于引用类型，存储在堆上。

2. 值类型，引用类型都支持静态调度，但是动态调度只有引用类型是可以支持的。

3. 调度技术实际上是有 4 种的：
    - Inline 「Fastest」
    - Static Dispatch
    - Virtual Dispatch
    - Dynamic Dispatch 「Slowest」


### 静态 VS 动态

 > 何为 `静态` 和 `动态` 分派：在执行方法时，首先要做的事就是找到方法，而能够在编译期就确定方法的方式就叫做`静态分派`，而无法在编译期确定，只能在运行时确定执行方法的方式就是动态分派。

#### Static Dispatch

- 有时称为直接调度。
- 如果方法是静态分派的，编译器可以在编译时定位指令所在的位置。因此，当调用该函数时，系统会直接跳转到该函数的内存地址执行操作。这种直接行为导致执行速度非常快，并且还允许编译器执行各种优化，例如内联。事实上，由于巨大的性能提升，在编译管道中有一个阶段，在该阶段编译器尝试使函数静态（如果适用）。这种优化称为去虚拟化。

#### Dynamic Dispatch

- 使用动态调度只有在运行时才知道要执行的是哪个方法。
- Swift 提供了两种实现动态的方式：表调度「Table」和消息调度「Message」。

##### Table Dispatch

- 基于继承关系的多态实现「Inheritance-Based Polymorphism」，一个类与一个所谓的虚拟表相关联，该虚拟表「V-Table」包含一个函数指针数组，该数组指向对应于该类的实际实现。请注意，V-Table 是在编译时构建的。因此，与静态调度相比，只有两条额外的指令（读取和跳转）。所以理论上分派应该很快。

***[V-Table 内存结构]***
![V-Table 内存结构](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/swift-v-table.png)

- 没有继承或引用语义的多态 「Protocol-Types」, 管理这个协议类型的方法表是 `Protocol Witness Table` 简称 `PWT`.

***[PWT  内存结构]***
![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/swift-pwt.png)

##### Message Dispatch

- 事实上，是 Objective-C 提供了这种机制（有时，它被称为消息传递），而 Swift 代码只是使用了 Objective-C 运行时库。每次调用 Objective-C 方法时，调用都会传递给 objc_msgSend，后者处理查找。从技术上讲，该过程从给定的类开始，并迭代类层次结构以提取实现。
消息调度是三者中最动态的。作为权衡，虽然查找性能由缓存机制保护，但解析实现的成本可能会有点昂贵。
这种机制是 Cocoa 框架的基石。查看 Swift 的源码，你会发现 KVO 是使用 swizzling 实现的。

- Swift 中实现 Message Dispatch, 需要在方法前添加 `@objc dynamic`。 在 Swift 4.0 之前，`@Objc` 是被隐式添加的，在 4.0 以后，需要我们手动添加。


### Swift 中的方法分派 case

> Note: 带有 `final` 关键字的方法属于静态分派；普通扩展中的「不被 @objc, dynamic 描述的」方法也属于静态分派。

```swift

extension Animal {
	func eat() { }
	@objc dynamic func getWild() { }
}

class Dog: Animal {
	override func eat() { }	// Compiled error!
	@objc dynamic override func getWild() { }	// Ok :)
}


protocol Noisy {
	func makeNoise() -> Int	// Table Dispatch
}
extension Noisy {
	func makeNoise() -> Int { return 0 }	// Table Dispatch
	func isAnnoying() -> Bool { return true }	// Static Dispatch, extension 中普工方法
}

class Animal: Noisy {
	func makeNoise() -> Int { return 1 }	// Table Dispatch
	func isAnnoying() -> Bool { return false } // Table Dispatch
	@objc func sleep() { }	// Table Dispatch
}

extension Animal {
	func eat() { }	// Static Dispatch, extension 中普工方法
	@objc func getWild() { }	// Message Dispatch
}

```

### 参考
- [Method dispatch in Swift](https://trinhngocthuyen.github.io/posts/tech/method-dispatch-in-swift/)
- [making-the-value-witness-table-reference-relative](https://forums.swift.org/t/making-the-value-witness-table-reference-relative/1206)

