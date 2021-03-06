## Swift 下标和运算符重载


在Swift的基础类型中，比如数组、字典。我们通过`arr[0]`、`dict["name"]`等，在方括号中传入的参数就是下标。
在Swift语言中，我们不仅仅可以使用系统自带的下标来索引数组、字典中的元素，我们还可以为自己所创建的任何类型，例如：枚举、结构体或者类来定义下标。

以结构体为例，使用 `subscript` 关键字申明。如下:

```
struct Vector{
    var x: Double = 0.0
    var y: Double = 0.0
    var z: Double = 0.0
	
    // 下标：接受参数为整型，返回浮点型
    subscript(index: Int) -> Double? {
        get { // 调用下标的逻辑
            switch index{
            case 0: return x
            case 1: return y
            case 2: return z
            default: return nil
            }
        }

        set { // 设置下标的逻辑
            guard let newValue = newValue else{
                return
            }

            switch index{
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
            
        }
    }

    subscript (axis: String) -> Double? {
        // 不写 默认是 getter
        switch axis {
        case "x","X" : return x
        case "y","Y": return y
        case "z","Z": return z
        default: return nil
        }
    }
}

var v = Vector(x: 1.1, y: 2.2 , z: 3.3)

v.y // 2.2
v[2] // 3.3

v["x"] // 1.1
v["T"] // nil

// 使用下标对结构体属性赋值
v[1] = 9.09
v.y // 9.09
```

> 在一个结构体、枚举或者类中，我们可以根据需要写多个下标的定义。

### 多维下标

```
// 矩阵
struct Matrix {
    var data: [[Double]]
    var r: Int
    var c: Int
    
    init(row: Int, col:Int) {
        self.r = row
        self.c = col
        
        data = [[Double]]()
        
        for _ in 0..<r {
            
            let aRow = Array(repeating: 0.0, count: col)
            data.append(aRow)
        }
    }
    
    subscript(x: Int, y:Int) -> Double {
        get {
            assert(x >= 0 && x < self.r && y >= 0 && y < self.c, "Index out of range") // 断言
            return data[x][y]
        }
        set {
            assert(x >= 0 && x < self.r && y >= 0 && y < self.c, "Index out of range") // 断言
            data[x][y] = newValue
        }
    }
    
    subscript(row: Int) -> [Double] {
        get {
            assert(row >= 0 && row < self.r, "Index out of range.") // 断言
            return data[row]
        }
        set(vector) {
            assert(vector.count == self.c, "column number does not match.") // 断言
            data[row] = vector
        }
    }
}


var m = Matrix(row: 2, col: 2)

m[1, 1]
m[1, 1] = 100.0

m[1]
m[1][1]

m[0] = [1.5, 4.5]
m[0][1]
m[0, 0]
print(m)
```


### 运算符重载
假如有如下自定义结构体为例，进行运算符的重载。

```
struct Vector {
    var x: Double = 0.0
    var y: Double = 0.0
    var z: Double = 0.0
    
    // 下标
    subscript(index: Int) -> Double? {
        get{
            switch index{
            case 0: return x
            case 1: return y
            case 2: return z
            default: return nil
            }
        }
        
        set{
            guard let newValue = newValue else{
                return
            }
            
            switch index{
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
            
        }
    }
}


var va = Vector(x: 1.0, y: 2.0, z: 3.0)
var vb = Vector(x: 3.0, y: 4.0, z: 5.0)

// 定义函数重载 + 运算符
func +(left: Vector, right: Vector) -> Vector {
    return Vector(x: left.x + right.x, y: left.y + right.y, z: left.z + right.z)
}
va + vb

// 定义函数重载 - 运算符
func - (left: Vector, right: Vector) -> Vector{
    return Vector(x: left.x - right.x, y: left.y - right.y, z: left.z - right.z)
}
vb - va // Vector(x: 2.0, y: 2.0, z: 2.0)

// 定义函数重载 * 运算符
func * (left: Vector, right: Vector) -> Double {
    return left.x * right.x + left.x * right.y + left.z * right.z
}
va * vb // 22.0

// 定义函数重载 * 运算符
func * (left: Vector, a: Double) -> Vector {
    return Vector(x: left.x * a, y: left.y * a, z: left.z * a)
}
va * -1.0 // Vector(x: -1.0, y: -2.0, z: -3.0)
// 这次运算符接收的参数是不一样的，所以和上面的运算符没有影响.


// 复用重载的 * 运算符
func * (a: Double , right: Vector) -> Vector {
    return right * a // 由于上面写了向量 * 浮点数，这里可以复用上面的 * 运算符
}
-2.0 * va

// 定义函数重载 += 运算符
func +=( left: inout Vector , right: Vector) { // 左侧的值需要返回值，所以定义一个inout参数，所以也不需要定义函数的返回值
    left = left + right
}

va += vb
va // Vector(x: 4.0, y: 6.0, z: 8.0)

// 定义函数重载 -= 运算符
func -= (left: inout Vector, right: Vector){
    left = left - right
}
vb -= va

```

> 对于运算符的重载，我们是不可以重载 = 运算符的，或者将 = 不当做运算符号。


#### 重载运算符 - 表示对值取反 

```
使用 `prefix` 关键字，标识该运算符当做运算的前缀来使用。
prefix func - (vector: Vector) -> Vector{
    return Vector(x: -vector.x, y: -vector.y, z: -vector.z)
}

-va // Vector(x: -4.0, y: -6.0, z: -8.0)
```

#### 重载逻辑运算符

```
struct Vector {
    var x: Double = 0.0
    var y: Double = 0.0
    var z: Double = 0.0
    
    // 下标
    subscript(index: Int) -> Double? {
        get{
            switch index{
            case 0: return x
            case 1: return y
            case 2: return z
            default: return nil
            }
        }
        
        set{
            guard let newValue = newValue else{
                return
            }
            
            switch index{
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
            
        }
    }
}


var va = Vector(x: 1.0, y: 2.0, z: 3.0)
var vb = Vector(x: 3.0, y: 4.0, z: 5.0)

// 重载 == 比较运算符
func == (left: Vector, right: Vector) -> Bool {
    return left.x == right.x && left.x == right.y && left.z == right.z
}
va == vb // false

// 重载 == 比较运算符
func != (left: Vector, right: Vector) -> Bool{
    //    return left.x != right.x || left.y != right.y || left.z != right.z
    return !(left == right) // 复用上面重载的 == 运算符的结果，逻辑取反
}
va != vb // true

// 重载 < 比较运算符
func < (left: Vector, right: Vector) -> Bool {
    if left.x != right.x {return left.x < right.x}
    if left.y != right.y {return left.y < right.y}
    if left.z != right.z {return left.z < right.z}
    
    return false
}

// 重载 <= 比较运算符
func <= (left: Vector, right: Vector) -> Bool {
    return left < right || left == right
}

// 重载 > 比较运算符
func > (left: Vector, right: Vector)-> Bool{
    return !(left <= right)
}

// 重载 >= 比较运算符
func >= (left: Vector, right: Vector) -> Bool{
    return !(left > right)
}

```

> 对于比较运算符而言，返回值是Bool类型。

### 自定义运算符
#### 定义单目运算符
　　Custom operators can begin with one of the ASCII characters `/`, `=` , `-` , `+` , `!` , `*` , `%` , `<` , `>` , `&` , `|` , `^` , or `~` , or with one of the Unicode characters

```
struct Vector{
    var x: Double = 0.0
    var y: Double = 0.0
    var z: Double = 0.0
    
    // 下标
    subscript(index: Int) -> Double? {
        get{
            switch index{
            case 0: return x
            case 1: return y
            case 2: return z
            default: return nil
            }
        }
        
        set{
            guard let newValue = newValue else{
                return
            }
            
            switch index{
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
            
        }
    }
}

var va = Vector(x: 1.0, y: 2.0, z: 3.0)
var vb = Vector(x: 3.0, y: 4.0, z: 5.0)

// 重载 + 运算符
func + (left: Vector , right: Vector) -> Vector{
    return Vector(x: left.x+right.x, y: left.y+right.y
        , z: left.z+right.z)
}
// 重载 += 运算符
func += ( left: inout Vector , right: Vector) {
    left = left + right
}

// 后置 +++ 运算符
postfix operator +++ {} // 声明一个 swift 不存在的运算符
postfix func +++(vector: inout Vector) -> Vector {
    vector += Vector(x: 1.0, y: 1.0, z: 1.0)
    return vector
}
print(va+++) // Vector(x: 2.0, y: 3.0, z: 4.0)

// 前置 +++ 运算符
prefix operator +++ {} // 声明一个 swift 不存在的运算符
prefix func +++(vector: inout Vector) -> Vector {
    let ret = vector
    vector += Vector(x: 1.0, y: 1.0, z: 1.0)
    return ret
}
+++va
print(va) // Vector(x: 3.0, y: 4.0, z: 5.0)
```

#### 定义双目运算符

```
struct Vector{
    var x: Double = 0.0
    var y: Double = 0.0
    var z: Double = 0.0
    
    // 下标
    subscript(index: Int) -> Double? {
        get{
            switch index{
            case 0: return x
            case 1: return y
            case 2: return z
            default: return nil
            }
        }
        
        set{
            guard let newValue = newValue else{
                return
            }
            
            switch index{
            case 0: x = newValue
            case 1: y = newValue
            case 2: z = newValue
            default: return
            }
            
        }
    }
    
    func length() -> Double {
        return sqrt( self.x * self.x + self.y * self.y + self.z * self.z )
    }
}

var va = Vector(x: 1.0, y: 2.0, z: 3.0)
var vb = Vector(x: 3.0, y: 4.0, z: 5.0)

func * (left: Vector, right: Vector) -> Double {
    return left.x * right.x + left.x * right.y + left.z * right.z
}
```

> 使用 `infix` 关键字定义双目运算符。

##### 自定义 ^ 运算符
```
infix operator ^ {}
func ^(left: Vector, right: Vector) -> Double{
    return cos( (left * right) / (left.length() * right.length()) )
}

va ^ vb
```

##### 自定义 ** 运算符求幂运算
```
infix operator ** : ATPrecedence
precedencegroup ATPrecedence {
    associativity: left
    higherThan: AdditionPrecedence
    lowerThan: MultiplicationPrecedence
}
func ** (x: Double, p: Double)-> Double{
    return pow(x,p)
}

2**3**3 // 521

1+2**3**3 // 522

5*2**3**2 // 1000000
```

> Apple 的一些运算符信息参考这里： https://developer.apple.com/reference/swift/1851035-swift_standard_library_operators#//apple_ref/doc/uid/TP40016054






