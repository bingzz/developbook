### receipt拿到后台验证时候，出现in_app数组为空的情况

> 原因1: 用户支付成功后，appsore没有更新用户支付收据；
> 原因2: 伪造的receipt

解决方法：
调用`[[SKReceiptRefreshRequest alloc] initWithReceiptProperties:@{}]`，刷新用户购买凭证。如果刷新成功，`[NSBundle mainBundle].appStoreReceiptURL`存放的收据为最新，包含用户在该app的所以内购收据。

几个疑问：
1. 沙盒中存放的是一个文件，包含多个收据信息？
2. 对于未验证的data

###  防丢单处理
