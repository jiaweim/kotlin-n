# Kotlin 基础

2026-01-09⭐
@author Jiawei Mao
***

## Hello world

下面是一个打印 "Hello, world!" 的简单程序：

```kotlin
fun main() {
    println("Hello, world!")
    // Hello, world!
}
```

在 Kotlin 中：

- `fun` 用于声明函数
- `main()` 函数是程序入口

## JVM 基础

Java 虚拟机（Java Virtual Machine, JVM）是一种根据 JVM 规范实现的虚拟计算机程序。JVM 能够执行编译后的 Java 字节码，并将其翻译成计算机能够理解的底层指令。每个平台都有各自的 JVM 版本，但由于所有 JVM 都符合相同规范，因此相同程序在不同设备上表现完全相同。

Java 平台的一个核心概念之一是：write once, run anywhere。这个概念也常被称为平台独立性或可移植性（portability）。

> [!NOTE]
>
> 输入到 JVM 的代码是 platform-independent，而 JVM 输出的代码是 platform-dependent。

下图简单总结了 JVM 程序的工作流程：

<img src="./images/769d5ced-d7a7-45a2-a72c-f4082311eeed.svg" alt="jvm program work cycle" width="600" />

### JVM 语言

Java 平台允许使用多种编程语言来创建程序，只要代码能够转换为 Java 字节码。这类语言称为 JVM 语言，包括：Java, Kotlin, Scala, Groovy, Clojure 等。因此，在 Java 世界创建程序，你可以选择你喜欢的语言。

### JVM

Java 虚拟机（Java Virtual Machine, JVM）是对物理计算机的虚拟仿真。它执行 Java 字节码（bytecode）。在某种意义上，JVM 充当代码与真实机器之间的中介，它通过一组统一的 bytecode 指令运行，这些指令被解释并转换为机器指令。

JVM 适用于许多硬件和软件平台，所以几乎可以在任何地方运行字节码。编译成字节码的程序几乎总是平台无关。因此，使用 JVM 语言编写的代码，只需编译一次，获取字节码，就能够在任何支持 JVM 的平台上运行。

如今，有多种 JVM 实现，其中 Java HotSpot 虚拟机使用最多。

### JDK

Java 开发工具包（Java Development Kit, JDK）是用于为 Java 平台开发程序的包。它包含 JRE，JRE 包含开发所需的工具：Java Compiler, debugger, archiver, documentation generator 等。

在编译阶段，编译器将源代码转换为包含字节码的 `.class` 文件，并由 JVM 执行。如果使用 Java 以外的 JVM 语言，则需要单独下载对应的编译器，因为 JDK 只包含 java 编译器。

在实践中，程序通常包含多个 `.class` 文件，并通过归档工具打包为单个 Java Archive (JAR 文件)。JRE 可以直接运行 JAR 中的程序，无需解压。

### JRE

JRE (Java Runtime Environment)是一个执行环境，它包含运行已编译 JVM 程序所需组件：JVM 以及 Java 类库（Java Class Library, JCL）。

JCL 本质上是一组提供常见功能的标准库：基础类，输入/输出、数学包、集合、安全、用户界面工具包等。

<img src="./images/33aad287-2450-4386-8639-42c7a7eac874.svg" alt="JDK JRE components" width="500" />

## 值和变量

变量是存储值的空间，这个值可以是字符串、数字或其它对象。

每个变量都有名字（name）以与其它变量区分。可以通过变量名称访问编程存储的值。

### 声明变量

Kotlin 提供了两个关键字用于声明变量：

- `val` (value) - 声明 read-only 变量，初始化后无法更改
- `var` (variable) - 声明可变（mutable）变量，可以根据需要更改
- `const` - 用于在编译时已知的变量 `val`

用 `=` 为**变量赋值**。

**示例**：声明一个名为 `language` 的 immutable 变量，并使用字符串 "Kotlin" 对其初始化

```kotlin
val language = "Kotlin"
```

`language` 变量在初始化后无法修改。

> [!NOTE]
>
> Kotlin 的变量名称区分大小写。

**示例**：声明 mutable 变量

```kotlin
var dayOfWeek = "Monday"
println(dayOfWeek)
// Monday

dayOfWeek = "Tuesday"
println(dayOfWeek)
// Tuesday
```

Kotlin 是静态类型，因此不能将变量值修改为不兼容类型的值：

```kotlin
var number = 10
number = 11 // ok
number = "twelve" // error
```

### val 变量

下面声明两个 `val` 变量：

```kotlin
val pi = 3.1415
val helloMsg = "Hello"

println(pi)       // 3.1415
println(helloMsg) // Hello
```

两个变量都不能修改，但可以多次访问。

如果尝试修改 `val` 变量的值，编译器会报错，即不能重新为 `val` 分配值：

```kotlin
val pi = 3.1415
pi = 3.1416 // error line
```

在赋值前使用 `val` 变量也会报错：

```kotlin
val boolFalse: Boolean
println(boolFalse) // error line
```

访问前赋值就没问题：

```kotlin
val boolFalse: Boolean // not initialized
boolFalse = false      // initialized
println(boolFalse)     // no errors here
```

#### val 变量和 mutability

需要注意，`val` 不等价于 immutable。例如，下面使用一个 `MutableList`：

```kotlin
// list creation
val myMutableList = mutableListOf(1, 2, 3, 4, 5)
// trying to update the list
myMutableList = mutableListOf(1, 2, 3, 4, 5, 6) // error line
```

第二行无法编译，因为不能为 `val` 变量重新分配值。但是 `val` 变量的**内部状态可以改变**。例如：

```kotlin
val myMutableList = mutableListOf(1, 2, 3, 4, 5)
// 添加元素没问题
myMutableList.add(6)
println(myMutableList) // [1, 2, 3, 4, 5, 6]
```

`val` 变量与 Java 的 `final` 变量类似：禁止重新分配值，但可以修改对象的内部状态。

#### const 变量

Kotlin 还有一个 `const` 修饰符，放在 `val` 关键字前面声明编译时已知的常量。`const` 变量的值在编译时已知，运行时不会更改：

```kotlin
const val MY_STRING = "This is a constant string"
```

以下代码会报错，因为值在程序执行前未知，不是常数：

```kotlin
const val MY_STRING = readln() // not a constant String!!!
```

另外，`const` 的使用有一些限制。首先，`const` 只能用于 `String` 或 primitive 类型变量：

```kotlin
const val CONST_INT = 127
const val CONST_DOUBLE = 3.14
const val CONST_CHAR = 'c'
const val CONST_STRING = "I am constant"
const val CONST_ARRAY = arrayOf(1, 2, 3) // error: only primitives and strings are allowed
```

此外，`const` 变量需要在顶层声明，在函数外部：

```kotlin
const val MY_INT_1 = 1024 // correct line

fun main() {
    const val MY_INT_2 = 2048 // error: Modifier 'const' is not applicable to 'local variable'
}
```

#### 何时使用 val 变量

推荐**默认使用** `val` 变量。然后在代码需要时将其改为 `var` 变量：

```kotlin
var a = 1024
val b = 256
val c = 128
a += b * c
```

该方法使得程序中 mutable 变量最少，从而减少错误。

#### 命名习惯

在声明 `val` 和 `const` 变量时，最好遵循 Kotlin 的命名规范，以保证代码易于阅读和维护。

`val` 变量使用 **camelCase** 格式：即以小写开头，每个后续单词的首字母大写。例如：

```kotlin
val numberOfWheels: Int

val isConnectionAvailable: Boolean

val userFirstName: String
```

`const` 变量是编译时常量。由于它们是静态不可修改的值，因此名称采用大写字母，用下划线分隔单词。例如：

```kotlin
const val MAX_USER_COUNT = 50

const val COMPANY_NAME = "TechCorp"

const val VERSION_CODE = 3
```

## 注释

### 单行注释

编译器忽略每行 `//` 之后的内容：

```kotlin
fun main() {
    // The line below will be ignored
    // println("Hello, World")

    // This prints the string "Hello, Kotlin"
    println("Hello, Kotlin")  // Here can be any comment
    /// Valid single-line comment
}
```

### 多行注释

`/*` 到 `*/` 定义多行注释，需要注意配对：

```kotlin
fun main() {
    /*
    System.out.println("Hello")  // print "Hello"
    System.out.println("Kotlin") /* print "Kotlin" */
    */
}
```

### 文档注释

`/**` 到 `*/` 定义文档注释。专用工具可以根据文档注释生成关于源码的文档。

文档注释通常放在各个程序元素的声明上方。还有 `@param`, `@return` 等特殊标签用于辅助文档注释。

例如：

```kotlin
/**
 * The `main` function accepts string arguments from outside.
 *
 * @param args arguments from the command line.
 */
fun main(args: Array) {
    // do something
}
```

## Kotlin 代码样式约定

参考：https://kotlinlang.org/docs/coding-conventions.html

1. 使用 4 各空格缩进（不要用 1 各 tab）。因为不同操作系统和 IDE，一个 tab 并不总是对应 4 各空格
2. 省略分号 `;`，Kotlin 不需要
3. 在行尾放左大括号 `{`
4. 换行放右大括号 `}`

例如，下面的程序遵循该管理：

```kotlin
fun main() { // opening curly brace
    println("Hi") // this statement is offset by 4 spaces and has no `;` at the end
} // closing brace
```

## 命名

Kotlin 对变量的命名规则：

- 区分大小写
- 名称只能包含字母、数字和下划线
- 名称不能以数字开头
- 名称不能为关键字

因此，变量名称中不能有空格。除非放在反引号中：

```kotlin
val `good name` = 5 // 没问题
val bad name = 2 // will not work
```

以下是有效命名：

```kotlin
score, level, fruitType, i, j, abc, _cost, number1, `name with space`
```

以下为无效命名：

```kotlin
@pple, $dollar, 1number, !ab, val, var, _, name with space
```

### 命名约定

Kotlin 提供以下命名约定：

- 如果变量名是一个单词，则用小写，如 `number`, `value`
- 如果变量名包含多个单词，则用 `lowerCamelCase` 样式
- 不用用下划线开头。虽然从技术上来说，可以
- 选择有意义的变量名称，例如，`score` 比 `s` 有意义

### Magic Number

在代码中使用常数，例如一周的天数总是 7。但如果直接在代码中使用，很难理解：

```kotlin
println(7)
```

这类数字在代码中称为**魔数**（magic number）。魔数不一定是数字，而是对这类变量的称呼：不知道其含义，不知道其用途，所以应该避免使用魔数。

首先，这类值应存储在 immutable 变量中，即函数之外声明为 `const val`。例如：

```kotlin
const val DAYS_OF_THE_WEEK = 7

fun main() {
    // ...
    println(DAYS_OF_THE_WEEK) // 7
    // ...
}
```

其次，常量的名称应当有意义。使用 `SCREAMING_SNAKE_CASE` 样式。

下面是一个常量名称的错误示例：

```kotlin
const val s = 4
```

正确示例：

```kotlin
const val SEASONS = 4
```

