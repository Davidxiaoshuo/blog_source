---
title: Swift 中元类型理解
date: 2021-08-18 11:02:37
categories: iOS
tags: [swift, pattern]
---


## 什么是元类型 「Metatype」

元类型是指任何类型的的类型，包括类类型、结构体类型、枚举类型和协议类型。</br>类、结构体或枚举类型的元类型，在该类型的名称后面跟 `.Type`；协议类型的元类型，是该协议名称后跟 `.Protocol`。

> 如：</br>
> 类类型 SomeClass 的元类型是 SomeClass.Type;</br>
> 协议 SomeProtocol 的元类型是 SomeProtocol.Protocol

可以使用后缀 `.self` 表达式将类型作为值访问。

> 如：</br>
> SomeClass.self 返回 SomeClass 本身，而不是 SomeClass 的实例。</br>
> SomeProtocol.self 返回的是 SomeProtocol 本身，而不是在运行时符合 SomeProtocol 的类型的实例。

```swift

class BaseClass {
    class func printClassName() {
        print("BaseClass")
    }
}
class SubClass: BaseClass {
    override class func printClassName() {
        print("SubClass")
    }
}


let someInstance: BaseClass = SubClass()
/*                      |            |
                    compileTime    Runtime
                        |            | 
To extract, use:       .self       type(of)

  Check the runtime type of someInstance use `type(of:)`: */

print(type(of: someInstance) == SubClass.self) // True
print(type(of: someInstance) == BaseClass.self) // False

 /* Check the compile time type of someInstance use `is`: */

print(someInstance is SubClass) // True
print(someInstance is BaseClass) // True
type(of: someInstance).printClassName() // Prints "SomeSubClass"

```

`type(of:)` 解析

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/swift_synaxt_ypeof.png)

返回的是 `Metatype` 的实例，也就是一个具体的类型，用这个返回的类型，我们可以调用它的构造器或者静态属性或者方法。它返回的是一个值的动态类型。


使用初始化表达式从该类型的元类型值构造一个类型的实例。对于类类型，被调用的初始化构造器必须使用 `required` 关键字或整个类用 final 关键字标记。

```swift

class AnotherSubClass: SomeBaseClass {
    let string: String
    required init(string: String) {
        self.string = string
    }
    override class func printClassName() {
        print("AnotherSubClass")
    }
}
let metatype: AnotherSubClass.Type = AnotherSubClass.self
let anotherInstance = metatype.init(string: "some string")

```

## 类型 VS 元类型

两者的区别在于：元类型的值为类型，用类型名加上 `.self` 表示。而类型的值为该类型的实例对象。
如： `Int.Type` 的值为 `Int.self`, `Int` 的值可以为任意整数。

```swift

let intMetatype: Int.Type = Int.self
let intType: Int = 10

```

***Note: `SomeType.self` & `SomeProtocol.self` 表示元类型的值，而 `someInstance.self` 表示该实例对象本身***


## `AnyClass`

`AnyClass` 其实就是任意类类型的元类型。`AnyClass` 是 `AnyObject.Type` 的别名，而 `AnyObject` 是任何 `class` 都会默认实现的协议。

```swift
public typealias AnyClass = AnyObject.Type
```

## 元类型的应用场景

1. 我们经常用到的 `UITableView` & `UICollectionView` 中 cell 的注册方法

```swift

func register(AnyClass?, forCellReuseIdentifier: String)

tableView.register(UITableViewCell.self, forCellReuseIdentifier: "cell")

```

2. 我们平常在使用`工厂模式`时的对产品类型实例的创建方法。[例如](https://davidxiaoshuo.github.io/design_pattern/factory_pattern/)