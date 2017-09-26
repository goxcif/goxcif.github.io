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

Compromise between safety and convenience.

> Sometimes it is clear from a program’s structure that an optional will always have a value, after the value is first set. In these cases, it is useful to remove the need to check and unwrap the optional’s value every time it is accessed, because it can be safely assumed to have a value all of the time.
> -- [The Swift Programming Language](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

> it may be obvious that an optional will always have a value when you write the code … but code changes and assumptions change.
> -- [When Should You Use Implicitly Unwrapped Optionals](https://cocoacasts.com/when-should-you-use-implicitly-unwrapped-optionals/)

尤其在他人修改代码时，可能做出不符合这个假设的改动。

# 说到底，为什么要避免强制解包？

Implicitly Unwrapped Optionals 其实也是强制解包，那到底为什么要避免强制解包？

因为强制解包会造成运行时错误。

> Trying to use ! to access a nonexistent optional value triggers a runtime error. Always make sure that an optional contains a non-nil value before using ! to force-unwrap its value.
> -- https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html

那为什么要避免运行时错误呢。。
