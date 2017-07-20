### UIcollectionview

1. 必须注册
    - xib
    
             registerNib
     
    - 非xib
    
            registerClass
    
2. 

### UICollectionViewflowlayout

### 使用xib创建可复用的UITableViewHeaderFooterView
    
1）记录一个使用UITableViewHeaderFooterView复用的问题：
    
![](/assets/QQ20170718-150828.jpg)

展开 收起多点了几次 发现headerview的复用有问题，与此同时的，展开首次时候的跳动混乱问题，估计也与此有关系。

实践下来发现 只有 'viewForHeaderView'返回的view只能是代码创建的 IB创建的View都会存在诡异的跳动问题。

