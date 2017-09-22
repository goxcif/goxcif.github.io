---
title: "在 UIWebView 中加载本地 HTML 文件"
---

首先通过代码或者 storyboard 创建一个 UIWebView，这里命名为 webView。

UIWebView 加载页面的方法如下，

```swift
func load(_ data: Data, mimeType MIMEType: String, textEncodingName: String, baseURL: URL)
```

完整代码如下，

```swift
let url = Bundle.main.url(forResource: "demo", withExtension: "html")!

let webPageData = try! Data(contentsOf: url)

webView.load(webPageData, mimeType: "text/html", textEncodingName: "UTF-8", baseURL: Bundle.main.bundleURL)
```

因为是确定的本地文件，获取 url 和读取页面数据都用了强制解包。

注意 `load` 函数的最后一个参数 `baseURL` 关系到页面内获取其它资源的相对路径（比如网页中的图片路径），如果没有特殊需要，可以设置成 bundle 本身的 URL，即 `Bundle.main.bundleURL` 。

附完整的 playground 示例：https://github.com/goxcif/playgrounds/tree/master/local-html-webview.playground
