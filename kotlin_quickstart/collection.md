# 集合

## 简介

集合是存储对象的容易。一个集合通常包含若干个同类型的对象，集合中的对象称为元素（element）或项（item）。集合是一种抽象数据结构，通常支持以下操作：

- 添加元素
- 获取元素
- 删除元素
- 修改或替换元素

需要注意：添加、删除、修改元素这类操作只适用于可变集合（mutable）。

### Mutability

集合可分为可变集合（mutable）与不可变集合（immutable）。

不可变集合创建后不能修改，它们只允许执行不改变元素的操作，如访问元素。

可变集合除了允许访问元素，还支持通过添加、删除或更新元素来改变集合内容。

### Kotlin 集合

Kotlin 标准库提供了基本集合类型实现：list, set, map。这三种类型都包含 mutable 和 immutable 两个版本：

- `List` - 按指定顺序存储元素，支持通过索引访问元素
- `Set` - 存储唯一元素，元素顺序不固定
- `Map` - 存储键值对，key 是唯一的，但不同的 key 可以映射到相同的值

`List` 按照元素添加的顺序保存指定类型的元素，并且**允许重复元素**。

`Set` 不允许重复元素，`Set` 中所有元素都是唯一的。如果向 `Set` 添加一个已存在的 元素，则直接覆盖相同元素。此外，`Set` 不保证元素顺序。

`Map` 以键值对的形式存储元素，每个键对应一个值，`Map` 不允许重复的键，但允许重复的值。与 `Set` 类似，`Map` 也不保证元素顺序。

### 集合属性和方法

不同集合的结构和用途差别很大。不过所有集合有一些共同的属性和方法：

- `size` - 结合大小（元素个数）
- `contains(element)` - 检查集合是否包含指定元素
- `containsAll(elements)` - 检查集合是否包含 `elements` 所有元素
- `isEmpty()` - 判断集合是否为空
- `joinToString()` - 将集合转换为字符串
- ` indexOf(element)` - 返回 `element` 第一次出现的索引；若元素不存在，返回 -1

此外，所有可变集合共同包含一些改变集合的方法：

- `clear()` - 删除集合中的所有元素
- `remove(element)` - 从集合删除第一次出现的 `element`
- `removeAll(element)` - 从集合中删除所有 `element`

### 管用写法

除了 `contains(element)`，还可以使用关键字 `in` 检查集合是否包含指定元素。例如：

```kotlin
println(elem in collection)

println(collection.contains(elem))
```

`in` 的可读性似乎比 `contains` 更多，Kotlin 推荐使用 `in`。

## List

`List` 是一种不可变（immutable）集合。它在初始化后，无法改变大小。该集合允许重复元素，并按照特定顺序存储元素。

例如，使用 `List` 保存车辆信息：

```kotlin
val cars = listOf<String>("BMW", "Honda", "Mercedes")
println(cars) // output: [BMW, Honda, Mercedes]
```

理论上也可以使用 `MutableList` 存储相同信息：

```kotlin
val cars = mutableListOf<String>("BMW", "Honda", "Mercedes")
println(cars) // output: [BMW, Honda, Mercedes]
```

不过 `MutableList` 允许修改：

```kotlin
cars[0] = "Renault"
println(cars) // output: [Renault, Honda, Mercedes]
```

如果希望内容保持不变，使用 `List` 更好：

```kotlin
val cars = listOf<String>("BMW", "Honda", "Mercedes")
cars[0] = "Renault" // Error
```

### 初始化

`List` 是泛型类型，使用 `listOf<E>` 初始化，其中 `E` 是元素类型。

```kotlin
val textUsMethod = listOf<String>("SMS", "Email")
```

元素类型也可以从上下文推导出来，因此可以省略：

```kotlin
val textUsMethod = listOf("SMS", "Email")
```

也可以使用 `emptyList` 创建一个空 list：

```kotlin
val staff = emptyList<String>()
println(staff) // output: []
```

另外，也可以使用 `buildList()` 函数创建 list：

```kotlin
val names = listOf<String>("Emma", "Kim")

val list = buildList {
    add("Marta")
    addAll(names)
    add("Kira")
}
println(list) // output: [Marta, Emma, Kim, Kira]
```

### 方法和属性

`List` 的属性和方法：

- `size` 返回 `List` 的长度（元素个数）
- `isEmpty()` 判断 list 是否为空

`get(index)` 返回指定位置的元素，也可以使用 `[index]`，效果一样。

其它方法：

- `indexOf(element)` - 返回元素第一次出现的索引，如果列表不包含该元素，返回 -1
- `contains(element)` - 如果列表包含指定元素，返回 `true`，否则返回 `false`

### 遍历元素

可以使用 `for` 循环遍历 `List`。例如：

```kotlin
val participants = listOf("Fred", "Emma", "Isabella")

for (participant in participants) {
    println("Hello $participant!")
}

// Hello Fred!
// Hello Emma!
// Hello Isabella!
```

### List 总结

`List` 是一种常见的数据结构，是一个不可变集合类型。Kotlin 提供了多种创建 `List` 的方式，推荐最简短的方法：

```kotlin
val list = listOf("a", "b", "c")
```



