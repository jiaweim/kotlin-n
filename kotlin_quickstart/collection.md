# 集合

2026-02-09⭐
***
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

## Mutable List

`MutableList` 是 `List` 的可变版本：

- 允许重复元素
- 保留元素顺序
- 允许添加和删除元素

假设用 `List` 保存你去过的城市：

```kotlin
val places = listOf<String>("Paris", "Moscow", "Tokyo")
println(places) // output: [Paris, Moscow, Tokyo]
```

你继续旅行，又去了圣彼得堡，现在需要将它添加到 `List`，但这里有个问题：上面的 `List` 是不可变的，无法添加新元素。但没关系，不能添加新元素，但是可以重新赋值（虽然又慢又低效）：

```kotlin
var places = listOf<String>("Paris", "Moscow", "Tokyo") // note var keyword
places += "Saint-Petersburg" // reassignment, slow operation
println(places) // output: [Paris, Moscow, Tokyo, Saint-Petersburg]
```

此时用 `MutableList` 更合适：

```kotlin
val places = mutableListOf<String>("Paris", "Moscow", "Tokyo")
places.add("Saint-Petersburg")
println(places) // output: [Paris, Moscow, Tokyo, Saint-Petersburg]
```

`MutableList` 和 `List` 的主要差别：

1. **Mutability**

- `MutableList` 在创建后可以修改
- 不可变 `List` 只能读取，不能修改

2. **函数**

- `MutableList` 额外包含 `add`, `remove`, `clear` 等函数
- 这些函数在不可变 `List` 中不可用

### 初始化

初始化 `MutableList`：

```kotlin
val cars = mutableListOf("Ford", "Toyota", "Audi", "Mazda", "Tesla")
println(cars) // output: [Ford, Toyota, Audi, Mazda, Tesla]
```

如果初始化时包含元素，Kotlin 可以通过参数推断类型，此时可以不指定类型。如果创建空 `MutableList`，需要指定类型：

```kotlin
val cars = mutableListOf<String>()
println(cars) // output: []
```

可以使用 `toMutableList()` 将 `List` 转换为 `MutableList`；用 `toList()` 将 `MutableList` 转换为 `List`：

```kotlin
val cars = listOf("Ford", "Toyota").toMutableList()
cars.add("Tesla")
println(cars) // output: [Ford, Toyota, Tesla]
val carsList = cars.toList()
```

### 添加和替换元素

`MutableList` 包含与 `List` 相同的属性和方法：`size`, `get(index)`, `isEmpty()`, `indexOf(element)`, `contains(element)` 等。

修改`MutableList` 的方法：

- `add(element)` - 添加元素
- `set(index, element)` - 替换指定位置的元素，还有简洁写法：`mutableList[index] = element`
- `addAll(elements)` - 将指定集合中的所有元素添加到列表末尾

示例，购物清单：

```kotlin
val products = listOf("Milk", "Cheese", "Coke")
```

- 现在你改主意了，还想要买薯片，将牛奶替换为水，因此更新该清单

```kotlin
val finalList = products.toMutableList()
finalList.add("Chips")
finalList[0] = "Water" // or finalList.set(0, "Water")
println(finalList) // output: [Water, Cheese, Coke, Chips]
```

- 然后，你爸进来了，把他的清单也给你，需要合并两个清单

```kotlin
val products = mutableListOf("Milk", "Cheese", "Coke")
val dadsProducts = listOf("Banana", "Watermelon", "Apple")

products.addAll(dadsProducts)

println(products) // output: [Milk, Cheese, Coke, Banana, Watermelon, Apple]
```

### 删除元素

删除元素的方法：

- `removeAt(index)` - 删除指定索引处的元素
- `remove(element)` - 删除指定元素第一次出现
- `clear()` - 删除所有元素

示例：

```kotlin
val products = mutableListOf("Milk", "Cheese", "Coke")

products.removeAt(0)
println(products) // output: [Cheese, Coke]

products.remove("Coke")
println(products) // output: [Cheese]

products.clear()
println(products) // output: []
```

### 遍历元素

可以使用 `for` 循环遍历：

```kotlin
val products = mutableListOf("Cheese", "Milk", "Coke")

for (product in products) {
    println("$product")
}

// Cheese
// Milk
// Coke
```

## List 操作

下面介绍 List 的一些常用操作。

### 输出 list

`joinToString()` 函数以指定分隔符 `separator` 串联列表元素。示例：

```kotlin
val southernCross = mutableListOf("Acrux", "Gacrux", "Imai", "Mimosa")
println(southernCross.joinToString())   //  Acrux, Gacrux, Imai, Mimosa
```

显然，`joinToString()` 的默认分隔符为逗号 `,`。

使用其它分隔符：

```kotlin
println(southernCross.joinToString(" -> "))   //  Acrux -> Gacrux -> Imai -> Mimosa
```

### 多个 list

- 串联 mutable list

```kotlin
val southernCross = mutableListOf("Acrux", "Gacrux", "Imai", "Mimosa")
val stars = mutableListOf("Ginan", "Mu Crucis")

val newList = southernCross + stars
println(newList.joinToString())    //  Acrux, Gacrux, Imai, Mimosa, Ginan, Mu Crucis
```

- 使用 `==` 和 `!=` 比较 list：包括内容和长度

```kotlin
val firstList = mutableListOf("result", "is", "true")
val secondList = mutableListOf("result", "is", "true")
val thirdList = mutableListOf("result")

println(firstList == secondList)  //  true
println(firstList == thirdList)   //  false
println(secondList != thirdList)  //  true
```

只要两个 list 的元素完全匹配且顺序相同，才返回 `true`。

### 修改 list 内容

关键字 `val` 和 `var` 决定如何处理变量值或引用：

- `var` - 可以随时修改分配给变量的值或引用
- `val` - 只能给变量赋值一次值或引用

不管是 `val` 还是 `var`，都可以使用索引编辑 `MutableList` 的元素。因为更改列表内容，并没有创建新列表，即变量的引用不变：

```kotlin
val southernCross = mutableListOf("Acrux", "Gacrux", "Imai", "Mimosa")
var stars = mutableListOf("Ginan", "Mu Crucis")
southernCross[1] = "star"
stars[1] = "star"

println(southernCross[1]) // star
println(stars[1]) // star
```

使用 `add`, `remove`, `clear` 修改 list:

```kotlin
val southernCross = mutableListOf("Acrux", "Gacrux", "Imai", "Mimosa")
val stars = mutableListOf("Ginan", "Mu Crucis")
val names = mutableListOf("Jack", "John", "Katie")
val food = mutableListOf("Bread", "Cheese", "Meat")
val fruits = mutableListOf("Apple", "Banana", "Grape", "Mango")

southernCross.removeAt(0)
southernCross.remove("Mimosa")

stars.add("New star")
stars.add(0, "First star")

names.clear()

food.addAll(fruits)

println(names) // []
println(southernCross.joinToString()) // Gacrux, Imai
println(stars.joinToString()) // First star, Ginan, Mu Crucis, New star
println(food.joinToString()) // Bread, Cheese, Meat, Apple, Banana, Grape, Mango
```

- `add(element)` 添加元素到列表末尾
- `add(index, element)` 添加元素到指定位置
- `list1.addAll(list2)` 将 `list2` 中的所有元素添加到 `list1` 的末尾
- `remove(element)` 删除第一次出现的 `element`，删除成功返回 `true`
- `removeAt(index)` 删除指定位置的元素，并返回被删除的元素
- `clear()` 删除所有元素

也可以使用 `+=` 添加元素：

```kotlin
val vowels = mutableListOf('a', 'o', 'i', 'e', 'u')
val intList1 = mutableListOf(1, 2, 3, 4, 5)
val intList2 = mutableListOf(5, 4, 3, 2, 1)
    
vowels += 'y'
intList1 += intList2

println(vowels)   // [a, o, i, e, u, y]
println(intList1) // [1, 2, 3, 4, 5, 5, 4, 3, 2, 1]
```

### 复制 list

Kotlin 没有专门赋值 list 的函数，但可以使用 `toMutableList()` 实现：

```kotlin
val list = mutableListOf(1, 2, 3, 4, 5)
val copyList = list.toMutableList()

print(copyList) // [1, 2, 3, 4, 5]
```

该函数会创建一个新的 `MutableList`，并将 原列表的内容添加到新列表。等价于：

```kotlin
val list = mutableListOf(1, 2, 3, 4, 5)
val copyList = mutableListOf<Int>()
copyList.addAll(list)

print(copyList) // [1, 2, 3, 4, 5]
```

### 其它常用函数

- `list.isEmpty()` 和 `list.isNotEmpty()` 检查列表是否为空
- `list.subList(from, to)` - 创建子列表，该列表包含原列表中索引为 `from`, `from+1`, ..., `to-2`, `to-1` 的元素，即包含 `from`, 不包含 `to`

```kotlin
val numbers = mutableListOf(1, 2, 3, 4, 5)
var sublist = mutableListOf<Int>()
if (numbers.isNotEmpty() && numbers.size >= 4) {
     sublist = numbers.subList(1, 4)
}

print(sublist) // [2, 3, 4]
```

- `list.indexOf(element)` - 在列表中搜索指定元素，如果不存在该 元素，返回 -1，否则返回元素索引

```kotlin
val numbers = mutableListOf(1, 2, 3, 4, 5)

if (5 in numbers) {
    println(numbers.indexOf(5)) // 4
}

print(numbers.indexOf(7)) // -1
```

- `list.minOrNull()` 和 `list.maxOrNull()` - 查找列表中的最大和最小元素
- `list.sum()` - 返回列表中所有元素的加和
- `list.sorted()` 和 `list.sortedDescending()` - 返回一个排序后的新列表（升序或降序）

```kotlin
val numbers = mutableListOf(1, 2, 3, 4, 5)
    
val vowels = mutableListOf('e', 'a', 'y', 'i', 'u', 'o')
    
println(numbers.minOrNull()) // 1
println(numbers.maxOrNull()) // 5
println(numbers.sum())      // 15
    
println(vowels.sorted()) // [a, e, i, o, u, y]
println(vowels.sortedDescending()) // [y, u, o, i, e, a]
```

## for 循环遍历 MutabeList

使用 `for` 循环遍历 `MutableList` 很方便。

### 遍历 MutableList

使用如下语法遍历 mutable-list：

```kotlin
for (element in mutList) {
    // body of loop
}
```

应用示例：

```kotlin
fun main() {
    val daysOfWeek = mutableListOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")
    
    for (day in daysOfWeek){
        println(day)
    }
}
```

```
Sun
Mon
Tues
Wed
Thur
Fri
Sat
```

### 通过索引遍历

可以直接在循环中通过索引访问元素。为此，需要用到 `MutableList.indices` 属性，它代表有效索引范围。

示例：

```kotlin
fun main() {
    val daysOfWeek = mutableListOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")

    for (index in daysOfWeek.indices){
        println("$index: ${daysOfWeek[index]}")
    }
}
```

```
0: Sun
1: Mon
2: Tues
3: Wed
4: Thur
5: Fri
6: Sat
```

### 通过范围索引遍历

通过范围索引范围部分列表元素：

```kotlin
fun main() {
    val daysOfWeek = mutableListOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")

    for (index in 1..5) {
        println("$index: ${daysOfWeek[index]}")
    }
}
```

```
1: Mon
2: Tues
3: Wed
4: Thur
5: Fri
```

也可以使用 `mutList.lastIndex` 属性获取 list 的最后一个有效索引。示例：

```kotlin
for (index in 1 until daysOfWeek.lastIndex) {
    println("$index: ${daysOfWeek[index]}")
}
```

```
1: Mon
2: Tues
3: Wed
4: Thur
5: Fri 
```

可以使用 `downTo` 反向遍历 mutable-list，使用 `step` 指定遍历步长。

示例：以步长为 2 倒序遍历

```kotlin
fun main() {
    val daysOfWeek = mutableListOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")

    for (index in daysOfWeek.lastIndex downTo 0 step 2) {
        println("$index: ${daysOfWeek[index]}")
    }
}
```

```
6: Sat
4: Thur
2: Tues
0: Sun
```

### 读取元素

从输入逐个读取元素，将其添加到 mutable-list。

示例：读取整数，并逆序输出

```kotlin
fun main() {
    val size = readln().toInt()
    val mutList: MutableList<Int> = mutableListOf()
    for (i in 0 until size) {
        mutList.add(readln().toInt())
    }

    for (i in mutList.lastIndex downTo 0) {
        print("${mutList[i]} ")
    }
}
```

输入：

```
5
1
2
3
4
5
```

输出：

```
5 4 3 2 1 
```

## 多维 list

多维列表，就是包含列表的列表，即列表包含的元素也是列表。

### 创建二维列表

创建 3 行 4 列 `Int` 类型的二维列表：

```kotlin
val mutList2D = mutableListOf(
    mutableListOf<Int>(0, 0, 0, 0),
    mutableListOf<Int>(0, 0, 0, 0),
    mutableListOf<Int>(0, 0, 0, 0)
)
```

这是 4 个全是 0 的列表嵌套在 `mutList2D` 中。

内部列表的长度不需要相同：

```kotlin
val mutList2D = mutableListOf(
    mutableListOf<Int>(0),
    mutableListOf<Int>(1, 2),
    mutableListOf<Int>(3, 4, 5)
)
```

### 访问元素

第一个索引对应外部列表，第二个索引内部列表：

```kotlin
val mutList2D = mutableListOf(
    mutableListOf<Int>(0, 1, 2),   //[0]
    mutableListOf<Int>(3, 4, 5)    //[1]  
)

println(mutList2D[0][0])	// 0
```

依次访问所有元素：

```kotlin
print(mutList2D[0][0])  // 0
print(mutList2D[0][1])  // 1
print(mutList2D[0][2])  // 2
print(mutList2D[1][0])  // 3
print(mutList2D[1][1])  // 4
print(mutList2D[1][2])  // 5
```

### 创建不同类型的二维列表

字符串类型：

```kotlin
val mutListOfString2D = mutableListOf(
    mutableListOf<String>("to", "be", "or"),
    mutableListOf<String>("not", "to", "be")
)
```

字符类型：

```kotlin
val mutListOfChar2D = mutableListOf(
    mutableListOf<Char>('A', 'R', 'R'),
    mutableListOf<Char>('A', 'Y', 'S')
)
```

多种类型：

```kotlin
val mutListOfStringAndInt2D = mutableListOf(
    mutableListOf<String>("Practice", "makes", "perfect"),
    mutableListOf<Int>(1, 2)
)
```

### 高维列表（>2）

以三维 list 为例：

| [0, 1] | [2, 3] |
| ------ | ------ |
| [4, 5] | [6, 7] |

二维列表中的每个元素，也是一个列表。创建方法：

```kotlin
val mutList3D = mutableListOf(
    mutableListOf(mutableListOf<Int>(0,1), mutableListOf<Int>(2,3)),
    mutableListOf(mutableListOf<Int>(4,5), mutableListOf<Int>(6,7))
)

println(mutList3D)  // [[[0, 1], [2, 3]], [[4, 5], [6, 7]]]
```

此时访问元素需要三个索引：

```kotlin
println(mutList3D[0][0][1])   // 1
println(mutList3D[1][0][1])   // 5
println(mutList3D[1][1][1])   // 7
```

## Set

`Set` 是一种无序集合，不允许包含重复元素。`Set` 也属于不可变集合，初始化后不能修改。

```kotlin
val visitors = setOf<String>("Vlad", "Vanya", "Liza", "Liza")
println(visitors) // output: [Vlad, Vanya, Liza]
```

这里尝试添加两个相同元素到 `Set`，但由于 `Set` 不支持重复元素，所以得到的 `Set` 只包含一个 "Liza"。

另外，`Set` 是不可变的，创建后不能修改。

### 初始化 Set

`Set` 是一种泛型类型，使用 `setOf<E>` 初始化，其中 `E` 是元素类型：

```kotlin
val languages = setOf<String>("English", "Russian", "Italian")
```

也可以从上下文自动推导类型：

```kotlin
val languages = setOf("English", "Russian", "Italian")
```

但是用 `emptySet` 初始化空 `Set`，必须指定类型（无法推导）：

```kotlin
val numbers = emptySet<Int>()
println(numbers) // output: []
```

另外还可以 `buildSet()` 创建 `Set`：

```kotlin
val letters = setOf<Char>('b', 'c')

val set = buildSet<Char> {
    add('a')
    addAll(letters)
    add('d')
}
println(set) // output: [a, b, c, d]
```

### Set 方法和属性

`isEmpty` 和 `size`：

```kotlin
val visitors = setOf("Andrew", "Mike")

println("How many people visited our cafe today? ${visitors.size}") // 2
println("Was our cafe empty today? It's ${visitors.isEmpty()}") // Was our cafe empty today? It's false
```

`indexOf(element)` 和 `contains`：

```kotlin
val visitors = setOf("Paula", "Tanya", "Julia")

println("Is it true that Tanya came? It's ${visitors.contains("Tanya")}") // Is it true that Tanya came? It's true
println("And what is her index? ${visitors.indexOf("Tanya")}" ) // And her index is 1
```

`first()` 和 `last()` 访问第一个和最后一个元素。不过由于 `Set` 是无序集合，这个方法其实没啥用：

```kotlin
val students = setOf("Bob", "Larry")
println(students.first()) // Bob
println(students.last()) // Larry
```

使用 `joinToString()` 将集合转换为字符串：

```kotlin
val visitors = setOf("Paula", "Tanya", "Julia")

val joinToString = visitors.joinToString()

println(joinToString) // Paula, Tanya, Julia
```

使用 `containsAll(elements)` 检查是否包含指定集合中的所有元素：

```kotlin
val studentsOfAGroup = setOf("Bob", "Larry", "Vlad")
val studentsInClass = setOf("Vlad")

println("Are all the students in the group in class today? It's ${studentsInClass.containsAll(studentsOfAGroup)}") 
// Are all the students in the group in class today? It's false
```

使用 `+` 获得两个集合的并集，使用 `-` 获得两个集合的差集，两个操作都生成一个**新的集合**：

```kotlin
val productsList1 = setOf("Banana", "Lime", "Strawberry")
val productsList2 = setOf("Strawberry")

val finalProductsList1 = productsList1 + productsList2
println(finalProductsList1) // [Banana, Lime, Strawberry]

val finalProductsList2 = productsList1 - productsList2
println(finalProductsList2) // [Banana, Lime]
```

使用 `toSet()` 将 `MutableList` 转换为 `Set`：

```kotlin
val groceries = mutableListOf("Pen", "Pineapple", "Apple", "Super Pen", "Apple", "Pen")
println(groceries.toSet()) // [Pen, Pineapple, Apple, Super Pen]
```

### 遍历 Set 元素

使用 `for` 循环遍历 `Set` 的元素。示例：
```kotlin
val visitors = setOf("Vlad", "Liza", "Vanya", "Nina")

for (visitor in visitors) {
    println("Hello $visitor!")
}

// output:
// Hello Vlad!
// Hello Liza!
// Hello Vanya!
// Hello Nina!
```

## Mutable Set

`MutableSet` 是 `Set` 的可修改版本。例如，可以添加元素：

```kotlin
val groceries = mutableSetOf("Banana", "Strawberry")
groceries.add("Water")
println(groceries) // [Banana, Strawberry, Water]
```

### 创建 MutableSet

使用 `mutableSetOf` 创建 `MutableSet`：

```kotlin
val students = mutableSetOf("Joe", "Elena", "Bob")
println(students) // [Joe, Elena, Bob]
```

因为编译器可以从上下文推导类型，所以这里不需要指定类型。创建空 `MutableSet` 时必须指定类型：

```kotlin
val points = mutableSetOf<Int>()
println(points) // []
```

可以使用 `toMutableSet()` 将 `Set` 转换为 `MutableSet`：

```kotlin
val students = setOf("Joe", "Elena", "Bob").toMutableSet()
students.add("Bob")
println(students) // [Joe, Elena, Bob]
```

### 添加元素

`MutableSet` 与 `Set` 的属性和方法相同：`size`, `isEmpty()`, `indexOf(element)`, `contains(element)`, `first()`, `last()`。

另外，还有修改集合的方法：

- `add(element)` - 添加一个元素
- `addAll(elements)` - 添加多个元素

示例：

```kotlin
val words = mutableSetOf<String>("Apple", "Coke")
val friendsWords = mutableSetOf<String>("Banana", "Coke")

words.add("Phone")
words.add("Controller")

friendsWords.add("Phone")
friendsWords.add("Pasta")
friendsWords.add("Pizza")

words.addAll(friendsWords)

println(words) // [Apple, Coke, Phone, Controller, Banana, Pasta, Pizza]
```

### 删除元素

删除元素操作：

- `remove(element)` - 删除指定元素
- `clear()` - 删除所有元素
- `removeAll(elements)` - 移除 `elements` 中包含的所有元素

示例：

```kotlin
val groceries = mutableSetOf("Apple", "Water", "Banana", "Pen")

groceries.remove("Apple")
println(groceries) // [Water, Banana, Pen]

val uselessGroceries = setOf("Banana", "Pen")
groceries.removeAll(uselessGroceries)
println(groceries) // [Water]

groceries.clear()
println(groceries) // []
```

### 遍历 Set

依然是用 `for` 循环遍历 `MutableSet` 中的元素：

```kotlin
val places = mutableSetOf("Saint-Petersburg", "Moscow", "Grodno", "Rome")

for (place in places) {
    println(place)
}

// Saint-Petersburg
// Moscow
// Grodno
// Rome
```

## Map

`Map` 是一个不可变集合类型，存储成对对象，可以根据键高效地检索值。不可变对象初始化后，其大小和内容都无法修改。示例：

```kotlin
val students = mapOf(
   "Zhenya" to 5,
   "Vlad" to 4,
   "Nina" to 5
)
println(students) // output: {Zhenya=5, Vlad=4, Nina=5}
```

查询：

```kotlin
val grade = students["Nina"]
println("Nina's grade is: $grade") // output: Nina's grade is: 5
```

### Map 元素

`Map` 的元素用 `Pair` 类表示。例如：

```kotlin
val (name, grade) = Pair("Zhenya", 5) // easy way to get the first and the second values
println("Student name is: $name And their grade is: $grade")
// output: Student name is: Zhenya And their grade is: 5 
```

另外，也可以使用 `.first` 和 `.second` 属性访问 `Pair` 的元素。这种方式比较冗长，所以**推荐使用** `()`。

```kotlin
val p = Pair(2, 3)
println("${p.first} ${p.second}") // 2 3
val (first, second) = p 
println("$first $second")         // 2 3
```

使用 `to` 创建 `Map` 中的元素，即  `Pair`：

```kotlin
val (name, grade) = "Vlad" to 4
println("Student name is: $name And their grade is: $grade")
// output: Student name is: Vlad And their grade is: 4 
```

### Map 初始化

使用 `mapOf<K, V>(vararg pairs: Pair<K,V>)` 创建 `Map`，其中 `K` 是键的类型，`V` 是值的类型。

```kotlin
val staff = mapOf<String, Int>("John" to 1000)
```

非空时可以省略类型：

```kotlin
val staff = mapOf("Mike" to 1500)
```

使用 `emptyMap<K,V>` 创建空 map 不能省略类型：

```kotlin
val emptyStringToDoubleMap = emptyMap<String, Double>()
```

再就是灵活的 `buildMap()` 方法：

```kotlin
val values = mapOf<String, Int>("Second" to 2, "Third" to 3)

val map = buildMap<String, Int> {
    put("First", 1)
    putAll(values)
    put("Fourth", 4)
}
println(map) // output: {First=1, Second=2, Third=3, Fourth=4}
```

### Map 的方法和属性

`Map` 的基本属性和方法：

- `size` 属性对应元素个数
- `isEmpty()` 方法判断 `Map` 是否为空

使用 `[key]` 查看指定 key 对应的值。也可以使用 `get(key)` 方法，效果一样。

示例：

```kotlin
val employees = mapOf(
    "Mike" to 1500,
    "Jim" to 500,
    "Sara" to 1000
)
```

查询：

```kotlin
if (!employees.isEmpty()) {
    println("Number of employees: ${employees.size}")
    println("Mike wants to earn ${employees["Mike"]}")
}
```

使用 `containsKey(key)` 查看 `Map` 是否包含指定 key：

```kotlin
val isWanted = employees.containsKey("Jim")
println("Does Jim want to be our employee? It's $isWanted")
```

是否 `containsValue(value)` 查看是否包含指定 value：

```kotlin
val isAnyoneWilling = employees.containsValue(500)
println("Is anyone willing to earn $500? It's $isAnyoneWilling")
```

### 遍历 Map

依然使用 `for` 循环：

```kotlin
val employees = mapOf(
    "Mike" to 1500,
    "Jim" to 500,
    "Sara" to 1000
)

for (employee in employees)
    println("${employee.key} ${employee.value}")

for ((k, v) in employees)
    println("$k $v")
```

两种方式输出相同结果：

```
// Mike 1500
// Jim 500
// Sara 1000
```

### Map 建议

创建 `Map` 的推荐方式：

```kotlin
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```

访问元素的推荐方式：

```kotlin
println(map.get("a")) // 1
println(map["b"])     // 2, 推荐
```

遍历 `Map` 的推荐方式：

```kotlin
for ((k, v) in map) {
    println("$k -> $v")
}
```

## Mutable Map

`MutableMap` 是与 `Map` 对应的可变版本。

不能修改 `Map`，所以在需要修改数据时，只能创建一个新的 `Map`，然后重新分配引用：

```kotlin
var staff = mapOf( // you cannot reassign an immutable reference, so you need to use var
   "John" to 500,
   "Mike" to 1000,
   "Lara" to 1300
)
staff += "Jane" to 700 // reassignment
println(staff) // output: {John=500, Mike=1000, Lara=1300, Jane=700}
```

这种方式性能较低，使用 `MutableMap` 更合适：

```kotlin
val staff = mutableMapOf(
   "John" to 500,
   "Mike" to 1000,
   "Lara" to 1300
)

staff["Nika"] = 999

println(staff) // output: {John=500, Mike=1000, Lara=1300, Nika=999}
```

### 初始化 `MutableMap`

创建 `MutableMap`：

```kotlin
val students = mutableMapOf<String, Int>("Nika" to 19, "Mike" to 23)
println(students) // output: {Nika=19, Mike=23}
```

省略类型：

```kotlin
val carsPerYear = mutableMapOf(1999 to 30000, 2021 to 202111)
println(carsPerYear) // output: {1999=30000, 2021=202111}
```

使用 `toMutableMap()` 将 `Map` 转换为 `MutableMap`：

```kotlin
val mapCarsPerYear = mapOf(1999 to 30000, 2021 to 202111)
val carsPerYear = mapCarsPerYear.toMutableMap()
carsPerYear[2020] = 2020
println(carsPerYear) // output: {1999=30000, 2021=202111, 2020=2020}}
```

### 添加元素

`MutableMap` 包含 `Map` 所有的属性和方法：`size`, `isEmpty()`, `containsKey(key)`, `containsValue(element)` 等。

`MutableMap` 修改集合的方法：

- `put(key, value)` 添加键值对，**简写形式**：`mutableMap[key] = value`
- `putAll(Map)` - 添加指定 `Map` 中的所有 key-values
- `putIfAbsent(key, value)` - 如果 map 不包含指定 `key`，则添加指定 key-value，否则不加

示例：

```kotlin
val groupOfStudents = mutableMapOf<String, Int>() // empty mutable map
groupOfStudents.put("John", 4)
groupOfStudents["Mike"] = 5
groupOfStudents["Anastasia"] = 10

val studentsFromOregon = mapOf("Alexa" to 7)

groupOfStudents.putAll(studentsFromOregon)
    
println(groupOfStudents) // output: {John=4, Mike=5, Anastasia=10, Alexa=7}
```

对 map 已有的 key，继续添加会覆盖原有值：

```kotlin
val groceries = mutableMapOf<String, Int>() 

groceries["Potato"] = 5  
println(groceries)  // output: {Potato=5}
    
groceries["Potato"] = 10     
println(groceries)  // output: {Potato=10}
```

### 删除元素

删除方法：

- `remove(key)` - 删除指定的 key 和对应的值
- `clear()` - 删除所有元素

示例：

```kotlin
val groceries = mutableMapOf(
    "Potato" to 10,
    "Coke" to 5,
    "Chips" to 7
)

groceries.remove("Potato")
println(groceries) // output: {Coke=5, Chips=7}

groceries.clear()
println(groceries) // output: {}
```

也可以使用 `-=` 删除：

```kotlin
val cars = mutableMapOf<String, Double>()
cars["Ford"] = 100.500
cars["Kia"] = 500.15
    
println(cars)  // output: {Ford=100.5, Kia=500.15}
    
cars -= "Kia"   
println(cars)  // output: {Ford=100.5}
```



