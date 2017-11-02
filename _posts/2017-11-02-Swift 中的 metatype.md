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

Swift 中，一个类型的名字（比如 Int），除了声明和定义，是没法像值那样正常用在表达式当中的，所以 `.self` 的作用就是返回自己，但是这个返回的自己能用在和 metatype 相关的操作上。`.self` 返回的东西可以用在运行时。

这里补充下静态类型（编译时类型）、动态类型（运行时类型）的内容，详见 [type(of:) - Swift Standard Library \| Apple Developer Documentation](https://developer.apple.com/documentation/swift/2885064-type)。

一个值的静态类型指它的已知的、编译时的类型。
一个值的动态类型指运行时它实际的类型。（The dynamic type of a value is the value’s actual type at run-time, which can be nested inside its concrete type. 后半句我没看懂。）

```swift
func printInfo(_ value: Any) {
    let type = type(of: value)
    print("'\(value)' of type '\(type)'")
}

let count: Int = 5
printInfo(count)
// '5' of type 'Int'
```

当 count 传递给 printInfo(_:) 函数时，参数 `value` 的静态类型是 Any，动态类型是 Int。

> The dynamic type returned from type(of:) is a concrete metatype (T.Type) for a class, structure, enumeration, or other nonprotocol type T, or an existential metatype (P.Type) for a protocol or protocol composition P.

注意这句话中的 P.Type，应该是 P.Protocol。原因如下，

```swift
// run in playground
protocol P {}
class C: P {}

func printGenericInfo<T>(_ value: T) {
    type(of: value) // P.Protocol
}

let p: P = C()
printGenericInfo(p)

type(of: p) // C.Type
```

---

<details markdown="1">

<summary>关于 existential metatype</summary>

注：我对 existential metatype 的理解很可能是错误的，因为根据我的理解，对 [type(of:) - Swift Standard Library \| Apple Developer Documentation](https://developer.apple.com/documentation/swift/2885064-type) 文档中的“Finding the Dynamic Type in a Generic Context”部分的叙述解释不清。

关于 existential metatype，我没有找到准确的定义，我觉得一样可以像上面的“A 是 B，B 是 A”的模式理解，不影响使用。一个协议的 `.Protocol` 就是一个 existential metatype。

Haskell 中有类似的概念，见 [Existential type - HaskellWiki](https://wiki.haskell.org/Existential_type)，注意这里说的是 existential type 不是 existential metatype。

> Existential types can be used for several different purposes. But what they do is to 'hide' a type variable on the right-hand side.

```haskell
data Worker x y = Worker {buffer :: b, input :: x, output :: y}
```

上面这句会报错，因为右边的 buffer 没有指定类型，注意这是一个类型变量，需要提供的是一个具体的类型。

```haskell
data Worker b x y = Worker {buffer :: b, input :: x, output :: y}
```

如果写成这样，不会报错，但是这里实际想表达的是 buffer 必须是 Buffer（一个 class，这里可以暂且当成 Swift 中的 protocol 理解）的一个 instance（可以理解成遵守一个协议的某一个类型）。

于是，Worker 出现在函数中时，必须加上 b 的限定，如下

```haskell
foo :: (Buffer b) => Worker b Int Int
```

通过使用 existential types，我们可以避免这个问题，

```haskell
data Worker x y = forall b. Buffer b => Worker {buffer :: b, input :: x, output :: y}
 
foo :: Worker Int Int
```

我的理解是，existential types 就是限定其它类型的类型，对应到 Swift 上就是协议，existential metatype 就应该是限定其它类型的类型的类型，也就是上面提到的一个协议的 `.Protocol`。

</details>

---

对于实现了一个协议的一个类型，

  问题：这个类型的 metatype 是什么？

  这个类型的 `.Type`，因为问的就是这个类型的 metatype。如果问的就是一个协议的 metatype，那就是这个协议的 `.Protocol`。

  问题：这个类型的一个值的 metatype 是什么？

  按理说返回的应该是这个值的类型的 metatype，也就是上个问题中的这个类型的 `.Type`。但是如果这个值被一个变量或常量持有，而该常量或变量的类型有可能是这个类型，也可能是这个协议，此时，如果问这个常量或者变量的 metatype，取决于它的静态类型，如果是这个类型，就是这个类型的 `.Type`，如果是这个协议，就是这个协议的 `.Protocol`。


# type(of:) 的用法

```swift
func type<T, Metatype>(of value: T) -> Metatype
```

返回的是 Metatype 的实例，也就是一个具体的类型，用这个返回的类型，我们可以调用它的构造器或者静态属性或者方法。

> Returns the dynamic type of a value.

它返回的是一个值的动态类型。

## 范型上下文下的动态类型

在一个范型上下文下，一个类型参数 T 可能是一个类型也可能是一个协议，对 T 类型的值调用 `type(of:)` 返回的是 T.Type，它有可能是一个类型的 `.Type`，也可能是一个协议的 `.Protocol`，如果这里的 T 是一个协议，那我们得到的就是这个协议的 `.Protocol`，而我们使用 `type(of:)` 想要得到的是这个值的动态类型，其类型应该是它的类型的 `.Type`。

这时，我们需要将类型为 T 的值在传递给 `type(of:)` 之前先 `as Any`，这样 `type(of:)` 的返回值 T.Type 就成了 Any.Type 了。

---

其它非官方参考：

- [Anyclass, meta type and .self](http://en.swifter.tips/self-anyclass/)

- [Swift - what's the difference between metatype .Type and .self?](https://stackoverflow.com/a/41709040)
