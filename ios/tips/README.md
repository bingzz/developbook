# Tips

## swift中使用markdown

```text
editor-show rendered markup
代码中/\*:  ...  \*/
```

## webview中链接编码问题

* javascript encodeURI\(\) &lt;=&gt; stringByAddingPercent 用于整个uri的编码

  ```text
    http://www.oschina.net/search?scope=bbs&q=C语言
    ->http://www.oschina.net/search?scope=bbs&q=C%E8%AF%AD%E8%A8%80 
  ```

* js: encodeURIComponents\(\) 用于参数拼接url编码

  ```text
   http://www.oschina.net/search?scope=bbs&q=C语言
   ->http%3A%2F%2Fwww.oschina.net%2Fsearch%3Fscope%3Dbbs%26q%3DC%E8%AF%AD%E8%A8%80
  ```

以上两种编码 都可以通过`stringByRemovingPercentEncoding`解码

## app中使用safari -- SFSafariViewControllerDelegate

## 集成pod时候遇见的错误 以及解决方案

…………………………………… other linker flags

-ObjC标志，它的作用就是将静态库中所有的和对象相关的文件都加载进来。 -all\_load:本来这样就可以解决问题了，不过在64位的Mac系统或者iOS系统下，链接器有一个bug，会导致只包含有类别的静态库无法使用-ObjC标志来加载文件。变通方法是使用-all\_load 或者-force\_load标志，它们的作用都是加载静态库中所有文件，不过all\_load作用于所有的库，而-force\_load后面必须要指定具体的文件。

libPods-WeekendHotels.a文件显示为红色miss状态 编译报错：找不到library not found for -lPods-WeekendHotels

能在32位机型上运行，64位处理器的机型上，编译报错，

Showing Recent Issues Target 'Pods-WeekendHotels' of project 'Pods' was rejected as an implicit dependency for 'Pods\_WeekendHotels.framework' because its architectures 'x86\_64' didn't contain all required architectures 'i386 x86\_64'

会报未找到其他文件的错误，ld: framework not found INTULocationManager 被误导 ~

最终找到的解决方案： build setting - architectures - build active architecture only属性， 这个属性设置为yes，是为了debug的时候编译速度更快，它只编译当前的architecture版本。 而设置为no时，会编译所有的版本。

编译出的版本是向下兼容的，比如你设置此值为yes，用iphone4编译出来的是armv7版本的，iphone5也可以运行，但是armv6的设备就不能运行。 所以，一般debug的时候可以选择设置为yes，release的时候要改为no，以适应不同设备。

这也解决了当年，疑惑为什么选择不同设备进行archieve的时候，得到的包大小不一样。……

………………………………………………

引入.framework 和 .a/.bundle文件（静态文件），需要手动设置 Build Setting - search paths - Framework Search Paths & Library Search Paths设置相应的文件编译路径

