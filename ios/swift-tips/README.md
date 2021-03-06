# Swift Tips

## guard

> guard: n. 守卫；警戒；

## static VS class

都标识类方法/属性。 区别在于，class修饰的方法可以被子类重载

```swift
    class SomeClass {

        class let p1
        static let p2

        class func f1(){}
        static func f2(){}

    }
```

## `computed properties` VS `stored properties`

> 计算属性: they provide a getter and an optional setter to retrieve and set other properties and values indirectly 不支持lazy修饰 每次引用，都会执行getter方法 =&gt; 一般只是放一些计算操作，不能进行对象的创建。

```swift
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
```

> 存储属性: var/lazy var,let\(constant\) ... 可以直接设置默认值  
> 也可以使用闭包进行初始化，闭包内初始化函数只会走一次

```swift
    var v: UIView = UIView()
    var v: UIView = { 
        retrn UIView()
    }()
```

