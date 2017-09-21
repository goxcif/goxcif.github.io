---
title: "Launch Screen 告诉系统在多大的屏幕上进行布局"
---

如果不人为添加约束，UIKit 也还是会自动添加的。

所以，如果不人为添加约束，只要系统认为该应用支持当前屏幕尺寸，仍然会按照自动添加的约束按照当前屏幕尺寸进行自动布局。但是 UIKit 怎样自动添加约束是 undefined，显示结果就是不确定的。所以，如果没有人为加约束，就一定不要让系统误以为该应用支持当前屏幕尺寸进而用 undefined 的方式进行布局。

那么，怎样告诉系统该应用支持的屏幕尺寸？如果不支持当前的屏幕尺寸，系统怎么处理？

Launch screen 支持的屏幕尺寸就是该应用支持的屏幕尺寸。

Launch screen 默认是一个 storyboard，它本身是能够自动布局的（人为或自动添加的约束），也就是说这样的 launch screen 支持各种屏幕尺寸，系统也支持。

如果删除掉这个 storyboard，但是没有删除掉 Info.plist 中的设置，系统会提供一个纯白的支持所有屏幕尺寸的 launch screen（我猜的），所以这种情况下，系统还是支持所有尺寸。

如果删掉 Info.plist 中的 launch screen 的设置，我看到的是系统用撑大的程序图标做 launch screen。这种情况下，系统认为该应用只支持 3.5 inch。

![](/img/002.png)

写到这里，launch screen 留或不留，要么支持所有屏幕尺寸，要么只支持 3.5 inch。如果想只支持某个其它的尺寸呢？

可以指定 launch screen 图片（添加特殊命名的图片文件来给系统作为 launch screen），比如 Default-568h@2x.png，这样系统会把它作为 4 inch 屏幕设备下该应用的 launch screen，进而系统认为该应用支持 4 inch 设备。这种情况下，4.7 inch 和 5.5 inch 设备都能够按照 4 inch 布局，然后撑大（4 inch、4.7 inch、5.5 inch 的屏幕长宽比例几乎是一样的）。

别的尺寸的 launch screen 图片的特殊命名可以参考 http://www.ios-developer.net/iphone-ipad-programmer/icons_and_graphics/default-image。

至于单独指定或混合指定 launch screen 图片是什么效果，我不知道且认为没有意义。
