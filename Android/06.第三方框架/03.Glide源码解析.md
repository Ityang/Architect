## Glide 源码解析

参考： 

1. [面试官：简历上最好不要写Glide，不是问源码那么简单](https://juejin.cn/post/6844903986412126216)
1. [Glide生命周期绑定](https://blog.csdn.net/xiaopangcame/article/details/118913626)
1. [聊一聊关于Glide在面试中的那些事](https://juejin.cn/post/6844904002551808013)
1. [Android源码分析：手把手带你深入了解Glide的缓存机制](https://juejin.cn/post/6844903583280791559)



### 1. Glide 如何管理生命周期

1. Glide 在操作 **Glide.with(Context)** 的时候加载了一个 `SupportRequestManagerFragment`,这个特殊的Fragment 与 Activity 相关联  。通过Lifecycle在Fragment关键生命周期通知RequestManger进行相关的操作。
2. 在生命周期onStart时继续加载，onStop时暂停加载，onDestory是停止加载任务和清除操作。


### 2. Glide 三级缓存

 `Lru算法缓存`与`弱引用缓存`是互斥的，如果在`Lru算法缓存`中，则不会在`弱引用缓存`中出现。

读取一张图片的时候，获取顺序： Lru算法缓存-》弱引用缓存-》磁盘缓存（如果设置了的话）

> 当我们的APP中想要加载某张图片时，先去LruCache中寻找图片，如果LruCache中有，则直接取出来使用，并将该图片放入WeakReference中，如果LruCache中没有，则去WeakReference中寻找，如果WeakReference中有，则从WeakReference中取出图片使用，如果WeakReference中也没有图片，则从磁盘缓存/网络中加载图片。

注：图片正在使用时存在于 activeResources 弱引用map中

将图片缓存的时候，写入顺序： 弱引用缓存-》Lru算法缓存-》磁盘缓存中

> 当图片不存在的时候，先从网络下载图片，然后将图片存入弱引用中，glide会采用一个acquired（int）变量用来记录图片被引用的次数， 当acquired变量大于0的时候，说明图片正在使用中，也就是将图片放到弱引用缓存当中； 如果acquired变量等于0了，说明图片已经不再被使用了，那么此时会调用方法来释放资源，首先会将缓存图片从弱引用中移除，然后再将它put到LruResourceCache当中。 这样也就实现了正在使用中的图片使用弱引用来进行缓存，不在使用中的图片使用LruCache来进行缓存的功能。

引深： 关于LruCache

> 最近最少使用算法，设定一个缓存大小，当缓存达到这个大小之后，会将最老的数据移除，避免图片占用内存过大导致OOM。 LruCache 内部用LinkHashMap存取数据，在双向链表保证数据新旧顺序的前提下，设置一个最大内存，往里面put数据的时候，当数据达到最大内存的时候，将最老的数据移除掉，保证内存不超过设定的最大值。

关于LinkedHashMap

> LinkHashMap 继承HashMap，在 HashMap的基础上，新增了双向链表结构，每次访问数据的时候，会更新被访问的数据的链表指针，具体就是先在链表中删除该节点，然后添加到链表头header之前，这样就保证了链表头header节点之前的数据都是最近访问的（从链表中删除并不是真的删除数据，只是移动链表指针，数据本身在map中的位置是不变的）







