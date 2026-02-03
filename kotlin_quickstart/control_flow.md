# 控制流

2026-01-29⭐
@author Jiawei Mao
***

## if

条件表达式使得程序可以根据布尔表达式的值执行不同操作。条件表达式有多种形式：

- if
- if-else
- if-else-if
- 嵌套 if

与其它编程语言不同，Kotlin 的 `if` 是**表达式**而非语句，即 `if` 会返回一个值，返回的值为 `if` 块中最后一个表达式的值。例如：

```kotlin
val max = if (a > b) {
    println("Choose a")
    a
} else {
    println("Choose b")
    b
}
```

其中，变量 `max` 的值为最后一个表达式的值。需要注意，使用**表达式样式**的 `if`，必须包含一个 `else` 分支。

如果分支只有一个语句，可以省略大括号：

```kotlin
val max = if (a > b) a else b
```

也可以不声明变量，直接使用 `if` 表达式的值：

```kotlin
fun main() {
    val a = readln().toInt()
    val b = readln().toInt()

    println(if (a == b) {
        "a equal b"
    } else if (a > b) {
        "a is greater than b"
    } else {
        "a is less than b"
    })
}
```

### 使用建议

如果需要获得结果，建议使用表达式样式：

```kotlin
val max = if (a > b) a else b // one line
```

Kotlin 的 `if` 表达式类似 Java 的三元运算符：

```java
final String msg = num > 10 
  ? "Number is greater than 10" 
  : "Number is less than or equal to 10";
```

### when

`when` 简化多重 if-else-if。例如：

```kotlin
val number = 5

when (number) {
    1 -> println("One")
    2 -> println("Two")
    3 -> println("Three")
    4 -> println("Four")
    else -> println("Number is greater than four")
}
```

这里 `when` 检查 `number` 变量的值，执行匹配条件的表达式。

和 `if` 类似，`when` 也可以作为表达式使用，返回一个值。返回的值为匹配分支的最后一个表达式的结果。例如：

```kotlin
val number = 3
val message = when (number) {
    1 -> "One"
    2 -> "Two"
    3 -> "Three"
    4 -> "Four"
    else -> "Number is greater than four"
}

println(message) // Output: Three
```

### when 范围匹配

`when` 可用于范围检查和复杂的布尔表达式。例如：

```kotlin
val number = 15

when {
    number < 0 -> println("Negative number")
    number in 1..10 -> println("Number between 1 and 10")
    number % 2 == 0 -> println("Even number")
    else -> println("Odd number greater than 10")
}
```

## Range

- 检查 `c` 是否在 `a` 和 `b` 之间，可以采用如下方式：

```kotlin
val within = a <= c && c <= b
```

该代码没问题，不过Kotlin 提供了一种更简洁的方式：

```kotlin
val within = c in a..b
```

这里 `a..b` 是从 `a` 到 `b` 的区间（闭区间，包含两个边界）。`in` 是用于检查值是否在某个区间的关键字。

- 构造开闭区间：`a..<b`，从 `a` 到 `b`， 但不包含 `b` 边界。示例：

```kotlin
println(5 in 5..15)  // true
println(12 in 5..15) // true
println(15 in 5..15) // true
println(20 in 5..15) // false
println(5 in 5..<15)  // true
println(15 in 5..<15) // false
```

- 使用 `!in` 直接检查某个数值**不在**某个范围

```kotlin
val notWithin = 100 !in 10..99 // true
```

- 使用逻辑运算符组合不同区间

```kotlin
val within = c in 5..10 || c in 20..30 || c in 40..50 // true if c is within at least one range
```

- range 本身是一个类型，可以分配给变量：

```kotlin
val range = 100..200
println(300 in range) // false
```

- 除了整数类型，也使用字符或字符串（按字典顺序）

```kotlin
println('b' in 'a'..'c') // true
println('k' in 'a'..'e') // false

println("hello" in "he".."hi") // true
println("abc" in "aab".."aac") // false
```

## when

`when` 根据变量值执行不同操作。

**示例**：执行两个整数的加法、减法或乘法。使用 `when` 确定执行的操作

```kotlin
fun main(){
    val (var1, op, var2) = readln().split(" ")

    val a = var1.toInt()
    val b = var2.toInt()

    when (op) {
        "+" -> println(a + b)
        "-" -> println(a - b)
        "*" -> println(a * b)
        else -> println("Unknown operator")
    }
}
```

`op` 依次与 `"+"`, `"-"`, `"*"` 比较，当全部不匹配，执行 `else` 分支。其中 `else` 分支是可选的（可以不写）。

可以将多个条件合并到一个分支，用逗号分隔（默认也可以选择加逗号）。例如：

```kotlin
when (op) {
    "+", "plus" -> println(a + b)
    "-", "minus", -> println(a - b) // trailing comma
    "*", "times" -> println(a * b)
    else -> println("Unknown operator")
}
```

此时该代码对 `5 + 8` 和 `5 plus 8` 都可以执行。

在 `when` 的不同分支中，也可以包含多个语句：

```kotlin
when (op) {
    "+", "plus" -> {
        val sum = a + b
        println(sum)
    }
    "-", "minus" -> {
        val diff = a - b
        println(diff)
    }
    "*", "times" -> {
        val product = a * b
        println(product)
    }
    else -> println("Unknown operator")
}
```

### when 表达式

`when` 可以返回结果，此时 `else` 分支是必需的。示例：

```kotlin
val result = when (op) {
    "+" -> a + b
    "-" -> a - b
    "*" -> a * b
    else -> "Unknown operator"
}
println(result)
```

如果分支包含多个语句，则最后一行必须是一个数值或返回值的表达式。例如：

```kotlin
"+" -> {
    val sum = a + b
    sum
}
```

### 分支条件和 range

Kotlin 的 `when` 与 Java 的 `switch` 类似，但更强大，它除了匹配值，还可以执行更复杂的检查。

**示例**：读取三个整数 `a`, `b`, `c`，然后尝试通过 `a` 和 `b` 计算 `c`。如果有多种方式计算 `c`，只取第一种

```kotlin
fun main(){
    val (var1, var2, var3) = readln().split(" ")

    val a = var1.toInt()
    val b = var2.toInt()
    val c = var3.toInt()

    println(when (c) {
        a + b -> "$c equals $a plus $b"
        a - b -> "$c equals $a minus $b"
        a * b -> "$c equals $a times $b"
        else -> "We do not know how to calculate $c"
    })
}
```

如果输入为 `5 3 2`，输出 `2 equals 5 minus 3`；输入 `0 0 0`，输出 `0 equals 0 plus 0`。

**示例**：检查值是否属于某个范围

```kotlin
when (n) {
    0 -> println("n is zero")
    in 1..10 -> println("n is between 1 and 10 (inclusive)")
    in 25..30 -> println("n is between 25 and 30 (inclusive)")
    else -> println("n is outside a range")
}
```

如果整数 `n` 为 `0`，则执行第一个分支；如果 `n` 在 1-10 之间，则执行第二个分支；如果 `n` 在 25-30 之间，则执行第三个分支；不满于以上所有条件，则执行 `else` 分支。

也可以将多个范围检查用逗号隔开，合并到一个分支：

```kotlin
in a..b, in c..d -> println("n belongs to a range")
```

### 无参 when

`when` 表达式也可以不要参数。此时每个分支是一个布尔表达式，值为 `true` 的分支执行。如果多个分支为 `true`，只执行第一个分支。

示例：

```kotlin
fun main(){
    val n = readln().toInt()
    
    when {
        n == 0 -> println("n is zero")
        n in 100..200 -> println("n is between 100 and 200")
        n > 300 -> println("n is greater than 300")
        n < 0 -> println("n is negative")
        // else-branch is optional here
    }
}
```

## repeat

`repeat(n)` 是最简单的循环，`n` 是一个整数，表示重复次数。语法：

```kotlin
repeat(n) {
    // statements
}
```

**示例**：打印 3 次 `Hello`

```kotlin
fun main() {
    repeat(3) {
        println("Hello")
    }
}
```

输出：

```
Hello
Hello
Hello
```

> [!TIP]
>
> 如果 `n` 为 0 或负数，忽略该循环。如果 `n` 为 1，执行一次

`it` 表示当前循环索引，例如：

```kotlin
fun main() {
    repeat(3) {
        println(it)
    }
}
```

输出：

```
0
1
2
```

### 循环中读取和处理数据

在 `repeat` 语句中从标准输入读取数据、声明变量甚至执行计算。

**示例**：从标准输入读取数字，然后计算它们的加和。其中，第一个数字为数字个数，不用于计算加和

```kotlin
fun main() {    
    val n = readln().toInt()
    var sum = 0
    
    repeat(n) {
        val next = readln().toInt()
        sum += next
    }
    
    println(sum)
}
```

## for 循环

`for` 循环可用于迭代数值范围、数组以及集合对象。`for` 循环语法：

```kotlin
for (element in source) {
    // body of loop
}
```

### 迭代范围

迭代一个整数范围的所有元素：

```kotlin
for (i in 1..4) {
    println(i)    
}
```

```
1
2
3
4
```

或者指定范围的字符：

```kotlin
for (ch in 'a'..'c') {
    println(ch)
}
```

```
a
b
c
```

注意，这里不能使用类似 `"da".."dd` 的字符串范围获取 ``"da" "db" "dc" "dd"`。在范围中只能使用单个字符。

### 迭代字符串

`String` 包含字符序列，迭代字符串，访问每个字符：

```kotlin
val str = "Hello!"
for (ch in str) {
    println(ch)    
}
```

```
H
e
l
l
o
!
```

### 反向迭代

```kotlin
for (i in 4 downTo 1) {
    println(i)
}
```

```
4
3
2
1
```

> [!NOTE]
>
> 反向迭代只能用 `in 4 downTo 1`，不能用 `in 4..1`。

### 不包含边界

使用 `until` 代替 `..`，表示不包含上限：

```kotlin
for (i in 1 until 4) {
    println(i)
}
```

只打印 1 到 3.

### 指定步长

使用 `step` 指定步长。

示例：打印 `1..7` 之间的奇数

```kotlin
for (i in 1..7 step 2) {
    println(i)
}
```

```
1
3
5
7
```

也可以反向：

```kotlin
for (i in 7 downTo 1 step 2) {
    println(i)
}
```

```
7
5
3
1
```

### 示例：阶乘

```kotlin
fun main() {
    val n = readln().toInt()
    var result = 1 // starting value of the factorial

    for (i in 2..n) { // the product from 2 to n
        result *= i
    }

    println(result)
}
```

### 示例：偶数乘法

使用嵌套 `for` 循环打印 2 到 10 偶数之间的乘法：

```kotlin
fun main() {
    for (i in 2..10 step 2) {
        for (j in 2..10 step 2) {
            print(i * j)
            print('\t')  // print the product of i and j followed by one tab
        }
        println()
    }
}
```

```
4   8   12  16  20  
8   16  24  32  40  
12  24  36  48  60  
16  32  48  64  80  
20  40  60  80  100  
```

### for 循环总结

```kotlin
for (i in 1..6) { ... }        // closed range: 1, 2, 3, 4, 5, 6
for (i in 1 until 6) { ... }   // half-open range: 1, 2, 3, 4, 5
for (x in 1..6 step 2) { ... } // step 2: 1, 3, 5
for (x in 6 downTo 1) { ... }  // closed range, backward order: 6, 5, 4, 3, 2, 1 
```

## while 循环

下面介绍 `while` 和 `do...while` 循环，两者都包含执行者主体和条件，只是执行顺序不同：

- `while` 先检查条件，再执行
- `do...while` 先执行，再检查条件

`do...while` 使用相对较少，一个典型用例：从标准输入读取内容，直到用户输入指定内容停止。

### while

`while` 循环语法：

```kotlin
while (condition) {
    // body: do something repetitive
}
```

当 `condition` 为 `true`，执行循环内部语句。即在执行前先检查是否满足条件。

无限循环：

```kotlin
while (true) {
    // body: do something indefinitely
}
```

**示例**：当整数小于 5，打印整数

```kotlin
fun main() {
    var i = 0

    while (i < 5) {
        println(i)
        i++
    }

    println("Completed")
}
```

### do...while

`do...while` 先执行，然后检查条件，如果条件为 `true`，则重复执行。

```kotlin
do {
    // body: do something
} while (condition)
```

**示例**：从标准输入读取整数，并打印整数。如果读取的整数不为正整数，打印后即停止。

```kotlin
fun main() {
    do {
        val n = readln().toInt()
        println(n)
    } while (n > 0)
}
```

输入：

```
1
2
4
0
```

输出：

```
1
2
4
0
```

## jump 和 return

Kotlin 提供了 3 个结构跳转表达式：

- break
- continue
- return

以及配套使用的 labels。

### break

`break` 表达式用于终止外层循环：

```kotlin
for (i in 1..10) {  
   // do something
   if (checkCondition){  
       break 
   }  
}
```

当 `if` 条件语句执行 `break`，`for` 循环终止。

示例：

```kotlin
println("Before the loop")  
for (i in 1..10) {  
    if (i == 3) {  
        break  
    }  
    println(i)  
}  
println("After the loop")  
```

```
Before the loop  
1
2
After the loop
```

### continue

跳过当前循环的余下代码，执行下一个循环：

```kotlin
var result = ""
for (i in "hello world") {
    if (i == 'o') continue
    result += i
}
println(result)
```

```
hell wrld
```

### 内部循环和跳转

示例：

```kotlin
for (i in 1..4) {
    for (j in 1..4) {
        if (j == 2) continue // you want to ignore j = 2
        if (i <= j) break    // you will print the string if i is greater than j
        println("i = $i, j = $j")
    }
    println("Finished to examine i = $i")
}
```

```
Finished to examine i = 1
i = 2, j = 1
Finished to examine i = 2
i = 3, j = 1
Finished to examine i = 3
i = 4, j = 1
i = 4, j = 3
Finished to examine i = 4
```

可以看到，`break` 和 `continue` 都只终止紧挨着的内部循环。

### Label

label 是以 `@` 结尾的识别符。除了 Kotlin 的保留单词，可以用任何单词作为 label。例如，为循环添加 label：

```kotlin
loop@ for (i in 1..10) {
    // do something
}
```

下面将 label 与跳转表达式结合：

```kotlin
loop@ for (i in 1..3) { 
    for (j in 1..3) {
        println("i = $i, j = $j")   
        if (j == 3) break@loop  
    }  
}  
```

在第三次迭代，内部和外部 `for` 循环都终止：

```
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
```

再试试 `continue`：

```kotlin
loop@ for (i in 1..3) {
    for (j in 1..3) {
        for (k in 1..3) {
            if (k == 2) continue@loop
            println("i = $i, j = $j, k = $k")
        }
    }
}
```

```
i = 1, j = 1, k = 1
i = 2, j = 1, k = 1
i = 3, j = 1, k = 1
```

通过 label 直接跳转到外部循环。

### when 和结构跳转表达式

Kotlin 1.4+ 在 `when` 中可以使用 `break` 和 `continue`。

在 Kotlin 1.4 之前，需要编写如下的嗲吗：

```kotlin
Loop@ for (i in 1..10) {
    when (i) {
        3 -> continue@Loop
        6 -> break@Loop
        else -> println(i)
    }
}
```

很容易理解其功能，当 `i` 等于 3，跳过循环，当 `i` 等于 6，终止循环。结果：

```
1
2
4
5
```

现在不需要这些标签，效果相同：

```kotlin
for (i in 1..10) {
    when (i) {
        3 -> continue
        6 -> break
        else -> println(i)
    }
}
```

### return

没有 label 的 `return` 语句直接将结果返回到最近的外部函数。当需要跳出循环或退出当前函数非常有用：

```kotlin
fun foo() {
    val number = intArrayOf(1, 2, 3, 4, 5)
    for (it in number) {
        if (it == 3) return // non-local return directly to the caller of foo()
        print(it)
    }
    println("this point is unreachable")
}

fun main() { 
    foo() // calling foo()
    println()
    println("foo() is over")
    for (i in 1..10) {
        for (j in 1..10) {
            println("i = $i, j = $j")
            if (j == 3) return // local return to the caller of main()
        }
        println("this point is unreachable")
    }
    println("this point is unreachable")
}
```

```
12
foo() is over
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
```

`return` 也可以与 label 结合使用，但情况更复杂。晚会儿再讨论。

