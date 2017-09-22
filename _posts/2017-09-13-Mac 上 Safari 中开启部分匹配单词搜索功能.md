---
title: "Mac 上 Safari 中开启部分匹配单词搜索功能"
---

Mac 上 Safari 中使用 Command+F 搜索网页文字默认是从单词的开始匹配，比如，要想搜索页面中的 command 单词，一定要从头开始输入，如果只输入 mmand 则找不到页面中的 command 单词。

这种部分匹配单词的搜索功能对于在页面中搜索代码很有帮助，因为像 `viewDidLoad` 这样的标识符在网页中会被认为是一个单词，必须要从头输入才能匹配。

通过下面的命令可以开启该功能，打开终端，输入 `defaults write com.apple.Safari FindOnPageMatchesWordStartsOnly -bool FALSE`。

关于搜索性能方面，多少会有些影响，但对于一般大小的页面，几乎感觉不到差别。
