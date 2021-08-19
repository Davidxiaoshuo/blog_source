---
title: 工厂模式
date: 2021-08-18 11:02:37
categories: Pattern
tags: [swift, pattern]
---

工厂模式「Factory Pattern」，属于创建型模式，它提供了一种创建对象的最佳方式。创建对象时，不会对使用者「客户端」暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的的对象。

- 意图： 定义一个创建对象接口，让其子类自己决定实例化哪个工厂类，使其创建过程延迟到子类。
- 主要解决：解决接口选择的问题。
- 优点：
    1. 调用者创建一个对象，只要知道其名称就可以了。
    2. 扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以了。
    3. 屏蔽产品的具体实现，调用者只关心产品的接口。
- 缺点：
    1. 每次增加一个产品，都需要增加一个具体的类和对工厂接口的实现。使得系统中类的个数成倍增加，增加了系统的复杂度和具体类的依赖性。


## 具体实现

### 定义

```swift

import Cocoa

protocol Shape {
    
    func draw()
}

class Rectangle: Shape {
    
    required init() { }
    
    func draw() {
        print("I'm a rectangle")
    }
}

class Circle: Shape {
    
    required init() { }
    
    func draw() {
        print("I'm a circle")
    }
}

class ShapeFactory: Shape {
    
    private var shape: Shape?
    
    class func create(type: Shape.Type) -> Shape? {
        if let rectangle = type as? Rectangle.Type {
            return rectangle.init()
        }
        if let circle = type as? Circle.Type {
            return circle.init()
        }
        return nil
    }
    
    class func create<S: Shape>() -> S? {
        if let rectangle = S.self as? Rectangle.Type {
            return rectangle.init() as? S
        }
        if let circle = S.self as? Circle.Type {
            return circle.init() as? S
        }
        return nil
    }
    
    func draw() {
        shape?.draw()
    }
}

```

### 调用

```swift

let rectangle: Rectangle? = ShapeFactory.create()
rectangle?.draw()

let circle: Circle? = ShapeFactory.create()
circle?.draw()

let circle2 = ShapeFactory.create(type: Circle.self)
circle2?.draw()

```