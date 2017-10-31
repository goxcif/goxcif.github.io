---
title: "Simulated Metrics 的 size 设置中的 Page Sheet 和 Form Sheet 是什么意思？"
---

这个设置中有如下几个选项（Xcode 9）：

- Inferred
- Freeform（见 [两个 simulated size 用哪个？]({% post_url 2017-10-31-两个 simulated size 用哪个？ %})
- Page Sheet
- Form Sheet
- Detail
- Master

都是方便在 IB 中看效果的，和实际运行效果无关。

关于 Page Sheet 和 Form Sheet 的效果，[iOS Page sheet & Form sheet](https://stackoverflow.com/a/41255242) 这个答案中有图，基本就是一个大点一个小点。

[UIModalPresentationStyle](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle) 这个枚举类型的 case 们中有 [pageSheet](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle/1621358-pagesheet) 和 [formSheet](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle/1621491-formsheet)，其中有详细的效果解释。有意思的地方，

- Page Sheet 会在横屏模式下让展示视图的宽度为设备在竖屏模式下的宽度。
- Form Sheet 会在弹出键盘时自动上移。

当然，这两个都是作用在水平方向 regular（非 compact）的情况下的，具体行为还得看文档。

UIViewController 的 [modalPresentationStyle](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621355-modalpresentationstyle) 属性的类型是上面提到的 [UIModalPresentationStyle](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle)，其文档讨论部分说了，对于水平方向 compact 的，都是全屏显示，对于水平方向 regular 的，这些 case 们才有效果。

要想在 IB 中看到效果，得先设置 view as 一个水平方向是 regular 的设备，比如 iPad。
