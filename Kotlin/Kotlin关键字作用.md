#### 1.TODO

在 Kotlin 中，`TODO()` 是一个用于标记代码中需要完成或实现的部分的函数。它的作用是在开发过程中暂时占位，并提醒开发者需要在该处添加更多的代码或功能。

当你使用 `TODO()` 函数时，编译器会警告你在该处存在未完成的工作，并且编译程序仍然会继续运行，不会因为未完成的部分而中断。

```
fun exampleFunction(number: Int): String {
    if (number < 0) {
        // 未实现的部分，用 TODO() 标记
        TODO()
    }
    // 这里是一些已实现的代码
    return "Number is positive"
}

```

在上述示例中，如果传入的 `number` 参数为负数，`TODO()` 函数会暂时占位，表示这个分支需要后续实现。在运行时，当这段代码被执行到时，会触发一个 `NotImplementedError` 异常，提醒开发者这部分代码需要进一步完善。

在实际开发中，开发者可以使用 `TODO()` 函数来标记临时性的占位，这有助于快速地编写框架、测试代码或者在开发过程中标记未来需要添加的功能点。然后在后续开发中，逐步完善这些 `TODO()` 标记的部分，确保代码的完整性和功能的实现。

#### 2. with()

用于简化对特定对象执行多个操作的过程。它允许你在特定对象上执行多个操作，而无需在每个操作中重复引用对象本身。

`with()` 函数接收一个对象作为参数，以及一个 lambda 表达式。在 lambda 表达式中，你可以直接使用对象的属性和方法，而不需要重复引用对象名称。

示例：

```
data class Person(var name: String, var age: Int)

fun main() {
    val person = Person("Alice", 30)

    val result = with(person) {
        println("Name: $name")
        println("Age: $age")
        "Processed ${age + 5} years old" // Lambda 表达式中的最后一行作为 with 函数的返回值
    }

    println(result)
}

```

在上面的示例中，`with()` 函数接收一个 `Person` 对象 `person` 和一个 lambda 表达式。在 lambda 表达式中，直接使用了 `person` 对象的属性 `name` 和 `age`，而不需要重复引用 `person` 对象。lambda 表达式中最后一行的结果会被 `with()` 函数返回，并赋值给 `result`。

使用 `with()` 函数可以让代码更具可读性，减少重复性代码的编写，并且可以更清晰地表达对特定对象的一系列操作。但需要注意的是，`with()` 并不会改变对象本身，它只是提供一个在 lambda 表达式中操作对象的便利方法。

#### 3. apply()

它允许你在对象上执行指定的操作，并返回该对象本身。它通常用于在对象创建后立即对其属性进行初始化或配置。

`apply` 函数接收一个 lambda 表达式作为参数，lambda 表达式中可以直接访问对象的属性和方法。该函数执行 lambda 表达式中的代码块，并将当前对象作为 lambda 表达式的上下文，最后返回该对象。

示例：

```
data class Person(var name: String, var age: Int)

fun main() {
    val person = Person("Alice", 30).apply {
        age += 5
        name = "Alice Smith"
    }

    println("Name: ${person.name}, Age: ${person.age}")
}

```

在上面的示例中，`apply` 函数被调用在 `Person` 对象上。在 `apply` 的 lambda 表达式中，直接对 `Person` 对象的属性进行了修改和赋值操作。`apply` 函数返回了修改后的 `Person` 对象。

使用 `apply` 函数可以在对象创建后立即对其进行初始化，而不需要重复引用对象名称。它提供了一种简洁的方式来操作对象的属性，并返回该对象本身。通常用于进行对象的配置和属性的设置。

#### 4.also()

`also` 是 Kotlin 标准库中的一个函数，类似于 `apply` 函数，它也可以用于对对象进行操作并返回对象本身。不同之处在于 `also` 函数的 lambda 表达式中，对象本身并不作为上下文对象，而是作为 lambda 表达式的参数传入。

`also` 函数接收一个 lambda 表达式作为参数，lambda 表达式中的 `it` 引用指向调用 `also` 函数的对象。在 lambda 表达式中可以对 `it` 进行操作，但最终 `also` 函数返回的是调用者对象本身。

示例：

```
data class Person(var name: String, var age: Int)

fun main() {
    val person = Person("Alice", 30).also {
        it.age += 5
        it.name = "Alice Smith"
    }

    println("Name: ${person.name}, Age: ${person.age}")
}

```

在上面的示例中，`also` 函数被调用在 `Person` 对象上。在 `also` 的 lambda 表达式中，使用 `it` 引用来对 `Person` 对象的属性进行修改和赋值操作。`also` 函数返回了修改后的 `Person` 对象。

`also` 通常用于需要对对象进行一些操作并返回对象本身的场景，虽然在 lambda 表达式中 `it` 引用对象，但它的返回值是调用者对象本身。

#### 5.let()

let扩展函数的实际上是一个作用域函数，当你需要去定义一个变量在一个特定的作用域范围内，let函数的是一个不错的选择；let函数另一个作用就是可以避免写一些判断null的操作。

`let` 是 Kotlin 标准库中的一个函数，它用于对对象执行操作，并将对象作为 lambda 表达式的参数传入。与 `also` 和 `apply` 不同，`let` 函数的 lambda 表达式中使用 `it` 引用传入的对象。

`let` 函数接收一个 lambda 表达式作为参数，lambda 表达式中的 `it` 引用指向调用 `let` 函数的对象。在 lambda 表达式中，可以使用 `it` 对象来执行各种操作，并返回 lambda 表达式中最后一行的结果作为 `let` 函数的返回值。

示例：

```
data class Person(var name: String, var age: Int)

fun main() {
    val person = Person("Alice", 30)
    val result = person.let {
        println("Name: ${it.name}, Age: ${it.age}")
        "Processed ${it.age + 5} years old"
    }

    println(result)
}

```

在上面的示例中，`let` 函数被调用在 `Person` 对象上。在 `let` 的 lambda 表达式中，使用 `it` 引用来访问 `Person` 对象的属性，并进行一些操作。lambda 表达式的最后一行是一个字符串，这个字符串就是 `let` 函数的返回值，被赋值给了 `result` 变量。

`let` 函数通常用于对一个对象进行操作，并且需要将操作结果作为返回值使用。它提供了一种简洁的方式来执行对对象的操作，并且返回操作结果。

#### 6.lazy

在 Kotlin 中，`lazy` 是一个属性委托，它用于延迟初始化属性，直到该属性首次被访问时才会进行初始化。这有助于提高性能，避免不必要的计算或初始化操作。

使用 `lazy` 可以将属性的初始化推迟到第一次访问属性时才执行初始化操作。它接收一个 lambda 表达式作为参数，在第一次访问该属性时，lambda 表达式会被执行，并返回初始化的值。之后，该属性就会持有 lambda 表达式执行后的值，并在后续访问时直接返回该值，不再执行 lambda 表达式。

示例：

```
val lazyValue: String by lazy {
    println("Initializing...")
    "Hello"
}

fun main() {
    println(lazyValue) // 第一次访问属性，执行初始化 lambda 表达式
    println(lazyValue) // 直接返回已初始化的值，不再执行 lambda 表达式
}

```

在上述示例中，`lazyValue` 属性使用了 `lazy` 委托，并且包含一个 lambda 表达式。在第一次访问 `lazyValue` 属性时，会执行 lambda 表达式中的初始化操作，并输出 "Initializing..."，然后返回字符串 "Hello"。在之后的访问中，直接返回已初始化的值，不再执行 lambda 表达式。

使用 `lazy` 属性委托可以避免在初始化时进行不必要的计算或操作，只有当属性被首次访问时才会进行初始化，有助于提高性能和减少资源消耗。

#### 7.takeif()

在 Kotlin 中，`takeIf` 是一个标准库函数，用于根据条件判断是否获取对象。它执行一个条件判断，如果条件满足，则返回原始对象；如果条件不满足，则返回 null。

`takeIf` 接收一个 lambda 表达式作为参数，该 lambda 表达式中包含一个条件。如果条件满足，`takeIf` 返回对象本身；如果条件不满足，则返回 null。

示例：

```
val number = 20
val result = number.takeIf { it > 10 }

println(result) // 输出：20

```

在这个示例中，`takeIf` 函数对 `number` 变量执行了条件判断（大于 10）。由于 `number` 的值是 20，满足条件，因此 `takeIf` 返回了对象本身（即 20），并将其赋值给 `result`。最终输出结果为 `20`。

`takeIf` 在某些情况下非常有用，特别是当你想要根据条件选择性地获取对象时。如果条件符合，则获取对象本身；如果条件不符合，则返回 null。

#### 8.takeUnless()

在 Kotlin 中，`takeUnless` 是一个标准库函数，与 `takeIf` 函数相对应。它执行与 `takeIf` 函数相反的操作。`takeUnless` 函数接收一个 lambda 表达式作为参数，如果 lambda 表达式的条件不满足时，则返回原始对象；如果条件满足，则返回 null。

示例：

```
val number = 20
val result = number.takeUnless { it > 10 }

println(result) // 输出：null

```

在上面的示例中，`takeUnless` 函数对 `number` 变量执行了条件判断（大于 10）。由于 `number` 的值是 20，满足条件，因此 `takeUnless` 返回了 null。所以最终输出结果为 `null`。

`takeUnless` 函数与 `takeIf` 函数相似，但返回值的条件相反。如果条件不符合，则返回对象本身；如果条件符合，则返回 null。这两个函数可以根据特定条件选择性地获取对象或者判定对象是否符合特定条件。

#### 9.repeat()

在 Kotlin 中，`repeat` 是一个标准库函数，用于重复执行指定次数的操作。它接收一个整数参数来确定需要重复执行的次数，并执行 lambda 表达式中定义的操作相应次数。

示例：

```
repeat(3) {
    println("Hello, world!")
}

```

在上面的示例中，`repeat` 函数被调用，并指定了重复执行 3 次的次数。在 lambda 表达式中，打印了 "Hello, world!" 3 次。

`repeat` 函数提供了一种简单的方式来重复执行指定次数的操作。这在需要执行特定次数循环或多次操作的场景下非常有用。
