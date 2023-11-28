

[官网地址：](https://developer.huawei.com/consumer/cn/training/course/slightMooc/C101667303102887820?ha_linker=eyJ0cyI6MTY5OTg2MDczMTM4NSwiaWQiOiIxYzAyZTQ0MzAzYzNhYmE4NGE1MGIyMGE3YzE4OWI2MiJ9)



其中详细如下：

- AppScope中存放应用全局所需要的资源文件。
- entry是应用的主模块，存放HarmonyOS应用的代码、资源等。
- oh_modules是工程的依赖包，存放工程依赖的源文件。
- build-profile.json5是工程级配置信息，包括签名、产品配置等。
- hvigorfile.ts是工程级编译构建任务脚本，hvigor是基于任务管理机制实现的一款全新的自动化构建工具，主要提供任务注册编排，工程模型管理、配置管理等核心能力。
- oh-package.json5是工程级依赖配置文件，用于记录引入包的配置信息。

在AppScope，其中有resources文件夹和配置文件app.json5。AppScope>resources>base中包含element和media两个文件夹，

- 其中element文件夹主要存放公共的字符串、布局文件等资源。
- media存放全局公共的多媒体资源文件。