# 控制流

## if

条件表达式使得程序可以根据布尔表达式的值执行不同操作。条件表达式有多种形式：

- if
- if-else
- if-else-if
- 嵌套 if

与其它编程语言不同，Kotlin 的 `if` 是表达式，而非语句。即 `if` 会返回一个值，为 `if` 块中最后一个表达式的值。例如：

```kotlin
val max = if (a > b) {
    println("Choose a")
    a
} else {
    println("Choose b")
    b
}
```

其中，变量 `max` 的值为最后一个表达式的值。需要注意，使用表达式样式的 `if`，必须包含一个 `else` 分支。

如果分支只有一个语句，则可以省略大括号：

```kotlin
val max = if (a > b) a else b
```

不声明变量，直接使用 `if` 表达式的值：

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

`when` 表达式简化多重 if-else-if。例如：

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

`when` 表达式检查 `number` 变量的值，执行匹配条件的表达式。

和 `if` 表达式类似，`when` 也可以作为表达式使用，返回一个值。返回的值为匹配分支的最后一个表达式的结果。例如：

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

`when` 表达式也可用于范围检查和复杂的布尔表达式。例如：

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

检查 `c` 是否在 `a` 和 `b` 之间，可以采用如下方式：

```kotlin
val within = a <= c && c <= b
```

该代码没问题，不过Kotlin 提供了一种更简洁的方式：

```kotlin
val within = c in a..b
```

这里 `a..b` 是从 `a` 到 `b` 的区间（闭区间，包含两个边界）。`in` 是用于检查值是否在某个区间的关键字。

也可以构造开闭区间：`a..<b`，从 `a` 到 `b`， 但不包含 `b` 边界。

示例：

```kotlin
println(5 in 5..15)  // true
println(12 in 5..15) // true
println(15 in 5..15) // true
println(20 in 5..15) // false
println(5 in 5..<15)  // true
println(15 in 5..<15) // false
```

 还可以使用 `!in` 直接检查某个数值**不在**某个范围。

```kotlin
val notWithin = 100 !in 10..99 // true
```

可以使用逻辑运算符组合不同区间：

```kotlin
val within = c in 5..10 || c in 20..30 || c in 40..50 // true if c is within at least one range
```

range 本身是一个类型，可以分配给变量：

```kotlin
val range = 100..200
println(300 in range) // false
```

除了整数类型，也使用字符或字符串（按字典顺序）：

```kotlin
println('b' in 'a'..'c') // true
println('k' in 'a'..'e') // false

println("hello" in "he".."hi") // true
println("abc" in "aab".."aac") // false
```

## when

`when` 表达式根据变量值执行不同操作。

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

**示例**：读取三个整数 `a`, `b`, `c`，然后尝试确定如何通过 `a` 和 `b` 计算 `c`。如果有多种方式计算 `c`，只取第一种

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

`when` 表达式也可以不要参数。此时每个分支是一个简单的布尔表达式，值为 `true` 的分支执行。如果多个分支为 `true`，只执行第一个分支。

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

