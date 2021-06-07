# 减少APP体积



1. 清楚无用资源文件工具：[LSUnusedResources](https://github.com/tinymind/LSUnusedResources)
2. 图片无损压缩网站：[TinyPNG](https://tinypng.com/)
3. 使用 Assets & Bitcode
4. 基础配置：target - build setting - optimization level =&gt; Fastest, Smallest \[-Os\]
5. cocoapods 优化

> Q1：增加系统包的引用是否回增加包体积？
>
> Q2：如何获取 IPA 文件？
>
> Q3：Bitcode 做了什么？
>
> Q4：APP 更新会全尺寸下载吗？

Further reading: 

[Reducing Your App’s Size](https://developer.apple.com/documentation/xcode/reducing_your_app_s_size)

[Doing Basic Optimization to Reduce Your App’s Size](https://developer.apple.com/documentation/xcode/reducing_your_app_s_size/doing_basic_optimization_to_reduce_your_app_s_size)

[Doing Advanced Optimization to Further Reduce Your App’s Size](https://developer.apple.com/documentation/xcode/reducing_your_app_s_size/doing_advanced_optimization_to_further_reduce_your_app_s_size)



