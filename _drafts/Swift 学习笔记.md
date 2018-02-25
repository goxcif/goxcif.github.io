The Swift Programming Language (Swift 4) 2017-09-19

按照斯坦福公开课 [Developing iOS 10 Apps with Swift](https://itunes.apple.com/us/course/developing-ios-10-apps-with-swift/id1198467120) 中的阅读作业的优先级标注。

# [The Basics](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

## Constants and Variables !!!!

## Comments !!

> Unlike multiline comments in C, multiline comments in Swift can be nested inside other multiline comments.

## Semicolons !!!

## Integers !!!

> Use UInt only when you specifically need an unsigned integer type with the same size as the platform’s native word size. If this isn’t the case, Int is preferred, even when the values to be stored are known to be nonnegative. A consistent use of Int for integer values aids code interoperability, avoids the need to convert between different number types, and matches integer type inference, as described in Type Safety and Type Inference.

只在需要的无符号整型的大小必须符合系统字长的情况下，才选择使用 UInt，即便要存储的值只能是非负的。这样可以避免数字类型转换，且有助于整型类型推断。

## Floating-Point Numbers !!!

> Double has a precision of at least 15 decimal digits, whereas the precision of Float can be as little as 6 decimal digits. The appropriate floating-point type to use depends on the nature and range of values you need to work with in your code. In situations where either type would be appropriate, Double is preferred.

Double 和 Float 都可以的时候，用 Double。

## Type Safety and Type Inference !!!!

## Numeric Literals !!

* 前缀

  - 十进制没前缀，和 C 语言不同，017 还是十进制
  - 二进制 0b
  - 八进制 0o
  - 十六进制 0x

* 浮点数 literals

  - 十进制 或 十六进制
  - 必须有小数点（decimal point）或者 exponent（十进制用 e，十六进制用 p）
  - 十六进制必须有 exponent（用 p 表示）

* 有 exponent 就是浮点数，如 17e2

* 为了 readability，可以 pad 0，加下划线。

  ```swift
  let paddedDouble = 000123.456
  let oneMillion = 1_000_000
  let justOverOneMillion = 1_000_000.000_000_1
  ```

## Numeric Type Conversion !!!

* 尽量用 Int

* 仅在需要时使用特定大小的整型，如 explicitly sized data from an external source, or for performance, memory usage, or other necessary optimization。

* 用浮点数初始化一个整型时，总是会采用截取（truncate）的方法。

## Type Aliases !!

用途：通过一个更符合上下文的名字来引用一个已经存在的类型。

## Booleans !!

## Tuples !!!!

* 除了通过名字访问其中的元素，还可以通过序号（index numbers）。

  ```swift
  print("The status code is \(http404Error.0)")
  // Prints "The status code is 404"
  print("The status message is \(http404Error.1)")
  // Prints "The status message is Not Found"
  ```

* Tuple 只适合临时把相关的值聚集（group）到一起，如果超过了临时的范围，还是用 class 或 structure 吧。

  > Tuples are useful for temporary groups of related values. They’re not suited to the creation of complex data structures. If your data structure is likely to persist beyond a temporary scope, model it as a class or structure, rather than as a tuple.

## Optionals !!!!
## Error Handling

## Assertions and Preconditions !!

* 二者都是用于运行时的。

* Assertions 用于 debug builds，preconditions 用于 debug builds 和 production builds。

* 除了在运行时验证程序的状态预期，它们也能起到文档的作用。

* 和 Error Handling 的区别，assertions 和 preconditions 不能用于可恢复的或预期中的错误。因为在 assertions 和 preconditions 失效时，意味着程序处在一个无效的状态下，这种错误是预料之外的，是不能恢复的，当然，你也可以修改代码，使其能够应对这种无效的状态，这种情况下，也就不能使用 assertions 和 preconditions 了。

* 使用这二者不能替代尽量写好代码以便使无效状态不可能发生。

  > Using assertions and preconditions isn’t a substitute for designing your code in such a way that invalid conditions are unlikely to arise.

  也就是说，某种程度上应该尽量避免使用。

  但这二者也是有好处的，

  - 让程序在无效状态下终止，有助于调试代码。（assertions 和 preconditions）

  - 在无效状态时尽早崩溃有助于减少程序异常造成的损害。（preconditions）

* 如果以 unchecked 优化模式编译（compile in unchecked mode (-Ounchecked)），preconditions 将不会被检查，编译器假设其总是为 true。fatalError 没这问题，总是能中断程序，不管做了什么优化设置。所以开发初期可以用 fatalError 放在暂时没有实现的位置，这样做比用 preconditions 更稳妥些。


# [Basic Operators](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html)

## Terminology !!

## Assignment Operator !!

* 赋值操作本身不返回值，所以不能用在 if 语句的条件判断中。（同样适用于 Compound Assignment Operators，如 `+=`。）

* tuple 的 decompose

  ```swift
  let (x, y) = (1, 2)
  ```

## Arithmetic Operators !!

* 溢出产生运行时错误

* Remainder operator 的结果的符号和被除数（dividend）一致，见 [Remainder 和 modulo 的区别]({% post_url 2017-10-07-Remainder 和 modulo 的区别 %})。

* Unary Minus Operator `-` 和操作数之间不能有空格

  ```swift
  let i = - 9 // error: unary operator cannot be separated from its operand
  ```

* Unary Plus Operator `+` 什么也不做，但有时候可以用来和 `-` 形成对称的代码。

  ```swift
  var board = [Int](repeating: 0, count: finalSquare + 1)
  board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
  board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
  ```
  -- [Control Flow](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html)

## Compound Assignment Operators !!

* 如 `+=`

* 同一般赋值语句，操作本身不返回值。

## Comparison Operators !!

* 注意有两个 identity operators (=== and !==)，用于检测两个对象引用是否指向同一个对象实例。只能用于类实例，closure 虽然是 refetene type，但不是类实例，所以不能对 closure 使用 identity operators。

* tuple 也能比，从左往右比。注意，其中包括的类型本身必须可以比较，比如 Bool 就不能比，包含 Bool 的 tuple 就不能比较。

* tuple 的比较是在标准库里实现的，但是只实现了小于七个元素的情况，再多就要自己实现了。

## Ternary Conditional Operator !!

* 适当使用能简化代码，不当使用会使代码难以阅读，避免多个 ternary conditional operator 用在一个语句中。

## Nil Coalescing Operator !!!!

* 注意 `a ?? b` 是 short-circuit 求值。如果 `a` 不为空，`b` 不会被求值。

## Range Operators !!!

* 分类

  - Closed Range Operator
  - Half-Open Range Operator
  - One-Sided Ranges （本身也有 closed 和 half-open 两种）

* Half-open range 很适合用于遍历数组，因为它不包括右边界（数组长度）。但其实 one-sided range 更合适，都不用考虑数组长度。

* One-sided range 的用法很符合直觉，没有左边界的不能用在 for in 语句中，没有右边界的要考虑什么时候终止 iteration。但是可以用在数组上，对于没有左边界的 range，用在数组上时就是从 0 开始到这个 range 的右边界，对于没有右边界的 range 同理到数组的末尾。

### cs193p_F17 (iOS 11) lecture 3 slides

for-in 语句需要作用在 CountableRange 上，但是 0.5...15.25 只是 Range，不是 CountableRange。需要用 stride:from:to:/by: 方法。

### cs193p (iOS 10) lecture 3 slides

* Range vs CountableRange

  * 如果上下界是 Int 类型，则是 CountableRange。

    * 准确地说，上下界是 "strideable by Int"。

    * 更准确地，CountableRange 的类型如下，

      > public struct CountableRange<Bound> : RandomAccessCollection where Bound : Comparable, Bound : _Strideable, Bound.Stride : SignedInteger

  * CountableRange 有更多功能，包含的值可以被 iterated over（for-in）或 indexed（subscript）。

    * 0.5...15.25 只是 Range<Double>，Double 不符合 strippable by Int，所以不能用在 for-in 语句中。但可以用 `stride(from: 0.5, through: 15.25, by: 0.3)`。

      注：for-in 语句中要求 `in` 后面是 `Sequence`，见 [Swift Language Reference - Statements](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Statements.html)。

* 使用 `..<` 或 `...` 创建 range 时，如果下界大于上界，会出现运行时错误，因为本质上 `..<` 和 `...` 只是 Range 和 ClosedRange 的特殊语法，本身并没有被编译器验证。

* 使用 range 访问数组时越界也会产生运行时错误。

* `[2..<4]` 不能用在 `"hello"` 上，因为前者的类型是 `Range<Int>`，而后者的类型是 `Range<String.Index>`。

## Logical Operators !!

* `!` 后面不能有空格。

* 注意命名，避免双重否定或者令人困惑的逻辑语句。

* `&&` 和 `||` 都 short-circuit。

* 文档推荐 explicit parentheses，即便优先级已经明确。

  ```swift
  if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    ...
  ```


# [Strings and Characters](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html)

## String Literals !!

* 多行字符串可以用一对 `"""` 包裹。

* 注意多行字符串的内容（一对`"""`中间包裹的部分）和结尾分隔符必须 begin on a new line。

  ```swift
  """abc""" // error: multi-line string literal closing delimiter must begin on a new line

  """abc
  """
  // error: multi-line string literal content must begin on a new line

  """
  abc
  """
  // correct way
  ```

* 多行字符串中的换行符也有效，但你换行只是想让源代码更易读，不想让换行生效，可以在行尾加 `\`。

* 如果想在多行字符串一开始和结尾的地方加空行，直接加好了。一对 `"""` 各自所在行之间的行都算。

  ```swift
  let lineBreaks = """

  This string starts with a line break.
  It also ends with a line break.

  """
  ```

* 多行字符串中内容的缩进是从最后的 `"""` 所在的列算起的。

  ![multiline string whitespace](/img/multilineStringWhitespace_2x.png)

  图片来自 Swift 官方文档。

  如果内容中的缩进比结尾 `"""` 的缩进还靠前，会报错。

  ```swift
  """
  abc
      """
  error: insufficient indentation of line in multi-line string literal
  ```

* 多行字符串中如果要包含 `"`，不需要转义，但是如果要包含 `"""`，则需要至少转义一个 `"`。

  ```swift
  let threeDoubleQuotes = """
  Escaping the first quote \"""
  Escaping all three quotes \"\"\"
  """
  ```

## Initializing an Empty String !!!

## String Mutability !!
## Strings Are Value Types !!!!

## Working with Characters !!!

* 可以用 for-in 循环访问一个字符串中的每一个字符。

* 可以创建独立的字符变量通过类型标记。

  ```swift
  let exclamationMark: Character = "!"
  ```

* 字符串可以通过字符数组构建。

  ```swift
  let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
  let catString = String(catCharacters)
  print(catString)
  // Prints "Cat!🐱"
  ```

## Concatenating Strings and Characters !!

* 注意使用 `"""` 标记的多行字符串连接时，字符串的末尾是否有空行，容易把没有空行误以为有空行。

  ```swift
  let badStart = """
  one
  two
  """
  let end = """
  three
  """
  print(badStart + end)
  // Prints two lines:
  // one
  // twothree
  ```

## String Interpolation !!!!

## Unicode !!!

- Unicode 标量（Unicode Scalars）

- Extended Grapheme Clusters

  * Swift 中的每个字符对应一个 extended grapheme cluster。

  * 一个 extended grapheme cluster 是由一个或多个 Unicode 标量组合成的一个人类可读的字符。（An extended grapheme cluster is a sequence of one or more Unicode scalars that (when combined) produce a single human-readable character.）


## Counting Characters !!!!

Swift 中 String 类型的 `count` 使用 extended grapheme clusters 来获取字符串的字符个数，所以字符串拼接和增删字符并不总是会影响字符串的字符个数。

比如，字符串 "cafe" 添加上 COMBINING ACUTE ACCENT (U+0301) 变成 "café"，两个字符串的字符个数不变，都是 4。

Extended grapheme clusters 可能由多个 Unicode 标量组成，这就意味着，不同的字符，或者相同字符的不同表示，可能需要不等量的内存来存储。所以 Swift 中的不同字符的存储空间大小也不总是相同的。要想获取字符串长度，`count` 会遍历整个字符串，对于特别长的字符串，需要注意开销。

`NSString` 的 `length` 就不同了，"based on the number of 16-bit code units within the string’s UTF-16 representation"。

```swift
("cafe" as NSString).length // 4
("café" as NSString).length // 5
```

## Accessing and Modifying a String

* `endIndex` 对应哪个位置？使用 `endIndex` 通过下标访问字符串时是什么结果？

  > The endIndex property is the position after the last character in a String. As a result, the endIndex property isn’t a valid argument to a string’s subscript.

* 如果一个字符串为空，它的 `startIndex` 和 `endIndex` 有什么特点？

  > If a String is empty, startIndex and endIndex are equal.

* 如何获取一个字符串的所有索引？

  > Use the indices property to access all of the indices of individual characters in a string.

  ```swift
  for index in greeting.indices {
      print("\(greeting[index]) ", terminator: "")
  }
  ```

* String、Array、Dictionary、Set 都实现了 `Collection` 协议，该协议包含了哪些方法？

  - `startIndex` `endIndex`
  - `index(before:)` `index(after:)` `index(:offsetBy:)`

* 如何在一个字符串的某个位置处插入字符或者字符串？

  - `insert(_:at:)` 插入字符
  - `insert(contentsOf:at:)` 插入字符串，~~或者是 String.CharacterView（通过字符串的 `characters: String.CharacterView` 获取，可以进一步对其做 range 的下标操作。从 Swift 4 (Xcode 9) 开始，有些操作已经可以直接用在 String 上了比如，`func index(of: Character)` ）。~~ Swift 4 中该方法已弃用。

* String、Array、Dictionary、Set 都实现了 `RangeReplaceableCollection` 协议，该协议包含了哪些方法？

  - `insert(_:at:)` `insert(contentsOf:at:)`
  - `remove(at:)` `removeSubrange(_:)`

## Substrings (Swift 4 新增内容)

* Substring 的典型用法？

  - you use substrings for only a short amount of time while performing actions on a string
  - When you’re ready to store the result for a longer time, you convert the substring to an instance of String

* Substring 有什么特别？

  - a substring can reuse part of the memory that’s used to store the original string, or part of the memory that’s used to store another substring.

  - (Strings have a similar optimization, but if two strings share memory, they are equal.) 我认为这句话的意思是，字符串只有在完全相同的情况下才会共享内存。

* 自写字符串操作函数对于字符串参数的类型有什么新建议？

  > Both String and Substring conform to the StringProtocol protocol, which means it’s often convenient for string-manipulation functions to accept a StringProtocol value. You can call such functions with either a String or Substring value.

## Comparing Strings !!!

> Two String values (or two Character values) are considered equal if their extended grapheme clusters are canonically equivalent. Extended grapheme clusters are canonically equivalent if they have the same linguistic meaning and appearance, even if they’re composed from different Unicode scalars behind the scenes.

- `é` 可以由两个 Unicode 标量组合而成，也可以是一个 Unicode 标量，但它们有 same linguistic meaning and appearance，所以 Swift 认为它们是相同的字符。
- 相反地，LATIN CAPITAL LETTER A (U+0041, or "A"), as used in English, is not equivalent to CYRILLIC CAPITAL LETTER A (U+0410, or "А"), as used in Russian. 两个字符看着像，但是 linguistic meaning 不同，所以 Swift 认为它们是不同的字符。

* 方法 `hasPrefix(_:)` 和 `hasSuffix(_:)` 也是按照 canonical equivalence 比较的。

## Unicode Representations of Strings

* Unicode 标量是 21-bit，当把它们写入文本文件或者其它存储时，有几种 Unicode 定义的编码方式（Unicode-defined encoding forms）。这些 encoding forms 把字符串转换为被称作 code units 的小块。UTF-8、UTF-16、UTF-32 encoding form 分别对应 8-bit、16-bit、32-bit 的 code units。

* Swift 中的字符串有几个属性分别对应这三种 encoding forms。

  - var utf8: String.UTF8View { get set }
  - var utf16: String.UTF16View { get set }
  - var unicodeScalars: String.UnicodeScalarView { get set }

  前二者包含的 `Unicode.UTF8.CodeUnit` 和 `Unicode.UTF16.CodeUnit` 打印出来都是数值，unicodeScalars 包含的 `Unicode.Scalar` 可以打印成字符，可以通过 `value` 属性获取一个 `Unicode.Scalar` 的数值。

# [Collection Types](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html)

## Mutability of Collections !!!!
## Arrays !!!!

* 怎样以默认值创建指定长度的数组？

  ```swift
  var threeDoubles = Array(repeating: 0.0, count: 3)
  // threeDoubles is of type [Double], and equals [0.0, 0.0, 0.0]
  ```

* **怎样把数组中的连续 3 个元素替换成某 2 个元素？**

  ```swift
  shoppingList[4...6] = ["Bananas", "Apples"]
  ```

* 怎样删除数组最后一个元素？

  > If you want to remove the final item from an array, use the removeLast() method rather than the remove(at:) method to avoid the need to query the array’s count property.

* Iterate 数组时怎样获取 index？

  ```swift
  for (index, value) in shoppingList.enumerated() {
      print("Item \(index + 1): \(value)")
  }
  ```

## Sets (ignore first NOTE box in this section) !!!

### Hashable

* 符合该协议的类型可以作为集合中的值类型或者字典中的 key 的类型。

* Swift 中的基础类型（String，Int，Double，Bool）默认都实现了该协议。没有关联值（associated value）的 enum 默认也实现了该协议。

* 注意！`hashValue` **不要求**在不同程序、或者同一程序的不同执行下保持相同。不要存储一个 hash 值在以后执行时使用。

* 实现该协议除了需要实现 `hashValue` 的 gettable 属性，因为 `Hashable` 协议继承了 `Equatable`，还需要实现 `Equatable` 的 (==) 操作。

### Creating a Set with an Array Literal

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
```

这里的 `Set` 必须显式声明，但因为后面的数组中的元素都是 String，Swift 可以推断出这个集合中元素的类型，所以可以省去 `<String>`。

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

### Iterating Over a Set

集合中的元素是没有顺序的，如果需要按照一定的顺序 iterate，可以使用 `sorted()` 方法，它返回一个有序数组。

## Performing Set Operations !!!

![set venn diagram](/img/setVennDiagram_2x.png)

![set euler diagram](/img/setEulerDiagram_2x.png)

- `==`
- `isSubset(of:)` 两个集合也可以相等
- `isSuperset(of:)` 两个集合也可以相等
- `isStrictSubset(of:)` 两个集合**不**可以相等
- `isStrictSuperset(of:)` 两个集合**不**可以相等
- `isDisjoint(with:)` 没有交集

## Dictionaries !!!

### 命名参考

- `var namesOfIntegers = [Int: String]()`
- `var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]`

### Accessing and Modifying a Dictionary

* 字典也有 `count` 和 `isEmpty` 属性。

* 修改字典除了可以用 subscript，也可以使用 `updateValue(_:forKey:)`，如果原来有旧值则返回旧值，没有则返回 nil。

* 删除字典除了可以用 subscript 设置为 nil，也可以使用 `removeValue(forKey:)`，如果原来有旧值则返回旧值，没有则返回 nil。

### Iterating Over a Dictionary

* 用 for-in 语句，每次返回 `(key, value)` 的 tuple。

  ```swift
  for (airportCode, airportName) in airports {
    ...
  ```

* 除了上面这样的用法，也可以不 decompose 这个 tuple。这个 tuple 的元素是 labeled 的，可以用 `key` 和 `value` 分别获取其中的两个元素。

  ```swift
  for t in dictionary {
    print(t)
    print(t.key)
    print(t.value)
  }
  ```

* 也可以只 iterate keys 或者 values，通过字典的 `keys` 和 `value` 属性，注意，它们返回的不是数组，但是是一个可以用 for-in 语句 iterate 的类型。

  * 如果需要数组，可以用它们返回的类型构造数组。

    ```swift
    let airportCodes = [String](airports.keys)
    ```

* 和集合一样，字典中元素没有顺序，如果需要，可以在 `keys` 或者 `values` 上使用 `sorted()` 方法。


# [Control Flow](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html)

## For-In Loops !!

* 怎样 iterate 字典？

  > Each item in the dictionary is returned as a (key, value) tuple when the dictionary is iterated, and you can decompose the (key, value) tuple’s members as explicitly named constants for use within the body of the for-in loop.

  ```swift
  let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
  for (animalName, legCount) in numberOfLegs {
    ...
  ```

  > The contents of a Dictionary are inherently unordered, and iterating over them does not guarantee the order in which they will be retrieved.

* for-in 循环中 for 后面跟的是常量。

* 怎样每 5 个取 1 个？**不包括**最后一个

  `stride(from:to:by:)`

  ```swift
  stride(from: 0, to: 60, by: 5)
  // (0, 5, 10, 15 ... 45, 50, 55)
  ```

* 怎样每 3 个取 1 个，**包括**最后一个

  `stride(from:through:by:)`

  ```swift
  stride(from: 3, through: 12, by: 3)
  // (3, 6, 9, 12)
  ```

## Conditional Statements !!!!

### Switch !!!!

* > based on the first pattern that matches successfully

* 如何在一个 case 分句中标识多种情况（pattern），使其只要符合其中一个，就算符合这个 case？

  通过逗号分割（[Compound Cases](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html#//apple_ref/doc/uid/TP40014097-CH9-ID548)）

  ```swift
  case "a", "A":
    print("The letter A")
  ```

  > For readability, a compound case can also be written over multiple lines.

  > All of the patterns of a compound case have to include the same set of value bindings, and each binding has to get a value of the same type from all of the patterns in the compound case.

  一个 case 下的 compound case 中包含的每个 patterns 绑定的值必须是相同类型，且只需是相同类型（顺序无所谓）。

  ```swift
  case (let distance, 0), (0, let distance):
      print("On an axis, \(distance) from the origin")
  ```

* 神奇的 fallthrough

  > The fallthrough keyword does not check the case conditions for the switch case that it causes execution to fall into. The fallthrough keyword simply causes code execution to move directly to the statements inside the next case (or default case) block, as in C’s standard switch statement behavior.
  
  在一个 case 中遇到 fallthrough 后会直接进入下面的 case 中，不需要检查是否符合下一个 case 的条件，这符合 C 语言中的语义。最好下一个 case 正好是 default，这样不容易乱。

---

插播小知识

- 1..<5 用 a few
- 5..<12 用 several

```swift
let approximateCount = 62
let countedThings = "moons orbiting Saturn"
let naturalCount: String
switch approximateCount {
case 0:
    naturalCount = "no"
case 1..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
case 12..<100:
    naturalCount = "dozens of"
case 100..<1000:
    naturalCount = "hundreds of"
default:
    naturalCount = "many"
}
print("There are \(naturalCount) \(countedThings).")
// Prints "There are dozens of moons orbiting Saturn."
```

---

* Tuple 也可以用在 switch 的 case 语句中，其中的单个元素可以是 值 或者 **range** 或者 下划线（wildcard pattern）。

  ```swift
  let somePoint = (1, 1)
  switch somePoint {
  case (0, 0):
      print("\(somePoint) is at the origin")
  case (_, 0):
      print("\(somePoint) is on the x-axis")
  case (0, _):
      print("\(somePoint) is on the y-axis")
  case (-2...2, -2...2):
      print("\(somePoint) is inside the box")
  default:
      print("\(somePoint) is outside of the box")
  }
  // Prints "(1, 1) is inside the box"
  ```

## Control Transfer Statements !!!!

* 有时 labeled statements 会使代码更清晰。

  ```swift
  gameLoop: while ... {
    switch ... {
      case ...:
        continue gameLoop
      ...
    }
  }
  ```

  因为内层的 switch 语句不可能会用到 continue，所以 continue 只可能是说 continue 外层的 while 语句，但是这里使用标签能使代码更清晰。

## Early Exit !!!

## Checking API Availability !!!

* `if #available(platform name version, ..., *)`，最后的 `*` 的含义是对于其它平台，支持你指定的最小部署目标。

  > The last argument, *, is required and specifies that on any other platform, the body of the if executes on the minimum deployment target specified by your target.

# [Functions](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html)

## Defining and Calling Functions !!!!
## Function Parameters and Return Values !!!!

* 如果函数返回 tuple，有什么需要注意的？

  在函数类型中的返回值类型 tuple 中给其中的元素命名，如，`func minMax(array: [Int]) -> (min: Int, max: Int)`，这样函数中的返回值就不需要命名 tuple 中的元素了，函数调用者也不需要命名函数调用后返回的 tuple 了，可以直接使用这些命名。

* 准确地说，没有返回值的函数实际上是返回了什么值？什么类型？

  返回的值是 `()`（empty tuple），类型是 `Void`。这点在 Calling Methods Through Optional Chaining 时可用于判断函数是否被调用了。

## Function Argument Labels and Parameter Names !!!

* Argument labels 和 parameter names 在使用场景上有什么区别？

  前者用于函数调用，后者用于函数实现。

* 如果函数定义时不写 argument labels 会怎样？

  默认情况下，参数使用 parameter names 作为它们的 argument labels。如果不想要 argument labels，需要在定义函数时用 `_` 标注。

* Swift 为什么要为没有写 argument label 的参数自动添加 parameter name 作为其 argument label？

  见 [Customizing Initialization](#customizing-initialization-)

* 一个函数中的 argument labels 能否相同？parameter names 呢？

  前者可以但不推荐，后者不可以。

* 函数的默认参数值（default parameter values）为什么要放后面？

  这样在函数被调用时，不管是否省略了默认参数，都能很容易地看出是同一个函数被调用了。

  **重点是**，不这样做，函数就没法被调用了。

  ```swift
  func test(_ i: Int = 0, _ b: Bool) {

  }

  test(1, false)
  test(true) // error: missing argument for parameter #2 in call
  ```

  函数可以这样定义，也可以在不省略默认参数的情况下调用，但是没法在省略参数的情况下调用（编译错误）。

* 有默认值的参数不能是哪类参数？

  in-out 参数

* Variadic parameters 怎么用？

  - 在函数声明中的 variadic parameter 的类型后面紧跟着加上 `...`。

  - 函数实现中，这个参数可以作为声明时 `...` 前面的类型的数组使用。

  ```swift
  func arithmeticMean(_ numbers: Double...) -> Double {
      var total: Double = 0
      for number in numbers {
          total += number
      }
      return total / Double(numbers.count)
  }
  arithmeticMean(1, 2, 3, 4, 5)
  // returns 3.0, which is the arithmetic mean of these five numbers
  arithmeticMean(3, 8.25, 18.75)
  // returns 10.0, which is the arithmetic mean of these three numbers
  ```

* 使用 variadic parameters 有什么注意事项？

  - 一个函数至多只能有一个

  - 不能标记为 `inout`，原因我猜是编译器无法判断传进函数的参数们所构成的数组的长度在函数实现中没有被修改，只能产生运行时错误。

* In-out parameters 调用时有什么要求？

  - 接受的实参必须是变量，不能是常量或者字面值。

  - 调用时，实参前面加 ampersand（&）。

* In-out parameters 不能是哪类参数？

  - 有默认值的
  - variadic parameters

* In-out 参数的本质？

  对调用者所在 scope 产生影响。包含 in-out 参数的函数是有返回值函数的一种替代，都是对调用者所在 scope 产生某种程度上的影响。

  > In-out parameters are not the same as returning a value from a function. The swapTwoInts example above does not define a return type or return a value, but it still modifies the values of someInt and anotherInt. In-out parameters are an alternative way for a function to have an effect outside of the scope of its function body.

## Function Types !!!!

* 把函数作为参数传递给另一个函数本质上是？

  把函数自身实现中的某些方面交给函数调用者提供。

  > You can use a function type such as (Int, Int) -> Int as a parameter type for another function. This enables you to leave some aspects of a function’s implementation for the function’s caller to provide when the function is called.

## Nested Functions !!!

* Nested functions 的两个用途？

  - 向 enclosing function 的外部隐藏 nested functions

  - 返回一个 nested function 给函数的调用者使用

# [Closures](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html)

见 [Swift 的 closures（闭包）总结]({% post_url 2017-09-20-Swift 的 closures（闭包）总结 %})

## Closure Expressions !!!!
## Trailing Closures !!
## Capturing Values !!!!
## Closures are Reference Types !!!!
## Nonescaping Closures !!!!
## Autoclosures !!!!

# [Enumerations](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html)

## Enumeration Syntax !!!!
## Matching Enumeration Values with a Switch Statement !!!!

## Associated Values !!!!

关联值

* 通过 switch 语句提取某个 enum 变量的关联值，需要在匹配关联值的变量前面加上 let 或者 var 用以标识这个变量。若某个 case 有多个 associated values，如果这些关联值都希望以常量或者都希望以变量匹配，可以直接在 enum 的这个 case 值前面加 let 或 var。

```swift
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
```

```swift
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
```

## Raw Values !!!!

* Raw values 可以是什么类型？

  > Raw values can be strings, characters, or any of the integer or floating-point number types.

* 一个 enum 中用到的 raw value 们有什么要求？

  > Each raw value must be unique within its enumeration declaration.

* 如何指定一个 enum 的 raw values 的类型？

  ```swift
  enum ASCIIControlCharacter: Character {
    ...
  ```

* Raw values 和 associated values 有什么不同？

  - Raw values 像是对 enum 中一个 case 的不同形式的表示，在定义这个 enum 的时候， raw values 和 case 的关系就明确了。

  - Associated values 像是对一个 case 的补充描述，虽然是补充，但是它的存在与否和类型在定义 enum 时就明确了。

  - 这些在定义 enum 类型时明确的部分是无法修改的，包括 raw values 和 cases 的关系，每个 case 是否具有 associated value、具有什么类型的 associated value。

  - 对于某个 enum 的给定的值，它的 case、raw value 和 associated value（如果有的话）的值都已经给定了。当然，其中 case 和 raw value 遵循 enum 定义中的一对一的关系。

  |              | Case         | Raw value                                                   | Associated values                                               |
  |--------------|--------------|-------------------------------------------------------------|-----------------------------------------------------------------|
  | Enum 定义    | 可能的 cases | 可能的 raw values，分别和可能的 cases 的对应关系。          | 定义每个 case 是否有 associated value，如果有的话，是什么类型。 |
  | 该 enum 的值 | 某一个 case  | 某一个 raw values，与当前的 case 符合类型中定义的对应关系。 | 没有，或者某个符合定义中要求的符合当前 case 的类型的值。        |

* 如何指定指定 enum 中 cases 的 raw values？

  - case 后面加 = raw value

  - 对于 raw values 是整型或字符串类型的 enum，Swift 给 cases 自动分配 raw values。

    - 对于整型，每个 case 的隐式值都比前一个大 1，如果第一个 case 没有指定 raw value，则是 0.

    - 对于字符串，每个 case 的 raw value 就是这个 case 的名字。

* 怎样使用 raw value？

  - 通过 `rawValue` 属性获取一个 enum 值的 raw value。

  - 定义了 raw value 的 enum 会自动获得一个 failable 构造器，接收 raw value，返回 optional enum。

## Recursive Enumerations !!!!

# [Classes and Structures](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html)

通常类（class）的实例（instance）被称作对象（object），结构体（struct）的实例我也不知道被称作啥，反正不是对象。而 Swift 中类和结构体在功能上非常接近，所以，在 Swift 中，很多时候会说“实例”（instance），用来表示它是类或结构体的实例。

## Comparing Classes and Structures !!!!

> Classes have additional capabilities that structures do not:
>
> * Inheritance enables one class to inherit the characteristics of another.
>
> * Type casting enables you to check and interpret the type of a class instance at runtime.
>
> * Deinitializers enable an instance of a class to free up any resources it has assigned.
>
> * Reference counting allows more than one reference to a class instance.

对这四种情况应该有更深入的理解 ????

* 和类不同，结构体有自动生成的 memberwise 构造器。

  ```swift
  struct Resolution {
      var width = 0
      var height = 0
  }
  let vga = Resolution(width: 640, height: 480)
  ```

* 和类不同，一个结构体中的方法如果会改变结构体中的值，该方法需要标记为 mutating。

  因为结构体是 copy on write，结构体是 value type，传值不传引用，每次传值时实际上不会立刻 copy，只有在值发生改变（write）时才会 copy，那么 Swift 怎么知道什么时候发生了改变，在调用 mutating 方法时。

## Structures and Enumerations are Value Types !!!!

* 什么是 value type？

  > A value type is a type whose value is copied when it is assigned to a variable or constant, or when it is passed to a function.

* 有哪些常见的 value type？

  > all of the basic types in Swift—integers, floating-point numbers, Booleans, strings, arrays and dictionaries—are value types, and are implemented as structures behind the scenes.

## Classes are Reference Types !!!!

* 什么是 reference type？

  > Unlike value types, reference types are not copied when they are assigned to a variable or constant, or when they are passed to a function. Rather than a copy, a reference to the same existing instance is used instead.

* 什么是 identity operators？

  - Identical to (===)
  - Not identical to (!==)

* Identity operators 和 “equal to” 有什么关系？

  - “Identical to”意味着两个变量或常量指得完全是同一个类的实例。

  - “Equal to”字面意思是两个值相等，但具体含义完全由类的设计者决定。

  - 二者其实没什么关系，如果愿意，可以让“equal to”在任何时候都为 false，即使是同一个实例自己和自己比。

## 关于 value types 和 reference types

* 从语言语法上说，value types 和 reference types 可以用来描述什么？即，什么可以是 value types 或 reference types？

  - 可以说 classes、structures、enumerations 是 value types 或者 reference types。

  - 可以说一个类型是 value types 或者 reference types，比如整形。

  - 到目前为止，还没有在官方文档上看到说某个值或者变量是 value types 或者 reference types。

* 目前来看，主要是从哪几个方面来讨论 value types 和 reference types 的？

  - 能否修改常量的属性

  - 赋值给另一个变量（或常量），再通过这两个变量（或常量）中的任意一个修改其中的属性，看是否影响另一个变量（或常量）中的该属性。

## Choosing Between Classes and Structures !!!!

如何选择？

除了选 structures 的这几个条件，其它情况都用 class。

> In practice, this means that most custom data constructs should be classes, not structures.

那什么情况下选 structures？

* value types or reference types

* 是否需要继承

* 是否只是简单数据值的封装

* 其属性是否也是 value types，如果属性是 reference types，自身是 structures 的话，那么自身复制值，属性还是指同一个实例，可能会不符合语义。

## Assignment and Copy Behavior for Strings, Arrays, and Dictionaries !!!!

- 都是 value types

- 拷贝有优化

  > Swift only performs an actual copy behind the scenes when it is absolutely necessary to do so

# [Properties](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html)

## Stored Properties !!!!

类和结构体的一点不同，类的一个**不可变**的实例的 variable stored property 可以修改，但结构体的一个**不可变**的实例的 variable stored property 不可以修改，尽管它是 variable stored property。原因是类是 reference type，结构体是 value type。

* Lazy stored properties 可变不？

  可变，而且必须声明为 var，文档给出的解释如下，

  > You must always declare a lazy property as a variable (with the var keyword), because its initial value might not be retrieved until after instance initialization completes. Constant properties must always have a value before initialization completes, and therefore cannot be declared as lazy.

  但我觉得说不通，lazy properties 的初始值可能直到实例初始化完成都不被获得，constant properties 在初始化完成之前必须有值，所以 constant properties 不能是 lazy，但 variable stored properties 不也是在初始化完成之前必须有值吗？

  找到了另一个解释，

  > > The way to think about it is that "let" in Swift's current implementation is about physical immutability, not about logical immutability. Physical immutability means that ones the bits are set in memory (i.e. that the let value is initialized) they can never be changed. -- [Why "lazy var" is not "lazy let" in Swift?](https://swanros.com/2015/04/17/-why-lazy-var-is-not-lazy-let-in-swift/)

  可变不可变说的是物理不可变，对于 lazy properties 来说，“whose initial value is not calculated until the first time it is used”，也就是说，在被使用之前，它的物理位置存放的是未被求值的信息，在使用时，它被求值，物理位置变成了求出的值，因为这个变化，所以不可以是 constant stored properties。

* Lazy stored properties 声明时等号后面可以是，

  - 一个实例的初始化
  - 一个方法或函数的调用
  - 一个 closure 的定义后跟“()”表示定义的同时被调用

  以上三个情况中，实例的初始化方法，方法或函数的调用，closures，本质上都可以算作 closures，（见 [Swift 的 closures（闭包）总结]({% post_url 2017-09-20-Swift 的 closures（闭包）总结 %}) 中对方法、函数和 closures 关系的描述），所以以 lazy stored properties 本质上是对 closures 的推迟执行，对计算的推迟执行。

* Lazy stored properties 的用途？

  - 推迟开销大的操作，直到需要时再执行，尤其当这个 stored property 可能一直不会被用到，将其标记为 lazy 可以减少不必要的开销。

  - “get around some initialization dependency conundrums”。初始化时，可能某个 stored property 获取其初始化值时由于一些外部因素只能在所在实例初始化之后才能获得，这样就会形成一个困境，要想初始化这个 stored property 需要先完成其所在实例的初始化，而要完成其所在实例的初始化，就必须获取其所有 stored properties 的初始值，包括这个 stored property 在内，而这个 stored property 的初始化需要这个实例先初始化。

* Lazy stored properties 在多个线程同时访问时会发生什么？

  如果多个线程同时访问，且其初始化（求值）没有完成，它将可能会被初始化多次。

* 对一个没有访问过的 lazy stored property 直接赋值，是否会先执行它的初始化求值？

  不会。详见 [为什么 lazy stored properties 不能有 property observers]({% post_url 2017-10-15-为什么 lazy stored properties 不能有 property observers %})。

## Computed Properties !!!!

## Property Observers !!!!

* 给 value types 类型的属性加 observers 有什么特别注意的？

  "Will also be invoked if you mutate a struct (e.g. add something to a Dictionary)" -- cs193p 2016

  标记为 mutating 的方法即便什么都不做，也会使 observers 被调用。

* 为什么不能给 lazy stored properties 添加 property observers？

  见 [为什么 lazy stored properties 不能有 property observers]({% post_url 2017-10-15-为什么 lazy stored properties 不能有 property observers %})。

* 如何给继承的属性加 observers？

  通过 overriding property observers。说是覆盖属性，其实是在当前类中为这个属性加了一层 observers，父类中该属性的 observer 还是会被调用。

  ```swift
  class C {
      var i = 42 {
          willSet {
              print("willSet i in D")
          }
          didSet {
              print("didSet i in D")
          }
      }
  }

  class D: C {
      override var i: Int {
          willSet {
              print("willSet i in C")
          }
          didSet {
              print("didSet i in C")
          }
      }
  }

  let d = D()
  d.i = 3

  // willSet i in C
  // willSet i in D
  // didSet i in D
  // didSet i in C
  ```

* 如何给 computed properties 加 observers？

  不需要且不能，应该在 computed properties 的 setter 中观察和响应属性值的变化。

* 可否在 didSet 中为属性设置新值？

  可以。但是需要注意，

  * 如果**不是**覆盖父类属性添加 observers，可以在 didSet 中设置新值，Swift 足够聪明，知道这是在 didSet 中设置的，不会再次调用 didSet，也不会调用 willSet。

  * 但是，如果**是**覆盖父类属性添加 observers，这时要小心，didSet 中的设置新值的操作会导致 didSet 被调用，从而无限调用下去。这时应该通过 `super` 引用父类中的该属性进行设置，因为当对这个属性使用了 overriding property observers 之后，就不能再对其进行 overriding setters 了，所以这里引用父类的该属性应该不会造成含义上的改变，但需要考虑当前类的 observers 将不再被调用。

## Global and Local Variables !!!

* > The capabilities described above for computing and observing properties are also available to global variables and local variables.

* 二者的定义？

  - > Global variables are variables that are defined outside of any function, method, closure, or type context.
  - > Local variables are variables that are defined within a function, method, or closure context.

* 二者和惰性求值的关系？

  * 全局变量总是惰性求值，效果和 lazy stored properties 相似，但不用加 `lazy`。

## Type Properties !!!!

* 是否惰性求值？

  是。

  文档中只说 stored type properties 是惰性求值。

  > Stored type properties are lazily initialized on their first access.

  但 computed type properties 肯定也是。

* 是否标记 `lazy`？

  都不需要标记为 `lazy`，也就是说，除了 lazy stored instance properties，其它包括全局属性、类型属性，都不需要 `lazy` 关键字。

* Stored type properties 的惰性求值和 stored instance properties、global properties 有什么不同？

  即便在多线程同时访问时，也只初始化一次。> They are guaranteed to be initialized only once, even when accessed by multiple threads simultaneously

  Stored instance properties 文档有说不保证多线程访问时只初始化一次。

  Global properties 文档中没有提。

* static 和 class 关键字的区别？

  static 的类属性不能被覆盖。

  class 只能用在 computed type properties 上，可以被覆盖。


# [Methods](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Methods.html)

> methods are just functions that are associated with a type.

* 怎样在 value types 的 mutating 实例方法中修改自己？

  - 通过隐式 self 修改成员变量

    ```swift
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
    ```

  - 直接将一个新实例赋值给隐式的 self

  > Mutating methods can assign an entirely new instance to the implicit self property.

  ```swift
  mutating func moveBy(x deltaX: Double, y deltaY: Double) {
      self = Point(x: x + deltaX, y: y + deltaY)
  }
  ```

  我猜 Swift 具体是这样实现的，见 [Value types 是如何在 mutating 实例方法中修改自己的？]({% post_url 2017-10-16-Value types 是如何在 mutating 实例方法中修改自己的？ %})

* static vs class？

  这对关键字再次出现，之前是用在 type properties 上，class 的允许被覆盖，但自己只能是 computed properties，不可以是 stored properties。static 不允许覆盖，但是可以是任意 properties。

  对于 type methods，二者的区别还是能否被覆盖，static 不可以，class 可以，除此之外，都是用来表示该方法是和类型关联的，而非实例。

* Type methods 中的 self 关键字有什么作用？

  只自己的类型，而不是这个类型的实例。

  > This means that you can use self to disambiguate between type properties and type method parameters, just as you do for instance properties and instance method parameters.

* Type methods 中如果不加限定地（unqualified，指前面不写 self 或者类型）引用其它类型属性或方法，指的就是当前这个类的属性或方法。

# [Subscripts](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Subscripts.html)

* 语法

  - 是否有 func 关键字？

    没有

  - 如何定义读写？

    定义 get 和 set。

  - 是否有 argument labels？

    没有

  ```swift
  subscript(index: Int) -> Int {
      get {
          // return an appropriate subscript value here
      }
      set(newValue) {
          // perform a suitable setting action here
      }
  }
  ```

* 可 overload

* 可接受多个参数

  ```swift
  struct Matrix {
    ...
    subscript(row: Int, column: Int) -> Double {
      ...
    }
  }

  matrix[0, 1] = 1.5
  ```

# [Inheritance](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html)

## Defining a Base Class !!
## Subclassing !!

## Overriding !!!!

* 什么是 overriding？

  通常情况下，子类会继承父类的方法，如果其中某个方法对子类并不适合，子类中可能想为其提供一个自定义的自己的实现，这里的“提供自己的实现”就是 overriding。

  > A subclass can provide its own custom implementation of an instance method, type method, instance property, type property, or subscript that it would otherwise inherit from a superclass. This is known as overriding.

  覆盖就是在子类中为父类中的某个方法提供自己的实现，否则的话，将会继承这个方法。这里的后半句也很重要，“否则就会继承”，也就是说，没有继承下来的方法是谈不上覆盖的。

### Overriding Properties

有两种，

  - Overriding property getters and setters

    > to provide your own custom getter and setter for that property

  - Overriding property observers

    > to add property observers to enable the overriding property to observe when the underlying property value changes.

* 被覆盖的属性是 stored properties 或 computed properties 对当前类中的该属性有什么影响？覆盖时有什么需要注意的？

  没有影响，因为子类根本就不知道父类的这个属性是 stored 还是 computed，子类知道的只有其名字和类型。

  我猜 stored properties 和 computed properties 本质上都是 computed properties，见 [Swift 中关于属性的猜测]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})。

* Overriding property getters and setters 是否必须都提供？和被覆盖的属性的只读与否是否有关系？

  可以都不提供，或者只提供 getters，或者同时提供 getters 和 setters，
  不能只提供 setters 不提供 getters。

  如果被覆盖的属性如果是只读，可以只提供 getters 或者同时提供 getters 和 setters。
  被覆盖的属性如果是可读可写，则必须同时提供 getters 和 setters，即，不可以把一个 read-write 属性覆盖成 read-only 属性。

  我猜属性的覆盖本质上就是方法的覆盖，是 getters 和 setters 的覆盖，所以如果把有 getters 和 setters 的属性覆盖成了只有 getters 的属性，那相当于隐藏了父类中的方法，而隐藏父类方法当然是不被允许的。见 [Swift 中关于属性的猜测]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})。

* Overriding property getters and setters 中能否访问和/或修改父类中的该属性？

  可以，通过 super 关键字，注意，修改也是可以的。

* 为什么不能为 overriding properties 同时提供 setters 和 observers？

  我猜原因和不能为 computed properties 提供 observers 是一样的。而为什么 computed properties 不能提供 observers，见 [Swift 中关于属性的猜测]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})。

* 考虑到 overriding property observers 本身是用来获取该属性改变的通知的，所以它不能用在哪种属性上？

  常量属性（constant stored properties）或者只读计算属性（read-only computed properties）上，其实子类只能看出它们是只读属性，看不出是 stored 还是 computed。因为 observers 是用来观察变化的，只读属性没得变，当然也就不能添加 observers 了。

  但可以为 overriding 只读属性提供 getters 和 setters 从而使其变成 read-write 属性。

  细说地话，我猜，read-only computed properties 不能有 observers 还因为它的 setters 不是 Swift 提供的，所以在属性改变时不会自动调用 observers。见 [Swift 中关于属性的猜测]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})。

* Overriding property observers 的定义中，为什么用的是“add” property observers 而非“override”？

  因为为 overriding properties 提供 observers 时，并不能覆盖父类中该属性的 observers，当当前类中的该属性被修改时，父类和子类中的 observers 都会被调用。

  * 那既然如此，overriding property observers 是不是不应该被算作 overriding？

  我猜，仍然应该被算作 overriding。因为为 overriding properties 添加 observers 时，Swift 会在背地里覆盖该属性从而使其在被修改时能够调用当前类中的 observers。见 [Swift 中关于属性的猜测]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})。

## Preventing Overrides !!

* 可以用在什么上面？

  > final var, final func, final class func, and final subscript

  也包括 extensions 里的。

  还可以用在整个 class 上使其不能被继承。


# [Initialization](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html)

## Setting Initial Values for Stored Properties !!!

* 为 stored properties 设置初始值（通过 default property values 或者 在 initializer 中设置）时，是否会调用该属性的 observers？

  不会。Swift 可能为此做了特殊处理，见 [Swift 中关于属性的猜测]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})。

## Customizing Initialization !!!!

* Swift 为构造器专门设置了一个关于函数参数的规则，是什么？

  因为构造器不同于一般函数，构造器都叫 init，没有一个有标识作用的函数名，所以构造器中的参数名和类型对于区分哪个构造器应该被调用就非常重要了，为此，Swift 为每一个没有提供参数标签的参数自动添加参数标签。原文是这样说的，

  > However, initializers do not have an identifying function name before their parentheses in the way that functions and methods do. Therefore, the names and types of an initializer’s parameters play a particularly important role in identifying which initializer should be called. Because of this, Swift provides an automatic argument label for every parameter in an initializer if you don’t provide one.

## Default Initializers !!!!

Swift 在不同的情况下为类和结构体提供默认构造器、为结构体提供 memberwise 构造器。

Swift 会尽力提供两种构造器，只要可能。

比如，尽管有的可变属性已经在定义时指定了初始值，Swift 仍然会生成包含该属性参数的 memberwise 构造器。

但是，如果一个常量属性在定义时指定了初始值，Swift 在生成 memberwise 构造器时就不会包含该属性的参数，因为这样会让这个常量被两次赋值。

```swift
struct S {
    var i: Int?
    let c: Int = 1
}

_ = S()
_ = S(i: 1)
// _ = S(i: 1, c: 1) // Cannot invoke initializer for type 'S' with an argument list of type '(i: Int, c: Int)'
```

## Initializer Delegation for Value Types !!!

* Value type 中的构造器内可以通过 `self.init` 调用（或称代理 delegate）自己的另一个构造器。

* 如果手动定义了构造器，就没有 default 构造器和/或 memberwise 构造器了。

* 怎样既保留 default 构造器和 memberwise 构造器，同时又自己定义构造器？

  可以在 extension 中定义构造器。

## Class Inheritance and Initialization

* Designated initializers 和 convenience initializers 是 class 才有的。Swift 为什么要定义这两种构造器？

  > to help ensure all stored properties receive an initial value

* 构造器之间的相互调用（delegation call）规则？

  - Designated 构造器必须调用直接父类的 designated 构造器
  - Convenience 构造器必须调用当前类的其它构造器
  - Convenience 构造器最终必须调用当前类的 designated 构造器

* 两步构造规则的四条 safety checks？

  前两条是说 designated 构造器的

  - Designated 构造器在调用父类 designated 构造器前，必须先初始化当前类引入的所有 properties。

  - Designated 构造器在给**继承属性**赋值（阶段二）之前，必须先调用直接父类的 designated 构造器。能够防止继承的父类属性被父类构造器重写。

  总的来说，对于 designated 构造器，

  初始化当前类中的所有属性 -> 调用直接父类的 designated 构造器 -> 阶段二（修改继承自父类的属性）

  第三条是说 convenience 构造器的

  - Convenience 构造器在给**属性（包括当前类的属性和继承的属性）**赋值之前，必须先调用其它构造器。能够防止属性被其他构造器重写。

  第四条就是说阶段一完成后才能阶段二。

* 两步构造的最终效果？都什么被执行了？

  * 阶段一

    - 任一构造器被调用

    - 分配内存

    - 不管被调用的是 designated 还是 convenience 构造器，最终都会调用当前类的 designated 构造器，在该构造器的实现中，当前类引入的所有 stored 属性会被初始化。

    - 这个构造器紧接着调用其直接父类的 designated 构造器，其中会先初始化父类引入的所有属性，接着再调用父类的父类的 designated 构造器，在其中初始化父类的父类引入的属性并接着往上调用。

    - 直到到达整个继承链的顶端。

    - 此时所有的 stored 属性都有值了，内存被完全初始化，阶段一完成。

  * 阶段二

    - 这时这个初始化的实例其实已经可以用了，可以通过 self 关键字访问其中的属性、调用其中的方法。

    - 阶段一截止到继承链顶端的类的 designated 构造器初始化了其所有属性，这个构造器中可能还有其他的设置，这时可以通过 self 做进一步的设置。

    - 之前调用上层类 designated 构造器的 designated 构造器在上层 designated 构造器返回后接着执行当前的其他自定义操作。

    - 直到最底层的类（也就是整个两步构造要构造实例的类），该类的 designated 构造器结束后，如果它是由其它 convenience 构造器调用的，那其它 convenience 构造器在调用完这个 designated 构造器后会接着执行自己的其它自定义操作。

### Initializer Inheritance and Overriding

* 默认不继承构造器的原因？什么情况下会继承？

  父类的构造器可以确保自己的属性都被正确初始化，当父类的构造器被子类继承后，子类也可以调用这个构造器来初始化子类，但这个构造器只能初始化父类中的属性，子类自身的属性就不能被初始化了。

  因为上述原因，只要子类没有加新属性或者新属性都有初始值，那么 Swift 就应该允许继承构造器，如果子类中没有写 designated 构造器，父类的 designated 构造器就都能被继承下来，这被称为 automatic initializer inheritance。

  可以认为 convenience 构造器只是实现它的类单独所有的，只有在子类具有父类所有 designated 构造器的情况下才会获得 bonus，具有所有父类的 convenience 构造器，否则不会有的。这里所说的“子类具有父类所有 designated 构造器的情况”可以是子类自身覆盖了父类所有的 designated 构造器，也可以是通过 automatic initializer inheritance 获得的。

  * Automatic initializer inheritance 的准确条件？

    假设子类中的新属性都有默认值了，此时，

    - 如果子类没有定义任何一个 designated 构造器，则自动继承父类的 designated 构造器。否则，只要定义了一个，就不能继承。

    - 不管是自动继承还是手动覆盖了父类的所有 designated 构造器，自动继承父类所有的 convenience 构造器。

    注意：规则一中说的是**子类**没有定义任何一个 designated 构造器，规则二中说的是覆盖了**父类**的所有 designated 构造器，所以可以通过把一个父类的 designated 构造器覆盖成 convenience 构造器，从而不违背规则一，从而继承所有构造器。如下，

  * 想覆盖父类的某个 designated 构造器，又想自动继承父类的其它 designated 构造器和 convenience 构造器，有什么办法吗？

    把那个 designated 构造器覆盖成 convenience 构造器，如果逻辑允许的话。

    ```swift
    class C {
        init() {}
        init(i: Int) {}
        convenience init(b: Bool) { self.init() }
    }

    class D: C {
        convenience override init() { self.init(i: 0) }
    }

    D()
    D(i: 0)
    D(b: true)
    ```

* 为什么在子类中写与父类中的 designated 构造器同名的方法就算 overriding（需要加 override 关键字），但如果写与父类中的 convenience 构造器同名的方法就不算 overriding（不能加 override 关键字）？

  文档中的原因如下，

  > When you write a subclass initializer that matches a superclass designated initializer, you are effectively providing an override of that designated initializer. Therefore, you must write the override modifier before the subclass’s initializer definition.

  > Conversely, if you write a subclass initializer that matches a superclass convenience initializer, that superclass convenience initializer can never be called directly by your subclass, as per the rules described above in Initializer Delegation for Class Types. Therefore, your subclass is not (strictly speaking) providing an override of the superclass initializer. As a result, you do not write the override modifier when providing a matching implementation of a superclass convenience initializer.

  第一段中并没有解释，第二段中说 “that superclass convenience initializer can never be called directly by your subclass”，父类中的 convenience 构造器从来不可能在子类中被直接调用，放到覆盖的定义来说就是，父类的这个方法子类中不可能调用，不可能调用就相当于子类中没有继承到该方法，所以这不能算是覆盖。就是说，要想覆盖，首先得有，压根就没有从父类那拿到这个方法，谈什么覆盖。

  我的理解，把 convenience 构造器理解成当前类自己独有的就好了，不存在被继承和被覆盖，只有在特殊情况下才会被继承。

* 在子类中写与父类中构造器同名的构造器，对父类和子类的构造器的种类（designated 或 convenience）有没有什么要求？

  父类的 designated 构造器，子类可以写同名的，可以是 designated 或 convenience，但都叫覆盖。

  父类的 convenience 构造器，子类也可以写同名的，可以是 designated 或 convenience，但都不叫覆盖，因为按照我的理解，convenience 构造器是当前类独有的，不存在被继承和被覆盖。虽说子类的同名构造器可以是 designated 或 convenience，但是这个同名构造器的实现需要遵循 designated 或 convenience 构造器的实现规则，前者得调用父类的 designated 构造器，后者得调用当前类的 designated 构造器。

## Failable Initializers

* Swift 会为哪种类型自动生成 failable initializers？

  有 raw values 的 enum，`init?(rawValue:) `。

* 构造器之间的相互调用如何影响一个构造器是否是 failable 的？

  Delegate（调用）其它 failable 构造器的构造器得是 failable。一个 failable 构造器也可以 delegate 不是 failable 的构造器，如果你想给那个构造器增加失败的状态。

* 一个构造器调用另一个 failable 构造器如果失败会怎样？

  当前构造器也立即失败，之后的初始化代码将不被执行。

* 覆盖时有什么需要注意的？

  > You can override a failable initializer with a nonfailable initializer but not the other way around.

## Required Initializers

- 为什么 required designated 构造器不写 override 关键字了？

  因为 required 的意思就是子类必须要覆盖，本身就包含了覆盖的意思，所以也就无所谓写不写 override 了。

- 子类中可以不覆盖父类中的 required 构造器吗？

  如果子类中能够继承这个构造器（automatic initializer inheritance），就不用写了。当然，要符合子类能够继承父类 designated 构造器的条件，当前类新引入的属性都有默认值，且当前类没有写 designated 构造器。

## Setting a Default Property Value with a Closure or Function !!!!

* 有什么注意的？

  这里的 closure 里面不能引用 self，因为此时还没有完成该实例的初始化。


# [Optional Chaining](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html)

* > To reflect the fact that optional chaining can be called on a nil value, the result of an optional chaining call is always an optional value, even if the property, method, or subscript you are querying returns a nonoptional value.

  ```swift
  class A {
    let b: B
  }

  a : A?
  a?.b : B?
  ```

  ```swift
  class A {
    let b: B?
  }

  a : A?
  a?.b : B?
  ```

  无论 A 中的 b 是否是可选值，a?.b 都是 B? 类型。

* Optional chaining 可以用来求值，也可以用来赋值（做左值）。

  ```swift
  func createAddress() -> Address {
      print("Function was called.")

      let someAddress = Address()
      someAddress.buildingNumber = "29"
      someAddress.street = "Acacia Road"

      return someAddress
  }
  john.residence?.address = createAddress()
  ```

  > The assignment is part of the optional chaining, which means none of the code on the right-hand side of the = operator is evaluated.

  注意，当左边的 optional chaining 为 nil 时，等号右边都不会被求值。

  同样适用于方法调用，

  ```swift
  class C {
      func m(_ i: Int) {}
  }

  let c: C? = nil

  func f() -> Int {
      print("f called")
      return 0
  }

  c?.m(f()) // f() 不求值

  func g(_ i: Int) {
      return
  }

  g(f()) // f called
  ```

  也可以看成是一种 short-circuit。

* 通过 optional chaining 调用方法（Calling Methods Through Optional Chaining），对于没有返回值的函数，如何判断函数是否被调用？

  这时整个 optional chaining 返回的是 Void? 类型，检测其值如果是 nil 则说明函数调用失败，如果是 `()`（empty tuple），则说明调用成功。

* 通过 optional chaining 给属性赋值，如何判断是否赋值成功？

  和上面检测函数调用的方法一样。正如我猜的，[属性本质上就是函数]({% post_url 2017-10-17-Swift 中关于属性的猜测 %})。

* 通过 optional chaining 访问下标，需要注意什么？

  问号总是跟在可选的部分之后，对于这里的情况就是在下标之前。

  > When you access a subscript on an optional value through optional chaining, you place the question mark before the subscript’s brackets, not after. The optional chaining question mark always follows immediately after the part of the expression that is optional.

* 多层 optional chaining 不会给返回值添加多层可选性（不会多加很多问号）。

  > multiple levels of optional chaining do not add more levels of optionality to the returned value.

# [Type Casting](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TypeCasting.html) !!!!

* 注意定义，称作类型转换好像也不大妥当，

  > Type casting is a way to check the type of an instance, or to treat that instance as a different superclass or subclass from somewhere else in its own class hierarchy.

  是一种方式，用于检查实例类型，或者把实例当作其某个子类或者父类来对待。

  > Casting does not actually modify the instance or change its values. The underlying instance remains the same; it is simply treated and accessed as an instance of the type to which it has been cast.

* 如 [Class and Structures](#classes-and-structures) 中所提到的 object 和 instance 的区别，类的 instance 叫做 object，结构体的 instance 不是 object。所以 struct 的实例不能是 AnyObject。

  ```swift
  struct S {

  }
  let s: AnyObject = S()
  // error: value of type 'S' does not conform to specified type 'AnyObject'
  ```

* 如果一个数组是 [Any] 类型，如果其中有数字 0，在 switch 语句中，不能直接拿 0 去匹配，编译错误如下，

  ```swift
  switch 0 as Any {
  case 0: // error: expression pattern of type 'Int' cannot match values of type 'Any'
      print("zero")
  default:
      break
  }
  ```

  错误提示是 Int 类型的表达式模式不能匹配 Any 类型的值，表达式模式的类型是什么意思???? 等我读了 [Patterns](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Patterns.html) 再回来补充。

  可以用 `0 as Int` 或者 `0 as Double`，去匹配。

  如果只匹配类型不匹配值，可以用 `is` 去匹配，如，`case is Double:`。

* Any 可以表示 optional，但是会有警告，所以需要显示的 `as Any`

  ```swift
  let optionalNumber: Int? = 3
  things.append(optionalNumber)        // Warning
  things.append(optionalNumber as Any) // No warning
  ```

* Any 也可以是 optional（`Any?`）。


# [Memory Safety](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/MemorySafety.html)

Swift 提供的内存访问冲突检测机制，只用在单线程中，单线程中出现的冲突 Swift 能够保证产生编译错误或者运行时错误。

文中并没有提到运行时错误，可能大部分错误都能在编译时产生。

内存访问的几个特征：

- 读或写

- 访问的持续时间

- 内存位置

针对这三个特征，两个产生冲突的内存访问必须满足下面的所有条件：

- 只是有一个写

- 访问持续时间有重合

- 访问内存同一位置

struct 在一些特殊情况下会被认为是安全的，但对用户来说不重要。






# [Advanced Operators](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html)

## Overflow Operators !!!

---

API Design Guidelines 2017-09-30

# Argument Labels

* 如果几个参数没区别，忽略标签。

  > **Omit all labels when arguments can’t be usefully distinguished**, e.g. `min(number1, number2)`, `zip(sequence1, sequence2)`.

* 构造方法如果是执行无损转换，忽略第一个标签。

  > **In initializers that perform value preserving type conversions, omit the first argument label**, e.g. `Int64(someUInt32)`

  - 之后的参数标签还得留

  - 如果是有损转换，建议通过标签强调这是有损的。

    ```swift
    extension UInt32 {
      /// Creates an instance having the specified `value`.
      init(_ value: Int16)            ← Widening, so no label
      /// Creates an instance having the lowest 32 bits of `source`.
      init(truncating source: UInt64)
      /// Creates an instance having the nearest representable
      /// approximation of `valueToApproximate`.
      init(saturating valueToApproximate: UInt64)
    }
    ```

  - 注意：前文还提到，

    > Initializer and factory method calls should form a phrase that does not include the first argument, e.g. `x.makeWidget(cogCount: 47)`.

    不管是无损或有损转换，构造器所构成的短语不包含第一个参数。

* 在方法所构成的短语中，第一个参数构成了介词短语的一部分，这时需要给第一个参数标签。通常这个标签也应该包含这个介词。

  > **When the first argument forms part of a [prepositional phrase](https://en.wikipedia.org/wiki/Adpositional_phrase#Prepositional_phrases), give it an argument label**. The argument label should normally begin at the [preposition](https://en.wikipedia.org/wiki/Preposition), e.g. `x.removeBoxes(havingLength: 12)`.

  我并不理解这个例子中的 length 构成了介词短语的一部分。

  有一个例外情况下，介词不放在标签中，而是应该放在前面的函数主体，这个情况是前两个或者前几个参数表示差不多的东西。

  An exception arises when the first two arguments represent parts of a single abstraction.

      a.move(**toX:** b, **y:** c) a.fade(**fromRed:** b, **green:** c, **blue:** d)

  In such cases, begin the argument label _after_ the preposition, to keep the abstraction clear.

      a.moveTo(**x:** b, **y:** c) a.fadeFrom(**red:** b, **green:** c, **blue:** d)

* 此外，如果第一个参数构成了一个普通的短语的一部分（不是介词短语），则标签和函数主体放一起。

  **Otherwise, if the first argument forms part of a grammatical phrase, omit its label**, appending any preceding words to the base name, e.g. `x.addSubview(y)`

  这条规则意味着，如果第一个参数没有构成短语，就应该有标签。

  This guideline implies that if the first argument _doesn’t_ form part of a grammatical phrase, it should have a label.

    ```swift
    view.dismiss(**animated:** false)
    let text = words.split(**maxSplits:** 12)
    let studentsByName = students.sorted(**isOrderedBefore:** Student.namePrecedes)
    ```

  原文这段被我忽略了，

  Note that it’s important that the phrase convey the correct meaning. The following would be grammatical but would express the wrong thing.

    view.dismiss(false) Don't dismiss? Dismiss a Bool?
    words.split(12) Split the number 12?

  我觉得这两个例子中的第一个参数都没有构成短语，本就不该去掉标签，本来就不符合前面提到的标准。而原文却给出了一种错觉，好像这两个例子符合标准，但没有表达正确的意思。

  如果第一个参数有默认值，也就是说在调用函数时这个参数可以忽略，这种情况当然不能构成短语，所以要加标签。

* 其它参数都要加标签。

---

官方文档实在难读，下面我按照自己的思路捋一遍。

首先解决一个文档叙述中出现多次的描述，

  the (first) argument forms (part of) a (prepositional/grammatical) phrase

这段话的前提是，前文提到的，

> Prefer method and function names that make use sites form grammatical English phrases.

就是让方法或函数在被调用时可以形成一个英语语法短语。

这个多次出现的描述中所说的短语就是这个意思，这个短语是否包含某个参数**取决于你如何定义这个函数**。

下面正式整理这段规则。总的来说，这段的目的就是解决什么时候给函数（方法）加标签（argument label）。

* 如果有多个参数且多个参数没什么区别，就都省略标签。e.g. `min(number1, number2)`, `zip(sequence1, sequence2)`.

* 除了第一条规则，所有不是第一个的参数都得加标签。

* 如果是构造函数（包括工厂方法），调用时形成的短语不应该包含第一个参数。

  * 基本上就是构造函数（或工厂方法）的第一个参数不加标签。

  * 但有个例外，如果是丢失精度（narrowing 或称非单射）的类型转换，则通过标签描述。

    ```swift
    extension UInt32 {
      /// Creates an instance having the specified `value`.
      init(_ value: Int16)            ← Widening, so no label
      /// Creates an instance having the lowest 32 bits of `source`.
      init(truncating source: UInt64)
      /// Creates an instance having the nearest representable
      /// approximation of `valueToApproximate`.
      init(saturating valueToApproximate: UInt64)
    }
    ```

* 如果不是构造函数

  * 如果第一个参数有默认值，则函数命名不应该使其在被调用时形成的短语中包含第一个参数，进而按照下面的“如果在函数调用时形成的短语没有包含第一个参数，则需要加标签”，第一个参数应该有标签。

  * 如果在函数调用时形成的短语包含了第一个参数

    * 如果第一个参数构成了介词短语的一部分，介词和标签放一起。e.g. `x.removeBoxes(havingLength: 12)`.

      * 除非，前两个参数表示的是一类东西（the first two arguments represent parts of a single abstraction）。这种情况，介词前移，和函数主体放一起。

    * 如果第一个参数不是介词短语的一部分，而只是一个符合语法的短语，那么，标签前移到函数主体（这是我的理解，原文 omit its label, appending any preceding words to the base name）。e.g. `x.addSubview(y)`.

  * 如果在函数调用时形成的短语没有包含第一个参数，则需要加标签。






