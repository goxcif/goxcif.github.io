---
title: "UIImage 的 capInsets 和 resizingMode"
---

决定一个图片如何变大的两个属性：

- `var capInsets: UIEdgeInsets { get }` （哪些部分变大？）

- `var resizingMode: UIImageResizingMode { get }` （怎样变大？）

注意：阅读 UIImage 的文档时，除非明确指明在说 resizingMode，否则文中提到的 tile 和 stretch 都只是指把图片弄大，并不是指相应的 resizingMode。详情见下段，

文档中出现的 "the entire image is tiled"、"the entire image is subject to stretching"、"when an image is stretched" 等并不是在说 `resizingMode`，而只是想说这个图片被弄大了。另外，`resizingMode` 文档下的 "the region will be stretched instead" 也不是在说 `resizingMode`，强调的是 stretch 这个动作，这句是说，虽然 `resizingMode` 是 `.tile`，但这里的动作还是 stretch，因为在这里，二者效果一样，效率不同。

`UIImage` 本身就是不可变的，要想修改这些属性，只能通过特定函数返回一个新的 UIImage。

# resizingMode

没什么说的，`.tile` 或 `.stretch`。

比如 1 平方米的图片要放在 4 平方米的地方，

前者，放四张图片在这个地方，

后者，把图片的长宽都拉长。

# capInsets

默认情况下，图片整体变大，但是有些情况下希望图片的边缘部分尽量不变，如图，

![](/img/001.png)

准确地说，是四个角不变，四个边缘横变或竖变，中间区域横竖两个方向都变。

`capInsets: UIEdgeInsets` 就是用来决定这个边缘部分的，注意这四个边都是可以为零的。

修改 `capInsets` 的两个方法：

- `func resizableImage(withCapInsets capInsets: UIEdgeInsets) -> UIImage`

- `func resizableImage(withCapInsets capInsets: UIEdgeInsets, resizingMode: UIImageResizingMode) -> UIImage`

区别就是是否指定 `resizingMode` (`.tile` and `.stretch`)

若不指定，`var resizingMode: UIImageResizingMode { get }` 的值不变，capInsets 的文档中所说的 "the entire image is tiled" 只是强调动作针对整个 image，并不是说 `resizingMode` 是 `.tile`。为此，做了个[试验](https://github.com/goxcif/playgrounds/tree/master/UIImage%20%E7%9A%84%20capInsets%20%E5%92%8C%20resizingMode)。

# capInsets、resizingMode 的效率影响

## TL;DR

- `UIEdgeInsets.zero` 的肯定比非零的快

- 一个 pixel 的 resizable areas 的 tile 和 stretch 都是一回事，不管你设置成什么，系统都是照着 stretch 来。

- 不是一个 pixel 的话，我猜 tile 快，可以试验，不过没有意义，这是不同的需求。

---

`UIImage` 的文档中两处涉及关于这两个属性的效率，

- `var resizingMode: UIImageResizingMode { get }` 文档的 discussion 部分

  `resizingMode` 默认 `.tile`，但在 resizable areas 只有 1 pixel 的时候，系统会用 stretch 的方式 resize，因为更高效 (dramatically faster)。

- `func resizableImage(withCapInsets capInsets: UIEdgeInsets) -> UIImage` 文档的 discussion 部分，这里讨论了 `resizingMode` 为默认值 `.tile` 时的情况。

  - `UIEdgeInsets.zero` 效率最高

  - 其次是 resizable areas have a width or height of 1 pixel，此时实际上系统采用的是 stretch 的方式。

  - 最后，是 resizable areas have a width or height greater than 1 pixel，此时还是 `.tile`。
