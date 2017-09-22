
关于weex

1. 环境搭建
2. 记一次weex项目开发：

   weex init
   npm install
   npm run build
   npm run dev 可以开启实时调试
   
   weex platform add ios/android
   
3. vue实例 相当于一个viewmodel。 可以参详一下mvvm设计模式概念。

4. 一个小坑：
   weex init的一个项目，在npm run dev在web端进行调试的时候，发现没有内容，发现是官方给的demo，在html的js资源引用路径有错。

5. weex POST请求，需要设置content-Type才能拿到数据 /冷汗

   设置请求头：
   
       headers:{   
            "Content-Type": 'application/x-www-form-urlencoded',              
         }

   
6. 一个小坑
   
         weex create
         weex platform add ios/android
         写完页面后 web和iOS都显示正常，唯独android上显示 “render error:-2013”//各种查没有结果。。。
         
   后来把页面的.vue文件 放到了 
      