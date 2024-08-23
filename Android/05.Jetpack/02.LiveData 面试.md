## LiveData 面试

### 一. 详细解释：

1. [Android业务架构 · 基础篇 · Jetpack四件套](https://juejin.cn/post/7207263350487924773#heading-19)
2. [Android Jetpack组件LiveData基本使用和原理分析](https://juejin.cn/post/6867036161446969352#heading-2)
3. [Activity销毁重建导致LiveData数据倒灌](https://juejin.cn/post/6986895858239275015)



### 二. 面试回答

Android Jetpack LiveData是一种用于管理应用程序界面和数据交互的组件。

LiveData是一种可观察的数据持有者，用于在应用程序组件（如Activity、Fragment和Service）之间共享数据，并在数据发生更改时通知观察者。

#### 1.  LiveData 数据重放原因分析

根据`LiveData`的设计原则：

> **在页面重建时，LiveData自动推送最后一次数据，而不必重新去向后台请求。**

`LiveData`自动推送最后一次数据条件是页面重建，也就是`Activity`生命周期经过了销毁到重建，那什么情况下会发生`Activity`重建？ 常见的操作是：

- 屏幕旋转
- 用户手动切换系统语言

Activity异常销毁然后重建，ViewModel会保存销毁之前的数据，然后在Activity重建完成后进行数据恢复，所以LiveData成员变量中的`mVersion`会恢复到重建之前的值。

但是Activity重建后会调用LiveData的`observe()`方法，方法内部会重新new一个实例，会将`mLastVersion`恢复到初始值。

由于LiveData本身的特性，Activity的生命周期由非活跃变成活跃时，LiveData会触发事件分发，导致屏幕旋转或者切换系统语言后出现数据倒灌。

但是这里有一点要非常注意：系统内存不足，杀到应用后台，也会导致Activity重建，但是不会LiveData导致数据倒灌。

问题找到了，那如何防止数据倒灌呢？



LiveData 的数据重放问题也叫作数据倒灌、粘性事件，核心源码在 LiveData#considerNotify(Observer) 中：

- 首先，LiveData 和观察者各自会持有一个版本号 version，每次 LiveData#setValue 或 postValue 后，LiveData 持有的版本号会自增 1。在 LiveData#considerNotify(Observer) 尝试分发数据时，会判断观察者持有版本号是否小于 LiveData 的版本号（Observer#mLastVersion >= LiveData#mVersion 是否成立），如果成立则说明这个观察者还没有消费最新的数据版本。
- 而观察者的持有的初始版本号是 -1，因此当注册新观察者并且正好宿主的生命周期是大于等于可见状态（STARTED）时，就会尝试分发数据，这就是数据重放。

为什么 Google 要把 LiveData 设计为粘性呢？LiveData 重放问题需要区分场景来看 —— 状态适合重放，而事件不适合重放：

- 当 LiveData 作为一个状态使用时，在注册新观察者时重放已有状态是合理的；
- 当 LiveData 作为一个事件使用时，在注册新观察者时重放已经分发过的事件就是不合理的。

#### 2. LiveData为啥连续postValue两次，第一次的值会丢失？

https://juejin.cn/post/7056431977523740709