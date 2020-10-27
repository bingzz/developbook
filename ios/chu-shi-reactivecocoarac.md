# 初识ReactiveCocoa\(RAC\)

### 简单介绍下RAC

> * FRP思想：函数响应式编程思想（Function Reactive Programming）。[看下这篇文章](https://blog.csdn.net/fly1183989782/article/details/62053973)介绍
> * 我门之前熟知的布局Masonry就是一个很好函数式编程的代表作。其代码风格：
>
>   ```text
>   make.edges.equalTo(superview).with.insets(padding);
>   ```
>
>   ![image.png](https://upload-images.jianshu.io/upload_images/967427-f75d8f8dc469fef0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>
> * 在开发过程中，例如按钮点击事件、上下拉刷新控件的状态变化监听、网络请求等，经常需要我们响应这些事件来处理一些事物逻辑，通过selector、block、delegate、KVO等方式。这些场景通过使用RAC方式处理，可以很清晰、有逻辑、高集中地将代码和业务逻辑关联。

### 用法举例

**按钮点击事件替代**

```text
    // 传统方式
    [button addTarget:self action:@selector(click:) forControlEvents:UIControlEventTouchUpInside];
    // RAC方式
    RACSignal *clickSignal = [button rac_signalForControlEvents:UIControlEventTouchUpInside];
    [clickSignal subscribeNext:^(__kindof UIControl * _Nullable x) {
    }];
```

**KVO事件替代**

比如写一个跟随键盘弹出的输入框为例。 传统写法是这样的：

```text
- (void)setup {
   [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillHide:) name:UIKeyboardWillHideNotification object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillShow:) name:UIKeyboardWillShowNotification object:nil];
}
```

```text
- (void)keyboardWillHide:(NSNotification *)notification {
}
- (void)keyboardWillShow:(NSNotification *)notification {
}
```

```text
- (void)dealloc {
    [[NSNotificationCenter defaultCenter] removeObserver:self name:UIKeyboardWillShowNotification object:nil];
    [[NSNotificationCenter defaultCenter] removeObserver:self name:UIKeyboardWillHideNotification object:nil];
}
```

RAC写法是这样：

```text
    RACSignal *willShowSignal = [[NSNotificationCenter defaultCenter] rac_addObserverForName:UIKeyboardWillShowNotification object:nil];
    [[willShowSignal deliverOn:[RACScheduler mainThreadScheduler]] subscribeNext:^(id  _Nullable x) {
       //keyboardWillShow:
    }];

    RACSignal *willHideSignal = [[NSNotificationCenter defaultCenter] rac_addObserverForName:UIKeyboardWillHideNotification object:nil];
    [[willHideSignal deliverOn:[RACScheduler mainThreadScheduler]] subscribeNext:^(id  _Nullable x) {
       //keyboardWillHide
    }];
```

**textView文字输入**

之前要通过代理/通知的方式才能实现监听文字的变化。 RAC的方式是这样：

```text
    WeakObj(self)
    [[_subTitle rac_textSignal] subscribeNext:^(id x) {
        if (selfWeak.textFiledInputBlock) {
            selfWeak.textFiledInputBlock(x,nil);
        }
    }];
```

此外，RAC对于控件的支持广泛且完善，以及这种易读、高聚合、低耦合的代码风格，方便我们集中管理业务相关的代码。 ![RAC&#x5BF9;&#x5E38;&#x7528;&#x63A7;&#x4EF6;&#x7684;&#x652F;&#x6301;](https://upload-images.jianshu.io/upload_images/967427-e57b0e19154d3e9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### RACSignal

> 看到[AKyS佐毅 ](https://www.jianshu.com/p/827f95568cea)同学的文章里面有这样一个比喻特别好： Signal信号是数据流，可以被绑定和传递。可以把信号想象成水龙头，只不过里 面不是水，而是玻璃球\(value\)，直径跟水管的内径一样，这样就能保证玻璃球是依次排列，不会出现并排的情况\(数据都是线性处理的，不会出现并发情况\)。水龙头的开关默认是关的，除非有了接收方\(subscriber\)，才会打开。 这样只要有新的玻璃球进来，就会自动传送给接收方。接收方就是放在水龙头下的盆子，对于水龙头不同的出水状况，有自己的处理方式。水龙头出水时会通知下方的水盆，如果没有水盆，水龙头始终处于关闭状态。 信号Signal是ReactiveCocoa核心部件之一。有信号选择器内置UIKit组件。例如，有一个rac\_textsignal UITextField的值与在文本字段中的每一个新的按键发送。我们将在下一节中看到如何使用信号来执行工作。 信号也可以被链接和转化。当我们映射或过滤一个流时，我们创建一个新的流。随后这流被映射，过滤，随意摆弄都没问题。

### RACScheduler

> 任务队列调度器，保证所有任务的有序执行，是基于GCD的封装。

RACScheduler ｜-- RACImmediateScheduler：立刻执行，唯一一个支持同步执行 ｜-- RACQueueScheduler - RACTargetQueueScheduler：在一个以一个任意的 GCD 队列为 target 的串行队列中异步调度所有任务 ｜-- RACSubscriptionScheduler：用来调度订阅

### RACSequence

> 和RACSignal一样，RACSequence也继承自`RACStream`。

### 处理逻辑 · 数据内容

**订阅** 处理逻辑指的是创建信号的时候，它是如何通知订阅者（Subscriber）并选择发送何种事件的。 数据内容指的是信号会传递给订阅者（Subscriber）什么样的数据。这就像一个水龙头，它什么时候告诉水盆自己正在滴水，或是已经滴完水了。以及它把什么丢入水盆，是原始的水滴，还是水滴的质量？ 如果我们需要对这些内容进行自定义的修改，那么修改原信号显然是不可行的（信号已经被创建了）。因此，这就牵涉到信号之间的转换（Map）与组合（Combine）。 我们从RACSignal类最基础的方法开始讨论信号之间的转换（Map）与组合（Combine）。对于每一个方法，我们需要关注这个方法的功能、返回值类型。由于ReactiveCocoa大量使用了block，还需要关注方法中block的参数类型和返回值类型。 订阅是在数据流上进行的，通常是信号，当你想被通知时，一个新的值被发送（或下一个错误，或完成）。信号经常被用来产生一些其他的作用，例如下面的例子。 让我们添加一个文本字段的视图控制器的视图，并将它连接到一个IBOutlet。我要使用内置的脚本和助理编辑这样做，但你做任何你喜欢做的事。

### 导出状态

导出状态中的另一个核心部件ReactiveCocoa。我们可以将属性抽象成一个流，而不是在一个类中有一个属性设置为新的值。让我们以前一个例子，并用派生的状态来扩充它。 让我们假装我们的观点是一种形式为创建一些帐户，我们只希望允许电子邮件地址，包含一个' @ '符号。当，只有当一个有效的用户名已输入，然后一个按钮的启用状态将是肯定的。我们还想提供反馈给用户的文本颜色的文本字段的方式。 让我们首先添加按钮，创建一个IBOutlet。

### 参考

[唐巧整理的关于RAC响应式编程的讨论](http://blog.devtang.com/2016/01/03/reactive-cocoa-discussion/) [ReactiveCococa官方主页](http://reactivecocoa.io) [ReactiveObjC README](https://github.com/ReactiveCocoa/ReactiveObjC/#readme)

