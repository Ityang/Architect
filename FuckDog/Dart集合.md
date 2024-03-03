## 三. Dart 数据类型

在 Dart 编程语言中，有几种主要的集合类型，包括列表（`List`）、集（`Set`）和映射（`Map`）。以下是它们的简要介绍：

**List**：列表是有序的集合，可以包含重复的元素。列表中的元素可以通过索引访问，索引从 0 开始。

**Set** ：Set 是无序的集合，不允许包含重复的元素；

**Map**：以键值对的形式存储元素，键（key）是唯一的

### 3.1 List

`List` 是一种有序的集合，可以包含重复的元素。

```
var list = ["java", "kotlin", "dart"];

//访问list,输出 java
print(list[0]);

//修改第一个元素为 Hello Java
list[0] = "Hello Java";

//向list 尾部添加元素 arkTs
list.add("arkTs");

// 第一种方式遍历： 使用 for 循环遍历
for (var i = 0; i < list.length; i++) {
   print(list[i]);
}

// 第二种方式遍历： 使用 for .. in 遍历
for (var x in list) {
   print(x);
}

// 第三种遍历方式：使用 forEach 遍历
list.forEach((element) {
   print(element);
});
```

### 3.2 Set

`Set` 是一种无序的集合，它不允许包含重复的元素。`Set` 提供了高效的查找和插入操作。`Set`集合中允许 `null`存在，但是一个`Set`中只能包含一个`null`.

```
Set set = Set();

//添加单个元素
set.add("java");
//添加一些元素
set.addAll(["kotlin", "dart", "arkTs"]);

//判断集合中是否包含指定的元素
set.contains("dart");
//判断集合中是否包含一些元素
set.containsAll(["java", "dart"]);

//第一种方式遍历： 使用 for .. in 遍历
for (var info in set) {
  print(info);
}

//第二种遍历方式：使用 forEach 遍历
set.forEach((element) {
  print(element);
});
```

### 3.3 Map

`Map` 是一种键值对的集合，其中每个键对应一个值。`Map` 是无序的，但是它允许您根据键来查找和访问相应的值。

```
var mMap = {"name": "Bob", "age": 18};

//获取 Map 中的值，输出结果： Bob
print(mMap["name"]);

//从 Map 中移除键值对
mMap.remove('age');

//检查 Map 中是否包含某个键 ,输出结果 ：false
print(mMap.containsKey('city'));

//使用 forEach 方法遍历 Map 中的键值对：
mMap.forEach((key, value) {
  print("$key:$value");
});
```