## UIButton 简单的交互控件

**UIButton 控件**是最基础的显示控件。故名思意，UIButton 控件 是一个按钮，提供给用户让用户与我们设计的App进行交互。
其中给 button 添加事件的方法是 `addTarget`，如下文例举中的 `button.addTarget(self, action: #selector(self.click), for: UIControlEvents.touchUpInside)`
```
override func viewDidLoad() {
        super.viewDidLoad()
        
        // 创建一个 button
//        let button: UIButton = UIButton(frame: CGRect( )
        
        // 创建并设置 button 类型
        let button: UIButton = UIButton(type: UIButtonType.system) // 自定义类型 contactAdd、infoDark、infoLight、detailDisclosure、system、custom

        // 设置 button 的位置
        button.frame = CGRect(x: 100, y: 100, width: 100, height: 100)

        // button 颜色
        button.backgroundColor = UIColor.green

        // button 添加事件 - 用于用户交互
        button.addTarget(self, action: #selector(self.click), for: UIControlEvents.touchUpInside)
        
        // button 标题
        button.setTitle("按钮", for: UIControlState.normal)
        
        // 设置 button 内容区域的偏移量
        button.contentEdgeInsets = UIEdgeInsetsMake(-10, -10, 0, 0)
        
        // 设置按钮图片
//        button.setBackgroundImage(UIImage(named:"0.png"), for: UIControlState.normal) // 设置背景图片
        button.setImage(UIImage(named:"0.png"), for: UIControlState.normal) // 设置图片
        
        // 开启点击效果
        button.showsTouchWhenHighlighted = true
        
        // 设置高亮状态按钮标题
        button.setTitle("新标题", for: UIControlState.highlighted)
        
        self.view.addSubview(button)
    }

    // 点击后的处理函数
    func click (button: UIButton){
        print("按钮被点击！")
        
        let redPart: CGFloat = CGFloat(arc4random()%255) / 255
        let greenPart: CGFloat = CGFloat(arc4random()%255) / 255
        let bluePart: CGFloat = CGFloat(arc4random()%255) / 255
        
        // 随机 button 背景色
        button.backgroundColor = UIColor(red: redPart, green: greenPart, blue: bluePart, alpha: 1.0)
    }
```