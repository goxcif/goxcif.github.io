---
title: "Swift 中的短路（short-circuit）"
---

# 与或逻辑

```swift
func returnTrue() -> Bool {
    print("returnTrue called")
    return true
}

func returnFalse() -> Bool {
    print("returnFalse called")
    return false
}

true && returnTrue() // returnTrue called
true && returnFalse() // returnFalse called
false && returnTrue()
false && returnFalse()

true || returnTrue()
true || returnFalse()
false || returnTrue() // returnTrue called
false || returnFalse() // returnFalse called
```

# Nil-coalesce operator `??`

```swift
func returnOne() -> Int {
    print("returnOne called")
    return 1
}

let nine: Int? = 9
nine ?? returnOne()
nil ?? returnOne() // returnOne called
```

# 可选值的方法调用

对可选值的方法调用在该可选值为 nil 时该方法不被调用。

```swift
class C {
    func m() {
        print("m")
    }
}

let c: C? = nil
c?.m() // m 方法不会被执行
```

## 可选值的属性 getter 和 setter

因为属性的 getter 和 setter 实际上也是该属性所在类的方法，所以它们也存在上面的情况。

```swift
class C {
    var i: Int {
        get {
            print("i's getter")
            return 42
        }
        set {
            print("i's setter")
        }
    }
}

let c: C? = nil
c?.i = 1 // i 的 setter 不会被执行
print(c?.i) // i 的 getter 不会被执行
```

## 可选值的属性的 observers

```swift
class C {
    var i: Int = 0 {
        willSet {
            print("i's willSet")
        }
        didSet {
            print("i's didSet")
        }
    }
}

let c: C? = nil
c?.i = 1 // i 的 observers 不会被调用
```

## 可选值的方法调用接收的参数的求值

当可选值为 nil 时，其上的方法调用不会被执行，接收到的参数也不会被求值。

```swift
class C {
    func m(_ i: Int) {
        print("m")
    }
}

let c: C? = nil
c?.m(returnOne()) // returnOne 不会被调用
```

## 可选值的属性被赋值时的右值的求值

因为属性本质上就是方法（见 [Swift 中关于属性的猜测]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})），当对属性赋值时，实际上是把右值作为参数传递给该属性的 setter，当该属性所在类的实例为 nil 时，该属性的 setter 不会被调用，而作为其参数的右值同样也不会被求值。

```swift
class C {
    var i = 0
}

let c: C? = nil
c?.i = returnOne() // returnOne 不会被调用
```
