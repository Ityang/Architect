## Lifecycle原理面试

### 一. 详细解析

1. [官网地址](https://developer.android.google.cn/jetpack/androidx/releases/lifecycle#2.5.1)
2. [官网使用简介](https://developer.android.google.cn/topic/libraries/architecture/lifecycle)
3. [Android业务架构 · 基础篇 · Jetpack四件套](https://juejin.cn/post/7207263350487924773#heading-15)
4. [Android Jetpack组件Lifecycle基本使用和原理分析](https://juejin.cn/post/6865568507698872328#heading-7)

### 二. 面试问题

LifecycleRegistry类是对Lifecycle这个抽象类的具体实现，可以处理多个观察者，如果你自定义 LifecycleOwner可以直接使用它。

* LifecycleOwner：可获取Lifecycle的接口，可以再 Activity、Fragment生命周期改变时，通过LifecycleRegistry类处理对应的生命周期事件，并通知 LifecycleObserver这个观察者
* Lifecycle：是被观察者，用于存储有关组件（如 Activity 或 Fragment）的生命周期状态的信息，并允许其他对象观察此状态。
* LifecycleObserver：观察者，可以通过被**LifecycleRegistry**类通过 addObserver(LifecycleObserver o)方法注册,被注册后，LifecycleObserver便可以观察到LifecycleOwner对应的**生命周期事件**
* Lifecycle.Event：分派的生命周期事件。这些事件映射到 Activity 和 Fragment 中的回调事件。
* Lifecycle.State：Lifecycle组件的当前状态。



ReportFragment



#### 1.Lifecycle是怎样感知生命周期的？

就是在ReportFragment中的各个生命周期都调用了`dispatch(Lifecycle.Event event)` 方法，传递了不同的**Event**的值

#### 2.Lifecycle是如何处理生命周期的？

通过调用了`((LifecycleRegistry) lifecycle).handleLifecycleEvent(event);`方法，也就是LifecycleRegistry 类来处理这些生命周期。

#### 3.LifecycleObserver的方法是怎么回调是的呢？

LifecycleRegistry 的 handleLifecycleEvent方法，然后会通过层层调用最后通过反射到LifecycleObserver方法上的@OnLifecycleEvent(Lifecycle.Event.XXX)注解值，来调用对应的方法

#### 4.为什么LifecycleObserver可以感知到Activity的生命周期

LifecycleRegistry调用handleLifecycleEvent方法时会传递Event类型，然后会通过层层调用，最后是通过反射获取注解的值，到LifecycleObserver方法上的@OnLifecycleEvent(Lifecycle.Event.XXX)注解上对应的Event的值，注意这个值是和Activity/Fragment的生命周期的一一对应的，所以就可以感知Activity、Fragment的生命周期了。



### 三. LifeCycle注意事项

##### 1.  不要在 onCreate() 方法中使用 Lifecycle 组件

Lifecycle 组件在 onCreate() 方法中尚未初始化完成，因此在该方法中使用它们可能会导致崩溃或不可预测的行为。建议在 onStart() 方法中使用 Lifecycle 组件。

##### 2.  不要手动调用 onDestroy() 方法

手动调用 onDestroy() 方法会破坏 Lifecycle 组件的生命周期，从而导致应用程序行为异常。Lifecycle 组件应该由系统自动管理，应该避免手动干预。

##### 3.  避免在 Fragment 中使用多个 LifecycleOwner

Fragment 自身就是一个 LifecycleOwner，因此不应该在 Fragment 中创建其他的 LifecycleOwner。这样会导致多个 LifecycleOwner 之间的状态不同步，从而导致应用程序出现问题。

