# 函数

## 声明函数

在 Kotlin 中，函数是代码的基础，用于定义可重复使用的代码块。

### 基本语法

声明函数的语法：

```kotlin
fun functionName(p1: Type1, p2: Type2, ...): ReturnType {
    // body
    return result
}
```

函数组成：

- `fun` 关键字
- 函数名称，其命名规范与变量名称一样
- 参数放在括号中，用逗号分隔
- 每个参数包含名称和类型，两者用冒号分隔
- 返回值类型，可省略
- 函数主体

### 定义一个简单函数

声明一个计算整数加和的函数：

```kotlin
fun sum(a: Int, b: Int): Int {
    val result = a + b
    return result
}

fun main() {
    val result1 = sum(2, 5)
    println(result1) // 7

    val result2 = sum(result1, 4)
    println(result2) // 11
}
```

使用函数名称 `sum` 调用该函数，传入参数 `2` 和 `5`，这两个值被赋值给参数 `a` 和 `b`。`sum` 返回 `result` 的值，其类型与函数声明的返回值类型 `Int` 一致。第二次调用类似，只是将 `result1` 的值作为参数传递给 `a`。

### 函数参数

参数为函数提供输入数据：

- 函数可以声明**一个或多个参数**
- 这些参数的类型可以相同，也可以不同
- 也可以声明一个完全不带参数的函数
- 函数名后的圆括号为必填项

示例：

```kotlin
/**
 * The function returns its argument
 */
fun identity(a: Int): Int {
    return a
}

/**
 * The function returns the sum of two Ints
 */
fun sum(a: Int, b: Int): Int {
    return a + b
}

/**
 * The function just returns 3
 */
fun get3(): Int {
    return 3
}

fun add3(a: Int): Int {
    return identity(a) + get3()
}

fun main() {
    println(identity(1000)) // 1000
    println(sum(200, 300))  // 500    
    println(get3())         // 3
    println(add3(5))        // 8
}
```

可以看到，可以在 `main` 函数中调用某个参数，也可以其它普通函数中嗲用函数，如 `add3` 所示。

### 返回类型

函数可以返回一个值，或不返回任何值。

若函数要返回某个值，其函数主体内必须包含 `return` 关键字，在其后跟上要返回的结果。

声明不返回任何值的函数，有两种方式：

- 忽略返回类型

```kotlin
/**
 * The function prints the values of a and b
 */
fun printAB(a: Int, b: Int) {
    println(a)
    println(b)
}
```

- 使用特殊的 `Unit` 类型作为返回类型

```kotlin
/**
 * The function prints the sum of a and b
 */
fun printSum(a: Int, b: Int): Unit {
    println(a + b)
}
```

### 函数主体

- 在函数主体中可以编写任何语句。除了参数，还可以在函数内声明只在函数内部使用的变量。

例如，下面这个函数提取一个数字的随后一位：

```kotlin
fun extractLastDigit(number: Int): Int {
    val lastDigit = number % 10
    return lastDigit
}
```

- 可以省略多余的变量，简化代码：

```kotlin
fun extractLastDigit(number: Int): Int {
    return number % 10
}
```

- 在函数内可以执行任意操作。例如，检查一个数字是否为正数：

```kotlin
fun isPositive(number: Int): Boolean {
    return number > 0
}
```

`isPositive` 函数接收一个整数类型参数，返回一个 `Boolean` 值。

- `return` 语句之后的代码不会被执行

```kotlin
/**
 * It returns "hello"
 */
fun getGreeting(): String {
    return "hello"   // Ends the function
    println("hello") // Will not be executed
}
```

### 单表达式函数

- 如果函数很简单，只有一个表达式，此时可以省略大括号：

```kotlin
fun sum(a: Int, b: Int): Int = a + b

fun sayHello(): Unit = println("Hello")

fun isPositive(number: Int): Boolean = number > 0
```

- 也可以不指定返回值类型，因为编译器可以自动推断

```kotlin
fun sum(a: Int, b: Int) = a + b // Int

fun sayHello() = println("Hello") // Unit

fun isPositive(number: Int) = number > 0 // Boolean
```

## 函数式分解

将复杂程序拆解为多个函数的编程方法，称为**函数式分解**（functional decomposition）。

### 解决复杂任务

将一个复杂任务拆解为若干个子问题，其实很直观。

例如，假设有一个智能家居 app，它用于远程控制家居设备，它有三项操作：开启或关闭音乐、开关灯、开关门。

这款 app 的通用操作流程，可以拆解为以下步骤：

1. 解析输入数据（用户输入的密码）
2. 验证密码是否正确
3. 询问用户的操作需求
4. 若支持该操作，则执行相应操作

直接在 `main` 中定义整个流程：

```kotlin
fun main() {
    // ...
    val password = "76543210"
    var speakersState: String
    var lampState: String
    var doorState: String
    // ...

    // reading the password
    println("Enter password: ")
    val passwordInput = readln()

    // checking if the password is correct
    if (passwordInput != password) {
        println("Incorrect password!")
    } else {
        // asking the user what they want to do
        println("Choose the object: 1 – speakers, 2 – lamp, 3 – door")
        val action = readln()

        when (action) {
            "1" -> {
                // asking the user about the speakers
                when (speakersState) {
                    "on" -> {
                        // ...
                    }
                    "off" -> {
                        // ...
                    }
                    else -> {
                        // ...
                    }
                }
            }
            "2" -> {
                // asking the user about the lights...
            }
            "3" -> {
                // asking the user about the door...
            }
            else -> {
                // ...
            }
        }
    }
}
```

虽然进行了精简，但这段代码看起来依然臃肿。它能解决当前问题，但后续调整或扩展功能会比较麻烦。

例如，支持多 用户使用，或者增加操作数量、提升操作的复杂度等。为了让这段代码适用性更强、更灵活，可以采用函数式分解。

### 将程序分解为函数

**函数式分解**指把一个问题拆解为若干函数的过程。每个函数负责一项特定任务，按顺序执行函数，就能得到所需结果。在分析问题时，需要找出哪些会被多次重复执行，或者可以独立执行的操作。通过这种提炼出的函数，更容易阅读、理解、复用、测试。

针对上例，可以使用一个函数控制音乐、另一个函数开关等、再用一个函数控制门锁。控制音乐的函数 `controlMusic()` 如下，`controlLight()` 和 `controlDoor()` 的定义类似：

```kotlin
// turns the music on and off
fun controlMusic() {
    println("on/off?")
    val tumbler = readln()
    when (tumbler) {
        "on" -> println("The music is on")
        "off" -> println("The music is off")
        else -> println("Invalid operation")
    }
}
```

再定义一个函数验证密码：

```kotlin
// verifies the password and gives the access to Smart home actions if the password is correct
fun accessSmartHome() {
    val password = "76543210"
    print("Enter password: ")
    val passwordInput = readln()
    if (passwordInput == password)
        chooseAction()
    else
        println("Incorrect password!")
}
```

还定义一个带操作菜单的函数 `chooseAction()`，用户可以在菜单中选择需要执行的操作。该函数会询问用户想要进行的操作，并调用对应的函数来执行。

最后，在 `main` 函数中执行经过分解的程序：

```kotlin
fun main() {
    accessSmartHome()
}
```

### 新增功能

如果想要添加一项新功能，只需定义对应的函数即可。例如，新增一款智能设备电水壶。可以编写一个函数来控制它的开关。然后修改 `chooseAction()` 函数，为其添加一个新的操作选项。

```kotlin
// controls electric kettle
fun controlKettle() {
    // ...
}

// main menu for choosing the action
fun chooseAction() {
    // ...

    // adding new action 4
    println("Choose the object: 1 – speakers, 2 – lamp, 3 – door, 4 – kettle")
    // ...
        "4" -> controlKettle()
    // ...
}
```

将各个功能封装在独立的函数中，使我们能够轻松对这些组件进行测试。使得程序的维护更便捷。

## 默认参数

Kotlin 支持在声明函数时为参数指定默认值。在调用该函数时，可以省略带默认值的参数，也可以指定新的参数值。

下面是一个名为 `printLine` 的函数，它包含两个参数：

1. 参数一定义一行文本
2. 参数二定义结束这行文本的字符

这两个参数都赋予了默认值，分别为空字符串和换行符。

```kotlin
fun printLine(line: String = "", end: String = "\n") = print("$line$end")
```

在参数类型后面使用 `=` 为参数赋值。

使用示例：

```kotlin
fun printLine(line: String = "", end: String = "\n") = print("$line$end")

fun main() {
    printLine("Hello, Kotlin", "!!!") // prints "Hello, Kotlin!!!"
    printLine("Kotlin") // prints "Kotlin" with an ending newline
    printLine() // prints an empty line like println()
}
```

第一次调用传入两个参数，`"Hello, Kotlin"` 和 `"!!!"`；第二次调用，只传入第一个参数 `"Kotlin"`，第二个参数取默认值 `"\n"`；第三次调用没有传入任何参数，两个参数都使用默认值。

输出：

```kotlin
Hello, Kotlin!!!Kotlin 
```

### 混合使用默认参数与普通参数

在声明函数时可以混合使用默认参数与普通参数。例如:

```kotlin
fun findMax(n1: Int, n2: Int, absolute: Boolean = false): Int {
    val v1: Int
    val v2: Int

    if (absolute) {
        v1 = Math.abs(n1)
        v2 = Math.abs(n2)
    } else {
        v1 = n1
        v2 = n2
    }

    return if (v1 > v2) n1 else n2
}

fun main() {
    println(findMax(11, 15)) // 15
    println(findMax(11, 15, true)) // 15
    println(findMax(-4, -9)) // -4
    println(findMax(-4, -9, true)) // -9
}
```

`findMax` 函数内声明了两个变量。它们会根据 `absolute` 参数存储传入参数的绝对值或原始值。函数最后一行比较两个临时变量，并返回对应传入参数的值。

 ### 惯用写法

默认函数参数是 Kotlin 惯用写法的一部分。在编写复杂函数时使用默认参数非常有用。

```kotlin
fun foo(a: Int = 0, b: String = "") { 

}
```

## 命名参数

在调用包含多个参数的函数时，可以通过**参数名称**传入实参。当函数包含大量参数时，这种方式能够提高代码的可读性，而且可以调整调用时实参的顺序。

### 提高代码可读性

假设一位售票员卖电影票，单日票价保持不变。现在需要计算这位售票员在工作日结束时有多少钱。函数如下：

```kotlin
fun calcEndDayAmount(startAmount: Int, ticketPrice: Int, soldTickets: Int) =
        startAmount + ticketPrice * soldTickets
```

- `startAmount` 收银台初始现金
- `ticketPrice` 为票价
- `soldTickets` 为当日卖出的电影票数量

这是一个常规函数，可以按以下方式调用：

```kotlin
val amount = calcEndDayAmount(1000, 10, 500)  // 6000
```

这完全没问题，但是参数的含义不是很明确。Kotlin 支持用命名实参调用：

```kotlin
val amount = calcEndDayAmount(
    startAmount = 1000,
    ticketPrice = 10,
    soldTickets = 500
)  // 6000
```

这更容易理解。

### 调整实参顺序

命名实参可以调整参数顺序：

```kotlin
val amount = calcEndDayAmount(
    ticketPrice = 10,
    soldTickets = 500,
    startAmount = 1000
)  // 6000
```

### 命名实参和位置实参

在调用函数时可以混用命名参数和位置参数，但要确保命名实参在位置参数后面：

```kotlin
calcEndDayAmount(1000, ticketPrice = 10, soldTickets = 500)  // 6000
```

Kotlin 1.4+ 命名实参可以放在位置参数前面，但要保证参数顺序与函数前面一致：

```kotlin
calcEndDayAmount(ticketPrice = 10, 500, 1000)   // Incorrect invocation!
calcEndDayAmount(startAmount = 1000, 10, 500)   // OK

calcEndDayAmount(soldTickets = 500, ticketPrice = 10, startAmount = 500) // OK
calcEndDayAmount(soldTickets = 500, ticketPrice = 10, 500)  // Incorrect invocation!
```

### 默认参数和命名实参

命名实参可以与默认参数一起使用。默认参数有时会引发歧义，使得 Kotlin 无法判断为哪些参数赋值。

修改上一个函数，为第一个参数指定默认值

```kotlin
fun calcEndDayAmount(startAmount: Int = 0, ticketPrice: Int, soldTickets: Int) =
        startAmount + ticketPrice * soldTickets
```

调用该函数时，采用位置实参只指定后面两个参数不行：

```kotlin
val amount = calcEndDayAmount(10, 500)  // Won't work —
                                        // no value for soldTickets
```

这里 `10` 会赋值给第一个可选参数 `startAmount`。

使用命名参数可以避免该问题：

```kotlin
val amount = calcEndDayAmount(ticketPrice = 10, soldTickets = 500)  // 5000
```

### 命名参数和默认值

函数参数的默认值可以是常量，也可以是其它变量，甚至可以使另一个命名实参或某个函数。例如：

```kotlin
fun sum2(a: Int, b: Int = a) = a + b
 
sum2(1)    // 1 + 1
sum2(2, 3) // 2 + 3
```

以下代码无法运行，因为参数 `b` 没有完成初始化：

```kotlin
fun sum2(a: Int = b, b: Int) = a + b
```

## 调用栈

在编写程序时，经常需要在函数中调用其它函数，计算机如何确定函数的执行顺序，如何在不同函数之间切换执行，又如何判断程序何时执行完毕，这些都通过**调用栈（call stack）**实现。

JVM 通过调用栈确定下一个要执行的函数。调用栈由**栈帧**（stack frame）组成，栈帧存储未执行完毕的函数信息，包括返回地址、参数、局部变量、中间计算结果等。

<img src="./images/c2f77b5f-3e74-4d65-834e-a435045f9f99.svg" alt="stack structure" width="300" />





## scope

## 递归基础

## 对象和属性

