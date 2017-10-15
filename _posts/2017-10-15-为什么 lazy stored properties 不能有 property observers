---
title: "为什么 lazy stored properties 不能有 property observers"
---

文档有说，

> You can add property observers to any stored properties you define, except for lazy stored properties. -- [The Swift Programming Language (Swift 4): Properties](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html)

但文档中没有解释为什么要这样，这里我来猜猜看。

首先做个试验，

```swift
class SomeClass {
    lazy var lazyStoredProperty: Int = {
        print("execute expensive computation")
        return 42
    }()
}

let c = SomeClass()
c.lazyStoredProperty = 0
```

上面例子中的 `lazyStoredProperty`，一直没有被访问过，在最后一句中被赋值（不算访问），这个例子的执行结果是什么都不打印，也就是说它的初始值中定义的 closure 一直没有被执行。

可以看出，Swift 在处理 lazy stored properties 时，只要不访问就不求值，对其赋值也不会强制求它的初始值。这样做是合理的，尽其所能避免 lazy stored properties 的初始化求值，因为这可能是一笔很大的开销。

但想象一下，如果 lazy stored properties 允许有 property observers，那当它被赋值时，会调用两个函数，`willSet` 和 `didSet`。我们考虑一个 lazy stored property 在定义后一直没有被访问过，当它第一次被赋值时，

- 在赋值之前，调用 `willSet`，其实现中可能会访问这个 property 的旧值，这里也就是其初始值，但是因为这是个 lazy stored property，它的初始值此前还没求过，访问这个 property 就会执行对它的初始值求值，从而产生开销，这个求值过程不是代码作者容易意识到的。

- 赋值之后，调用 `didSet`，因为该函数实际上接受了一个 `oldValue` 的参数，这里就是这个 property 的初始值，因为函数传递参数必须先进行求值，所以这里会对其初始值求值。

总的来说，让 lazy stored properties 支持 property observers 后，即便没有访问过一个 property，对其赋值也会造成其初始值求值，但是赋值操作本不需要知道原来的值，这是一笔不必要的开销。
