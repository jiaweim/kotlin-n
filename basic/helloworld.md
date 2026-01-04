# HelloWorld

## 简介

下面是 Hello World 的 Kotlin 实现：

```kotlin
fun main() {
    println("Hello, World!")
    // Hello, World!
}
```

解释：

- **程序**（program）是一系列指令（语句 statement），它们以可预测的方式依次执行。按顺序执行是最常见的情况，即从上到下，一个接一个；
- **语句**（statement）是要执行的单个命令（例如打印文本）
- **表达式**（expression）是一段生成单个值的代码，例如 `2*2` 是一个表达式
- **代码块**（block）是一组包含在一对大括号 `{...}` 内的 0 或多个 statements
- **关键字**（keyword）是在编程语言中具有特殊含义的单词，关键字的名字不能更高
- **标识符**（identifier）是程序员用来标识某物的单词
- **注释**（comment）是执行程序时忽略的文本，它们只是用于解释代码。注释以`//` 开头
- **空白**（whitespace）是空白区域、制表符合换行符等，用于分割程序中的单词，并提高可读性

## Hello World 程序详解

Hello World 程序展示了 Kotlin 程序基本元素的使用方法。下面解释几个要点。

**执行入口**（entry point）

关键字 `fun` 定义一个函数，其中包含一段要执行的代码。该函数有一个特殊的名字 `main`。`main` 函数是 Kotlin 程序的入口，其主体用大括号括起来：

```kotlin
fun main() { 
    // ...
}
```

该函数名称必须为 `main`，不能修改。如果命名为 `Main` 或 `MAIN` 或其它名称，程序将无法启动。

**打印 "Hello, World!"** 

函数的主体包含的语句定义程序要执行的操作。本示例使用以下语句打印  **"Hello, World!"**：

```kotlin
println("Hello, World!")
```

这里调用函数 `println` 在屏幕上显示一个字符串，后跟一个换行符。

## 多个语句

一个程序通常包含多个语句。每个语句单独一行。例如，下面的程序包含两条语句：

```kotlin
fun main() {
    println("Hello")
    println("World")
}
```

运行得到：

```
Hello
World
```

