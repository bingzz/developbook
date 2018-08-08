## Swift Tips

### guard

> guard:  n. 守卫；警戒；



### static VS class

都标识类方法/属性。
区别在于，class修饰的方法可以被子类重载

``` swift
class SomeClass {

    class let p1
    static let p2

    class func f1(){}
    static func f2(){}

}

```




