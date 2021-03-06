# Swift 拓展和泛型

## Swift 拓展 Extension

### 扩展计算型属性和便利构造函数
```
struct Point {
    var x = 0.0
    var y = 0.0
}

struct Size {
    var width = 0.0
    var height = 0.0
}


class Rectangle {
    var origin  = Point()
    var size = Size()
    
    init (origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
}

// 拓展 Rectangle
extension Rectangle {
    func translate(x: Double,y: Double) {
        self.origin.x += x
        self.origin.y += y
    }
}

let rect = Rectangle(origin: Point(), size: Size(width: 4, height: 5))
rect.translate(x: 10, y: 10)

rect

// 扩展计算型属性和扩展便利构造函数
extension Rectangle{
    //    var center: Point = Point() // 不允许扩展存储型属性
    var center: Point {
        get{
            let centerX = origin.x + size.width/2
            let centerY = origin.y + size.height/2
            return Point(x: centerX, y: centerY)
        }
        set{
            origin.x = newValue.x - size.width/2
            origin.y = newValue.y - size.height/2
        }
    }
    
    // 扩展构造函数 - 扩展中加入构造函数必须是一个便利的 convenience 构造函数，使用 self.init() 的方式引用当前对象实例化的指定构造函数
    convenience init(center: Point, size: Size){
        let originX = center.x - size.width/2
        let originY = center.y - size.height/2
        
        self.init(origin: Point(x: originX, y: originY),size: size)
    }
}
```

### 扩展嵌套枚举

```
class UI{
    
    enum Theme{
        case DayMode
        case NightMode
    }
    
    var fontColor: UIColor!
    var backgroundColor: UIColor!
    var themeMode: Theme = .DayMode{
        didSet{
            self.changeTheme(themeMode: self.themeMode)
        }
    }
    
    init(){
        self.themeMode = .DayMode
        self.changeTheme(themeMode: self.themeMode)
        
    }
    
    
    init(themeMode: Theme){
        self.themeMode = themeMode
        self.changeTheme(themeMode: themeMode)
    }
    
    func changeTheme( themeMode: Theme ){
        switch(themeMode){
        case .DayMode:
            fontColor = UIColor.black
            backgroundColor = UIColor.white
        case .NightMode:
            fontColor = UIColor.white
            backgroundColor = UIColor.black
        }
    }
}


let ui = UI()
ui.themeMode
ui.fontColor
ui.backgroundColor

ui.themeMode = UI.Theme.NightMode
ui.themeMode
ui.fontColor
ui.backgroundColor
```

### 使用 extension 扩展系统类库

```
extension Int{
    // 计算平方
    var square: Int{
        return self * self
    }
    
    // 计算立方
    var cube: Int{
        return self * self * self
    }
    
    // 扩展方法 判断整型是否在前闭后开的范围内
    func inRange(closedLeft left: Int, opendRight right: Int) -> Bool {
        return self >= left && self < right
    }
    
    // 扩展方法
    func repetitions( task: () -> Void ){
        for _ in 0 ..< self {
            task()
        }
    }
    
}

let num = 8
num.square // 64
num.cube // 512

let index = 10
index.inRange(closedLeft: 0, opendRight: 20) // 判断 index 是否在 0 ..< 20

num.repetitions{
	print("Hello,Swift!")
}
```

## Swift 泛型 Generic

泛型：是指有一套通用的逻辑，不与类型相关。例如交换两个参数的值，如下：

```
func swapTwoInt( _ a: inout Int , _ b: inout Int) {
    (a,b) = (b,a)
}

var a: Int = 0
var b: Int = 6
swapTwoInt(&a, &b)
a
b

// 上述函数定义中，如果需求要交换两个浮点数，那么我们又需要定义类似如下函数来适应变化。
func swapTwoDouble(_ a: inout Double , _ b: inout Double) {
    (a,b) = (b,a)
}
```

> 以上函数不管是交换整型还是交换浮点型，都是给两个遍历交换相互的值。那我们可以通过泛型对这种需求做改进。

```
// generic function
func swapTwoThings<T>(_ a: inout T , _ b: inout T) {
    (a,b) = (b,a)
}

var hello = "Hello"
var bye = "Bye"
swapTwoThings(&hello, &bye)
hello
bye

swapTwoThings(&a, &b)
a
b

swap(&a, &b) // Swift 中的一个泛型函数
```

> 使用泛型可以创建更加通用的函数。


### 泛型类型

泛型函数与泛型语法。

```
let arr:Array<Int> = Array<Int>()
let dict = Dictionary<String,Int>()
let set  = Set<Float>()

// 创建自定义数据结构 栈（后进先出）
struct Stack<T>{
    // 存储栈的内容，数组类型
    var items = [T]()
    
    func isEmpty() -> Bool{
        return items.count == 0
    }
    
    mutating func push(item: T) {
        items.append(item)
    }
    
    mutating func pop() -> T? {
        
        guard !self.isEmpty() else {
            return nil
        }
        
        return items.removeLast()
    }
    
}
// 扩展 Stack
extension Stack{
    
    func top() -> T? {
        return items.last
    }
    
    func count() -> Int {
        return items.count
    }
}


var s = Stack<Int>() // 存储Int类型的栈
s.push(item: 1)
s.push(item: 2)
s.pop() // 2

var ss = Stack<String>() // 声明存储String类型的栈

ss.top() // nil

// 两个数据类型的泛型
struct Pair<T1,T2> { // 取不同的泛型类型名称
    
    var a: T1
    var b: T2
    
}

var pair = Pair<Int,String>(a: 0 , b: "Hello") // 成功创建两个数据类型的泛型
pair
```