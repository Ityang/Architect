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

- `HashSet`：**线程不安全**，基于哈希表实现的集合，不保证元素顺序，不允许重复元素。
- `LinkedHashSet`:**线程不安全**，继承自 HashSet 类，它按照元素插入的顺序维护元素顺序。内部使用链表维护元素的顺序。
- `TreeSet`：**线程不安全**，基于红黑树实现的有序集合，保证元素有序且不重复。

**Map 接口：**

- `HashMap`：**线程不安全**，基于哈希表实现的键值对集合，提供快速的查找性能。不保证顺序，允许键和值为 `null`
- `LinkedHashMap`：**线程不安全**，基于哈希表和双向链表的实现。它保留了插入顺序，也可以选择按照访问顺序进行排序。`LinkedHashMap`允许null键和null值。
- `HashTable`:**线程安全**
- `ConcurrentHashMap`：**线程安全**的`HashMap`实现。它采用分段锁机制，使得多个线程可以并发地读取和修改`ConcurrentHashMap`，而不需要同步整个map。它不保证顺序，允许null键和null值。
- `TreeMap`：**线程不安全**，基于红黑树的实现，按照键的自然顺序或者自定义顺序进行排序。因此，它的键是有序的。`TreeMap`不允许null键，但允许null值。

#### 1.2.1 ArrayList

`ArrayList` 是 `java.util` 包下的一个动态数组实现的类，它实现了 `List` 接口，可以根据需要动态增长和缩减大小

##### 特点和功能：

- **动态数组**：`ArrayList` 内部是一个数组，可以动态增长和缩减大小，根据需要自动扩容。
- **快速随机访问**：通过索引可以快速随机访问元素，时间复杂度为 O(1)。
- **允许重复元素**：`ArrayList` 允许存储重复元素。

示例：

```
// 创建一个 ArrayList
ArrayList<String> list = new ArrayList<>();

// 添加元素
list.add("Java");
list.add("Kotlin");
list.add("Dart");

String info = list.get(1); // 获取索引为 1 的元素（返回 "Kotlin"）

list.remove("Dart");
list.remove(0); // 删除索引为 0 的元素

// 遍历集合
for (String info : list) {
    System.out.println(info);
}
```

##### 面试题1:ArrayList 的扩容机制

ArrayList扩容的本质就是计算出新的扩容数组的size后实例化，并将原有数组内容复制到新数组中去。（不是原数组，而是新数组然后给予数组对象地址）。

默认情况下，新的容量会是原容量的 **1.5** 倍。 `新容量=旧容量右移一位（相当于除于2）+ 旧容量`

　　ArrayList 的底层是用动态数组来实现的。`我们初始化一个ArrayList 集合还没有添加元素时，其实它是个空数组，只有当我们添加第一个元素时，内部会调用扩容方法并返回最小容量10，也就是说ArrayList 初始化容量为10`。 当前数组长度小于最小容量的长度时（前期容量是10，当添加第11个元素时就就扩容），便开始可以扩容了，ArrayList 扩容的真正计算是在一个grow()里面，新数组大小是旧数组的1.5倍，如果扩容后的新数组大小还是小于最小容量，那新数组的大小就是最小容量的大小，后面会调用一个Arrays.copyof方法，这个方法是真正实现扩容的步骤。

#### 1.2.2 LinkedList

`LinkedList` 是 `java.util` 包下的一个双向链表实现的类，它实现了 `List` 和 `Deque` 接口。与 `ArrayList` 不同，`LinkedList` 使用链表实现，每个元素都存储了对前一个和后一个元素的引用，因此在插入和删除操作时更加高效。

##### 特点和功能：

- **双向链表**：`LinkedList` 内部使用双向链表实现，支持双向遍历。
- **插入和删除**：在链表中进行插入和删除操作效率较高，因为只需要改变节点的指针指向即可，不需要像数组那样移动大量元素。
- **不支持随机访问**：`LinkedList` 不支持像 `ArrayList` 那样的快速随机访问，获取指定位置的元素需要遍历链表。

示例：

```
// 创建一个 LinkedList
LinkedList<String> list = new LinkedList<>();

// 添加元素
list.add("Java");
list.add("Kotlin");
list.add("Dart");

String info = list.get(1); // 获取索引为 1 的元素（返回 "Kotlin"）

list.remove("Dart");
list.remove(0); // 删除索引为 0 的元素

// 遍历集合
for (String info : list) {
    System.out.println(info);
}
```

`LinkedList` 是 Java 中另一个常用的集合类，使用双向链表实现，适合于对插入和删除操作较多的场景，但不适合需要频繁随机访问的场景。

##### 面试题2: ArrayList 和 LinkedList 的区别？

ArrayList和LinkedList，前者是Array(动态数组)的数据结构，后者是Link(链表)的数据结构，此外他们两个都是对List接口的实现

当随机访问List时（get和set操作），ArrayList和LinkedList的效率更高，因为LinkedList是线性的数据存储方式，所以需要移动指针从前往后查找。

当对数据进行增删的操作时（add和remove），LinkedList比ArrayList的效率更高，因为ArrayList是数组，所以在其中进行增删操作时，会对操作点之后的所有数据的下标索引造成影响，需要进行数据的移动

从利用效率来看，ArrayList自由性较低，因为需要手动的设置固定大小的容量，但是他的使用比较方便，只需要创建，然后添加数据，通过调用下标进行使用，而LinkedList自由性交给，能够动态的随数据量的变化而变化，但是它不便于使用。

##### 面试题3: 数组和链表的区别

数组：是将元素在内存中连续的存储的，因为数据是连续存储的，内存地址连续，所以在查找数据的时候效率比较高，但在存储之前，需要申请一块连续的内存空间，并且在编译的时候就必须确定好他的空间大小。在运行的时候空间的大小是无法随着需要进行增加和减少的，当数据比较大时，有可能出现越界的情况，当数据比较小时，有可能浪费内存空间，在改变数据个数时，增加、插入、删除数据效率比较低。

链表：是动态申请内存空间的，不需要像数组需要提前申请好内存的大小，链表只需在使用的时候申请就可以了，根据需要动态的申请或删除内存空间，对于数据增加和删除以及插入比数组灵活，链表中数据在内存中可以在任意的位置，通过应用来关联数据。

#### 1.2.3 Vector

`Vector` 是 `java.util` 包下的一个同步（线程安全）的动态数组实现的类，类似于 `ArrayList`，但 `Vector` 的所有方法都是同步的，即保证线程安全，适合在多线程环境下使用。

##### 特点和功能：

- **动态数组**：`Vector` 内部是一个数组，可以动态增长和缩减大小，根据需要自动扩容。
- **同步性**：`Vector` 的所有方法都是同步的，保证线程安全，在多线程环境下使用时不需要额外的同步措施（如加锁）。
- **允许重复元素**：`Vector` 允许存储重复元素。

```
// 创建一个 Vector
Vector<String> vector = new Vector<>();

// 添加元素
vector.add("Java");
vector.add("Kotlin");
vector.add("Dart");

String info = vector.get(1); // 获取索引为 1 的元素（返回 "Kotlin"）

vector.remove("Dart");
vector.remove(0); // 删除索引为 0 的元素

// 遍历集合
for (String fruit : vector) {
    System.out.println(fruit);
}
```

`Vector` 是 Java 中同步的动态数组实现类，在多线程环境下使用较为安全，但由于同步操作可能会降低性能，因此在单线程环境下，通常使用 `ArrayList` 更为常见。

#### 1.2.4 CopyOnWriteArrayList

`CopyOnWriteArrayList` 是 Java 中的一个并发集合类，它提供了一种线程安全的 List 实现。与普通的 `ArrayList` 不同，`CopyOnWriteArrayList` 内部采用了一种称为 "Copy-On-Write"（写时复制）的机制来实现线程安全。

##### 特点和工作原理：

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

#### 1.2.5 HashSet

`HashSet` 是 Set 接口的一个实现类，它基于哈希表实现，不保证元素的顺序，并且不允许集合中存在重复元素。

##### 特点和功能：

- **不允许重复元素**：`HashSet` 不允许包含重复元素，如果向 `HashSet `中添加重复元素，不会产生错误，但不会将重复的元素添加到集合中。
- **基于哈希表**：`HashSet `使用哈希表实现，具有 O(1) 的时间复杂度进行添加、删除和查找操作。
- **无序性**：`HashSet` 中的元素没有特定的顺序，不保证元素的存储顺序。

示例：

```
// 创建一个 HashSet
Set<String> set = new HashSet<>();

// 添加元素
set.add("Java");
set.add("Kotlin");
set.add("Dart");
set.add("Java"); // 添加重复元素，不会被添加到集合中

// 遍历集合
for (String info : set) {
     System.out.println(info);
}
```

在此示例中，尽管我们添加了两次 "Java"，但由于 `HashSet `的特性，重复元素不会被添加到集合中，因此只会输出一次 "Java"。``HashSet` 是常用的集合实现类之一，适合需要高效查找和去重的场景。

#### 1.2.6 LinkedHashSet

`LinkedHashSet` 是 `HashSet` 类的一个子类，它是基于哈希表和链表实现的集合，同时保留了元素的插入顺序。因此，它具备了 HashSet 的快速查找和不允许重复元素的特性，同时又能够按照元素插入的顺序进行迭代遍历。

##### 特点和功能：

- **不允许重复元素**：与 `HashSet `类似，`LinkedHashSet `不允许包含重复元素。
- **有序性**：`LinkedHashSet` 内部使用哈希表和双向链表来维护元素的顺序，保留了元素的插入顺序。因此，遍历 `LinkedHashSet `时可以按照插入顺序获取元素。
- **基于哈希表和链表**：`LinkedHashSet `采用哈希表来实现集合的查找和删除等操作，并使用链表维护元素的顺序。

```
// 创建一个 LinkedHashSet
Set<String> linkedHashSet = new LinkedHashSet<>();

// 添加元素
linkedHashSet.add("Java");
linkedHashSet.add("Kotlin");
linkedHashSet.add("Dart");
linkedHashSet.add("Java"); // 添加重复元素，不会被添加到集合中

// 遍历集合
for (String info : linkedHashSet) {
    System.out.println(info);
}
```

在此示例中，尽管添加了两次 "Java"，但由于 `LinkedHashSet `的特性，重复元素不会被添加到集合中，因此只会输出一次 "Java"。同时，遍历 `LinkedHashSet `时会按照元素的插入顺序进行迭代。`LinkedHashSet `是保持插入顺序并具备 `HashSet `查找效率的一种集合实现。

#### 1.2.7 TreeSet

`TreeSet` 是 `java.util` 包中的一个集合类，它实现了 `SortedSet` 接口，基于红黑树（自平衡二叉搜索树）实现，能够保持元素的自然顺序或根据自定义的比较器进行排序。

##### 特点和功能：

- **有序性**：`TreeSet `保持元素的排序状态，可以根据元素的自然顺序（例如，整数的升序、字符串的字典序等）或者自定义的比较器进行排序。
- **不允许重复元素**：`TreeSet `不允许集合中包含重复元素，如果尝试添加重复的元素，不会产生错误，但不会将重复的元素添加到集合中。
- **基于红黑树**：`TreeSet `使用红黑树数据结构来实现集合，这使得插入、删除和查找等操作具有较好的性能（时间复杂度为` O(log n)`）。

示例：

```
// 创建一个 TreeSet
Set<String> treeSet = new TreeSet<>();

// 添加元素
treeSet.add("Java");
treeSet.add("Kotlin");
treeSet.add("Dart");
treeSet.add("Java"); // 添加重复元素，不会被添加到集合中

// 遍历集合
for (String info : treeSet) {
    System.out.println(info);
}
```

在此示例中，尽管添加了两次 "Java"，但由于 `TreeSet `的特性，重复元素不会被添加到集合中，因此只会输出一次 "Java"。另外，`TreeSet `会按照元素的自然顺序（字典序）进行排序，默认是`升序`。

#### 1.2.8 HashMap



#### 1.2.9 LinkedHashMap

#### 1.2.10 HashTable

#### 1.2.11 ConcurrentHashMap

#### 1.2.12 TreeMap
