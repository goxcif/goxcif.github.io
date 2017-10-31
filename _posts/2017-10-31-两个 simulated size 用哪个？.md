---
title: "两个 simulated size 用哪个？"
---

Interface Builder 中选中一个 view controller，右边 inspector 中能找到两个 simulated size，

- Attributes inspector -> Simulated Metrics -> Size -> Freeform
- Size inspector -> Simulated Size -> Freeform

[How to change the size of a view controller on the storyboard for editing purpose?](https://stackoverflow.com/a/17871868) 这个答案中提到后者是 Xcode 5 之后加入的。

二者的目的都是设置一个在 IB 上展示用的尺寸，不会影响运行时的实际大小。

按照上面提到的答案中的说法，二者在设置之后，都可以到这个 view controller 所包含的 view 的属性中设置其尺寸，但我观察，就目前的 Xcode 9，前者在设置后是无法再到其 view 中设置尺寸的，所以只用后者就好了。

后者在设置时后面紧跟着该 view controller 的大小设置，这个大小也可以在其 view 的大小设置处设置，二者的变化是一致的。

结论就是，如果需要设置 simulated size，直接在 view controller 的 Size inspector 下设置为 Freeform 并设置大小就可以了。
