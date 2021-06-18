---
title: Swift 总结之属性重写 「override」
tags: iOS
---

```swift
import Cocoa

class Student {
    
    // 存储属性
    var age: Int = 0
    var chineseScore: Double = 0.0
    var englishScore: Double = 0.0
    
    // 计算属性
    var averageScore: Double {
        
        // 访问（获取）调用get
        get{
            return (chineseScore + englishScore) / 2
        }
        
        // 设置值的时候
        set{
            print("set\(newValue)")
            // 千万不要在这里设置值 会死循环  外部参数起名字 newvalue
            // self.averageScore = newValue
        }
        
    }
    // 只读计算属性
    var averageScore2: Double {
        return (chineseScore + englishScore) / 2
    }
    
    // 类属性, 不能被重写
    static var couseCount  = 3
    
    // 懒加载属性
    lazy var courses: [String] = { ()->[String] in
        print("懒加载属性")
        return ["java", "html", "swift"]
    }()
    
    // 属性观察器-有的属性很重要，我希望关注每一次赋值的变化。
    // 验证 willSet 和 didSet 作用
    // 内置变量：newValue oldValue
    var name: String = "tt" {
        
        // newValue
        willSet{
            print("父类 willSet 被调用, newValue\(newValue)")
        }
        
        // oldValue
        didSet{
            print("父类 didSet 被调用, oldValue\(oldValue)")
        }
    }
    
}

// 属性的继承与重写
class SeniorStudent : Student{
    
    private var _chineseScore: Double = 0.0
    
    // 子类都可以通过提供getter和setter对属性进行重写
    // 重写后，存储属性变为计算属性
    override var chineseScore: Double{
        
        get {
            return _chineseScore
        }
        
        set { _chineseScore = newValue }
    }
    
    // 不可以将继承来的读写属性重写为只读属性
    override var averageScore: Double{
        
        get {
            return 90.5
        }
        
        set{ }
    }
    
    // 如果父类已经添加了属性观察器，当属性发生变化时，父类与子类都会得到通知
    override var name:String {
        
        // newValue
        willSet{
            print("子类 willSet 被调用, newValue\(newValue)")
        }
        
        // oldValue
        didSet{
            print("子类 didSet 被调用,oldValue\(oldValue)")
        }
    }
    
    /// 懒加载属性，重写后变为计算属性
    override var courses: [String] {
        get {
            return ["swift", "OC"]
        }
        
        set { }
    }
    
    override var averageScore2: Double {
        return 96.1
    }

}


let student = SeniorStudent()
student.chineseScore = 91

print(student.chineseScore)


```

