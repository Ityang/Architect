## MVC

MVC 全名是 Model--View--Controller，是模型(model)-视图(view)-控

制器(controller)的缩写，一种软件设计典范，用一种业务逻辑、数据、界 面显示分离的方法组织代码，在改进和个性化定制界面及用户交互的同 时，不需要重新编写业务逻辑。其中 Model 层处理数据，业务逻辑等;

View 层处理界面的显示结果;Controller 层起到桥梁的作用，来控制 View 层和 Model 层通信以此来达到分离视图显示和业务逻辑层。





## MVP

MVP 其实是 MVC 的一种演进版本，它更简单，将 MVC 中的 Controller 改为了 Presenter，

View 通过接口与 Presenter 进行交互，降低耦合，方便进行单元测试。

**View**:负责绘制 UI 元素、与用户进行交互(Activity、View、Fragment 都可以做为 View 层); 

**Model**:对数据的操作、对网络等的操作，和业务相关的逻辑处理;

 **Presenter**:作为 View 与 Model 交互的中间纽带，处理与用户交互的逻辑。可以把 Presenter 理解为一个中间层的角色，它接受 Model 层的数据，并且处理之后传递给 View 层，还需要 处理 View 层的用户交互等操作。



![MVP](/Users/Mac/Documents/Document/架构师/Images/MVC、MVP、MVVM/MVP.jpg)



### 关键点

1. View 不再负责同步的逻辑，而是由 Presenter 负责。Presenter 中既有业务逻辑也有同步逻辑。
2.  View 需要提供操作界面的接口给 Presenter 进行调用。(关键)

对比在 MVC 中，Controller 是不能操作 View 的，View 也没有提供相应的接口;而在 MVP 当中，Presenter 可以操作 View，View 需要提供一组对界面操作的接口给 Presenter 进行调用;Model 仍然通过事件广播自己的变更，但由 Presenter 监听而不是 View。



### 优点

1、便于测试。Presenter 对 View 是通过接口进行，在对 Presenter 进行不依赖 UI 环境的单元测试的时 候。可以通过 Mock 一个 View 对象，这个对象只需要实现了 View 的接口即可。然后依赖注入到 Presenter 中，单元测试的时候就可以完整的测试 Presenter 业务逻辑的正确性。

2、View 可以进行组件化。在 MVP 当中，View 不依赖 Model。这样就可以让 View 从特定的业务场景中脱离出来，可以说 View 可以做到对业务逻辑完全无知。它只需要提供一系列接口提供给上层操作。这样就可 以做高度可复用的 View 组件。



### 缺点

1、Presenter 中除了业务逻辑以外，还有大量的 View->Model，Model->View 的手动同步逻辑，造成 Presenter 比较笨重，维护起来会比较困难。



## MVVM

MVVM 模式(Model--View--ViewModel 模式)，和 MVP 模式相比，MVVM 模式用 ViewModel 替换了 Presenter ，其他层基本上与 MVP 模式一致，ViewModel 可以理解成 是 View 的数据模型和 Presenter 的合体。

MVVM 采用双向绑定(data-binding):View 的变动，自动反映在 ViewModel，反之亦然， 这种模式实际上是框架替应用开发者做了一些工作(相当于 ViewModel 类是由库帮我们生 成的)，开发者只需要较少的代码就能实现比较复杂的交互。



![MVVM](/Users/Mac/Documents/Document/架构师/Images/MVC、MVP、MVVM/MVVM.jpg)



MVVM 的调用关系和 MVP 一样。但是，在 ViewModel 当中会有一个叫 Binder，或者是 Data-binding engine 的东西。以前全部由 Presenter 负责的 View 和 Model 之间数据同步操 作交由给 Binder 处理。你只需要在 View 的模版语法当中，指令式地声明 View 上的显示的 内容是和 Model 的哪一块数据绑定的。当 ViewModel 对进行 Model 更新的时候，Binder 会自动把数据更新到 View 上去，当用户对 View 进行操作(例如表单输入)，Binder 也会 自动把数据更新到 Model 上去。这种方式称为:Two-way data-binding，双向数据绑定。 可以简单而不恰当地理解为一个模版引擎，但是会根据数据变更实时渲染。



![mvvm1](/Users/Mac/Documents/Document/架构师/Images/MVC、MVP、MVVM/mvvm1.png)



### 关键点

MVVM 把 View 和 Model 的同步逻辑自动化了。以前 Presenter 负责的 View 和 Model 同步不再手动地进行 操作，而是交由框架所提供的 Binder 进行负责。

只需要告诉 Binder，View 显示的数据对应的是 Model 哪一部分即可。

`ViewModel` 中的事件整体上来看就是 App 的状态数据，这样 `ViewModel` 并不是告诉 UI 层应该做什么，而是 UI 层根据 App 的状态去做一些事情，这是思维方式上的转变。

### 优点

1、提高可维护性。解决了 MVP 大量的手动 View 和 Model 同步的问题，提供双向绑定机制。提高了代码的可维护性。

2、简化测试。因为同步逻辑是交由 Binder 做的，View 跟 着 Model 同时变更，所以只需要保证 Model 的正确性，View 就正确。大大减少了对 View 同步更新的测试。

### 缺点

1、过于简单的图形界面不适用，或说牛刀杀鸡。

2、对于大型的图形应用程序，视图状态较多，ViewModel 的构建和维护的成本都会比较高。

## MVI

`MVI` 的全称是 `Model-View-Intent`，这里的 Intent 并不是指 Android 中的 Intent 类，而是表示一种意图，可以简单理解为对用户 Event 的一种抽象。其交互图大致如下：

![MVI](/Users/Mac/Documents/Document/架构师/Images/MVC、MVP、MVVM/MVI.jpeg)



`MVI` 并不像 `MVC`、`MVP`、`MVVM` 一样，不论是 Controller、Presenter 还是 ViewModel 都是 View 与 Model 的之间的桥接类，负责这两者之间的通信与交互（虽然 MVC 可以跨过 Controller 直接进行交互）。而 `Intent` 并没有类似的职责，仅仅是约束了 View 的事件通过类似枚举的方式定义.

有的 `MVI` 在实现还需要借助 `ViewModel`，仅仅是把 View 的事件定义成的对应的密封类。目的仅仅是为了强制实现单向数据流的方式，根据之前介绍实现单向数据流的方式还是比较简单的，上层只能依赖下层实现，下层的处理结果通过 LiveData、Flow 方式更新。

那再来聊一下 `MVC`、`MVP`、`MVVM` 与 Android 官方的推荐的 MAD Arch 之间的关系。其实经常提到的 MVVM 与 Android 官方的架构还是有本质区别的。`MVX` （对 MVC、MVP、MVVM的统称）的架构方式对 Model 这一层提到的非常少，留下的印象可能就是除了 VX 之外剩下的就是 Model 的部分。但是这部分在整个 App 的架构中也是非常重要的。我们还是有大量的业务逻辑是在 Model 层处理的。

而 Android 官方的架构中却包含了这部分的描述，新增了 `Data Layer` 与 `Domain Layer`。所以总结下来就是 `MVX` 处理的仅仅是 `UI Layer` 中的问题，描述的是状态管理的部分；官方文档中描述的确是整个 App 的架构，是一种包含的关系。