# 组件化之路

### 前言

> 了解weex的同学可能清楚，weex的开发都是组件化的，标签、cell、view、network、animation等，都是以组件的形式进行封装。那么组件化有什么好处？为什么说组件化是业务不断迭代，规模壮大后的首选之路？如何实现一个高效、拓展性强、定制化的组件化架构呢？

文末有几篇大厂或者大神讨论关于组件化的文章。

### 为什么要组件化？

功能、业务不断迭代，开发人员不断增多，项目的可拓展性已经到了捉襟见肘的状态。为了提高项目的开发效率，保证团队的交付能力，解除非相关业务之间代码的耦合，这个时候项目的架构设计就需要提上日程。

> 将每个模块作为一个组件。并且建立一个主项目，这个主项目负责集成所有组件。这样带来的好处是很多的：
>
> * 业务划分更佳清晰，新人接手更佳容易，可以按组件分配开发任务。
> * 项目可维护性更强，提高开发效率。
> * 更好排查问题，某个组件出现问题，直接对组件进行处理。
> * 开发测试过程中，可以只编译自己那部分代码，不需要编译整个项目代码。
> * 方便集成，项目需要哪个模块直接通过CocoaPods集成即可。

### 中间件

三个轮子： [MGJRouter](https://github.com/meili/MGJRouter) [HHRouter](https://github.com/Huohua/HHRouter) [**JLRoutes**](https://github.com/joeldev/JLRoutes)

### 组件划分

**SOLID原则**

* 单一责任原则：对象功能单一
* 开放封闭原则：扩展是开放的，修改是封闭的
* 里氏替换原则：子类对象是可以替代基类对象的
* 接口分离原则：接口的用途要单一，不要在一个接口上根据不同入参实现多个功能
* 依赖倒置原则：方法应该依赖抽象，不要依赖实例。iOS 开发就是高层业务方法依赖于协议

**上层：业务组件** iOS 组件，应该是包含 UI 控件、相关多个小功能的合集，是一种粒度适中的模块。

**中层：核心组件** 在项目中存在很多公共部分的东西，例如封装的网络请求、缓存、数据处理等功能，以及项目中所用到的资源文件。蘑菇街将这些部分也当做组件，划分为基础组件，位于业务组件下层。所有业务组件都使用同一套基础组件，也可以保证公共部分的统一性。

**下层：基础组件** 比如相互独立，没有依赖关系的三方库等

**依赖关系是由上到下的，同层之间的组件需要借助`中间件`进行解耦，**项目实施中，需要将公用部分由上层下沉到核心组件层。

### 具体实施

组件化架构中，需要一个主工程，主工程负责集成所有组件。每个组件都是一个单独的工程，创建不同的git私有仓库来管理，每个组件都有对应的开发人员负责开发。开发人员只需要关注与其相关组件的代码，不用考虑其他组件，这样来新人也好上手。 组件的划分需要注意组件粒度，粒度根据业务可大可小。组件划分可以将每个业务模块都划分为组件，对于网络、数据库等基础模块，也应该划分到组件中。项目中会用到很多资源文件、配置文件等，也应该划分到对应的组件中，避免重复的资源文件。项目实现完全的组件化。 每个组件都需要对外提供调用，在对外公开的类或组件内部，注册对应的URL。组件处理中间件调用的代码应该对其他代码无侵入，只负责对传递过来的数据进行解析和组件内调用的功能。

这里有比较详细的步骤介绍，基于`CTMediator`，工程配置相关的，作者都写好了脚本。 [在现有工程中实施基于CTMediator的组件化方案](https://casatwy.com/modulization_in_action.html)

### 参考

[项目大了人员多了，架构怎么设计更合理？](https://time.geekbang.org/column/article/86522) 

[蘑菇街 App 的组件化之路](https://limboy.me/tech/2016/03/10/mgj-components.html) 

[蘑菇街 App 的组件化之路\(续\)](https://limboy.me/tech/2016/03/14/mgj-components-continued.html) 

[bang神的iOS 组件化方案探索](http://blog.cnbang.net/tech/3080/)

 [iOS应用架构谈 组件化方案](https://casatwy.com/iOS-Modulization.html) 

[百度App组件化之路](https://mp.weixin.qq.com/s/P-vREnrw4xGyhiugpzB-1Q) 

[爱奇艺视频组件化之路](https://mp.weixin.qq.com/s?__biz=MzI0MjczMjM2NA%3D%3D&mid=2247483846&idx=1&sn=be10e857dc4415d6adb4edbe5bf04152) 

[有赞移动 iOS 组件化（模块化）架构设计实践](https://mp.weixin.qq.com/s/V2odpnJudpBXTtG0R3jpuw)

 [蘑菇街、滴滴、淘宝、微信的组件化架构解析](https://mp.weixin.qq.com/s?src=11&timestamp=1563330240&ver=1733&signature=9MiDvs0*8Bspsas3EAKcLTZJrN08g1CyNpCQcoWZaMRakq35QTIibCsmzCN6ICQrK6JmkpFW3QuRC9HAZmSYqCheemroShRuDlgGMqk-eB1vQUKV060tihMbttmZlBFT&new=1) 

[手机淘宝客户端架构探索实践](https://yq.aliyun.com/articles/129)

