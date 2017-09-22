---
title: "Swift 的 closures（闭包）总结"
---

# 什么是 closures？

官方文档定义如下，

> Closures are self-contained blocks of functionality that can be passed around and used in your code.

按照文档中对 function 和 closure 关系的描述，

> Global and nested functions, as introduced in [Functions](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-ID158), are actually special cases of closures. Closures take one of three forms:
>
> - Global functions are closures that have a name and do not capture any values.
>
> - Nested functions are closures that have a name and can capture values from their enclosing function.
>
> - Closure expressions are unnamed closures written in a lightweight syntax that can capture values from their surrounding context.

函数是 closures 的特例。

按照 [Functions](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-ID158) 一章的描述，提到了两种函数，全局函数和嵌套函数。

又按照 methods 的定义，

> Methods are functions that are associated with a particular type.

所以方法（instance methods 或 type methods）也是函数。

总的来说，

* Closures
  * Functions
    * Global Functions
    * Nested Functions
    * Methods
      * Instance Methods
      * Type Methods

注意，全局函数并没有捕获任何值（当然也包括全局变量），所以全局变量不算是被捕获。

# Closures 的语法优化（syntax optimizations）

Swift 对 closures 有一些语法优化，让它写起来更简洁（succinct）。

从一开始的把函数名作为参数，一步步越来越简洁。

1. Closures 作为参数

2. 如果函数体只有一句，可以和 closures 类型申明放一行。

3. Closures 的参数和返回值在可以推断类型的情况下，可以省去。（Inferring Type From Context）

4. 如果函数体只有一句，可以省略 `return`。（Implicit Returns from Single-Expression Closures）

5. 可以省去 closures 类型申明和 `in` 关键字，通过 `$0` `$1` 等访问对应位置的参数。（Shorthand Argument Names）

6. 一个 operator method 可以把 operator 直接作为参数传递。

7. 如果一个 closure 传递给一个函数的最后一个参数，这个 closure 可以放在函数调用之后。（Trailing Closures）

# 捕获变量（Capturing Values）

> A closure can capture constants and variables from the surrounding context in which it is defined. The closure can then refer to and modify the values of those constants and variables from within its body, even if the original scope that defined the constants and variables no longer exists.
> -- [The Swift Programming Language (Swift 4): Closures](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#)

- 从定义该 closure 的上下文（from the surrounding context in which it is defined）

- 该 closure 内可以引用和修改这些常量或变量

- **即使定义这些变量的 scope 已经不在**

Swift 文档中的例子如下，

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}
```

几点注意，

- 在 Swift 中，最简单的 closures 捕获变量的形式是 nested functions，这个例子中的 `incrementer` 就是 nested functions。

- 这个 nested function 捕获了两个变量，分别代表了下面描述中的两类变量，surrounding context 中定义的变量（`runningTotal`），outer function 的参数（`amount`），二者都是 from surrounding context which the closure (the nested function) is defined.

  > A nested function can capture any of its outer function’s arguments and can also capture any constants and variables defined within the outer function.

- `makeIncrementer` 函数返回了 `incrementer` 这个 nested function，outer function 执行完毕后，按理说，`runningTotal` 和 `amount` 变量应该消失，但由于 `incrementer` 函数捕获了这两个变量，所以他们不会随着 outer function 的结束而消失。

  ```swift
  let incrementByTen = makeIncrementer(forIncrement: 10)

  incrementByTen()
  // returns a value of 10
  incrementByTen()
  // returns a value of 20
  incrementByTen()
  // returns a value of 30
  ```

- 对于返回的函数（`makeIncrementer` 函数执行后返回其中的 nested function），其中捕获的两个变量更像是静态变量，每次调用这个返回的函数时，这两个变量都不重新初始化，而是维持上次调用之后的值。

- 不像静态变量的地方是，这两个变量是跟着生成（返回）的这个函数（`incrementByTen`）走的。也就是说 ，如果通过调用 `makeIncrementer` 再生成一个函数，其中捕获的变量和 `incrementByTen` 是独立的，如下，

  ```swift
  let incrementBySeven = makeIncrementer(forIncrement: 7)
  incrementBySeven()
  // returns a value of 7

  incrementByTen()
  // returns a value of 40
  ```

- 更容易的感觉是，把 closures 想象成实例（对象），捕获的变量是其成员变量。

## 必考题 [[:

> If you assign a closure to a property of a class instance, and the closure captures that instance by referring to the instance or its members, you will create a strong reference cycle between the closure and the instance. Swift uses capture lists to break these strong reference cycles. For more information, see [Strong Reference Cycles for Closures](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-ID56).

如果把一个 closure 赋值给一个实例的属性，并且这个 closure 中引用了这个实例本身或者实例的成员变量，这时，就会在这个 closure 和这个实例之间形成强引用循环。

这里对在 closure 中引用成员变量必须写出 self 有了明确的理解。实际上，对成员变量的访问，其实都是通过 self 定位的，平时省去 self 不写，让人有种直接访问了成员变量的感觉，但实际上仍然是有对 self 的访问的，这点在 closures 中是不能忽略的，你必须知道 closures 中对成员变量的访问是通过 self 的，从而明确这个 closure 是捕获了 self，**而非这个成员变量**。

## 一点故事

在理解捕获变量的时候，我隐约记得似乎在哪种语言中有这样一种处理，能做一种字面上的绑定，让两个变量完完全全指同一个变量，同时在读 Swift 官方手册的时候有种莫名的自信，质疑手册中所说的 strong references 是错误的，认为应当是字面上的绑定。为此，我专门写了一个例子。

注意，下面的例子须在真实环境下运行，而非 playground，**playground 可能会导致某些引用计数高于预期**。

因为手册中说的 closures 中捕获的变量是 strong reference，那么捕获一定会让这个变量指向的实例的引用计数增加，如果引用计数没有增加，即可证明手册错了。

于是我首先查到了怎样获取一个实例的引用计数，

```swift
import Foundation

// public func CFGetRetainCount(_ cf: CFTypeRef!) -> CFIndex
class C {}

CFGetRetainCount(C()) // 1
let i = C()

CFGetRetainCount(i) // 2
```

猜测 `CFGetRetainCount` 在获取某个实例的引用计数时，进入函数后函数自身的参数让其引用计数增加了 1，所以得到的结果比我们预期的要多 1。

注意，虽然 closures 是引用类型，但按照 Swift 文档的说法，引用计数只应用在类的实例上，所以，讨论 closure 的引用计数是错误的，更不应该把一个 closure 传递给 `CFGetRetainCount` 了。

```swift
let closure = {}

let alsoClosure = closure

CFGetRetainCount(alsoClosure as CFTypeRef) // 1, may expected 2, but in fact, closures don't have reference counts.
```

然后我开始做实验来验证自己的观点，被捕获的实例并不会增加引用计数。

```swift
import Foundation

class C {}

let i = C()

print("Before closure declaration: \(CFGetRetainCount(i))")

let closure = {
  print("In closure: \(CFGetRetainCount(i))") // In fact, the var i is not captured, because it's a global var.
}

print("Before closure calling: \(CFGetRetainCount(i))")

closure()

print("After closure calling: \(CFGetRetainCount(i))")

// Before closure declaration: 2
// Before closure calling: 2
// In closure: 2
// After closure calling: 2
```

事实上，变量 `i` 并没有捕获，因为它是全局变量，无须捕获。

基于这个错误的结论去解释其它问题就会出现矛盾的地方，而又很难想明白原因，更可怕的是，以此又得出了另一个错误的结论。

实际上，如果这个例子中的 `i` 不是全局变量，就能很容易地得出我之前的假设是错误的。

```swift
import Foundation

class C {}

func test() { // Wrap the code in the function to make sure var `i` is not a global var.
    let i = C()

    print("Before closure declaration: \(CFGetRetainCount(i))")

    let closure = {
        print("In closure: \(CFGetRetainCount(i))") // In fact, the var i is not captured, because it's a global var.
    }

    print("Before closure calling: \(CFGetRetainCount(i))")

    closure()

    print("After closure calling: \(CFGetRetainCount(i))")
}

test()

// Before closure declaration: 2
// Before closure calling: 3
// In closure: 4
// After closure calling: 3
```

我们可以看到，在 `closure` 申明之后，`i` 的引用计数增加了。
另外，在 `closure` 执行时，`i` 的引用计数又增加了，这个现象不在语言本身的规范中，分析原因没有实际意义，因为可能在某些情况下这个值也会发生改变。

# Closures 是引用类型（Reference Types）

- 如果一个 closure 是常量（constant），如前面例子中的 `incrementBySeven` 和 `incrementByTen`，但是它捕获的变量仍然可以被改变，类似于实例（instance）也是 reference types，尽管一个常量被设置为一个实例，但这个实例的成员变量仍然是可变的。

- 如果把一个 closure 赋值给两个不同的常量或变量，这两个常量或变量指的是同一个 closure，并没有对 closure 进行了 copy。对上面的例子，通过两个变量调用 closure，都会改变同一个 closure 中捕获的值。

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// returns a value of 50
```

# Escaping Closures

一个 closure escapes 一个函数，那么这个 closure 就叫做 escaping closure。
那么如何定义 escape，一个函数接受一个 closure 作为其参数，而这个 closure 在函数返回之后才被调用，那么就说，这个 closure escapes 了这个函数。

一个 closure escapes 了一个函数，是一个动作，看起来好像是这个 closure 想这样做的，实际上是这个函数的具体实现决定的。

对于这个函数来说，被传入的 closure 不一定总是 escape，可能在某些情况下不会 escape，但不管怎样，只要传入的 closure 有 escape 的可能，这个参数就得被标记为 `@escaping`。

其实这个函数也挺“冤”的，它只是没有执行这个 closure，并不是做了什么特殊的事情了，像是“不作为”。这也就不难理解在早先的 Swift 版本中，标记的是 `@nonescaping`。

常见的情况是，把这个传入的 closure 保存在了函数之外，在之后的某一时刻通过其它方式调用，比如 completionHandler。

如果一个函数的某个参数被指定为 `@escaping` 的 closure，那这个 closure 中不能隐式引入 self。目的和前面提到的一样，访问成员变量本质都是通过 self 访问的，省略 self 给人一种能直接访问成员变量的感觉，显式 self 访问确保写代码的人明确这一点，从而在需要的时候通过一些办法来避免 strong reference cycle。

# Autoclosures

Autoclosures 是在一定情况下自动创建的 closure，用来把作为参数传入函数的表达式包裹在这个 closure 中。Autoclosure 是名词，最常用的说法是一个函数接受一个 autoclosure（a function takes an autoclosure）。

一般情况，如果一个函数接受一个表达式作为参数，这个表达式会先求值，然后再传入函数，如果这个参数换成了 autoclosure（标记为 `@autoclosure`）传入的就是包含这个表达式的 closure，也就是说这个 closure 的执行取决于这个函数，可以，

- 不执行

- 执行

- 在函数返回之后执行

上面三种情况分别对应了，

- 避免表达式求值时产生的副作用和计算开销

- 需要时求值

- 推迟表达式求值，让这个 autoclosure escapes 这个函数

assert 中的两个 autoclosure 参数分别体现了前两种情况。

```swift
func assert(_ condition: @autoclosure () -> Bool, _ message: @autoclosure () -> String = default, file: StaticString = #file, line: UInt = #line)
```

第一个参数是要判断真假的表达式，assert 函数内部会判断是否是在 debug，是的话才对这个表达式求值，否则，就不求值。如果这个参数不是 autoclosure，那么被传入的表达式会在进入函数之前就求值，那么在不是 debug 的情况下，这个表达式也会进行求值，会多出额外的开销，更坏的情况下，如果这个表达式还有副作用，会产生意料之外的效果，不过对于 assert 来说，这样的情况并不常见。

可参考 [Building assert() in Swift, Part 1: Lazy Evaluation](https://developer.apple.com/swift/blog/?id=4)。

注意：一般情况下，接受 autoclosure 的函数，用用就行了，不要自己写，会让代码难以理解。文档中两次强调。

> It’s common to call functions that take autoclosures, but it’s not common to implement that kind of function.

> NOTE
>
> Overusing autoclosures can make your code hard to understand. The context and function name should make it clear that evaluation is being deferred.

这就是那种不常见的情况，那种需要写注释的情况，说明为什么要用 autoclosure，为了避免副作用，或是计算开销。
