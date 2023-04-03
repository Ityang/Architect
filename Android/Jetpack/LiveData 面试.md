## LiveData 面试

### 一. 详细解释：

1. [Android业务架构 · 基础篇 · Jetpack四件套](https://juejin.cn/post/7207263350487924773#heading-19)
2. [Android Jetpack组件LiveData基本使用和原理分析](https://juejin.cn/post/6867036161446969352#heading-2)



### 二. 面试回答

Android Jetpack LiveData是一种用于管理应用程序界面和数据交互的组件。

LiveData是一种可观察的数据持有者，用于在应用程序组件（如Activity、Fragment和Service）之间共享数据，并在数据发生更改时通知观察者。

LiveData可以确保UI与数据的同步更新，避免了一些常见的错误，如内存泄漏和UI组件无法正确更新的问题。

LiveData具有生命周期感知功能，可以自动感知应用程序组件的生命周期，并在组件处于活动状态时更新UI，而在组件处于非活动状态时停止更新，从而有效地减少了资源消耗。

LiveData还提供了线程安全的访问数据的机制，避免了 题



livedata数据倒灌，

粘性事件