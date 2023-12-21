上一章节主要是基本数据类型的使用[Java、Kotlin、Flutter、HarmonyOS基本类型](https://github.com/Ityang/Architect/blob/main/FuckDog/Java%E3%80%81Kotlin%E3%80%81Flutter%E3%80%81HarmonyOS%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B.md),这一章节总结集合类的使用以及对比。

## 一. Java 数据类型

### 1.1 String

`String` 是一个类，用于表示字符串。

```
String message = "Hello, String!";
```

#### 问题：String 为什么是不可变的？

由于效率和安全性问题 String 被设计为不可变的。不可变就是第二次给一个 String  变量赋值的时候，不是在原内存地址上修改数据，而是重新指向一个新对象，新地址。

从效率角度看：

1. 从内存角度看

字符串常量池的要求：创建字符串时，如果该字符串已经存在于池中，则将返回现有字符串的引用，而不是创建新对象。多个String变量引用指向同一个内存地址。如果字符串是可变的，用一个引用更改字符串将导致其他引用的值错误。这是很危险的。

2. 缓存 hashCode

字符串的Hashcode在java中经常配合基于散列的集合一起正常运行，这样的散列集合包括HashSet、HashMap以及HashTable。不可变的特性保证了hashcode 永远是相同的。不用每次使用hashcode就需要计算hashcode。这样更有效率。因为当向集合中插入对象时，是通过hashcode判别在集合中是否已经存在该对象了（不是通过equals方法逐个比较，效率低）。

3. 方便其它类使用

其他类的设计基于String不可变，如set存储String，改变该string后set包含了重复值。

安全性：

String被广泛用作许多java类的参数，例如网络连接、打开文件等。如果对String的某一处改变一不小心就影响了该变量所有引用的表现，则连接或文件将被更改，这可能导致严重的安全威胁。 不可变对象不能被写，所以不可变对象自然是线程安全的，因为不可变对象不能更改，它们可以在多个线程之间自由共享。

### 1.2 集合

集合（Collections）是一组用于存储和操作一组对象的数据结构。Java 提供了一系列集合类来处理不同类型的数据集合。常见的集合类位于 `java.util` 包下，其中最常用的集合类有以下几种：

**List 接口：**

- `ArrayList`：**线程不安全 **，基于数组实现的动态数组，支持快速随机访问和增删操作。
- `LinkedList`：**线程不安全**，基于链表实现的双向链表，适合频繁插入、删除操作。
- `Vector`：**线程安全**，类似于 ArrayList，但性能相对较差，较少被使用。
- `CopyOnWriteArrayList`:**线程安全**，与普通的 `ArrayList` 不同，`CopyOnWriteArrayList` 内部采用了一种称为 "Copy-On-Write"（写时复制）的机制来实现线程安全。适用于读多写少的场景，如果写操作非常频繁，则性能可能不佳。

**Set 接口：**

- `HashSet`：基于哈希表实现的集合，不保证元素顺序，不允许重复元素。
- `TreeSet`：基于红黑树实现的有序集合，保证元素有序且不重复。

**Map 接口：**

- `HashMap`：基于哈希表实现的键值对集合，不保证顺序，允许键和值为 `null`。
- `TreeMap`：基于红黑树实现的有序键值对集合，保证键有序。

#### 1.2.1 ArrayList



#### 1.2.4 CopyOnWriteArrayList

`CopyOnWriteArrayList` 是 Java 中的一个并发集合类，它提供了一种线程安全的 List 实现。与普通的 `ArrayList` 不同，`CopyOnWriteArrayList` 内部采用了一种称为 "Copy-On-Write"（写时复制）的机制来实现线程安全。

**特点和工作原理：**

- **线程安全性**：`CopyOnWriteArrayList` 是线程安全的，支持并发访问，无需额外的同步措施（如加锁）。
- **写时复制**：当对集合进行修改（如添加、修改、删除操作）时，不直接在原始集合上进行操作，而是在修改时创建集合的副本，然后进行修改。这样可以保证修改操作不影响其他线程并发读取集合。
- **读取性能**：`CopyOnWriteArrayList` 的读取操作是不加锁的，因此适合读多写少的场景，对读取性能要求较高的场景。
- **适用场景**：适用于对集合进行频繁读取，但修改操作较少的情况。

示例：

```
// 创建一个 CopyOnWriteArrayList
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();

// 添加元素
list.add("Java");
list.add("Kotlin");
list.add("Dart");

// 使用迭代器遍历集合（读取操作）
for (String info : list) {
    System.out.println(info);
}

// 修改操作（写操作），不会影响迭代器的遍历
list.add("ArkTs");
list.remove("Dart");
```

虽然 `CopyOnWriteArrayList` 提供了一种并发安全的方式来处理集合，但它并不适用于所有场景。由于每次修改都会复制一份集合，因此适用于读多写少的场景，如果写操作非常频繁，则性能可能不佳。因此，在选择使用 `CopyOnWriteArrayList` 时需要根据实际场景和需求来权衡使用。
