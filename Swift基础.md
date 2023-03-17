
# Swift基础

## Swift简介
Swift 是一种支持多编程范式和编译式的开源编程语言,苹果于2014年WWDC（苹果开发者大会）发布，用于开发 iOS，OS X 和 watchOS 应用程序。

Swift 结合了 C 和 Objective-C 的优点并且不受 C 兼容性的限制。

Swift 在 Mac OS 和 iOS 平台可以和 Object-C 使用相同的运行环境。


## 变量
使用var定义变量, 使用let定义常量
```swift
var a = 10
var b = 3.1415926

let c = "str"
// c = "str2" // 会报错Cannot assign to value: 'c' is a 'let' constant

// 可以指定类型
var e: Float = 10.20
```



## 条件语句
### switch 语句

switch 语句必须要有default, 否则会报错


```swift
var index = 10

switch index {
   case 100  :
      print( "index 的值为 100")
      fallthrough
   case 10,15  :
      print( "index 的值为 10 或 15")
   case 5  :
      print( "index 的值为 5")
   default :
      print( "默认 case")
}
```

>注意：在大多数语言中，switch 语句块中，case 要紧跟 break，否则 case 之后的语句会顺序运行，而在 Swift 语言中，默认是不会执行下去的，switch 也会终止。如果你想在 Swift 中让 case 之后的语句会按顺序继续运行，则需要使用 fallthrough 语句。

## 循环
### for in 循环
```swift
for index in 1...5 {
    print("\(index) 乘于 5 为：\(index * 5)")
}

// 1~4的值, 不包含5
for index in 1..<5 {
    print("\(index) 乘于 5 为：\(index * 5)")
}

```

### While 循环

```swift
var index = 10

while index < 20 
{
   print( "index 的值为 \(index)")
   index = index + 1
}
```

### repeat...while 循环

```swift
var index = 15

repeat{
    print( "index 的值为 \(index)")
    index = index + 1
}while index < 20
```


## 字符串

使用字符串属性 isEmpty 来判断字符串是否为空  
使用字符串属性 count 来获取字符串长度

```swift
var stringA = "abcd"
print( stringA.isEmpty )
print( stringA.count )
```

字符串分割数组 -- 基于空格
```swift
let fullName = "First Last"
let fullNameArr = fullName.split{$0 == " "}.map(String.init)

fullNameArr[0] // First
fullNameArr[1] // Last
```

字符串的遍历
```swift
let str = "abcd1234"

for ch in str {
    print(ch)
}
```

## 数组

Swift 数组会强制检测元素的类型，如果类型不同则会报错。

> 如果将一个数组赋值给常量，数组就不可更改，并且数组的大小和内容都不可以修改。这一点和JavaScript不一样, JavaScript是可以修改数组内容的, 只是不能重新赋值为新的数组。
```swift
let someInts:[Int] = [10, 20, 30]
someInts[1] = 100 // Cannot assign through subscript: 'someInts' is a 'let' constant
```

创建数组
```swift
var someInts = [Int](repeating: 0, count: 3)
print(someInts) // [0, 0, 0]

var someInts2:[Int] = [10, 20, 30]
```

数组的方法
```swift
var someInts: [Int] = []

someInts.append(10)
someInts.append(20)
someInts.popLast()

for item in someInts {
   print(item)
}

for (index, item) in someInts.enumerated() {
    print("在 index = \(index) 位置上的值为 \(item)")
}

// 合并数组
var arr = [1, 2] + [3, 4]
print(arr) // [1, 2, 3, 4]
print(arr.count) // 4
print(arr.isEmpty) // false

```


## 字典
字典也有 count 属性和 isEmpty 属性

字典操作如下
```swift
// 初始化一个空字典, key是string, 值是int
var someDict: [String: Int] = [:]
someDict["a"] = 1
someDict["b"] = 2
print(someDict["a"]) // Optional(1)
print(someDict["b"]!) // 2


// 移除 Key-Value 对
someDict.removeValue(forKey: "a") // 或者使用someDict["a"] = nil
print(someDict) // ["b": 2]


// 遍历字典
for (key, value) in someDict {
   print("\(key) , \(value)") // b , 2
}

for (index, dict) in someDict.enumerated() {
    print("\(index) , \(dict)") // 0 , (key: "b", value: 2)
}


// 字典转换为数组
var someDict2:[Int:String] = [1:"One", 2:"Two", 3:"Three"]

let dictKeys = [Int](someDict2.keys)  // 值可能是[1, 2, 3], 也可能是[2, 1, 3], [3, 2, 1]等
let dictValues = [String](someDict2.values) // 同理, 值的顺序也是不确定的

```


## 函数
基础使用
```swift
// 无参数, 无返回值的函数
func log() {
    print("log")
}
log()


// 有参数, 有返回值的函数
func getInfo(name: String, age: Int) -> String {
    return name + ": " + String(age)
}
getInfo(name: "AA", age: 22) // "AA: 22"

```

### 元组作为函数返回值
```swift
// 在一个Int数组中找出最小值与最大值
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("最小值为 \(bounds.min)，最大值为 \(bounds.max)")
}

```

### 常量，变量及 I/O 参数
一般默认在函数中定义的参数都是常量参数，也就是这个参数你只可以查询使用，不能改变它的值。

如果想要声明一个变量参数，可以在参数定义前加 inout 关键字，这样就可以改变这个参数的值了。

例如：

```swift
func getName(_ name: inout String).........
```
此时这个 name 值可以在函数中改变。

一般默认的参数传递都是传值调用的，而不是传引用。所以传入的参数在函数内改变，并不影响原来的那个参数。传入的只是这个参数的副本。

当传入的参数作为输入输出参数时，需要在参数名前加 & 符，表示这个值可以被函数修改。

实例
```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}


var x = 1
var y = 5
swapTwoInts(&x, &y)

print(x) // 5
print(y) // 1
```

### 使用函数类型

定义一个类型为函数的常量或变量，并将适当的函数赋值给它：
```swift
func sum(a: Int, b: Int) -> Int {
   return a + b
}

var addition: (Int, Int) -> Int = sum
addition(40, 80) // 120
```


### 函数类型作为参数类型、函数类型作为返回类型
```swift
func addition(a: Int, b: Int) -> Int {
    return a + b
}

func another(addition: (Int, Int) -> Int, a: Int, b: Int) {
    print("输出结果: \(addition(a, b))")
}
another(addition: addition, a: 10, b: 20)
```



## Swift 闭包

```swift
let test = { print("Swift 闭包实例。") }
test()


var add = {(a: Int, b: Int) -> Int in
    return a + b
}
add(1, 2) // 3
```

### 使用 sorted 方法排序
```swift
let names = ["AT", "AE", "D", "S", "BE"]

// 使用普通函数(或内嵌函数)提供排序功能,闭包函数类型需为(String, String) -> Bool。
func backwards(s1: String, s2: String) -> Bool {
    return s1 > s2
}
var reversed = names.sorted(by: backwards)

print(reversed) // ["S", "D", "BE", "AT", "AE"]
```
### 参数名称缩写
Swift 自动为内联函数提供了参数名称缩写功能，可以直接通过$0,$1,$2来顺序调用闭包的参数。
```swift
let names = ["AT", "AE", "D", "S", "BE"]

var reversed = names.sorted( by: { $0 > $1 } )
print(reversed)
```

### 运算符函数
实际上还有一种更简短的方式来撰写上面例子中的闭包表达式。

Swift 的String类型定义了关于大于号 (>) 的字符串实现，其作为一个函数接受两个String类型的参数并返回Bool类型的值。 而这正好与sort(_:)方法的第二个参数需要的函数类型相符合。

```swift
let names = ["AT", "AE", "D", "S", "BE"]

var reversed = names.sorted( by: > )
print(reversed)
```

### 尾随闭包
尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。
```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // 函数体部分
}

// 以下是不使用尾随闭包进行函数调用
someFunctionThatTakesAClosure({
    // 闭包主体部分
})

// 以下是使用尾随闭包进行函数调用
someFunctionThatTakesAClosure() {
  // 闭包主体部分
}
```
```swift
let names = ["AT", "AE", "D", "S", "BE"]

var reversed = names.sorted( by: > )        // 运算符函数
reversed = names.sorted( by: { $0 > $1 } )  // 参数名称缩写
reversed = names.sorted() { $0 > $1 }       // 尾随闭包
reversed = names.sorted { $0 > $1 }         // 尾随闭包, 只需要一个参数时可以省略"()"

print(reversed)

```

### 捕获值
闭包可以在其定义的上下文中捕获常量或变量。

即使定义这些常量和变量的原域已经不存在，闭包仍然可以在闭包函数体内引用和修改这些值。

Swift最简单的闭包形式是嵌套函数
```swift
func makeIncrementor(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementor() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementor
}

let incrementByTen = makeIncrementor(forIncrement: 10)


print(incrementByTen()) // 10
print(incrementByTen()) // 20 
print(incrementByTen()) // 30

```

## Swift 枚举
枚举简单的说也是一种数据类型，只不过是这种数据类型只包含自定义的特定数据，它是一组有共同特性的数据的集合。  
和 C 和 Objective-C 不同，Swift 的枚举成员在被创建时不会被赋予一个默认的整型值。

```swift
// 定义枚举
enum DaysofaWeek {
    case Sunday
    case Monday
    case TUESDAY
    case WEDNESDAY
    case THURSDAY
    case FRIDAY
    case Saturday
}

var weekDay = DaysofaWeek.THURSDAY
weekDay = .THURSDAY
switch weekDay
{
case .Sunday:
    print("星期天")
case .Monday:
    print("星期一")
case .TUESDAY:
    print("星期二")
case .WEDNESDAY:
    print("星期三")
case .THURSDAY:
    print("星期四")
case .FRIDAY:
    print("星期五")
case .Saturday:
    print("星期六")
}
```


## Swift 结构体
Swift 结构体是构建代码所用的一种通用且灵活的构造体。

我们可以为结构体定义属性（常量、变量）和添加方法，从而扩展结构体的功能。

```swift
struct studentMarks {
   var mark1 = 100
   var mark2 = 78
   var mark3 = 98
}
let marks = studentMarks()
print(marks.mark1, marks.mark2, marks.mark3) // 100 78 98
```

```swift
struct markStruct{
    var mark1: Int
    var mark2: Int
    var mark3: Int
    
    init(mark1: Int, mark2: Int, mark3: Int){
        self.mark1 = mark1
        self.mark2 = mark2
        self.mark3 = mark3
    }
}


var marks = markStruct(mark1: 98, mark2: 96, mark3:100)
print(marks.mark1) // 98
```


## Swift 类

```swift
class MarksStruct {
    var mark: Int
    init(mark: Int) {
        self.mark = mark
    }
}

class studentMarks {
    var mark = 300
}
let marks = studentMarks()
marks.mark // 300
```

### 恒等运算符
因为类是引用类型，有可能有多个常量和变量在后台同时引用某一个类实例。

为了能够判定两个常量或者变量是否引用同一个类实例，Swift 内建了两个恒等运算符：

```text
恒等运算符       
运算符为：===       
如果两个常量或者变量引用同一个类实例则返回 true  

不恒等运算符      
运算符为：!==        
如果两个常量或者变量引用不同一个类实例则返回 true
```

```swift
class SampleClass: Equatable {
    let myProperty: String
    init(s: String) {
        myProperty = s
    }
}
func ==(lhs: SampleClass, rhs: SampleClass) -> Bool {
    return lhs.myProperty == rhs.myProperty
}

let spClass1 = SampleClass(s: "Hello")
let spClass2 = SampleClass(s: "Hello")

print(spClass1 === spClass2) // false
print(spClass1 == spClass2)  // true

print(spClass1.myProperty == spClass2.myProperty) // true
// print(spClass1.myProperty === spClass2.myProperty) // 报错, Argument type 'String' expected to be an instance of a class or class-constrained type
```


## Swift 属性


https://www.runoob.com/swift/swift-properties.html

...
