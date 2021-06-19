---
title: Swift 总结之关键字
categories: iOS
tags: swift
---

### 声明式关键字

##### Keyword： `class`

Description： Swift 语言中一种构造体，它具有以下特性：

- 一个类允许另一个类进行继承，形成父子关系。
- 支持类型转换「type-casting」，允许在运行时，检查 & 指定一个类的实际类型。
- 支持实现协议「protocol」。
- 支持 `deinit` 析构函数，并且在可以释放所有资源。
- 支持引用计数，允许多个引用指向同一个实例。
- 不支持默认成员初始化构造器「memberwise initializer」。
- 属于引用类型，存储在堆内存中



##### Keyword： `struct`

Description：与 class 一样，也是 Swift 语言中一个重要的构造体，它具有以下特性：

- 支持实现协议
- 支持默认成员初始化构造器「memberwise initializer」。
- 不支持类型转换「type-casting」
- 不支持 `deinit`析构函数
- 内部方法修改属性时，需要在方法上添加 `mutating` 关键字
- 属于值类型，存储在栈内存中



##### Keyword：`enum`

Description：Swift 中的一种数据类型，它是一组有共同特性的数据的集合。它具有以下特性：

- 支持实现协议
- 支持初始化方法「构造方法」
- 内部方法修改属性时，需要在方法上添加 `mutating` 关键字

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```



##### Keyword：`protocol`

Description：协议定义了适合特定任务或功能的方法、属性。然后，类、结构或枚举可以采用该协议，以提供这些要求的实际实现。任何满足协议要求的类型都被称为符合该协议。除了指定符合类型必须实现的要求之外，您还可以扩展协议以实现其中一些要求或实现符合类型可以利用的附加功能。[详见](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html)

```swift
protocol SomeProtocol {
    // protocol definition goes here
}
```



##### Keyword： `extension`

Description：向现有类、结构、枚举或协议类型添加新功能。这包括扩展您无法访问原始源代码的类型的能力（称为追溯建模）。扩展类似于 Objective-C 中的类别。 （与 Objective-C 类别不同，Swift 扩展没有名称。）[详见](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html)

在 swift 中 extension 有以下具体能力：

- 添加计算实例属性或计算类型属性
- 定义实例方法和类型方法
- 提供新的构造函数
- 定义下标
- 定义和使用新的嵌套类型
- 实现现有需要实现的协议



##### Keyword：`init`

Description: class, struct, enum 的构造方法。

``` swift
enum Gender: RawRepresentable {
    typealias RawValue = String
    
    case male
    case female

    var rawValue: String {
        switch self {
        case .male: return "male"
        case .female: return "female"
        }
    }
    
    init?(rawValue: String) {
        switch rawValue {
        case "male": self = .male
        case "female": self = .female
        default: return nil
        }
    }
}

struct ClassInfo {
    var grade: Int
    var classNum: Int
}

class Student {
    var name: String
    var gender: Gender
    var classInfo: ClassInfo
    
    init(name: String, gender: Gender, classInfo: ClassInfo) {
        self.name = name
        self.gender = gender
        self.classInfo = classInfo
    }
}
```



##### Keyword： `deinit`

Description：class 的析构函数，每个类只允许有一个析构函数。析构函数在类实例释放前自动调用，不允许手动调用。超类的析构器由子类继承，在子类析构函数调用结束时，自动调用父类析构函数。 父类的析构器总会被调用，即使子类没有实现析构函数。实际开发中，我们可以通过 deinit 来观察类实例是否被释放。

```swift
deinit {
  // perform the deinitialization
}
```

##### Keyword：`convenience`

Description: convenience init 是类中次要的，辅助型的构造器，可以调用便利构造来调用同一个类中的指定的构造器，并且为其提供默认值，

```swift
class Person: NSObject {
    var age: Int
    var name: String
  
    init(age:Int, name: String) {
        self.age = age;
        self.name = name;
    }
    
    convenience init(age: Int, firstName: String, lastName: String) {
        self.init(age:age, name:firstName+lastName);
    }
    
    convenience init(age: Int, firstName: String, lastName: String, height: CGFloat) {
        self.init(age: age, firstName: firstName, lastName: lastName);
    }
}


class Father: Person {
    var address: String
    init(age: Int, name: String, address: String) {
        self.address = address;
        super.init(age: age, name: name);
    }
    
    convenience init(age: Int, firstName: String, lastName: String, address: String) {
        self.init(age: age,name: firstName + lastName, address: address);
    }
}
```



##### Keyword：`required`

Description: 在类的构造器前添加 required 修饰符表明所有该类的子类都必须实现该构造器

```swift
class SomeClass {
    var str: String
    required init(str: String) {
        self.str = str
    }
}

class SomeSubclass: SomeClass {
    required init(str: String) {
        super.init(str: str)
    }
    init(i: Int) {
        super.init(str: String(i))
    }
}

var SomeSubclass(str:"Hello Swift")

```



##### Keyword：`import`

Description: 包导入。 [详见](https://swift.gg/2019/09/23/swift-import/)

```swift
import <#module#>
import <#kind#> <#module.symbol#>
import <#module.submodule#>
```



##### Keyword： `typealias`

Description：`typealias` 是特定类型的别名。换句话说，类型别名是在你的代码库里插入现有类型的另一个名称。[详见](https://swift.gg/2020/04/11/2019-05-15-the-usefulness-of-typealiases/)

```swift
typealias Money = Int
```



##### Keyword: `associatedtype`

Description: 关联类型定义。在协议中，定义一个类型的占位符名称。直到协议被实现，该占位符才会被指定具体的类型。[详见](https://docs.swift.org/swift-book/LanguageGuide/Generics.html#ID189)



##### Keyword：`let`

Description：定义一个常量。

```swift
let constantString = "hello world!"
```



##### Keyword：`var`

Description: 定义一个变量。

```swift
var variableString = "hello my world!"
variableString = "hello your world!"
```



##### Keyword： `func`

Description：方法定义。[详见](https://docs.swift.org/swift-book/LanguageGuide/Functions.html#ID159)

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```



##### Keyword：`inout`

Description: `inout`是按值传递，然后再写回原变量，而不是按引用传递。

```swift
func inc(inout i: Int) {
    ++i
}

var x = 0
inc(&x)
print(x) // 输出结果：“1”

func inc(inout i: Int) -> () -> Int {
    return { ++i }  // 闭包中截获inout参数i
}

var x = 0
let f = inc(&x)
print(f()) // 输出结果：“1”
print(x) // 输出结果：“0”。
```

##### Keyword：`override`

Description: 属性、方法重写需要用 `override`修饰。 属性重写[参考](../swift_override_property/)


Keyword: `final`

Description：可以在class、func和var前修饰。

```swift
class Parent {
    final func method1() {
        //权限验证（必须执行）
        //.....
         
        method2()
         
        //下面是日志记录（必须执行）
        //..........
    }
     
    func method2(){
        //父类的实现
        //......
    }
}
class Child : Parent {
    //只能重写父类的method2方法，不能重写method1方法
    override func method2() {
        //子类的实现
        //......
    }
}
```



##### Keyword: `mutating`

Description: [见](#Keyword： )



##### Keyword：`nonmutating`

```swift
protocol Settings {
    subscript(key: String) -> AnyObject? { get nonmutating set }
}

struct Test2 {
    
    var b: Int {
        get {
            return  2
        }
        nonmutating set {
            print("\(newValue)")
        }
    }
}

/// 由于这里 t 是常量，如果 setter 中不是使用 nonmutating，编译器会报错
/// Cannot assign to property: 't' is a 'let' constant
let t = Test2()
t.b = 3
print(t.b)
```



##### Keyword： `dynamic`

Description: 指明编译器不会对类成员或者函数的方法进行内联或虚拟化。这意味着对这个成员的访问是使用 Objective-C 运行时进行动态派发的（代替静态调用）。 [参考1](https://juejin.cn/post/6844903665745002503)，[参考2](https://tech.meituan.com/2018/11/01/swift-compile-performance-optimization.html)

```swift
class Person  
{  
    //隐式指明含有 "objc" 属性
    //这对依赖于 Objc-C 黑魔法的库或者框架非常有用
    //比如 KVO、KVC、Swizzling
    dynamic var name:String?  
}
```



### 属性声明相关的关键字

##### Keyword： `lazy`，`set`， `get`,  `willSet`,  `didSet`

```swift
// 懒加载属性
lazy var courses: [String] = { ()->[String] in
    print("懒加载属性")
    return ["java", "html", "swift"]
}()

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
```



### 权限控制关键字

##### Keyword：`open`

Description：公开权限，最高权限级别。可以被其他 Module 访问、继承、复写。只能用于类和类的成员

***Note：由于其具有复写的特性，而 `let「常量」`属性隐式为 `final`, 不可复写，所以 open 不能应用到 let 属性上。***

``` swift
open class Person {
    
    open var name: String?
    
    open func talking() -> String {
        return ""
    }
}
```



##### Keyword: `public`

Description：公有访问权限, 类或者类的公有属性或者公有方法可以从文件或者模块的任何地方进行访问. 一个`App`就是一个模块, 一个第三方`API`、第三方框架等都是一个完整的模块, 这些模块如果要对外留有访问的属性或者方法, 就应该使用`public`的访问权限. `public`的权限在`Swift 3.0`后无法在其它模块被复写方法/属性或被继承.

```swift
public class Person {
    
    public var name: String?
  	public let uid: String = UUID().uuidString
    
    public func talking() -> String {
        return ""
    }
}
```



##### Keyword: `internal`

Description: 在模块内部可以访问, 超出模块内部就不可被访问了. 在`Swift`中属隐式权限，如果不特别设置权限级别，默认即为 `internal`



##### Keyword：`fileprivate`

Description：文件私有访问权限, 被`fileprivate`修饰的类或者类的属性或方法可以在同一个物理文件中访问. 如果超出该物理文件, 那么有这`fileprivate`访问权限的类、属性和方法就不能被访问了.

```swift
class ClassModel {
     
  init() {
    let student = Student()
    let studentName = student.name //只有此文件内可访问
  }
}

fileprivate class Student {
    fileprivate var name: String = ""
    private var age: Int = 12 // 外部不可访问
}

```



##### Keyword：`private`

Description：私有访问权限, 被`private`修饰的类或者类的属性或方法可以在同一个物理文件中的同一个类型(包含`extension`)访问. 如果超出物理文件或不属于同一类型, 那么有着`private`访问权限的属性和方法就不能被访问.

```swift
private class Student {
    private let uid = UUID().uuidString
    private var name: String?
    
    private func taking() -> String {
        return "hello"
    }
}
```



### 类型范围作用域关键字

##### Keyword: `static`

Description:  用来修饰类型「class / struct / enum」的属性或方法。`static`具有以下特性：

- `static`可以修饰计算属性、存储属性、类型方法。
- 在 `protocol`中，如果需要，要使用 `static`  进行修饰。
- `static` 修饰的属性/方法具有隐式的 `final` 特性，因此不能够继承。
- 静态属性具有实例内存共享特性

```swift
struct Point {  
    let x: Double  
    let y: Double  
    // 存储属性  
    static let zero = Point(x: 0, y: 0)  
    // 计算属性  
    static var ones: [Point] {  
        return [
            Point(x: 1, y: 1),  
            Point(x: -1, y: 1),  
            Point(x: 1, y: -1),  
            Point(x: -1, y: -1)
        ]  
    }  
    // 类型方法  
    static func add(p1: Point, p2: Point) -> Point {  
        return Point(x: p1.x + p2.x, y: p1.y + p2.y)  
    }  
}
```



##### Keyword：`class`

Description：只能用来修饰类方法，计算属性。

- 在 `protocol` 中，不可以使用 `class` 来修饰。
- `class` 修饰的类方法，可以继承。

```swift
class MyClass {
    //修饰计算属性
    class var age: Int {
        return 10
    }
    // 修饰类方法
    class func testFunc() {
        
    }
}
```



### 逻辑运算关键字

##### Keyword： `if`，`else`

```swift
let number = 1

if number > 1 {
  print("greating many times")
} else if number < 1 {
  print("not greating")
} else {
  print("greating one times")
}

```



##### Keyword：`for`

```swift
let numbers: [Int] = [1, 2]
for index in 0..<numbers.count {
  print("number = \(numbers[index])")
}
```



##### Keyword: `switch`， `case`， `default`，`fallthrough`

```swift
enum GenderValue: Int {
  case male
  case female
}

switch gender {
  case .male:
  	print("The gender value is \(.male.rawValue)")
  case .female:
    print("The gender value is \(.female.rawValue)")
  default: break
}

var index = 10

switch index {
   case 100  :
      print( "index 的值为 100")
      fallthrough
   case 10,15  :
      print( "index 的值为 10 或 15")
      fallthrough
   case 5  :
      print( "index 的值为 5")
   default :
      print( "默认 case")
}

// index 的值为 10 或 15
// index 的值为 5
```



##### Keyword: `repeat`，`while`

```swift
// 在使用循环的判断条件之前，先执行一次循环中的代码。
repeat {
    // do something
} while n > 0

while n < 0 {
    // do something 
}
```



##### Keyword：`where`

Description：可以用来设置约束条件、限制类型，让代码更加简洁、易读。 [参考](https://www.hangge.com/blog/cache/detail_1826.html)

```swift
/// switch语句中使用
scores.forEach {
    switch $0 {
    case let x where x>=60:
        print("及格")
    default:
        print("不及格")
    }
}
 
/// for语句中使用
for score in scores where score>=60 {
    print("这个是及格的：\(score)")
}

/// 在 do catch 里面使用
enum ExceptionError: Error {
    case httpCode(Int)
}
 
func throwError() throws {
    throw ExceptionError.httpCode(500)
}
 
do{
    try throwError()
}catch ExceptionError.httpCode(let httpCode) where httpCode >= 500 {
    print("server error")
}catch {
    print("other error")
}

/// 与协议结合
protocol aProtocol{}
 
//只给遵守myProtocol协议的UIView添加了拓展
extension aProtocol where Self: UIView {
    func getString() -> String{
        return "string"
    }
}

/// 可以在 associatedtype 后面声明的类型后追加 where 约束语句
protocol Sequence {
    associatedtype Element where Self.Element == Self.Iterator.Element
    // ...
}

/// Sequence，Collection 同样新增了 where 语句约束
extension Sequence where Element: Numeric {
    var sum: Element {
        var result: Element = 0
        for item  in self {
            result += item
        }
        return result
    }
}

extension Collection where Element: Equatable {
    func prefieIsEqualSuffix(_ n: Int) -> Bool {
        let head = prefix(n)
        let suff = suffix(n).reversed()
        return head.elementsEqual(suff)
    }
}

```



##### Keyword：`guard`

Description： [详见](https://swift.gg/2016/02/14/swift-guard-radix/)

```swift
func updateWatchApplicationContext() {
    let session = WCSession.defaultSession()
    
    guard session.watchAppInstalled else { return }
    
    do {
        let context = ["token": api.token]
        try session.updateApplicationContext(context)
    } catch {
        print(error)
    }
}
```



##### Keyword：`return`

Description：方法返回值

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```



##### Keyword：`break`

Description：立刻结束整个控制流的执行。如果是嵌套循环（即一个循环内嵌套另一个循环），break 语句会停止执行最内层的循环，然后开始执行该块之后的下一行代码。

```swift
import Cocoa

var index = 10

repeat{
    index = index + 1
    
    if( index == 15 ){  // index 等于 15 时终止循环
        break
    }
    print( "index 的值为 \(index)")
}while index < 20
```



##### Keyword: `continue`

Description：一个循环体立刻停止本次循环迭代，重新开始下次循环迭代。对于 **for** 循环，**continue** 语句执行后自增语句仍然会执行。对于 **while** 和 **do...while** 循环，**continue** 语句重新执行条件判断语句。

```swift
import Cocoa
 
var index = 10

repeat{
   index = index + 1
    
   if( index == 15 ){ // index 等于 15 时跳过
      continue
   }
   print( "index 的值为 \(index)")
}while index < 20 
```



##### Keyword：`in`

```swift
/// 闭包中
{
    (s:String)->() in
    // body
}

/// for-in 循环
import Cocoa

for index in 1...5 {
    print("\(index) 乘于 5 为：\(index * 5)")
}
```



##### Keyword: `defer`

Description: [详见](https://onevcat.com/2018/11/defer/)

```swift
func operateOnFile(descriptor: Int32) {
    let fileHandle = FileHandle(fileDescriptor: descriptor)
    defer { fileHandle.closeFile() }
    let data = fileHandle.readDataToEndOfFile()

    if /* onlyRead */ { return }
    
    let shouldWrite = /* 是否需要写文件 */
    guard shouldWrite else { return }
    
    fileHandle.seekToEndOfFile()
    fileHandle.write(someData)
}
```



### 异常处理关键字

##### Keyword：`do`，`try`, `catch`，`throw`，`throws`，`rethrows`

Description: [参考1](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html)，[参考2](https://swifter.tips/error-handle/)



### 类型相关的关键字

##### Keyword：`Any`,`as`，`is`，`nil`， `super`，`self`， `Self`,  `Type`

```swift
/// Any：用于表示任意类型的实例，包括函数类型。
var anything = [Any]()
anything.append("Any Swift type can be added")  
anything.append(0)  
anything.append({(foo: String) -> String in "Passed in (foo)"})

/// as：类型转换运算符，用于尝试将值转成其它类型。
let intInstance = anything[1] as? Int

/// is：类型检查运算符，用于确定实例是否为某个子类类型。
class Person {}  
class Programmer : Person {}  
class Nurse : Person {}

let people = [Programmer(), Nurse()]

for aPerson in people  
{  
    if aPerson is Programmer  
    {  
        print("This person is a dev")  
    }  
    else if aPerson is Nurse  
    {  
        print("This person is a nurse")  
    }  
}

/// nil：在 Swift 中表示任意类型的无状态值。与 Objective-C 中的 nil 不同，Objective-C 中的 nil 表示指向不存在对象的指针。
//任何 Swift 类型或实例可以为 nil
var statelessPerson:Person? = nil  
var statelessPlace:Place? = nil  
var statelessInt:Int? = nil  
var statelessString:String? = nil

/// super：在子类中，暴露父类的方法、属性、下标。
class Person  
{  
    func printName()  
    {  
        print("Printing a name. ")  
    }  
}

class Programmer : Person  
{  
    override func printName()  
    {  
        super.printName()  
        print("Hello World!")  
    }  
}

let aDev = Programmer()  
aDev.printName() //打印 Printing a name. Hello World!

/// self：任何类型的实例都拥有的隐式属性，等同于实例本身。此外还可以用于区分函数参数和成员属性名称相同的情况。
class Person  
{  
    func printSelf()  
    {  
        print("This is me: (self)")  
    }  
}

let aPerson = Person()  
aPerson.printSelf() //打印 "This is me: Person"

/// Self：在协议中，表示遵守当前协议的实体类型。
protocol Printable  
{  
    func printTypeTwice(otherMe:Self)  
}

struct Foo : Printable  
{  
    func printTypeTwice(otherMe: Foo)  
    {  
        print("I am me plus (otherMe)")  
    }  
}

let aFoo = Foo()  
let anotherFoo = Foo()
aFoo.printTypeTwice(otherMe: anotherFoo) //打印 I am me plus Foo()

/// Type：表示任意类型的类型，包括类类型、结构类型、枚举类型、协议类型。
class Person {}  
class Programmer : Person {}

let aDev:Programmer.Type = Programmer.self
```



### 自定义运算符相关的关键字

##### Keyword：`left`，`right`,  `prefix`， `postfix`，`infix`，`operator`，`associativity`， `precedence` 

Description： [参考](https://nshipster.cn/swift-operators/)



##### Keyword: `none`

Description: 一个没有结合性的运算符。不允许这样的运算符相邻出现。

```swift
// "<" 是非结合性的运算符
1 < 2 < 3 //编译失败
```



### 内存管理相关的关键字

##### Keyword: `weak`, `unowned`

```swift
/// unowned：让循环引用中的实例 A 不要强引用实例 B。前提条件是实例 B 的生命周期要长于 A 实例。
class Person  
{  
    var occupation:Job?  
}

//当 Person 实例不存在时，job 也不会存在。job 的生命周期取决于持有它的 Person。
class Job  
{  
    unowned let employee:Person

    init(with employee:Person)  
    {  
        self.employee = employee  
    }  
}

/// weak：允许循环引用中的实例 A 弱引用实例 B ，而不是强引用。实例 B 的生命周期更短，并会被先释放。
class Person  
{  
    var residence:House?  
}

class House  
{  
    weak var occupant:Person?  
}

var me:Person? = Person()  
var myHome:House? = House()

me!.residence = myHome  
myHome!.occupant = me

me = nil  
myHome!.occupant // myHome 等于 nil
```



### 以#开头的关键字

```swift
/// #available：基于平台参数，通过 if，while，guard 语句的条件，在运行时检查 API 的可用性。
if #available(iOS 10, *)  
{  
    print("iOS 10 APIs are available")  
}
/// #colorLiteral：在 playground 中使用的字面表达式，用于创建颜色选取器，选取后赋值给变量。
let aColor = #colorLiteral //创建颜色选取器

/// #column：一种特殊的字面量表达式，用于获取字面量表示式的起始列数。
class Person  
{  
    func printInfo()  
    {  
        print("Some person info - on column (#column)")
    }  
}

let aPerson = Person()  
aPerson.printInfo() //Some person info - on column 53

/// #else：条件编译控制语句，用于控制程序在不同条件下执行不同代码。与 #if 语句结合使用。当条件为 true，执行对应代码。当条件为 false，执行另一段代码。
#if os(iOS)  
print("Compiled for an iOS device")  
#else  
print("Not on an iOS device")  
#endif

/// #elseif：条件编译控制语句，用于控制程序在不同条件下执行代码。与 #if 语句结合使用。当条件为 true，执行对应代码。
#if os(iOS)  
print("Compiled for an iOS device")  
#elseif os(macOS)  
print("Compiled on a mac computer")  
#endif

/// #endif：条件编译控制语句，用于控制程序在不同条件下执行代码。用于表明条件编译代码的结尾。
#if os(iOS)  
print("Compiled for an iOS device")  
#endif

/// #file：特殊字面量表达式，返回当前代码所在源文件的名称。
class Person  
{  
    func printInfo()  
    {  
        print("Some person info - inside file (#file)")
    }  
}

let aPerson = Person()  
aPerson.printInfo() //Some person info - inside file /*代码所在 playground 文件路径*/

/// #fileReference：playground 字面量语法，用于创建文件选取器，选取并返回 NSURL 实例。
let fontFilePath = #fileReference //创建文件选取器

/// #function：特殊字面量表达式，返回函数名称。在方法中，返回方法名。在属性的 getter 或者 setter 中，返回属性名。在特殊的成员中，比如 init 或 subscript 中，返回关键字名称。在文件的最顶层时，返回当前所在模块名称。
class Person  
{  
    func printInfo()  
    {  
        print("Some person info - inside function (#function)")
    }  
}

let aPerson = Person()  
aPerson.printInfo() //Some person info - inside function printInfo()

/// #if：条件编译控制语句，用于控制程序在不同条件下编译代码。通过判断条件，决定是否执行代码。
#if os(iOS)  
print("Compiled for an iOS device")  
#endif

/// #imageLiteral：playground 字面量语法，创建图片选取器，选择并返回 UIImage 实例。
let anImage = #imageLiteral //在 playground 文件中选取图片

/// #line：特殊字面量表达式，用于获取当前代码的行数。
class Person  
{  
    func printInfo()  
    {  
        print("Some person info - on line number (#line)")
    }  
}

let aPerson = Person()  
aPerson.printInfo() //Some person info - on line number 5

/// #selector：用于创建 Objective-C selector 的表达式，可以静态检查方法是否存在，并暴露给 Objective-C。
//静态检查，确保 doAnObjCMethod 方法存在  
control.sendAction(#selector(doAnObjCMethod), to: target, forEvent: event)

/// #sourceLocation：行控制语句，可以指定与原先完全不同的行数和源文件名。通常在 Swift 诊断、debug 时使用。
#sourceLocation(file:"foo.swift", line:6)

/// 打印新值
print(#file)  
print(#line)

// 重置行数和文件名
#sourceLocation()

print(#file)  
print(#line)
```

