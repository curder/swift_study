## Swift 数组之初始化

在 Swift 语言中，**存储的单元数据结构必须一致**，比如一个数组要么都存储字符串，要么都存储整型，不能既有整型又有字符串。**而且数组是有序的。**


### 数组的声明

#### 一般数组
```
var numbers = [1,2,3,4,5] // 整型数组 也可以显式的声明数据类型 var numbers: Array<Int> = [1,2,3,4,5]
var vowels = ["A","B","C","D","E"] // 字符串数组 也可以显式的声明数据类型 var vowels: [String] = ["A","B","C","D","E"]
```

#### 空数组
```
var emptuArray1: [Int] = [] // 空整型数组
var emptyArray2: Array<String> = [] // 空字符串数组
var emptyArray3 = [Double]() // 空浮点数数组
var emptyArray4 = Array<Float>() // 空浮点数数组
```

#### 创建具有默认值的数组
```
var allzeros = [Int]( repeating: 0 , count: 5 ) // [0, 0, 0, 0, 0]

var allZeros = Array<Int>(repeating: 0 , count: 5) // [0, 0, 0, 0, 0]
```

### Swift 数组之二维数组
数组中元素还是数组。例如：
```
var board = [ [1024,16,2,0] ,[256,4,2,0] ,[64,2,0,0] ,[2,0,0,0] ];
```

### Swift 数组之 NSArray

`NSArray` 数组的创建方式如下：
```
var array2 = [1,2,3,4,5] as NSArray

var array3: NSArray = [1,"2",3.1,4,5]
```

### 数组是值类型
```
var arr1 = [1,2,3]
var arr2 = arr1
arr2[1] = 3

print(arr1) // [1, 2, 3]
print(arr2) // [1, 3, 3]
```















