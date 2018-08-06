## 关于MVVM

![](/assets/1497256097808253.png)

1. view 引用viewModel ，但反过来不行；

2. viewModel 引用model，但反过来不行；

3. view 和 view controller 都不能直接引用model，而是引用视图模型（viewModel）；

4. viewModel 是一个放置用户输入验证逻辑，视图显示逻辑，发起网络请求和其他代码的地方；

5. viewController 只是一个中间人，接收 view 的事件、调用 viewModel 的方法、响应 viewModel 的变化；

6. 


