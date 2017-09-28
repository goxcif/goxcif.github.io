---
title: "关于 Optional Value"
---

对于一个 optional value，`var i: Int?` 或者 implicitly unwrapped optionals, `var i: Int!`，都可以做下面这些操作，

`if let i = i ...`

`i?.intMethod`

`if i == nil ...` 或者 `if i != nil ...`

但是对于 `if i == 5`，

如果 `i` 是普通 optional value 类型不匹配，无法通过编译，如果 `i` 是 implicitly unwrapped optionals，则编译通过，在 `i` 为 `nil` 时会出现运行时错误。

# 关于 Implicitly Unwrapped Optionals

> An implicitly unwrapped optional is a normal optional behind the scenes, but can also be used like a nonoptional value, without the need to unwrap the optional value each time it’s accessed.
> -- [The Swift Programming Language](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

## Check if nil

> You can still treat an implicitly unwrapped optional like a normal optional, to check if it contains a value)

> You can also use an implicitly unwrapped optional with optional binding, to check and unwrap its value in a single statement

> -- [The Swift Programming Language](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

如前面提到的，

`if let i = i ...`

`if i == nil ...` 或者 `if i != nil ...`

没错，你可以像上面这样检查 implicitly unwrapped optionals，但是在这种情况下，你应该使用普通 optional，而非 implicitly unwrapped optionals。

> Always use a normal optional type if you need to check for a nil value during the lifetime of a variable.
> -- [The Swift Programming Language](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

## Compromise between safety and convenience.

> Sometimes it is clear from a program’s structure that an optional will always have a value, after the value is first set. In these cases, it is useful to remove the need to check and unwrap the optional’s value every time it is accessed, because it can be safely assumed to have a value all of the time.
> -- [The Swift Programming Language](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

> it may be obvious that an optional will always have a value when you write the code … but code changes and assumptions change.
> -- [When Should You Use Implicitly Unwrapped Optionals](https://cocoacasts.com/when-should-you-use-implicitly-unwrapped-optionals/)

尤其在他人修改代码时，可能做出不符合这个假设的改动。

# 说到底，为什么要避免强制解包？

Implicitly Unwrapped Optionals 其实也是强制解包，那到底为什么要避免强制解包？

因为强制解包会造成运行时错误。

> Trying to use ! to access a nonexistent optional value triggers a runtime error. Always make sure that an optional contains a non-nil value before using ! to force-unwrap its value.

> If an implicitly unwrapped optional is nil and you try to access its wrapped value, you’ll trigger a runtime error. The result is exactly the same as if you place an exclamation mark after a normal optional that doesn’t contain a value.

> -- [The Swift Programming Language](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

那为什么要避免运行时错误呢。。

# 几个适合强制解包的场景

“尽早崩溃” 或者 逻辑错误（程序员犯错的结果）

- 必须设置但又无法在初始化时设置的属性。Interface Builder 的 outlets 或必要的 delegate

- 读取应用程序包里的文件的时候，因为是只读的，所以这个文件一定存在，使用 `try!` 读取，姑且也当作是强制解包吧。

- Swift 文档中的一个例子，https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html

  ```swift
  let digitNames = [
      0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
      5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
  ]

  let number = ...

  digitNames[number % 10]!
  ```

  保留意见，这种情况不容易界定，且修改代码时可能带来隐患，建议不使用强制解包。
