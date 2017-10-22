---
title: "Swift 中的 metatype"
---

按照 [The Swift Programming Language (Swift 4): Types](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Types.html) 中 Metatype Type 一节的定义，metatype 类型就是任意类型的类型。

> A metatype type refers to the type of any type, including class types, structure types, enumeration types, and protocol types.

首先，metatype 是类型，是某个具体类型的类型，比如说，对于 Int，你可以说 Int 的类型，也可以说 Int 的 metatype，指的是一回事，但是毕竟对着一个类型问它的类型有点乱，所以统一叫做 metatype，不过如果对着一个具体的值就不一样了，比如对于数字 42，42 的类型是 Int，但 42 的 metatype 是 Int 的类型，就不是一回事了。

文档中避免抠语义，直接上定义，

> The metatype of a class, structure, or enumeration type is the name of that type followed by .Type. The metatype of a protocol type—not the concrete type that conforms to the protocol at runtime—is the name of that protocol followed by .Protocol.

简单说，对于一个协议，它的 metatype 就是这个协议的名字后面跟上 `.Protocol`，对于一个不是协议的类型，可以是一个类或结构体或枚举，它的 metatype 就是这个类型的名字后面跟上 `.Type`。

这个定义简直如同“我是郭晓晨，郭晓晨是我”一般，metatype 是 `.Protocol` 和 `.Type`，`.Protocol` 和 `.Type` 是 metatype。谁也别纠心。

接下来是 `.self`，文档中更是不多解释，直接上用途，

> You can use the postfix self expression to access a type as a value.

Swift 中，一个类型的名字（比如 Int），除了声明和定义，是没法像值那样正常用在表达式当中的，所以 `.self` 的作用就是返回自己，但是这个返回的自己能用在和 metatype 相关的操作上。（`.self` 返回的可以用在运行时）

对于实现了一个协议的一个类型，

问题：这个类型的 metatype 是什么？

这个类型的 `.Type`，因为问的就是这个类型的 metatype。如果问的就是一个协议的 metatype，那就是这个协议的 `.Protocol`。

问题：这个类型的一个值的 metatype 是什么？






type(of:)

.Type
.self
[Swift - what's the difference between metatype .Type and .self?](https://stackoverflow.com/questions/31438368/swift-whats-the-difference-between-metatype-type-and-self)