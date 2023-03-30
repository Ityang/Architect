## ARouter 原理面试

### 一. ARouter 详细解析

1. [ARouter 源码详解](https://juejin.cn/post/6882553066285957134#heading-9)
2. [探索 ARouter 原理](https://juejin.cn/post/6885932290615509000#heading-26)

### 二. ARouter 面试

ARouter 是一个用于帮助 Android App 进行组件化改造的框架，支持模块间的路由、通信、解耦,支持直接解析标准的 URL进行跳转，并自动注入参数到目标页面中，支持添加多个拦截器，并支持自定义拦截顺序，也支持多种方式配置转场动画。

### 1. 聊一聊 ARouter 的实现原理

根据源码的解析，ARouter 的实现大概分为两大步：

#### 1. 初始化

在 Application 中做 ARouter 的初始化工作，也就是调用 `ARouter.init(application)`，此时会进入`_ARouter.init(application)`方法，然后进入` LogisticsCenter.init(mContext, executor)`方法里面，此时维护了一个 变量为 `routerMap`的 HashSet , ARouter 的跳转是基于路由表 `routerMap`来实现的。获取到的路由信息中包含了在 `com.alibaba.android.arouter.routes` 这个包下自动生成的辅助文件的全路径，通过判断路径名的前缀字符串，就可以知道该类文件对应什么类型，然后通过反射构建不同类型的对象，通过调用对象的方法将路由信息存到 `Warehouse` 的 Map 中。至此，整个初始化流程就结束了

此时产生了两个问题：

##### 1. `routerMap`的更新策略？

1. 如果当前开启了 debug 模式或者通过本地 SP 缓存判断出 app 的版本前后发生了变化，那么就重新获取全局路由信息，否则就还是使用之前缓存到 SP 中的数据
2. 获取全局路由信息是一个比较耗时的操作，所以 ARouter 就通过将全局路由信息缓存到 SP 中来实现复用。但由于在开发阶段开发者可能随时就会添加新的路由表，而每次发布新版本正常来说都是会加大应用的版本号的，所以 ARouter 就只在开启了 debug 模式或者是版本号发生了变化的时候才会重新获取路由信息

##### 2. `routerMap`如何获取到的？

负责`生成路由表`的是 `RouteProcessor` ，负责`加载路由表`的是 `LogisticsCenter` 或 `RegisterTransform` 。ARouter 生成路由表的方式有两种，一种是`运行时反射`，另一种是`编译时插入`。

`运行时反射`就是在 LogisticsCenter 的 init() 方法中，通过 `ClassUtils` 加载 `dex` 文件中的 Class 信息，然后再通过反射初始化这些类，并保存到仓库 `Warehouse` 中。

`Dex` 文件是 Android 平台的可执行文件，类似于 Windows 中的 `exec` 文件，每个 APK 安装包中都有 dex 文件，dex 文件中包含了 app 的`所有源码`，反编译后能看到对应的 java 源码。

`编译时插入`就是由 `RegisterTransform` 从 `Jar` 文件中读取路由表的信息。 RegisterTransform 继承了 `Transform`  类，Transform 是 Android 官方提供的用来修改 class 等资源的 `API` ，每个 Transform 都是一个 `Gradle` 任务，能读取和处理 jar、aar 和 resource 等资源，用户自定义的 Transform 会插在 Transform 队列的最前面。

`编译时插入`需要使用 Gradle 插件来完成，通过 ARouter 提供的注册插件进行路由表的自动加载(power by [AutoRegister](https://github.com/luckybilly/AutoRegister))， 默认通过扫描 dex 的方式 进行加载通过 gradle 插件进行自动注册可以缩短初始化时间解决应用加固导致无法直接访问 dex 文件，初始化失败的问题

#### 2. 逻辑跳转

初始化完成，跳转业务逻辑缕清以后，调用 ARouter 的navigation 方法进行跳转，具体的使用方法为`ARouter.getInstance().build(RoutePath.USER_HOME).navigation()`,如果需要调转的时候带一些参数，可以在此处添加；



当我们调用 ARouter 的 `build()` 方法后，会获取到一个 `Postcard` 对象，我们调用的 navigation() 方法就是 Postcard 的 navigation() 方法，Postcard 的 navigation() 方法会调用到  _ARoute 的 `navigation()` 方法中，在这方法中，首先会处理`预处理服务`，然后会让 `LogisticsCenter` 填充 `Postcard` 中的信息，如果 LogisticsCenter 没有找到对应的路由信息的话，就会走`降级策略`的逻辑，如果 LogisticsCenter 找到对应的路由信息的话，就会判断是不是走`绿色通道`，如果不走绿色通道的话就由`拦截器链`决定要不要跳转。如果走绿色通道的话，就直接按 Fragment 和 Activity 等不同的类型进行跳转，在跳转完成后，如果设置了`跳转回调`， LogisticsCenter 就会调用这个回调。

如果是跳转到 Activity 页面，其实最终调用的还是 `startActivity`方法。
