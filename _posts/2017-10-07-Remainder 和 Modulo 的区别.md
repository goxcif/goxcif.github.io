---
title: "Remainder 和 Modulo 的区别"
---

| dividend | divisor | div | mod | quot | rem |
|----------|---------|-----|-----|------|-----|
| 4        | 3       | 1   | 1   | 1    | 1   |
| -4       | 3       | -2  | 2   | -1   | -1  |
| 4        | -3      | -2  | -2  | -1   | 1   |
| -4       | -3      | 1   | -1  | 1    | -1  |

* div 和 mod 是一对，quot 和 rem 是一对，因为它们符合，

  - (div x y) * y + (mod x y) == x

  - (quot x y) * y + (rem x y) == x

* 对于商来说（这里我指 div 和 quot，不知是否准确，姑且先这么称呼），div 是二者中最接近无穷小的，quot 是最接近 0 的。

* 对于余数来说（这里我指 mod 和 rem），mod 的符号和除数（divisor）一致，rem 的符号和被除数（dividend）一致。

* C 语言和 Swift 语言中的 `%` 都是指 remainder。