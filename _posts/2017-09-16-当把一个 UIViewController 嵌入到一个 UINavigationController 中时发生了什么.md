---
title: "当把一个 UIViewController 嵌入到一个 UINavigationController 中时发生了什么"
---

当你使用 Embed In 把一个 view controller 嵌入到一个 navigation controller 的时候，一个 navigation item 会被自动添加到这个 view controller 中。

每个被嵌入到 navigation controller 中的 view controller 都有自己的 navigation item 用来配置显示在 navigation bar 上的标题和 buttons 和 views（包括标题）。

> When the navigation controller slides a new view controller into the screen, it replaces the contents of the navigation bar with that view controller’s Navigation Item. 
> -- iOS Apprentice