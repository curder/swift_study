## Swift 运算符之范围运算符for-in

### 区间运算符

| 运算符 |举例 | 作用 |
| --- | --- | --- |
| `a...b` | 相当于数学里的`[a,b]` | 闭合区间 |
| `a..<b` | 相当于数学里的`[a,b)` | 前闭后开区间 |

### 闭合区间
```
for index in 1...10 {
    index
}
```

### 前闭后开区间
```
for index in 0 ..< 10 {
    index
}

//
let arr = [1,2,3,4,5,6,7,10]
for i in 0..<arr.count {
    print(arr[i])
}
```

### 对数组遍历
```
var array: Array<String> = ["A","V","X","S"]
for arr in array{
    arr
}

// 遍历数组的键值
for (key,value) in array.enumerated(){
    key
    value
}
```

### 遍历字典
```
var dict: Dictionary<Int,String> = [1:"A",2:"B",3:"d"]
// 遍历字典的key
for key in dict.keys{
    key
}

// 遍历字典的value
for value in dict.values{
   value
}
```

### 遍历集合
```
var set: Set<String> = ["A","V","X","S"]
for value in set{
    value
}
```