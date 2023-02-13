# Binder 原理





- Android中进程和线程的关系,区别
- 为何需要进行IPC,多进程通信可能会出现什么问题
- Android中IPC方式有几种、各种方式优缺点
- 为何新增Binder来作为主要的IPC方式
- 什么是Binder
- Binder的原理
- Binder Driver 如何在内核空间中做到一次拷贝的？
- 使用Binder进行数据传输的具体过程
- Binder框架中ServiceManager的作用
- 什么是AIDL
- AIDL使用的步骤
- AIDL支持哪些数据类型
- AIDL的关键类，方法和工作流程
- 如何优化多模块都使用AIDL的情况
- 使用 Binder 传输数据的最大限制是多少，被占满后会导致什么问题
- Binder 驱动加载过程中有哪些重要的步骤
- 系统服务与bindService启动的服务的区别
- Activity的bindService流程
- 不通过AIDL，手动编码来实现Binder的通信
