# 重新认识 Runloop

### 概念：

1. NSRunLoop 是对 CFRunLoopRef 的封装，后者是C语言的，前者提供了面向对象的API，但是这些API不是线程安全的；
2. CFRunLoop 是基于pthread来管理的，线程和RunLoop是一一对应的，RunLoop 的创建是发生在第一次获取时，RunLoop 的销毁是发生在线程结束时。
3. Runloop 有5个类，`CFRunLoopRef` `CFRunLoopModeRef` `CFRunLoopSourceRef` `CFRunLoopTimerRef` `CFRunLoopObserverRef`，他们之间的关系是：一个runloop中可以包含若干个mode，一个mode包含若干个 source｜timer｜observer，每次调用runloop主函数时候，只能制定其中一个mode，如果要切换mode，则需要结束当前runloop，指定新的mode重新进入；

   \`\`\`

   struct \_\_CFRunLoopMode {

    CFStringRef \_name;            // Mode Name, 例如 @"kCFRunLoopDefaultMode"

    CFMutableSetRef \_sources0;    // Set

    CFMutableSetRef \_sources1;    // Set

    CFMutableArrayRef \_observers; // Array

    CFMutableArrayRef \_timers;    // Array

    ...

   };

struct \_\_CFRunLoop { CFMutableSetRef \_commonModes; // Set CFMutableSetRef \_commonModeItems; // Set&lt;Source/Observer/Timer&gt; CFRunLoopModeRef \_currentMode; // Current Runloop Mode CFMutableSetRef \_modes; // Set ... };

\`\`\`

1. 每当 RunLoop 的内容发生变化时，RunLoop 都会自动将 \_commonModeItems 里的 Source/Observer/Timer 同步到具有 “Common” 标记的所有Mode里。
2. 主线程会预置两个mode，`NSDefaultRunLoopMode` 和 `UITrackingRunLoopMode`，
3. 
### 场景一：ScrollView 上timer问题

> 原因：主线程的 RunLoop 里有两个预置的 Mode：kCFRunLoopDefaultMode 和 UITrackingRunLoopMode。这两个 Mode 都已经被标记为”Common”属性。DefaultMode 是 App 平时所处的状态，TrackingRunLoopMode 是追踪 ScrollView 滑动时的状态。当你创建一个 Timer 并加到 DefaultMode 时，Timer 会得到重复回调，但此时滑动一个TableView时，RunLoop 会将 mode 切换为 TrackingRunLoopMode，这时 Timer 就不会被回调，并且也不会影响到滑动操作。

解决方案： 分别向两个mode分别添加timer，也可以向改runloop的commonModes中添加timer，会自动同步到所有具有common标识的mode。

### RunLoop 的内部逻辑

