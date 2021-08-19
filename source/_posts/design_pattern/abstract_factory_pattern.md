---
title: 抽象工厂模式
date: 2021-08-18 11:02:37
categories: Pattern
tags: [swift, pattern]
---

抽象工厂模式「Abstact Factory Pattern」，是围绕一个超级工厂创建其他工厂，可以说是工厂的工厂。属于创建型模式，它提供了创建对象的最佳方式。

- 意图：提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
- 主要解决：解决接口选择的问题。
- 优点：当一个产品族中的多个对象被设计成一起工作时，它能保证客户端始终使用一个产品族中的对象。
- 缺点：产品族扩展非常困难，想要增加一个系列的产品，既要在抽象的 Creator 里增加代码，也要在具体的产品实现类里增加代码。


## 具体实现

### 定义

```swift
import Cocoa

protocol ICarProducer { }

protocol ICar: ICarProducer {
    func drive()
}

protocol ICarColor: ICarProducer {
    func bodyColor()
}

class BenzCar: ICar {
    
    required init() { }
    
    func drive() {
        print("我开的是大奔...")
    }
    
}

class BMWCar: ICar {
    
    required init() { }
    
    func drive() {
        print("我开的是宝马...")
    }
}

class MaseratiCar: ICar {
    
    required init() { }
    
    func drive() {
        print("我开的是玛莎拉蒂...")
    }
    
}

class White: ICarColor {
    
    required init() { }
    
    func bodyColor() {
        print("车身颜色是白色的.")
    }
    
}

class Black: ICarColor {
    
    required init() { }
    
    func bodyColor() {
        print("车身颜色是黑色的.")
    }
    
}

class Yellow: ICarColor {
    
    required init() { }
    
    func bodyColor() {
        print("车身颜色是黄色的.")
    }
    
}

protocol ICarFactory {
    
    func createCar(type: ICar.Type) -> ICar?
    
    func createCar<Car: ICar>() -> Car?
    
    func getCarBodyColor(type: ICarColor.Type) -> ICarColor?
    
}

extension ICarFactory {
    
    func createCar(type: ICar.Type) -> ICar? { nil }
    
    func createCar<Car: ICar>() -> Car? { nil }
    
    func getCarBodyColor(type: ICarColor.Type) -> ICarColor? { nil }
}


class CarFactory: ICarFactory {
    
    required init() { }

    func createCar(type: ICar.Type) -> ICar? {
        if let benz = type as? BenzCar.Type {
            return benz.init()
        }
        
        if let bmw = type as? BMWCar.Type {
            return bmw.init()
        }
        
        if let maserati = type as? MaseratiCar.Type {
            return maserati.init()
        }
        return nil
    }
    
    func createCar<Car: ICar>() -> Car? {
        if let benz = Car.self as? BenzCar.Type {
            return benz.init() as? Car
        }
        if let bmw = Car.self as? BMWCar.Type {
            return bmw.init() as? Car
        }
        if let maserati = Car.self as? MaseratiCar.Type {
            return maserati.init() as? Car
        }
        return nil
    }
}

class CarBodyColorFactory: ICarFactory {
    
    required init() { }
    
    func getCarBodyColor(type: ICarColor.Type) -> ICarColor? {
        if let white = type as? White.Type {
            return white.init()
        }
        if let black = type as? Black.Type {
            return black.init()
        }
        if let yellow = type as? Yellow.Type {
            return yellow.init()
        }
        return nil
    }
}

class CarFactoryProducer {
    
    class func buildCarFactory<CarAbstractFactory: ICarFactory>() -> CarAbstractFactory? {
        if let carFactory = CarAbstractFactory.self as? CarFactory.Type {
            return carFactory.init() as? CarAbstractFactory
        }
        
        if let colorFactory = CarAbstractFactory.self as? CarBodyColorFactory.Type {
            return colorFactory.init() as? CarAbstractFactory
        }
        return nil
    }
    
}
```

### 调用

```swift

let carFactory: CarFactory? = CarFactoryProducer.buildCarFactory()
let benz: BenzCar? = carFactory?.createCar()
benz?.drive()

let bmw: BMWCar? = carFactory?.createCar()
bmw?.drive()

let maserati: MaseratiCar? = carFactory?.createCar()
maserati?.drive()

let carBodyColorFactory: CarBodyColorFactory? = CarFactoryProducer.buildCarFactory()
let whiteColor = carBodyColorFactory?.getCarBodyColor(type: White.self)
whiteColor?.bodyColor()

let blackColor = carBodyColorFactory?.getCarBodyColor(type: Black.self)
blackColor?.bodyColor()

let yellowColor = carBodyColorFactory?.getCarBodyColor(type: Yellow.self)
yellowColor?.bodyColor()

```