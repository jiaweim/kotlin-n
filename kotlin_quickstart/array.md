# 数组

2026-02-10⭐
@author Jiawei Mao
***
## 简介

数组是最基础的集合类型，用于存储同类型值，创建后大小固定，但可以修改存储的元素。下图是一个长度为 5 的浮点数组：

<img src="./images/array.svg" alt="An array with five floating-point elements" width="400" />

### 创建数组

Kotlin 针对基础类型提供专门的数组类：`IntArray`, `LongArray`. `DoubleArray`, `FloatArray`, `CharArray`, `ShortArray`, `ByteArray`, `BooleanArray`。每个数组分别存储对应类型的元素，并且有各自的创建方法：

- `intArrayOf` 创建 `IntArray`
- `charArrayOf` 创建 `CharArray`
- `doubleArrayOf` 创建 `DoubleArray`

以此类推。例如：

```kotlin
val numbers = intArrayOf(1, 2, 3, 4, 5) // It stores 5 elements of the Int type
println(numbers.joinToString()) // 1, 2, 3, 4, 5

val characters = charArrayOf('K', 't', 'l') // It stores 3 elements of the Char type
println(characters.joinToString()) // K, t, l

val doubles = doubleArrayOf(1.25, 0.17, 0.4) // It stores 3 elements of the Double type
println(doubles.joinToString()) // 1.25, 0.17, 0.4
```

```
1, 2, 3, 4, 5
K, t, l
1.25, 0.17, 0.4
```

`joinToString()` 函数将数组转换为字符串。

在创建数组后，可以在末尾加逗号，不影响创建数组：

```kotlin
val array = intArrayOf(1, 2, 3, 4,) // works since Kotlin 1.4
```

### 创建指定大小数组

创建指定大小数组，需要使用构造函数并指定大小：

```kotlin
val numbers = IntArray(5) // an array of 5 integer numbers
println(numbers.joinToString())

val doubles = DoubleArray(7) // an array of 7 doubles
println(doubles.joinToString())
```

数组填充默认值，对数值类型默认值为 0：

```
0, 0, 0, 0, 0
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0
```

不能修改数组长度，但可以修改其中的元素。

另外，还可以使用 `Array` 构造 函数，提供一个生成元素的 function，该函数根据索引返回数组元素的值。也可以使用 `apply` 集合 `fill()` 方法，用指定值填充数组。

```kotlin
val numbers = IntArray(10) { i -> i + 1 }
println("Numbers: ${numbers.joinToString()}") 
// 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
val onlyOne = IntArray(10) { 1 }
println("Only 1: ${onlyOne.joinToString()}") 
// 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
val onlyMinusOne = IntArray(10).apply { fill(-1) }
println("Only -1: ${onlyMinusOne.joinToString()}") 
// -1, -1, -1, -1, -1, -1, -1, -1, -1, -1
```

### 从控制台读取数组

```kotlin
val numbers = IntArray(5) { readln().toInt() } // on each line single numbers from 1 to 5
println(numbers.joinToString()) // 1, 2, 3, 4, 5
```

该代码从控制台读取 5 个数字，每个数字占一行。

也可以一行包含所有数字，用空格分隔：

```kotlin
// here we have an input string "1 2 3 4 5"

val numbers = readln().split(" ").map { it.toInt() }.toTypedArray()
println(numbers.joinToString()) // 1, 2, 3, 4, 5
```

另外，还可以忽略输入字符串中的多余空格和换行符，使用正则表达式实现：

```kotlin
val regex = "\\s+".toRegex()
val str = "1 2\t\t3  4\t5  6"
val nums = str.split(regex).map { it.toInt() }.toTypedArray()
println(nums.joinToString()) // 1, 2, 3, 4, 5, 6
```

### 数组大小

使用 `size` 属性查看数组包含的元素个数。

```kotlin
val numbers = intArrayOf(1, 2, 3, 4, 5)
println(numbers.size) // 5 
```

创建数组后，其大小无法修改。

### 访问元素

设置元素值：

```kotlin
array[index] = elem
```

获取元素值：

```kotlin
val elem = array[index]
```

数组索引从 `0` 开始，到 `array.size-1` 结束。

示例：

```kotlin
val numbers = IntArray(3) // numbers: 0, 0, 0

numbers[0] = 1 // numbers: 1, 0, 0
numbers[1] = 2 // numbers: 1, 2, 0
numbers[2] = numbers[0] + numbers[1] // numbers: 1, 2, 3

println(numbers[0]) // 1, the first element
println(numbers[2]) // 3, the last element
```

如果索引超出数组范围，抛出 `ArrayIndexOutOfBoundsException`：

```kotlin
val elem = numbers[3]
```

最后一个元素的索引为 `array.size - 1`：

```kotlin
val alphabet = charArrayOf('a', 'b', 'c', 'd')

val last = alphabet[alphabet.size - 1]    // 'd'
val prelast = alphabet[alphabet.size - 2] // 'c'
```

另外，Kotlin 还提供访问首尾元素和最后一个索引的便捷方式：

```kotlin
println(alphabet.first())   // 'a'
println(alphabet.last())    // 'd'
println(alphabet.lastIndex) // 3
```

这种代码可读性更强。

### 比较数组

`contentEquals()` 比较两个数组，当元素相同且顺序一致时返回 `true`：

```kotlin
val numbers1 = intArrayOf(1, 2, 3, 4)
val numbers2 = intArrayOf(1, 2, 3, 4)
val numbers3 = intArrayOf(1, 2, 3)

println(numbers1.contentEquals(numbers2)) // true
println(numbers1.contentEquals(numbers3)) // false
```

另外要注意，`==` 和 `!=` 只是比较引用是否相等，不会比较内容：

```kotlin
val simpleArray = intArrayOf(1, 2, 3, 4)
val similarArray = intArrayOf(1, 2, 3, 4)

println(simpleArray == simpleArray)  // true, simpleArray points to the same object
println(simpleArray === similarArray) // false, similarArray points to another object
```

## 遍历数组

可以使用 `for` 循环遍历整个数组：

```kotlin
for (element in array) {
    // body of loop
}
```

示例：

```kotlin
fun main() {
    val daysOfWeek = arrayOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")
    
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

`array.indices` 属性为数组的有效索引范围，通过该属性遍历数组：

```kotlin
fun main() {
    val daysOfWeek = arrayOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")

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

### 索引范围

通过范围访问子数组：

```kotlin
fun main() {
    val daysOfWeek = arrayOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")

    for (index in 1..5) {
        println("$index: ${daysOfWeek[index]}")
    }
}
```

返回闭区间 `1..5` 的元素：

```kotlin
1: Mon
2: Tues
3: Wed
4: Thur
5: Fri
```

也可以用半开区间：

```kotlin
fun main() {
    val daysOfWeek = arrayOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")

    for (index in 1..<5) {
        println("$index: ${daysOfWeek[index]}")
    }
}
```

```
1: Mon
2: Tues
3: Wed
4: Thur
```

使用 `array.lastIndex` 访问最后一个元素的索引：

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

使用 `downTo` 反向遍历，用 `step` 指定步长：

```kotlin
fun main() {
    val daysOfWeek = arrayOf("Sun", "Mon", "Tues", "Wed", "Thur", "Fri", "Sat")

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

### 从控制台读取数组

```kotlin
fun main() {
    val size = readln().toInt()
    val array = IntArray(size)

    for (i in 0..array.lastIndex) {
        array[i] = readln().toInt()
    }

    for (i in array.lastIndex downTo 0) {
        print("${array[i]} ")
    }
}
```

输入：

```kotlin
5
1
2
3
4
5
```

输出：

```kotlin
5 4 3 2 1 
```

## String Array

Kotlin 没有为字符串提供专门的数组类型，而是通用数组类型，使用 `arrayOf` 创建：

```kotlin
val stringArray = arrayOf("array", "of", "strings")
```

`arrayOf` 可以显式指定类型：

```kotlin
val stringArray = arrayOf<String>("array", "of", "strings")
```

使用 `emptyArray` 创建空数组：

```kotlin
val newEmptyArray = emptyArray<String>()
```

### 访问元素

通过索引访问元素：

```kotlin
val stringArray = arrayOf("sagacity", "and", "bravery")
print(stringArray[2])   // bravery
```

设置数组元素的方式类似：

```kotlin
stringArray[0] = "smart"
print(stringArray[0])   // smart
```

### 输出数组

使用 `joinToString()` 将数组转换为字符串：

```kotlin
val southernCross = arrayOf("Acrux", "Gacrux", "Imai", "Mimosa")
println(southernCross.joinToString())   //  Acrux, Gacrux, Imai, Mimosa
```

### 处理多个数组

- 数组相加

```kotlin
val southernCross = arrayOf("Acrux", "Gacrux", "Imai", "Mimosa")
val stars = arrayOf("Ginan", "Mu Crucis")

val newArray = southernCross + stars
println(newArray.joinToString())    //  Acrux, Gacrux, Imai, Mimosa, Ginan, Mu Crucis
```

- 比较数组

```kotlin
val firstArray = arrayOf("result", "is", "true")
val secondArray = arrayOf("result", "is", "true")
val thirdArray = arrayOf("result")

println(firstArray.contentEquals(secondArray))  //  true
println(firstArray.contentEquals(thirdArray))   //  false
```

### 修改数组内容

不管是 `val` 还是 `var`，都可以通过索引修改数组内容：

```kotlin
val southernCross = arrayOf("Acrux", "Gacrux", "Imai", "Mimosa")
var stars = arrayOf("Ginan", "Mu Crucis")
southernCross[1] = "star"
stars[1] = "star"

println(southernCross[1]) // star
println(stars[1]) // star
```

但是，`val` 和 `var` 在重新赋值上有很大差异，`val` 不允许重新赋值，`var` 可以。

创建空数组：

```kotlin
var southernCross = emptyArray<String>()
```

添加元素扩充数组：

```kotlin
southernCross += "Acrux"
southernCross += "Gacrux"
southernCross += "Imai"
println(southernCross.joinToString())   // Acrux, Gacrux, Imai
```

数组在创建时就大小固定，这里所谓的添加元素，其实是创建新数组，然后为变量重新赋值。

如果使用 `val` 就无法编译：

```kotlin
val southernCross = arrayOf("Acrux", "Gacrux", "Imai", "Mimosa")
southernCross += "Ginan" // will not compile
```

## 多维数组

多维数组，即将数组作为元素放入另一个数组。以二维数组为例，下面创建一个 3 行 4 列全 0 的二维数组：

```kotlin
val array2D = arrayOf(
    arrayOf(0, 0, 0, 0),
    arrayOf(0, 0, 0, 0),
    arrayOf(0, 0, 0, 0)
)
```

内部数组的长度不需要一样：

```kotlin
val array2D = arrayOf(
    arrayOf(0),
    arrayOf(1, 2),
    arrayOf(3, 4, 5))
```

创建空的 `Int` 类型二维数组：

```kotlin
val empty2DInt = arrayOf<Array<Int>>()
```

### 访问元素

二维数组需要两个索引，第一个对应外层数组索引，第二个对应内层嵌套数组的索引：

```kotlin
val array2D = arrayOf(
    arrayOf(0, 1, 2),
    arrayOf(3, 4, 5)
)

println(array2D[0][0])	// 0
```

查看其它元素：

```kotlin
print(array2D[0][0])  // 0
print(array2D[0][1])  // 1
print(array2D[0][2])  // 2
print(array2D[1][0])  // 3
print(array2D[1][1])  // 4
print(array2D[1][2])  // 5
```

### 创建二维数组

创建字符串类型的二维数组：

```kotlin
val arrayOfString2D = arrayOf(
    arrayOf<String>("to", "be", "or"),
    arrayOf<String>("not", "to", "be")
)
```

嵌套数组的类型可以不同。例如：

```kotlin
val arrayOfString2D = arrayOf(
    arrayOf("Practice", "makes", "perfect"),
    arrayOf(1, 2)
)
```

### toString

- 方法一：用 `joinToString()` 输出指定嵌套数组

```kotlin
val arrayString = arrayOf(
    arrayOf("A", "R", "R", "A", "Y")
)
print(arrayString[0].joinToString())    // A, R, R, A, Y
```

- 方法二：对多维数组，`joinToString()` 不好用，此时可以用 `contentDeepToString()` 输出整个数组的内容

```kotlin
val arrayOfChar2D = arrayOf(
charArrayOf('k'),
charArrayOf('o', 't'),
charArrayOf('l', 'i', 'n'))

println(arrayOfChar2D.contentDeepToString())	// [[k], [o, t], [l, i, n]]
```

### 多维数组（>2）

三维数组，即二维数组的每个元素也是数组：

| [0, 1] | [2, 3] |
| ------ | ------ |
| [4, 5] | [6, 7] |

创建三维数组：

```kotlin
val array3D = arrayOf(
    arrayOf(arrayOf(0,1), arrayOf(2,3)),
    arrayOf(arrayOf(4,5), arrayOf(6,7))
)

println(array3D.contentDeepToString())  // [[[0, 1], [2, 3]], [[4, 5], [6, 7]]]
```

访问元素：

```kotlin
println(array3D[0][0][1])   // 1
println(array3D[1][0][1])   // 5
println(array3D[1][1][1])   // 7
```

更高维度的数组的使用方式类似。