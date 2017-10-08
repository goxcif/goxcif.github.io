---
title: "Swift 中函数的 type casting"
---

这篇文章只是从 Swift 语言外部做一些试验，不涉及内部实现。

```swift
class A {}
class B: A {}

func a2b(_ a: A) -> B { return B() }
func a2a(_ a: A) -> A { return A() }
func b2a(_ b: B) -> A { return A() }
func b2b(_ b: B) -> B { return B() }



var general2general: (A) -> A = a2b
var general2specific: (A) -> B = a2b
var specific2general: (B) -> A = a2b
var specific2specific: (B) -> B = a2b

a2b is (A) -> A // false
a2b is (A) -> B // true
a2b is (B) -> A // false
a2b is (B) -> B // false

general2general is (A) -> B // false
general2specific is (A) -> B // true
specific2general is (A) -> B // false
specific2specific is (A) -> B // false



general2general = a2a
// general2specific = a2a // error: cannot assign value of type '(A) -> A' to type '(A) -> B'
specific2general = a2a
// specific2specific = a2a // error: cannot assign value of type '(A) -> A' to type '(B) -> B'

a2a is (A) -> A // true
a2a is (A) -> B // false
a2a is (B) -> A // false
a2a is (B) -> B // false

general2general is (A) -> A // true
specific2general is (A) -> A // false



// general2general = b2a // error
// general2specific = b2a // error
specific2general = b2a
// specific2specific = b2a // error

b2a is (A) -> A // false
b2a is (A) -> B // false
b2a is (B) -> A // true
b2a is (B) -> B // false

specific2general is (B) -> A // true



// general2general = b2b // error
// general2specific = b2b // error
specific2general = b2b
specific2specific = b2b

b2b is (A) -> A // false
b2b is (A) -> B // false
b2b is (B) -> A // false
b2b is (B) -> B // true

specific2general is (B) -> B // false
specific2specific is (B) -> B // true
```

代码除去开始的几行定义，剩下的分四组，每组中的第一部分都是试图把一个特定类型的函数赋值给比它更泛化的类型的变量，注释为 error 的表明左值没有比右值更泛化，所以出错了。

可以看到，**左值的函数类型相比右值，其中的参数更具体、返回值更泛化时，则左值整体的类型更泛化，一旦有一个参数更泛化、或者返回值更具体，左值则无法接受赋值。**

按理说，赋值成功说明左值比右值更泛化，右值应该 `is` 左值的类型，且赋值后的左值应该 `is` 右值的类型，如下，

```swift
class A {}
class B: A {}

let b = B()
let a: A = b
b is A // true
a is B // true
```

每组代码中后面的部分就是在测试这一点，但是结果并非如此，事实上，这些函数在用 `is` 测试类型时，只有在 `is` 它们声明时的类型时才能返回 true。

使用 `as?` 或 `as!` 将赋值成功后的左值转换为右值的类型按理说也应该成功，

```swift
a as? B // 返回非 nil 的值
```

但对于函数类型来说，并不成立，如下，当 `specific2general = b2b` 赋值成功后，`specific2general as? (B) -> B` 返回的是 nil。估计 `as?` 和 `as!` 是基于 `is` 的，`specific2general` 不 `is` `(B) -> B`，类型转换当然也就不会成立了。

总的来说，对于函数类型的值，

- 能赋值给更泛化的变量

- 只 `is` 其声明时的类型

- `as?` 和 `as!` 只能转换成更泛化的类型
