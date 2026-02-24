# 异常处理

## 简介

程序不可避免的存在 bug，例如：

- 对需求的误解
- 软件本身的复杂性
- 编程错误
- 第三方库的错误

在大型程序中完全避免 bug 几乎不可能，但可以减少 bug 数量：

1. **明确目标**：充分理解程序的需求
2. **化整为零**：将程序拆解为一个个小单元，这样既容易读懂也容易理解
3. **编写易读的代码**：遵循编程语言的规范
4. **用边界值测试**：在测试程序时考虑各种特殊情况
5. **编写自动化测试**：编写一些自动化测试，在构建程序时自动检查

要修复 bug，首先得在代码中把它找出来，然后进行修改。定位 bug 的方法：

- 通读代码：试着理解代码在处理输入时具体做了什么
- 使用调试器（debugger）：启动 debugger，观察变量的实时变化以及程序的控制流
- 打印日志：在代码的关键部分打印日志，然后进行分析

## 错误类型

Kotlin 将程序错误分为两类：

- 编译时错误（compile-time error）
- 运行时错误（run-time error）

### 编译时错误

编译时错误指那些导致程序无法通过编译的错误。例如：

- 语法错误
- 导入包名错误
- 调用不存在的方法

编译时错误示例：

```kotlin
fun main(args: Aray<String>) {
    printn("Hello!")
}
```

这个程序有两个错误：

- 类名 `Array` 拼写错误
- 函数 `println` 拼写错误

现代 IDE 内置了静态代码分析器，能够标出这些编译时错误，从而避免这类低级错误。

### 运行时错误

运行时错误指程序运行期间发生的错误，即所谓的 bug。运行时错误可以分为两类：

- 逻辑错误
- 未处理的异常：比如除以 0、找不到文件等

相比编译时错误，运行时错误更难处理。常见处理策略包括：

- 调试（debug）
- 编写自动化测试
- 代码审查

## 修复错误

下面介绍如何使用 Intellij IDEA 修复代码错误。

### 智能提示

Intellij IDEA 会提示编译时错误：

- 代码下红色波浪线表示代码有问题，把光标移到高亮代码上，就能看到具体的错误信息。

- 点击红色💡图标，或者按 `Alt+Enter`，可以看到推荐的修复方案。

![img](./images/c45142f0-a1e6-42c6-9dd5-337ad008a932.png)

文件右上角的红色图标，提示文件里错误个数。把鼠标悬停在右侧的红色标记上，可以查看具体的错误信息。

![img](./images/890d5982-c67c-41b1-acf8-99eee59e4d16.png)

修复完所有问题，右上角就会出现一个绿色的对勾：

![img](./images/2e1c98fb-eb77-45d4-a941-5e18dd2345aa.png)

### Debugger

**首先设置断点**：点击代码左侧的空白区域就可以设置断点，也可以使用快捷键 `Ctrl+F8`

![img](./images/1edd29b8-5c4b-4642-9ec8-7a27168523b8.png)

**启动 debugger**：用 `Shift+F9` 启动 debugger

![img](./images/07f8b112-ea8e-47c3-8021-90d7fbf6653e.png)

程序运行到断点就会停下来：

![img](./images/14052cd3-6dc4-43d9-aa59-717fb5e8a574.png)

然后按 `F8` 逐行执行代码，一边执行一边看，在 Debug 窗口可以查看所有变量信息：

![img](./images/5eb3d2b8-b388-4f1f-80af-bd17dab64a59.png)

把鼠标悬停在某个表达式上，也可以看到执行结果：

![img](https://ucarecdn.com/dc24e534-c427-4c1d-839a-3bcfd1e9c09b/)

按 `Alt+F8` 可以打开 `Evaluate` 对话框，在其中可以尝试计算各种表达式：

![img](./images/f24611a5-ae01-4ec3-951d-088b6045a5eb.png)

总结：

- 用灯泡：遇到报错先别慌，先看看自动修复选项
- 用断点：不要用 `println` 看错误，直接用 debugger 暂停程序查看变量值

## Exception

在程序运行期间发生的错误称为异常（Exception），例如：

```kotlin
fun readNextInt(): Int {
    return readln().toInt()
}

fun runIncrementer() {
    val increment = 1 + readNextInt()
    println(increment)
}

fun main() {
    runIncrementer()
}
```

`readNextInt()` 从标准输入读取一个整数，如果用户输入的不是整数，`toInt()` 就会失败。假设用户输入：

```sh
> Hi :)
```

此时就会发生异常，程序直接崩溃，输出如下：

```sh
Exception in thread "main" java.lang.NumberFormatException: For input string: "Hi :)"
	at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)
	at java.base/java.lang.Integer.parseInt(Integer.java:565)
	at java.base/java.lang.Integer.parseInt(Integer.java:662)
	at HelloWorldKt.readNextInt(HelloWorld.kt:2)
	at HelloWorldKt.runIncrementer(HelloWorld.kt:6)
	at HelloWorldKt.main(HelloWorld.kt:11)
	at HelloWorldKt.main(HelloWorld.kt)
```

即在第二行的 `readNextInt` 函数中，触发了 `NumberFormatException`。

> [!TIP]
>
> **什么是异常？**
>
> 代码中的有些问题不会阻止程序开始运行，当时，当程序运行包含异常的错误代码时，它就会崩溃。**异常**在程序**执行期间**发生，而**语法错误**是在程序**编译期间**发生。
>
> 引发异常的原因多种多样，比如无效的算术运算，或者像上例那样的错误输入。这些错误表明程序的某些行为与预期不符，因此你需要验证输入的数据，或改变处理数据的方式。

### 除以 0

下面用一个典型的算术运算错误演示异常。假设你买了几盒牛奶，记为 `amount`，花了 `total` 元。计算每盒牛奶的价格：

```kotlin
fun itemPrice(total: Int, amount: Int): Int {
    return total / amount
}
```

如果调用该函数时，数量 `amount` 为 0，分母为 0，典型的算术错误：

```kotlin
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at HelloWorldKt.itemPrice(HelloWorld.kt:2)
	at HelloWorldKt.main(HelloWorld.kt:6)
	at HelloWorldKt.main(HelloWorld.kt)
```

预防这个错误非常简单：

```kotlin
fun itemPrice(total: Int, amount: Int): Int {
    if (amount == 0) {
        println("Division by zero")
        return 0
    }
    return total / amount
}
```

> [!TIP]
>
> 这些异常可以通过增加一些数据验证进行预防，多考虑边界条件。

## 异常的类型结构

Kotlin 为各种不同的意外情况准备了专门的异常类型，这些类型共同构成了一套完整的类型分层体系（继承树）。

Kotlin 中的类型按照**子类**（subtype）和**超类**（supertype）定义类型关系。子类型会从超类型继承通用的特征和行为，从逻辑上讲，超类型定义 了所有子类型需要遵循的共有特征与行为规则的类型。

下图是简化的异常类型分层结构：

<img src="./images/f7ffede8-355a-4d41-b140-61e470f02a33.svg" alt="a simplified hierarchy of exceptions" width="400" />

`Throwable` 是所有异常的基类，是整个体系的根节点，并提供了一套处理异常的实用方法。`Throwable` 有两个直接子类：

- `Error` - 代表严重的系统错误（如内存溢出），应用程序通常不需要处理这类错误
- `Exception` - 这是需要重点关注的，开发应用时，需要捕获和处理这些异常

> [!TIP]
>
> 子类和超类是相对概念，例如，`Exception` 既是 `Throwable` 的子类，也是 `RuntimeException` 的超类。

### RuntimeException

`RuntimeException` 是 `Exception` 的一种特殊子类型，通常是由代码中检查不足引起的，可以通过编程手段预防。`RuntimeException` 的常见子类：

<img src="./images/cdfdf267-5906-4ee8-9b65-e3e7bd6fd594.svg" alt="a hierarchy of subtypes descended from RuntimeException" width="600" />

下面演示这几种常见 `RuntimeException`。

### ArithmeticException

`ArithmeticException` 是一种 `RuntimeException`，当代码执行错误的算术运算就会抛出该异常。例如：

```kotlin
val example = 2 / 0 // throws ArithmeticException
```

因为除以 0 是不对的，所以这行代码会抛出 `ArithmeticException`，程序因此会中断。

### NumberFormatException

在计算过程中还可以遇到 `NumberFormatException`，当某个方法期望输入为数字，但实际输入为其他类型时，就会抛出该异常。例如：

```kotlin
val string = "It's not a number"
val number = string.toInt() // throws NumberFormatException
```

这里尝试将字符串 `"It's not a number"` 转换为整数，显然无法实现，因此会抛出 `NumberFormatException`。

### IndexOutOfBoundsException

访问不存在的索引或抛出 `IndexOutOfBoundsException`。例如，在一个只有 5 个元素的集合中访问第 8 个元素，索引就会越界。

该类型还有更具体的子类型，如专门用于字符串的 `StringIndexOutOfBoundsException`：

```kotlin
val sequence = "string"
println(sequence[10])   // throws StringIndexOutOfBoundsException
```

字符串可以看作一组字符的集合，每个字符对应一个索引。如果访问字符串中不存在索引位置的字符，就会抛出该异常。

## 抛出异常

下面介绍其它类型的异常，以及如何预防和抛出异常。例如：

```kotlin
fun calculateSpentMoney(total: Int, itemPrice: Int): Int {
    val amountToBuy = total / itemPrice
    return amountToBuy * itemPrice
}
```

这里假设商品单价为 `itemPrice`，你手头有 `total` 元，该函数计算购买最大数量的商品会花费多少钱。例如，烧饼单价 2 元，你手头共有 37 元，该函数会返回 36.

这里有个问题，如果商品免费，比如试吃？该函数没有考虑商品免费的情况，调用 `calculateSpentMoney(37, 0)` 就会抛出 `ArithmeticException`。

**修复异常**

预先确定可能出问题的情况，就能预防这类异常的发生。在这里，当商品免费，函数应返回 0，只需添加一个条件判断就能处理该情况：

```kotlin
fun calculateSpentMoney(total: Int, itemPrice: Int): Int {
    if (itemPrice == 0) {
        return 0
    }
    val amountToBuy = total / itemPrice
    return amountToBuy * itemPrice
}
```

提前判断边界情况，是预防代码崩溃的常用手段。

**抛出异常**

对上例，当输入为负数，此时函数不会崩溃，但返回的结果毫无意义：

- 你手头的现金总额不会为负数
- 商品单价为负数更没意义

这两种情况，函数都无法返回合理的结果，此时主要抛出异常更合适。在 Kotlin 中可以用 `throw` 关键字抛出异常：

```kotlin
fun calculateSpentMoney(total: Int, itemPrice: Int): Int {
    if (total < 0) {
        throw Exception("Total can't be negative")
    }
    if (itemPrice < 0) {
        throw Exception("Item price can't be negative")
    }
    if (itemPrice == 0) {
        return 0
    }
    val amountToBuy = total / itemPrice
    return amountToBuy * itemPrice
}
```

如果有人调用 `calculateSpentMoney(-10, 4)`，第一个条件判定为 `true`，程序会终止并抛出附带指定信息的异常。

当抛出异常时，返回类型为 `Nothing`：

```kotlin
fun makeAnException(): Nothing {
    throw Exception("I'm an exception!")
}
```

