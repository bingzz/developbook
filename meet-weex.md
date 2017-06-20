
关于weex

1. 环境搭建
2. 记一次weex项目开发：

   weex init
   npm install
   npm run build
   npm run dev 可以开启实时调试
   
3. vue实例 相当于一个viewmodel。 可以参详一下mvvm设计模式概念。

4. 一个小坑：
   weex init的一个项目，在npm run dev在web端进行调试的时候，发现没有内容，发现是官方给的demo，在html的js资源引用路径有错。