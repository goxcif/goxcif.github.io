---
title: "Constant stored property 和 read-only computed properties"
---

```swift
let i = 0
```

```swift
var i: Int {
  return 0
}
```

用 let 好啊，自动 final，不能被覆盖。

[Is it impossible to override a constant stored property?](https://stackoverflow.com/questions/35671819/is-it-impossible-to-override-a-constant-stored-property) 这段讨论认为 final let 和 let 没区别，但是这个结论并没有找到来源。

只在需要时再改。比如，需要覆盖，就改成 read-only computed properties，需要修改，就改成 stored properties。

文档中说的 只读 能被覆盖为 读写，但 读写 不能被覆盖为 只读，说的是 computed properties，不是 constant stored properties。