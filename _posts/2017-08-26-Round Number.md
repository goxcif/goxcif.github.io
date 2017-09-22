---
title: "Round Number"
---

更新：这个方法更好。https://stackoverflow.com/a/41488326

原文中还给出了具体的实现和传统方法的对应。

The [`FloatingPoint` protocol](https://developer.apple.com/reference/swift/floatingpoint) (to which e.g. `Double` and `Float` conforms) blueprints the [`rounded(_:)` method](https://developer.apple.com/reference/swift/floatingpoint/2297073-rounded)

>     func rounded(_ rule: FloatingPointRoundingRule) -> Self

Where [`FloatingPointRoundingRule`](https://developer.apple.com/reference/swift/floatingpointroundingrule) is an enum enumerating a number of different rounding rules:

> **`case awayFromZero`**
>
> Round to the closest allowed value whose magnitude is greater than or
> equal to that of the source.
>
> **`case down`**
>
> Round to the closest allowed value that is less than or equal to the
> source.
>
> **`case toNearestOrAwayFromZero`**
>
> Round to the closest allowed value; if two values are equally close,
> the one with greater magnitude is chosen.
>
> **`case toNearestOrEven`**
>
> Round to the closest allowed value; if two values are equally close,
> the even one is chosen.
>
> **`case towardZero`**
>
> Round to the closest allowed value whose magnitude is less than or
> equal to that of the source.
>
> **`case up`**
>
> Round to the closest allowed value that is greater than or equal to
> the source.

其中，`toNearestOrAwayFromZero` 是远离 0 的四舍五入，`toNearestOrEven` 的类似，但不是完全的四舍五入，而是尽可能地靠向偶数，具体使用场景不清楚。其它几个都是不同方向的取整。

---

旧文分割线

注意：

- 库

- 参数和返回值

- 对于负数

# 四舍五入

## round

```swift

import Darwin.C.math
// included by UIKit

//public func roundf(_: Float) -> Float
//public func round(_: Double) -> Double
//
//public func lroundf(_: Float) -> Int
//public func lround(_: Double) -> Int
//
//public func llroundf(_: Float) -> Int64
//public func llround(_: Double) -> Int64

```

![](/img/011.png)

https://github.com/goxcif/playgrounds/tree/master/round-number.playground
