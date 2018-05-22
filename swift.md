1. swift常用库

   * Alamofire,SwiftyJSON,Moya,HandyJSON,Kingfisher
   * RxSwift,IQKeyboardManagerSwift,SQLite.swift
   * SnapKit

2. 重载运算符[参考](http://www.cocoachina.com/swift/20150204/11091.html)

   * 为什么要重载：
     * 现有的运算符拥有自己的定制的功能，支持自定义的数据类型，也可以使用心得运算符号。
   * 重载现有运算符
       - 
   * 设置优先级

3. swift访问控制：private fileprivate internal\(缺省\) public open

   * private: 当前类
   * fileprivate: 当前.swift文件
   * internal: 源代码所在的整个整个模块、整个app代码内部、整个框架内部
   * public: 可以被所有人访问，但是不同的module不可以被override/inherit
   * open: 可以被所有人访问，可以被重载和集成。

4. class VS static

   都可用来表示类方法（静态方法），

5. protocol & delegate

   protocol可扩展、集成，  
    协议扩展

6. struct VS Class

   struct: Value type  
    class: reference type



