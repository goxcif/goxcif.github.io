---
title: "UIView 的 alpha、backgroundColor、isOpaque、isHidden 之间的关系"
---

- UIView 的 alpha 和 backgroundColor 的 alpha 是可以叠加的。

- isOpaque 是一个 hint，仅用于通过 `draw(_:)` 方法自己画内容的 UIView 的子类。

- isHidden 隐藏整个 view，无视其它这几个属性。

十几天前，本来打算做几个 demo 验证一下这几个属性之间有什么关系，后来就不知道干嘛去了，今天拿起来一看，这有什么好试验的，写下来是为了防止以后自己再问自己同样的问题。
