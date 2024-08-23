### 1. 为什么会出现ANR？以及解决方案

[今日头条分析以及解决方案](https://blog.csdn.net/ByteDanceTech/article/details/116810918?ops_request_misc=&request_id=&biz_id=102&utm_term=%E4%BB%8A%E6%97%A5%E5%A4%B4%E6%9D%A1%20ANR%20&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-116810918.first_rank_v2_pc_rank_v29&spm=1018.2226.3001.4187)

[Android 解决 SharedPreferences 导致的ANR问题](https://blog.csdn.net/zhuoxiuwu/article/details/122577485)

`SharedPreference`可能在以下几种情况下产生卡顿，从而引起ANR：

- 创建`SharedPreference`时，调用`getPreferenceDir`，可能存在创建目录的行为
- `getBoolean`等方法，会等待直到`SharedPreference`将文件中的键值对全部读取到缓存里，才会返回
- `commit`方法直接同步写，如果不小心在主线程调用，会引起卡顿
- `apply`方法虽然是在异步线程写入，但是由于`Activity`和`Service`的生命周期会等待所有`SharedPreference`的写入完成，所以可能引起卡顿和ANR问题

`SharedPreference`从设计之初，就是为了存储少量`key-value`对，而存在的。其本身的设计，就存在很多缺陷。在存储特别少量数据的时候，性能瓶颈还不显著。但是现在很多开发同学在使用的时候，会往里面存一些大型的`JSON`字符串等，导致它的缺点被明显暴露出来。建议在使用`SharedPreference`的时候，只用于存储少量数据，不要存大的字符串。

### 2. MMKV 原理分析 一. 流程分析

[MMKV 原理分析](https://juejin.cn/post/7078640657807441934)

[Android 存储优化 —— MMKV 集成与原理](https://juejin.cn/post/6844903914119102472)

### 3. Android SharedPreferences和DataStore和 MMKV 对比

https://www.jianshu.com/p/74821465a522

### 4. 再见 SharedPreferences 拥抱 Jetpack DataStore

[再见 SharedPreferences 拥抱 Jetpack DataStore](https://juejin.cn/post/6881442312560803853)
