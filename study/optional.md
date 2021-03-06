### Swift 可选型

　　可选类型用来表示值缺失的情况。我们可以在具体的值与 `nil` 之间做一个选择。
  
### 可选类型定义

```
var x : Int?
var y : Int
y = 10
print("\(x) -- \(y)") // 输出：nil -- 10
```

> `Int?` 与 `Int` 是两种完全不同的数据类型，前者为整型的可选行，后者是整型。

如果我们不给 `y` 赋值的话，程序将抛出错误。如下图：

![](image/WX20161209-105334.png)

　　
因为如果声明了一个可选变量或者常量而没有赋值，则默认为 `nil` ，`nil` 不能用于非可选的常量或者变量。
其次，声明了一个非可选型的常量或者变量并没有赋值的之前，当我们输出的时候会抛出异常。


### 可选型的取值

#### 使用`!`进行强制解包

　　当确定可选类型确实包含值之后，可以在可选的名字后⾯加⼀个感叹号（ `!`  ）来获取值。这个感叹号表⽰“我知道这个可选有值，请使⽤它。”这被称为可选值的强制解析。

```
"The errorCode is " + errorCode! // The errorCode is 404
```
> **注意：** 这种写法是在明确知道了可选型不等于 `nil` 的情况下使用。如果可选型的值为 `nil` 则会抛出 **fatal error** 致命错误。

####  判断不是 `nil`
　　解包前对数据进行判断，如果不等于 `nil` 则进行解包操作。
```
var errorCode: String? = "404"

if errorCode != nil {
    "The errorCode is " + errorCode!
}else{
    "No Error"
}
```

####  `if-let` 解包

还可以给解包的值使用 `if let` 语句进行赋值。
```
var errorCode: String? = "404"

if let upwrappedErrorCode = errorCode { // 允许解包的常量名和可选型的名字一致
    "The errorCode is " + upwrappedErrorCode
}else{
    "No Error"
}
```

> 当然也可以使用 `if var` 的方式对可选型进行解包，不过通常在大多数的情况下，我们对一个可选性只是对其解包读取值，并不会去修改可选型的值。

#### 可以使用相同的变量名

也可以将上面的常量 `upwrappedErrorCode` 允许使用和 `errorCode` 一致的名称。

```
var errorCode: String? = "404"

if let errorCode = errorCode {
    "The errorCode is " + errorCode
}else{
    "No Error"
}
```

> 这种解包方式只能在 `{}` 中访问 `errorCode` 常量。在其他地方访问 `errorCode`变量依然是可选型。

#### 使用 `if-let` 同时解包多个变量

如果存在两种或者多种需要解包判断的情况，我们可以使用更加优雅的写法
```
var errorCode: String? = "404"
var errorMessage: String? = "Not Found"

if let errorCode = errorCode , 
   let errorMessage = errorMessage {
    "The errorCode is " + errorCode + "\nThe errorMessage is " + errorMessage
}

// 以上写法等同于
if let errorCode = errorCode {
    if let errorMessage = errorMessage {
        "The errorCode is " + errorCode + "\nThe errorMessage is " + errorMessage
    }
}
```

还可以新增一些判断条件，如下：
解包两个可选型，并判断 `errorCode` 的值是否等于 404 。
```
if let errorCode = errorCode , 
   let errorMessage = errorMessage , errorCode == "404" {
    "page not found"
}
```

#### 可选型 Chaining

```
var errorMessage: String? = "Not Found"
errorMessage?.uppercased()
```
　　虽然 `errorMessage` 是一个可选型，这里尝试对它进行解包，如果解包成功（即不等于 `nil`）那么对他进行 `uppercased()` 操作；如果解包失败则不回执行 `uppercased()` 操作。
　　如果可选型 `errorMessage` 等于 `nil` ，则会返回 `nil`。

逻辑等同于如下写法

```
var errorMessage: String? = "Not Found"
if let errorMessage = errorMessage {
    errorMessage.uppercased()
}
```

另一种写法

```
errorMessage!.uppercased()
```

　　将可选型 `errorMessage` 进行强制解包，并调用 `uppercased()` 操作，如果可选型 `errorMessage` 等于 `nil` 将会抛出 **fatel error** 致命错误，这种写法一般情况下是不安全的，也是不推荐的。

```
var uppercaseErrorMessage = errorMessage?.uppercased() // 变量 `uppercaseErrorMessage` 是一个可选型
```

```
if let errorMessage = errorMessage?.uppercased() {
    errorMessage
}
```
> 上面的写法将解包和调用操作以及赋值基于一体，使逻辑更加清晰，减少代码的出错率。

#### 可选型 `Nil-Coalesce`

　　如下需求： `message` 变量的值待定，如果 `errorMessage` 可选型的值不等于 `nil` 时，变量 `message` 的值将等于 `errorMessage` 可选型解包后的值，否则将被赋值为 `no error`
```
var errorMessage: String? = nil

let message: String

if let errorMessage = errorMessage {
    message = errorMessage
}else{
    message = "no error"
}
```

我们可以使用三元运算符进行改写（返回可选型解包后的值）

```
let message2: String = ( errorMessage != nil ) ? errorMessage! : "no error"
```

也可以使用 Swift 更简洁的方法（返回可选型解包后的值）
```
let message3 = errorMessage ?? "no error"
```

### 可选型在元组中的使用


#### 将元组中的某个单元设置为可选型
```
var error: ( errorCode: Int , errorMessage: String? ) = ( 404 , "Not Found" )

print(error) // (404, Optional("Not Found"))

error.errorMessage = nil // 将元组的第一个分量设置为 `nil`

print(error) // (404, nil)
```

#### 将整个元组设置为可选型
```
var error2: ( errorCode: Int , errorMessage: String )? = ( 404 , "Not Found" )

// error2.errorMessage = nil // 这样设置会报错

error2 = nil // 这样操作是允许的
```

#### 将元组中的某个值以及整个元组都设置为可选型
```
var error3: ( errorCode: Int , errorMessage: String? )? = ( 404 , "Not Found" )
```

### 可选型的一些实际应用

#### 获取用户输入的年龄

```
var ageInput: String = "19"

var age = Int( ageInput ) // 将用户输入强制转换成整型

if let age = age , age < 20 {
    print("You're a teenager!")
}
```

#### 字符串字串范围

```
var str = "Hello"

str.range(of: "o") // 4..<5
str.range(of: "is") // nil
```

### 隐式可选型

隐式可选性使用`!`来声明，相比于可选型的不同是，这种类型的变量或者常量**可以不使用解包就读取变量或者常量的值而直接使用。** 基本用于类的创建时属性的定义。


```
var errorMessage: String! = nil

errorMessage = "Not Found"

"The message is " + errorMessage
```

> 这种可选型在使用的时候可以不解包，直接使用，这种隐式可选性是不安全的，因为我们不需要解包就可以使用，当可选性的值是nil的时候讲会给程序带来error


```
class City {
    let cityName: String
    unowned var country: Country
    
    init(cityName: String, country: Country) {
        self.cityName = cityName
        self.country = country
    }
}

class Country {
    let countryName: String
    var capitalCity: City! // 隐式可选性
    
    init(countryName: String, capitalCity: String) {
        self.countryName = countryName
        self.capitalCity = City(cityName: capitalCity, country: self)
    }
    
    func showInfo() {
        print("This is \(countryName)")
        print("The capital is \(capitalCity.cityName)")
    }
}


let china = Country(countryName: "中国", capitalCity: "北京")
china.showInfo()
```