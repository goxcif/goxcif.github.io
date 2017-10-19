---
title: "对同一个 property 为什么不能在覆盖时同时写出 overriding setters 和 overriding property observers"
---

[The Swift Programming Language (Swift 4): Inheritance](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html) 文档中并没有给出 overriding setters 的例子。

可以在 setter 中通过 `super` 对父类中被覆盖的 property 赋值，如下。

```swift
class C {
    var i = 42
}

class D: C {
    override var i: Int {
        get {
            return super.i
        }
        set {
            super.i = newValue
        }
    }
}
```

回到标题中的问题，对同一个 property 为什么不能在覆盖时同时写出 overriding setters 和 overriding property observers？文档中只说了“不能”，但没说为什么。

> Note also that you cannot provide both an overriding setter and an overriding property observer for the same property. If you want to observe changes to a property’s value, and you are already providing a custom setter for that property, you can simply observe any value changes from within the custom setter. -- [The Swift Programming Language (Swift 4): Inheritance](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html)

看下面的例子，

```swift
class C {
    var i = 42 {
        didSet {
            print("i is set from \(oldValue) to \(i)")
        }
    }
}

class D: C {
    override var i: Int {
        get {
            return super.i
        }
        set {
            super.i = newValue
        }
    }
}

let d = D()
d.i = 3

// i is set from 42 to 3
```

D 类中覆盖了父类 C 中的 property `i`，并在 setter 中修改了 `super.i`，这使得父类中 `i` 的 property observers 被调用，打印出了“i is set from 42 to 3”。

~~如果覆盖时允许同时提供 overriding setters 和 overriding property observers，我们就可以在上面的 D 类中再添加 `i` 的 property observers。~~

~~在 observers 中我们还可以继承父类中 `i` 的 property observers。这时，当 D 类的实例中的 `i` 被修改时，D 类中 `i` 的 observers 会调用 C 类中的 observers，而 D 类中 `i` 的 setter 又会修改 C 类中的 `i` 从而导致 C 类中的 observers 再一次被调用。~~

根本无法在 didSet 中调用父类的该属性的 didSet，没这种语法。

---

标题的问题，对同一个 property 为什么不能在覆盖时同时写出 overriding setters 和 overriding property observers，其实这个问题和覆盖无关，对于一个 property，它本身就无法同时提供 setters 和 observers，见 [Swift 中关于属性的猜测]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})。

保留上面的段落，是想保留这部分思路，另外，能在子类 setter 中通过 super 修改父类的该属性这点，也值得保留。
