# 数据类型和交互

2026-01-12⭐
@author Jiawei Mao
***
## 数据类型

变量的数据类型，决定可以对该变量执行哪些操作。

变量类型在声明时确定：

```kotlin
val text = "Hello, I am studying Kotlin now."
val n = 1
```

Kotlin 知道 `text` 是字符串，而 `n` 是数字。Kotlin 通过类型推断确定变量类型。

通过类型推断方式声明变量：

```kotlin
val/var identifier = initialization
```

也可以在声明变量时指定变量类型：

```kotlin
val/var identifier: Type = initialization 
```

**示例**：与上一个示例相同的变量，但指明类型

```kotlin
val text: String = "Hello, I am studying Kotlin now."
val n: Int = 1
```

`Int` 表示整数类型，`String` 表示字符串类型。

在实践中，两种声明变量的方式都有人使用。类型推断更简洁，但延迟初始化的变量，则必须先指定类型。例如：

```kotlin
val greeting // error
greeting = "hello"
```

这里报错，是因为在声明变量时不赋值，Kotlin 无法推断类型。此时应指明类型：

```kotlin
val greeting: String // ok
greeting = "hello"
```

### 类型错配

数据类型最重要的功能之一，是防止你给变量分配不合适的值。例如：

```kotlin
val n: Int = "abc" // Type mismatch: inferred type is String but Int was expected
```

这里可以看到类型不匹配的错误。

## 基本类型

### 数字

Kotlin 为整数和小数提供了多种类型。

**整数类型**包括：`Long`, `Int`, `Short`, `Byte` （从大到小）。这些类型代表不同的整数区间，范围在 $-(2^{n-1})$ 到 $(2^{n-1})-1$ 之间，其中 `n` 为位数（bit）。

| 类型  | bit 数            | 范围                    |
| ----- | ----------------- | ----------------------- |
| Byte  | 8 bits (1 byte)   | [-128, 127]             |
| Short | 16 bits (2 bytes) | [-32768, 32767]         |
| Int   | 32 bits (4 bytes) | [$-2^{31}$, $2^{31}-1$] |
| Long  | 64 bits (8 bytes) | [$-2^{63}$, $2^{63}$-1] |

类型大小无法更改，不依赖于操作系统。

`Int` 和 `Long` 是最常见的整数类型。在实践中尽量使用 `Int`。如果需要更大范围，再考虑使用 `Long`：

```kotlin
val zero = 0 // Int, 默认
val one = 1  // Int
val oneMillion = 1_000_000  // Int

val twoMillion = 2_000_000L           // Long, 因为默认有 L
val bigNumber = 1_000_000_000_000_000 // Long, 超出 Int 范围，Kotlin 自动选择 Long
val ten: Long = 10                    // Long, 指明类型

val shortNumber: Short = 15 // Short, 指明类型
val byteNumber: Byte = 15   // Byte, 指明类型
```

浮点数包含小数部分。Kotlin 提供了两种浮点数类型：`Double` (64 bits) 和 `Float` (32 bits)。它们都只能存储有限的小数点位数，`Float` 6-7 位，`Double` 14-16 位。在实践中通常使用 `Double` 类型：

```kotlin
val pi = 3.1415              // Double, 默认
val e = 2.71828f             // Float, 指定类型
val fraction: Float = 1.51f  // Float, 指定类型
```

查看类型的最小值和最大值：

```kotlin
println(Int.MIN_VALUE)  // -2147483648
println(Int.MAX_VALUE)  // 2147483647
println(Long.MIN_VALUE) // -9223372036854775808
println(Long.MAX_VALUE) // 9223372036854775807
```

查看类型对应的字节或位数（1 byte= bits）：

```kotlin
println(Int.SIZE_BYTES) // 4
println(Int.SIZE_BITS)  // 32
```

### 字符

字符类型用 `Char` 表示，字符的位数与 `Short` 相同（2 字节）。示例：

```kotlin
val lowerCaseLetter = 'a'
val upperCaseLetter = 'Q'
val number = '1'
val space = ' '
val dollar = '$'
```

### 布尔

布尔类型用 `Boolean` 表示。`Boolean` 只有两个值：`true` 和 `false`：

```kotlin
val enabled = true
val bugFound = false
```

在条件语句中不可避免会用到布尔类型。

### String

`String` 表示字符串，是使用最广泛的类型之一。

```kotlin
val creditCardNumber = "1234 5678 9012 3456"
val message = "Learn Kotlin instead of Java."
```

## 标准输出

**标准输出**是在设备上显示信息的基本操作。标准输出默认在屏幕上显示数据，但也可以重定向到文件。

### println

Kotlin 提供两个写入标准输出的函数：`println` 和 `print`。

`println` 在屏幕上显示一个字符串，并换行。例如：

```kotlin
println("I")
println("know")
println("Kotlin")
println("well.")
```

```
I
know
Kotlin
well.
```

`println()` 不加内容则显示空行：

```kotlin
println("Kotlin is a modern programming language.")
println() // 打印空行
println("It is used all over the world!")
```

```
Kotlin is a modern programming language.

It is used all over the world!
```

`print()` 函数打印值，但不换行。例如：

```kotlin
print("I ")
print("know ")
print("Kotlin ")
print("well.")
```

```
I know Kotlin well.
```

### 打印数字和字符

`println` 和 `print` 除了可以打印字符串，也可以打印数字和字符。例如：

```kotlin
print(108)   // 打印数字
print('c')   // 打印字符
print("Q")   // 打印字符串
println('3') // 打印字符

print(22)
print('E')
print(8)
println('1')
```

```
108cQ3
22E81
```

### `$` 运算符

在 Kotlin 中，`$` 用于在字符串模板中插入变量或表达式的值。

例如，插入变量值：

```kotlin
val name = "Alice"
println("Hello, $name!") // 输出: Hello, Alice!
```

插入表达式值：

```kotlin
val a = 5
val b = 10
println("Sum of $a and $b is ${a + b}") // 输出: Sum of 5 and 10 is 15
```

> [!NOTE]
>
> 当需要插入复杂的表达式，或者访问对象属性，可以在表达式两侧加大括号 `{}`

打印 `$`：

```kotlin
val a = 20

// $ 后为 .，不是变量，不需要转义
println("The price is $a$.") // 输出:  The price is 20$. 

// 第二个 $ 插入变量值，第一个作为字面量处理
println("The price is $$a.") // 输出: The price is $20. 

// \$ 表示将 $ 以字面量处理，不做特殊解释
println("The price is \$a.") // 输出: The price is $a.

// 转义示例 2: \$
println("The price is \$$a.") // 输出: The price is $20. 
```

## 函数调用

函数（function）是一系列指令的集合，在程序中可以通过函数名称调用函数。

### 函数参数

使用函数名称加括号调用函数。如果函数需要参数，则将参数放在括号中。

例如，使用一个参数调用`println` 函数：

```kotlin
val text = "Hello"
println(text)
```

该函数也可以不要参数，用来打印换行符：

```kotlin
println()
```

所以，通常调用的一般形式如下：

```kotlin
function1() // 不带参数调用
function2(arg1) // 使用一个参数调用
function3(arg1, arg2) // 使用两个参数调用
// ... and so on
```

其中，`function1`, `function2` 和 `function3` 为参数名称。

### 生成结果

有些函数不仅接受参数，还可以生成一些结果。可以将函数计算的结果分配个变量：

```kotlin
val result = function(arg)
```

例如，返回数字绝对的函数：

```kotlin
val number = -10
val nonNegNumber = Math.abs(number)
```

所有函数都会返回一个结果，包括 `println` 函数：

```kotlin
val result = println("text")
println(result) // kotlin.Unit
```

返回的 `kotlin.Unit` 等价于 Java 中的 `void`，表示没有结果。

## Scanner 标准输入

Java 提供的 `Scanner` 是一种从标准输入读取数据的方式，在 Kotlin 中也可以使用。

实例化 `Scanner`：

```kotlin
val scanner = Scanner(System.`in`)
```

其中，`System.in` 代表标准输入流。`scanner` 将其作为数据源，并提供一套读取数据的方法。

从标准输入读取数据：

```kotlin
val line = scanner.nextLine() // 读取一行，例如, "Hello, Kotlin"
val num = scanner.nextInt()   // 读取一个整数，例如, 123
val string = scanner.next()   // 读取一个字符串,例如, "Hello"
```

> [!NOTE]
>
> `scanner.next()` 只读取一个单词，而不是一行。如果用户输入 `Hello, Kotlin`，那么只读取 `Hello,`。

调用 `scanner.nextLine()` 或 `scanner.nextInt()` 后，程序会预设输入的数据类型。

**示例**：从标准输入读取两个数字并输出：

```kotlin
import java.util.Scanner

fun main() {
    val scanner = Scanner(System.`in`) // reads data

    val num1 = scanner.nextInt() // 读取第一个数字
    val num2 = scanner.nextInt() // 读取第二个数字

    println(num2) // 打印第二个数字
    println(num1) // 打印第一个数字
}
```

使用完 `Scanner`，需要调用 `close()` 关闭，否则会一直占用资源：

```kotlin
scanner.close()
```

### 自定义分隔符

`Scanner` 支持直接输入数据，例如：

```kotlin
val scanner = Scanner("123_456")
```

但如何拆分 `123` 和 `456`？`Scanner` 支持通过 `useDelimiter()` 自定义分隔符。

例如：

```kotlin
val scanner = Scanner("123_456")
scanner.useDelimiter("_")
println(scanner.nextInt()) // 123
println(scanner.nextInt()) // 456
```

### 检查下一个元素

假设有 `Scanner` 有如下数据：

```kotlin
val scanner = Scanner("Hello, Kotlin!")
```

接下来，读取单词：

```kotlin
val word1 = scanner.next()
val word2 = scanner.next()
val word3 = scanner.next()
```

前两次没问题，第三次会抛出 `NoSuchElementException` 异常，因此数据只包含两个元素。对该问题，可以提前检查是否有下一个元素：

```kotlin
val scanner = Scanner("Hello, Kotlin!")

if (scanner.hasNext()) {
    val word1 = scanner.next() // Hello,
}
if (scanner.hasNext()) {
    val word2 = scanner.next() // Kotlin!
}
if (scanner.hasNext()) { // 返回 false
    val word3 = scanner.next()
}
```

## String 基础

字符串是由 0 到多个字符组成的序列，由双引号包围。

### 字符串长度

字符串长度，即字符串包含的字符个数，使用 `.length` 获得，返回 `Int` 类型：

```kotlin
val language = "Kotlin"
println(language.length) // 6

val empty = ""
println(empty.length) // 0
```

### 连接字符串

连接字符串：将两个字符串串联在一起。

两个字符串通过 `+` 连接：

```kotlin
val str1 = "ab"
val str2 = "cde"
val result = str1 + str2 // "abcde"
```

连接字符串操作，会创建一个新的字符串，不影响原来的两个字符串：

```kotlin
val one = "1"
val two = "2"
val twelve = one + two 
println(one)      // 1, no changes
println(two)      // 2, no changes
println(twelve)   // 12
```

通过连接多个字符串：

```kotlin
val firstName = "John"
val lastName = "Smith"
val fullName = firstName + " " + lastName
```

### 连接其它值

除了字符串，`+` 也支持将起来类型值附加到字符串，该值会自动转换为字符串。例如：

```kotlin
val stringPlusBoolean = "abc" + 10 + true
println(stringPlusBoolean) // abc10true

val code = "123" + 456 + "789"
println(code) // 123456789
```

表达式的第一个值必须为 `String` 类型。否则会报错，例如：

```kotlin
val errorString = 10 + "abc" // an error here!
```

再看一个情况：

```kotlin
val stringAndNumbers = "abc" + 11 + 22
println(stringAndNumbers) // abc1122
```

该表达式从左向右执行，所以先添加 11 到 `"abc"` 得到 `"abc11"`，然后将 `22` 添加到 `"abc11"` 得到 `"abc1122"`。

字符比较特殊，可以作为串联表达式的第一个值：

```kotlin
val charPlusString = 'a' + "bc"
println(charPlusString) // abc
val stringPlusChar = "de" + 'f'
println(stringPlusChar) // def
```

### 重复字符串

使用 `repeat` 函数可以将一个字符串重复指定次数。例如：

```kotlin
print("Hello".repeat(4)) // HelloHelloHelloHello
```

### Raw String

在字符串中使用特殊字符，如制表符、引号，可以借助转义完成。例如：

```kotlin
// prints 'H' is the first letter of "Hello world!" string.
println("\'H\' is the first letter of \"Hello world!\" string.")
```

如果是大段包含换行和特殊字符的文本，采用该方式阅读起来很麻烦。为此，Kotlin 提供了 raw string，将一段字符串作为字面量处理，用 `"""` 括起来：

```kotlin
val largeString = """
    This is the house that Jack built.
      
    This is the malt that lay in the house that Jack built.
       
    This is the rat that ate the malt
    That lay in the house that Jack built.
       
    This is the cat
    That killed the rat that ate the malt
    That lay in the house that Jack built.
""".trimIndent() // 删除收尾空白和缩进
print(largeString)
```

```
This is the house that Jack built.

This is the malt that lay in the house that Jack built.

This is the rat that ate the malt
That lay in the house that Jack built.

This is the cat
That killed the rat that ate the malt
That lay in the house that Jack built.
```

`.trimIndent()` 删除所有行公共最小缩进，以及第一个和最后一个空行。例如：

```kotlin
val unevenString = """
        123
         456
          789""".trimIndent()
print(unevenString)
println()

val rawString = """123
         456
          789
""".trimIndent()
print(rawString )
```

```
123
 456
  789

123
         456
          789
```

## String templates

通过字符串模板可以在字符串中插入变量值。

例如，显示某地今天的温度：

```kotlin
The temperature in ... is ... degrees Celsius.
```

其中 `...` 代表地名和温度。

现在有两个变量 `city` 和 `temp`，可以通过字符串连接构建结果：

```kotlin
val city = "Paris"
val temp = "24"

println("The temperature in " + city + " is " + temp + " degrees Celsius.")
```

这很简单，但不够优雅。Kotlin 提供的字符串模板方法更为简洁，使用 `$` 插入变量值：

```kotlin
val city = "Paris"
val temp = "24"

println("The temperature in $city is $temp degrees Celsius.")
```

所得结果相同，但代码可读性更强。

### 表达式模板

使用字符串模板，可以将任意表达式的结果放入字符串。不过需要将表达式放在 `{}` 中。例如：

```kotlin
val language = "Kotlin"
println("$language has ${language.length} letters in the name")
```

```
Kotlin has 6 letters in the name
```

其中，`{language.length}` 是要计算的表达式。如果没有大括号，输出如下：

```kotlin
Kotlin has Kotlin.length letters in the name
```

因此，在字符串模板中对表达式必须使用 `{}`。

> [!TIP]
>
> Kotlin 团队强烈推荐使用字符串模板。
>
> Kotlin 有许多工人的推荐代码样式，详细列表可参考 [Idioms](https://kotlinlang.org/docs/idioms.html)

## 数学运算

### 二元运算符

Kotlin 有 5 个算术运算符：

- 加：`+`
- 减：`-`
- 乘：`*`
- 整除：`/`
- 模：`%`

它们都是二元运算符，即需要两个操作数（operand）。操作符指运算符应用的值或变量，例如，在表达式 `1+3` 中，1 和 3 是操作数，`+` 是运算符。

**示例**：加减乘运算

```kotlin
println(13 + 25) // prints 38
println(20 + 70) // prints 90

println(70 - 30) // prints 40
println(30 - 70) // prints -40

println(21 * 3)  // prints 63
println(20 * 10) // prints 200
```

`/` 运算符为整除，即只保留结果的整数部分：

```kotlin
println(8 / 3)  // prints 2
println(41 / 5) // prints 8
```

`%` 计算除法的余数：

```kotlin
println(10 % 3) // 1
println(12 % 4) // 0
```

### 复杂表达式

合并多次运算的表达式称为复杂表达式：

```kotlin
println(1 + 3 * 4 - 2) // prints 11
```

计算顺序与算术运算规则一致，乘法优先级高于加减法。

可以使用括号指定执行顺序：

```kotlin
println((1 + 3) * (4 - 2)) // prints 8
```

### 一元运算符

一元运算符只有一个操作数。

- 一元加号（unary plus）直接输出值，为可选运算符，实践中可以省略

```kotlin
println(+5) // prints 5
println(+(-5)) // prints -5
```

- 一元减号（unary minus）对值求反

```kotlin
println(-8)  // prints -8
println(-(100 + 4)) // prints -104
```

这两个运算符的优先级高于乘法和除法。

### 运算符优先级

优先级从高到低：

1. 括号
2. 一元加/减
3. 乘、除、模
4. 加、减

## 递增和递减

### 赋值

前面已经介绍加法和减法等基本算术运算：

```kotlin
var a = 3
a = a + 1 // 4
a = a - 1 // 3
```

此外，还有将算法运算和赋值结合起来的符合运算符：

- `+=`，相加后赋值：`A += B` 等价于 `A = A + B`
- `-=`，相减后赋值：`A -= B` 等价于 `A = A - B`
- `*=`，相乘后赋值：`A *= B` 等价于 `A = A * B`
- `/=`，相除后赋值：`A /= B` 等价于 `A = A / B`
- `%=`，取模后赋值：`A %= B` 等价于 `A = A % B`

示例：

```kotlin
var a = 3
a += 2 // 5
a -= 2 // 3
a *= 2 // 6
a /= 2 // 3
a %= 2 // 1
```

这种复合运算符只能用于已定义变量：

```kotlin
var a: Int
a += 5 // 编译错误，变量 a 没有初始化
```

### 加 1 和减 1

`++` 和 `--` 提供 `+= 1` 和 `-= 1` 的便捷操作：

```kotlin
var num = 3
num++  // 4, increment
num--  // 3, decrement
```

该代码等价于：

```kotlin
var num = 3
num += 1  // 4
num -= 1  // 3
```

### 前缀形式

前缀形式，先修改值，后赋值。即返回修改后的值。例如：

```kotlin
var a = 10
val b = ++a
println(a)  // a = 11
println(b)  // b = 11
```

这里，先将变量 `a` 的值 +1，然后将其赋值给 `b`。因此 `a` 和 `b` 的值都是 11.

前缀加法类似：

```kotlin
var a = 10
val b = --a
println(a)  // a = 9
println(b)  // b = 9
```

### 后缀形式

后缀形式先赋值，然后修改值。返回修改前的值。例如：

```kotlin
var a = 10
val b = a++
println(a)  // a = 11
println(b)  // b = 10
```

首先，将变量 `a` 的值赋值给变量 `b`，然后将变量 `a` 的值 +1。因此，`a` 的值是 11，而 `b` 的值是10.

后缀加法类似：

```kotlin
var a = 10
val b = a--
println(a)  // a = 9
println(b)  // b = 10
```

### 优先级

按优先级降序：

1. 括号
2. 后缀递增/递减：`expr++`, `expre--`
3. 一元加/减，前缀递增/递减：`-expr`, `++expr`, `--expr`
4. 乘、除、模：`*`, `/`, `%`
5. 赋值：`=`, `+=`, `-=`, `*=`, `/=`, `%=`

 当不确定优先级，用括号即可。