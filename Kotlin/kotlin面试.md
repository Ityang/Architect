# 1.1 请简述一下什么是 Kotlin？它有哪些特性？

kotlin和java一样也是一门jvm语言最后的编译结果都是.class文件,并且可以通过kotlin的.class文件反编译回去java代码,并且封装了许多语法糖,其中我在项目中常用的特性有

1. 扩展,(使用非集成的方式 扩张一个类的方法和变量):比方说 px和dp之间的转换 之前可能需要写个Util现在,通过扩展Float的变量 最后调用的时候仅仅是 123.dp 这样px转成dp了
2. lamdba表达式,函数式编程. lamdba表达式并不是kotlin的专利,java中也有,但是有限制, 像setOnClickListener一样,接口方法只有一个的情况才能调用, 而在kotlin中对接口的lambda也是如此,有这样的限制,但是他更推荐你使用闭包的方式而不是实现匿名接口的方式去实现这样的功能,闭包对lambda没有接口这么多的限制,另外就是函数式编程 在java8中提供了streamApi 对集合进行map sort reduce等等操作,但是对android api有限制,为了兼容低版本,几乎不可能使用streamApi
3. 判空语法 省略了许多if xxx==null 的写法 也避免了空指针异常aaa?.toString ?: "空空如也" 当aaa为空的时候 它的值被"空空如也"替代



```bash
aaa?.let{
    it.bbb
}
```

当aaa不为空时 执行括号内的方法

1. 省略了findViewById ,使用kotlin 就可以直接用xml中定义的id 作为变量获取到这个控件,有了这个 butterknife就可以淘汰了,使用databinding也能做到,但是,非常遗憾,databinding的支持非常不好,每次修改视图,都不能及时生成,经常要rebulid才能生成.
2. 默认参数 减少方法重载 fun funName(a :Int ,b:Int = 123)通过如上写法 实际在java中要定义两个写法 funName(a)和funName(a,b)
3. kotlin无疑是android将来语言的趋势,我已经使用kotlin一年了,不仅App工程中使用,而且封装的组件库也是用kotlin,另外说明,kotlin会是apk大小在混淆后增加几百k.但对于更舒适的代码来说这又算的了什么呢

# 1.2 Kotlin 中注解 @JvmOverloads 的作用？

@JvmOverloads注解的作用就是:在有默认参数值的方法加上@JvmOverloads注解，则Kotlin就会暴露多个重载方法。可以减少写构造方法。
 例如：没有加注解，默认参数没有起到任何作用。



```kotlin
fun f(a: String, b: Int = 0, c: String="abc"){
    ...
}
那相当于在java中：
void f(String a, int b, String c){
}
如果加上注解@JvmOverloads ，默认参数起到作用
@JvmOverloads
fun f(a: String, b: Int = 0, c: String="abc"){
    ...
}
相当于Java中：
三个构造方法，
void f(String a)
void f(String a, int b)
void f(String a, int b, String c)
```

# 1.3 Kotlin中List与MutableList的区别？

List 返回的是EmptyList，MutableList 返回的是一个ArrayList，查看EmptyList的源码就知道了，根本就没有提供 add 方法。



```kotlin
internal object EmptyList : List, Serializable, RandomAccess {
private const val serialVersionUID: Long = -7390468764508069838L
override fun equals(other: Any?): Boolean = other is List<*> && other.isEmpty()
override fun hashCode(): Int = 1
override fun toString(): String = "[]"
override val size: Int get() = 0
override fun isEmpty(): Boolean = true
override fun contains(element: Nothing):
Boolean = false
override fun containsAll(elements: Collection<Nothing>): Boolean = elements.isEmpty()
override fun get(index: Int): Nothing = throw IndexOutOfBoundsException("Empty list doesn't contain element at index $index.")
override fun indexOf(element: Nothing): Int = -1
override fun lastIndexOf(element: Nothing): Int = -1
override fun iterator(): Iterator<Nothing> = EmptyIterator
override fun listIterator(): ListIterator<Nothing> = EmptyIterator
override fun listIterator(index: Int): ListIterator<Nothing> {
    if (index != 0) throw IndexOutOfBoundsException("Index: $index")
        return EmptyIterator
}
override fun subList(fromIndex: Int, toIndex: Int): List<Nothing> {
    if (fromIndex == 0 && toIndex == 0) return this
    throw IndexOutOfBoundsException("fromIndex: $fromIndex, toIndex: $toIndex")
}
private fun readResolve(): Any = EmptyList
```

# 1.4 Kotlin中实现单例的几种常见方式？

饿汉式：



```kotlin
object StateManagementHelper {
    fun init() {
        //do some initialization works
    }
}
```

懒汉式：



```kotlin
class StateManagementHelper private constructor() {
    companion object {
        private var instance: StateManagementHelper? = null
            @Synchronized get() {
                if (field == null) field = StateManagementHelper()
                return field
            }
    }

    fun init() {
        //do some initialization works
    }
}
```

双重检测：



```kotlin
class StateManagementHelper private constructor() {
    companion object {
        val instance: StateManagementHelper by lazy(mode = LazyThreadSafetyMode.SYNCHRONIZED) {
            StateManagementHelper()
        }
    }

    fun init() {
        //do some initialization works
    }
}
```

静态内部类：



```kotlin
class StateManagementHelper private constructor() {
    companion object {
        val INSTANCE = StateHelperHolder.holder
    }

    private object StateHelperHolder {
        val holder = StateManagementHelper()
    }

    fun init() {
        //do some initialization works
    }
}
```

# 1.5 谈谈你对Kotlin中的 data 关键字的理解？相比于普通类有哪些特点？

在kotlin中数据类通过data关键字来修饰。

### 数据类需满足的条件

- 主构造器必须至少有一个参数
- 主构造器中的参数需要用var/val声明为属性
- 数据类不能用abstract、open、sealed修饰，也不能定义成内部类
- 数据类可以实现接口也可以继承其他类

### 系统自动为数据类生成哪些内容

- 生成equals/hashCode的方法。
- 自动重写toString方法返回形如：”User(name=guojingbu,age=18)“的字符串
- 为每个属性生成operator修饰的componentN()方法，来支持解构。
- 生成copy()方法,方便完成对象复制。

### 示例代码

系统生成方法代码验证
 数据类代码如下：



```kotlin
data class Result(var code:Int,val msg:String)
```

反编译后java代码如下：



```public
   private int code;
   @NotNull
   private final String msg;
    //...省略setter和getter方法
   public Result(int code, @NotNull String msg) {
      Intrinsics.checkNotNullParameter(msg, "msg");
      super();
      this.code = code;
      this.msg = msg;
   }

   public final int component1() {
      return this.code;
   }

   @NotNull
   public final String component2() {
      return this.msg;
   }

   @NotNull
   public final Result copy(int code, @NotNull String msg) {
      Intrinsics.checkNotNullParameter(msg, "msg");
      return new Result(code, msg);
   }

   @NotNull
   public String toString() {
      return "Result(code=" + this.code + ", msg=" + this.msg + ")";
   }

   public int hashCode() {
      int var10000 = this.code * 31;
      String var10001 = this.msg;
      return var10000 + (var10001 != null ? var10001.hashCode() : 0);
   }

   public boolean equals(@Nullable Object var1) {
      if (this != var1) {
         if (var1 instanceof Result) {
            Result var2 = (Result)var1;
            if (this.code == var2.code && Intrinsics.areEqual(this.msg, var2.msg)) {
               return true;
            }
         }

         return false;
      } else {
         return true;
      }
   }
```

数据类可以继承其他类代码验证



```kotlin
open class Sup{}

interface Base{}

data class Result(var code:Int,val msg:String): Sup(),Base{}
```

如果想让JVM为数据类生成一个无参构造，那么数据类的主构造中的参数必须都有默认值，示例代码如下：
 数据类代码：



```kotlin
data class Result(var code:Int=0,val msg:String=""){}
```

反编译后生成的java代码：



```java
public final class Result {
    ...省略一些getter和setter方法
   public Result(int code, @NotNull String msg) {
      Intrinsics.checkNotNullParameter(msg, "msg");
      super();
      this.code = code;
      this.msg = msg;
   }

   // $FF: synthetic method
   public Result(int var1, String var2, int var3, DefaultConstructorMarker var4) {
      if ((var3 & 1) != 0) {
         var1 = 0;
      }

      if ((var3 & 2) != 0) {
         var2 = "";
      }

      this(var1, var2);
   }

   public Result() {
      this(0, (String)null, 3, (DefaultConstructorMarker)null);
   }
    ...省略不相关代码
}
```

可以看到上的代码JVM会为数据类生成一个没有参数的构造器。

# 1.6 什么是委托属性？请简要说说其使用场景和原理？

### 属性委托

有些常见的属性操作，我们可以通过委托方式，让它实现，例如:

- lazy 延迟属性：值只在第一次访问的时候计算
- observable 可观察属性：属性发生改变时通知
- map 集合：将属性存在一个map集合里面

### 类委托

- 可以通过类委托来减少 extend
- 类委托的时，编译器会优先使用自身重写的函数，而不是委托对象的函数



```kotlin
interface Base{
    fun print()
}
class BaseImpl(var x: Int):Base{
    override fun print(){
        print(x)
    }
}
// Derived 的 print 实现会通过构造函数的b对象来完成
class Derived(b: Base): Base by b
```

# 1.7 请举例说明Kotlin中with与apply函数的应用场景和区别？

- `with` 不怎么使用，因为它确实不防空
- 经常使用的是 `run` 和 `apply`

1. run 闭包返回结果是闭包的执行结果；apply 返回的是调用者本身。
2. 使用上的差别：run 更倾向于做一些其他复杂逻辑操作，而 apply 更多的是对调用者自身配置。
3. 大部分情况下，如果不是对调用者本身进行设置，我会使用 run。

# 1.8 Kotlin中 Unit 类型的作用以及与Java中Void 的区别？

Unit : Kotlin 中Any的子类, 方法的返回类型为Unit时，可以省略；
 Void：Java中的方法无法回类型时使用，但是不能省略；
 Nothing：任何类型的子类，编译器对其有优化，有一定的推导能力，另外其常常和抛出异常一起使用；

# 1.9 Kotlin 中 infix 关键字的原理和使用场景？

使用场景是用来修饰函数，使用了 infix 关键字的函数称为中缀函数，使用时可以省略 点表达式和括号。让代码看起来更加优雅，更加语义化。
 原理不过是编译器在语法层面给与了支持，编译为 Java 代码后可以看到就是普通的函数调用。
 kotlin 的很多特性都是在语法和编译器上的优化。

# 1.10 Kotlin中的可见性修饰符有哪些？相比于Java有什么区别？

kotlin存在四种可见性修饰符，默认是public。 private、protected、internal、public

1. private、protected、public是和java中的一样的。
2. 不同的是java中默认是default修饰符（包可见），而kotlin存在internal修饰符（模块内部可见）。
3. kotlin可以直接在文件顶级声明方法、变量等。其中protected不能用来修饰在文件顶级声明的类、方法、变量等。

构造方法默认是public修饰，可以使用可见性修饰符修饰constructor关键字来改变构造方法的可见性。

# 1.11 你觉得Kotlin与Java混合开发时需要注意哪些问题？

kotlin调用java的时候 如果java返回值可能为 null 那就必须加上@nullable 否则kotlin无法识别，也就不会强制你做非空处理，一旦java返回了 null 那么必定会出现null指针异常，加上@nullable 注解之后kotlin就能识别到java方法可能会返回null，编译器就能会知道，并且强制你做非null处理，这也就是kotlin的空安全

# 1.12 在Kotlin中，何为解构？该如何使用？

给一个包含N个组件函数（component）的对象分解为替换等于N个变量的功能，而实现这样功能只需要一个表达式就可以了。

例如
 有时把一个对象 解构 成很多变量会很方便，例如:



```kotlin
val (name, age) = person
```

这种语法称为 解构声明 。一个解构声明同时创建多个变量。 我们已经声明了两个新变量： name 和 age，并且可以独立使用它们：



```go
println(name)
println(age)
```

一个解构声明会被编译成以下代码：



```kotlin
val name = person.component1()
val age = person.component2()
```

# 1.13 在Kotlin中，什么是内联函数？有什么作用？

Kotlin里使用关键 inline 来表示内联函数，那么到底什么是内联函数呢，内联函数有什么好处呢？

1. 什么是内联inline？
    在 Java 里是没有内联这个概念的，所有的函数调用都是普通方法调用，如果了解 Java 虚拟机原理的，可以知道Java 方法执行的内存模型是基于 Java 虚拟机栈的：每个方法被执行的时候都会创建一个栈帧（Stack Frame），用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法被调用直至执行完成的过程，就对应着一个栈帧入栈、出栈的过程。
    也就是说每调用一个方法，都会对应一个栈帧的入栈出栈过程，如果你有一个工具类方法，在某个循环里调用很多次，那就会对应很多次的栈帧入栈、出栈过程。这里首先要记住的一点是，栈帧的创建及入栈、出栈都是有性能损耗的。
    下面以一个例子来说明，看段代码片段：



```kotlin
fun test() {
    //多次调用 sum() 方法进行求和运算
    println(sum(1, 2, 3))
    println(sum(100, 200, 300))
    println(sum(12, 34))
    //....可能还有若干次
}

/**
 *求和计算
 */
fun sum(vararg ints: Int): Int {
    var sum = 0
    for (i in ints) {
        sum += i
    }
    return sum
}
```

在测试方法 test() 里，我们多次调用了 sum() 方法。为了避免多次调用 sum() 方法带来的性能损耗，我们期望的代码类似这样子的：



```kotlin
fun test() {
    var sum = 0
    for (i in arrayOf(1, 2, 3)) {
        sum += i
    }
    println(sum)
    sum = 0
    for (i in arrayOf(100, 200, 300)) {
        sum += i
    }
    println(sum)
    sum = 0
    for (i in arrayOf(12, 34)) {
        sum += i
    }
    println(sum)
}
```

3次数据求和操作，都是在 test() 方法里执行的，没有之前的 sum() 方法调用，最后的结果依然是一样的，但是由于减少了方法调用，虽然代码量增加了，但是性能确提升了。那么怎么实现这种情况呢，一般工具类有很多公共方法，我总不能在需要调用这些公共方法的地方，把代码复制一遍吧，内联就是为了解决这一问题。
 定义内联函数：



```kotlin
inline fun sum(vararg ints: Int): Int {
    var sum = 0
    for (i in ints) {
        sum += i
    }
    return sum
}
```

如上所示，用关键字 inline 标记函数，该函数就是一个内联函数。还是原来的 test() 方法，编译器在编译的时候，会自动把内联函数 sum() 方法体内的代码，替换到调用该方法的地方。查看编译后的字节码，会发现 test() 方法里已经没了对 sum() 方法的调用，凡是原来代码里出现sum() 方法调用的地方，出现的都是 sum() 方法体内的字节码了。

1. noinline
    如果一个内联函数的参数里包含 lambda表达式，也就是函数参数，那么该形参也是 inline 的，举个例子：



```kotlin
inline fun test(inlined: () -> Unit) {...}
```

这里有个问题需要注意，如果在内联函数的内部，函数参数被其他非内联函数调用，就会报错，如下所示：



```kotlin
//内联函数
inline fun test(inlined: () -> Unit) {
    //这里会报错
    otherNoinlineMethod(inlined)
}

//非内联函数
fun otherNoinlineMethod(oninline: () -> Unit) {}
```

要解决这个问题，必须为内联函数的参数加上 noinline 修饰，表示禁止内联，保留原有函数的特性，所以 test() 方法正确的写法应该是：



```kotlin
inline fun test(noinline inlined: () -> Unit) {
    otherNoinlineMethod(inlined)
}
```

1. crossinline
    首先来理解一个概念：非局部返回。我们来举个栗子：



```kotlin
fun test() {
    innerFun {
        //return 这样写会报错，非局部返回，直接退出 test() 函数。
        return@innerFun //局部返回，退出 innerFun() 函数，这里必须明确退出哪个函数，写成 return@test 则会退出 test() 函数
    }
    //以下代码依旧会执行
    println("test...")
}

fun innerFun(a: () -> Unit) {
    a()
}
```

非局部返回我的理解就是返回到顶层函数，如上面代码中所示，默认情况下是不能直接 return 的，但是内联函数确是可以的。所以改成下面这个样子：



```kotlin
fun test() {
    innerFun {
        return //非局部返回，直接退出 test() 函数。
    }
    //以下代码不会执行
    println("test...")
    inline fun innerFun(a: () -> Unit) {
        a()
    }
}
```

也就是说内联函数的函数参数在调用时，可以非局部返回，如上所示。那么 crossinline 修饰的 lambda 参数，可以禁止内联函数调用时非局部返回。



```kotlin
fun test() {
    innerFun {
        return //这里这样会报错，只能 return@innerFun
    }
    //以下代码不会执行
    println("test...")
    inline fun innerFun(crossinline a: () -> Unit) {
        a()
    }
}
```

1. 具体化的类型参数 reified
    这个特性我觉得特别牛逼，有了它可以少写好多代码。在Java 中是不能直接使用泛型的类型，但是在 Kotlin 中确可以。举个栗子：



```kotlin
inline fun startActivity() {
    startActivity(Intent(this, T::class.java))
}
```

使用时直接传入泛型即可，代码简洁明了：



```undefined
startActivity()
```

# 1.14 谈谈kotlin中的构造方法？有哪些注意事项？

### 一、概要简述

1. kotlin中构造函数分为主构造和次级构造两类
2. 使用关键词constructor标记构造函数，部分情况可省略
3. init关键词用于初始化代码块，注意与构造函数的执行顺序，类成员的初始化顺序
4. 继承，扩展时候的构造函数调用逻辑
5. 特殊的类如data class、object/componain object、sealed class等构造函数情况与继承问题
6. 构造函数中的形参声明情况

### 二、详细说明

#### 主/次 构造函数

1. kotlin中任何class（包括object/data class/sealed class）都有一个默认的无参构造函数
2. 如果显式的声明了构造函数，默认的无参构造函数就失效了。
3. 主构造函数写在class声明处，可以有访问权限修饰符private,public等，且可以省略constructor关键字。
4. 若显式的在class内声明了次级构造函数，就需要委托调用主构造函数。
5. 若在class内显式的声明处所有构造函数（也就是没有了所谓的默认主构造），这时候可以不用依次调用主构造函数。例如继承View实现自定义控件时，三四个构造函数同时显示声明。

#### init初始化代码块

kotlin中若存在主构造函数，其不能有代码块执行，init起到类似作用，在类初始化时侯执行相关
 的代码块。

1. init代码块优先于次级构造函数中的代码块执行。
2. 即使在类的继承体系中，各自的init也是优先于构造函数执行。
3. 在主构造函数中，形参加有var/val，那么就变成了成员属性的声明。这些属性声明是早于init代码块的。

#### 特殊类

1. object/companion object是对象示例，作为单例类或者伴生对象，没有构造函数。
2. data class要求必须有一个含有至少一个成员属性的主构造函数，其余方面和普通类相同。
3. sealed class只是声明类似抽象类一般，可以有主构造函数，含参无参以及次级构造等。

# 1.15 谈谈Kotlin中的Sequence，为什么它处理集合操作更加高效？

处理集合时性能损耗的最大原因是循环。集合元素迭代的次数越少性能越好。
 我们写个例子：



```swift
list.map { it ++ }
.filter { it % 2 == 0 }
.count { it< 3 }
```

反编译一下，你会发现：Kotlin编译器会创建三个while循环。
 Sequences 减少了循环次数
 Sequences提高性能的秘密在于这三个操作可以共享同一个迭代器(iterator)，只需要一次循环即可完成。
 Sequences允许 map 转换一个元素后，立马将这个元素传递给 filter操作 ，而不是像集合(lists) 那样，等待所有的元素都循环完成了map操作后，用一个新的集合存储起来，然后又遍历循环从新的集合取出元素完成filter操作。

Sequences 是懒惰的
 上面的代码示例，map、filter、count 都是属于中间操作，只有等待到一个终端操作，如打印、sum()、average()、first()时才会开始工作，不信？你跑下下面的代码？



```swift
val list = listOf(1, 2, 3, 4, 5, 6)
val result = list.asSequence()
    .map { println("--map"); it * 2 }
    .filter { println("--filter");it % 3 == 0 }
println("go~")
println(result.average())
```

扩展：Java8 的 Stream(流) 怎么样呢?



```cpp
list.asSequence()
.filter { it < 0 }
.map { it++ }
.average()

list.stream()
.filter { it < 0 }
.map { it++ }
.average()
```

stream的处理效率几乎和Sequences一样高。它们也都是基于惰性求值的原理并且在最后(终端)处理集合。

# 1.16 请谈谈Kotlin中的Coroutines，它与线程有什么区别？有哪些优点？

说一下个人理解吧。先列出协程几个特点：

1. 在单个进程内，多个协程串行执行，只挂起不阻塞
2. 协程最终的执行还是在各个线程之中

优点：

1. 由于不阻塞线程，异步任务是编译器主动交到线程池中执行。因此，在异步任务执行上，切换和消耗的资源都较少。
2. 由于协程是跨多个线程，并且能够保持串行执行；因此，在处理多并发的情况上，能够比锁更轻量级。通过状态量实现

# 1.17 Kotlin中该如何安全地处理可空类型？

对于方法传入的参数直接通过if判断，例如：



```kotlin
fun a(tag: String?, type: String) {
    if (tag != null && type != null) {
        // do something
    }
}
```

还有就是



```css
a?.let{}
a?.also{}
a?.run{}
a?.apply{}
```

然后接着有一个疑问，假如同时判断两个变量，写成：



```bash
a?.let{
    b?.let {
        //do something
    }
}
```

# 1.18 说说Kotlin中的Any与Java中的Object有何异同？

同：都是顶级父类
 异：成员方法不同

Any只声明了toString()、hashCode()和equals()作为成员方法。

我们思考下，为什么 Kotlin 设计了一个 Any ？

当我们需要和 Java 互操作的时候，Kotlin 把 Java 方法参数和返回类型中用到的 Object 类型看作 Any，这个 Any 的设计是 Kotlin 兼容 Java 时的一种权衡设计。
 所有 Java 引用类型在 Kotlin 中都表现为平台类型。当在Kotlin 中处理平台类型的值的时候，它既可以被当做可空类型来处理，也可以被当做非空类型来操作。
 试想下，如果所有来自 Java 的值都被看成非空，那么就容易写出比较危险的代码。反之，如果 Java 值都强制当做可空，则会导致大量的 null 检查。综合考量，平台类型是一种折中的设计方案。

# 1.19 Kotlin中的数据类型有隐式转换吗？为什么？

不可隐式转换
 在Java中从小到大，可以隐式转换，数据类型将自动提升。下面以int为例
 这么写是ok的



```kotlin
int a = 2312;
long b = a;

//那么在Kotlin中
//隐式转换，编译器会报错
val anInt: Int = 5
val ccLong: Long = anInt

//需要去显式的转换,下面这个才是正确的
val ddLong: Long = anInt.toLong()
```

# 1.20 Kotlin中集合遍历有哪几种方式？

对于如下集合



```kotlin
val list = mutableListOf("a", "b", "c", "d", "e", "f", "g")

// kotlin中集合的遍历方式有下面几种
// 1、 通过解构的方式，可以方便的获取index和value
for ((index, value) in list.withIndex()){
    println("index = $index , value = $value")
}

// 2、 … 左闭右闭 [,]
for (i in 0 .. list.size - 1){
    println("index = $i , value = ${list[i]}")
}

// 3、 until 左闭右开 [,)
for (i in 0 until list.size){
    println("index = $i , value = ${list[i]}")
}

// 4、 downTo 递减的循环方式 左闭右闭 [,]
for (i in list.size - 1 downTo 0){
    println("index = $i , value = ${list[i]}")
}

// 5、带步长的循环step可以和其他循环搭配使用
for (i in 0 until list.size step 2){
    println("index = $i , value = ${list[i]}")
}

// 6、指定循环次数 it就代表了当前循环的计数,从0开始下面的语句循环了10次 每次的计数分别是 0，1…9
repeat(10){
    println(it)
}

// 7、不需要数据的下标，直接for循环list中的每个item
for (item in list){
    println(item)
}
```


