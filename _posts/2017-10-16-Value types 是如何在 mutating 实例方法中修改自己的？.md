---
title: "Value types 是如何在 mutating 实例方法中修改自己的？"
---

基于[文档](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Methods.html)中的以下描述，

> The method can then mutate (that is, change) its properties from within the method, and any changes that it makes are written back to the original structure **when the method ends**.

```swift
mutating func moveBy(x deltaX: Double, y deltaY: Double) {
    x += deltaX
    y += deltaY
}
```

> Mutating methods can assign an entirely new instance to the implicit self property.

```swift
mutating func moveBy(x deltaX: Double, y deltaY: Double) {
    self = Point(x: x + deltaX, y: y + deltaY)
}
```

我猜测 value types 的 mutating 方法是这样实现的，

在方法开始时，Swift 设置局部变量 self 为自身的完整可变拷贝，

`var self = 当前实例`

然后执行这个方法，期间可以隐式或显式地修改 self 这个局部变量。

在函数结束时，谁调用的该方法，将 self 赋值给谁，如果是常量则无法调用 mutating 方法，因为无法将 self 赋值给调用该方法的常量。
