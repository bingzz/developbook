## KVO (Key-Value Observing)

- 基于KVC，允许一个对象监听另一个对象的某个属性。

`-(void)addObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath options:(NSKeyValueObservingOptions)options context:(nullable void *)context;`
