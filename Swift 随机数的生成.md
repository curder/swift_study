# Swift 随机数的生成

在我们使用 Swift 开发的过程中，时不时地需要产生一些随机数。`arc4random()` 这个全局函数会生成10位数的随机整数（UInt32）。其生成的最大值是 4294967295（2^32 - 1），最小值为 0 。

## 随机整型生成

1，下面是使用 `arc4random()` 函数求一个 1~100 的随机数（包括1和100）

`let temp = Int(arc4random()%100)+1


2，下面是使用 `arc4random_uniform()` 函数求一个 1~100 的随机数（包括1和100）

```
let temp = Int(arc4random_uniform(100))+1
```

## 随机数的一些应用
生成随机背景色
```
let redPart: CGFloat = CGFloat(arc4random()%255) / 255
let greenPart: CGFloat = CGFloat(arc4random()%255) / 255
let bluePart: CGFloat = CGFloat(arc4random()%255) / 255

// 随机 button 背景色
btn.backgroundColor = UIColor(red: redPart, green: greenPart, blue: bluePart, alpha: 1.0)
```