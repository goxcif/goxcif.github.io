---
title: "macOS 下的按键绑定（key binding）参考"
---

* [DefaultKeyBinding.dict](http://osxnotes.net/keybindings.html)

* 支持 XML 版本的 plist 格式。

	```xml
	<dict>
		<key>^ </key>
		<string>setMark:</string>
		<key>^/</key>
		<string>undo:</string>
		<key>^k</key>
		<array>
			<string>insertText:</string>
			<string>「」</string>
			<string>moveBackward:</string>
		</array>
		<key>^y</key>
		<string>yankAndSelect:</string>
		<key>^~h</key>
		<string>deleteWordBackward:</string>
		<key>~$\@</key>
		<string>selectWord:</string>
		<key>~&lt;</key>
		<string>moveToBeginningOfDocument:</string>
	</dict>
	```

* 修改配置文件建议使用各类代码编辑器。

* 测试快捷键时可以使用系统自带的 TextEdit，每次配置之后需要重启 TextEdit。

* 不建议对配置文件使用软链接。

* Xcode 有自己的配置，位于 ~/Library/Developer/Xcode/UserData/KeyBindings/Default.idekeybindings 文件下的 `Key Bindings` 下。

