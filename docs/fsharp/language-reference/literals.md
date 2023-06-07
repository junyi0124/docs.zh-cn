---
title: 文本
description: '了解 F # 编程语言中的文本类型。'
ms.date: 08/15/2020
ms.openlocfilehash: 15f73db3c36f7c60ab1eeba96c63a28ebc6d7f01
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559149"
---
# <a name="literals"></a>文本

本文提供了一个表，其中显示了如何指定 F # 中的文本类型。

## <a name="literal-types"></a>文本类型

下表显示 F # 中的文本类型。 用十六进制表示法表示数字的字符不区分大小写;标识该类型的字符区分大小写。

|类型|说明|后缀或前缀|示例|
|----|-----------|----------------|--------|
|sbyte|8位有符号整数|y|`86y`<br /><br />`0b00000101y`|
|字节|无符号8位自然数|uy|`86uy`<br /><br />`0b00000101uy`|
|int16|16位带符号整数|s|`86s`|
|uint16|无符号16位自然数|us|`86us`|
|int<br /><br />int32|已签名32位整数|l 或无|`86`<br /><br />`86l`|
|uint<br /><br />uint32|无符号32位自然数|u 或 ul|`86u`<br /><br />`86ul`|
|nativeint|指向有符号自然数的本机指针|n|`123n`|
|unativeint|作为无符号自然数的本机指针|un|`0x00002D3Fun`|
|int64|已签名64位整数|L|`86L`|
|uint64|无符号64位自然数|UL|`86UL`|
|单个，float32|32位浮点数|F 或 f|`4.14F` 或 `4.14f`|
|||换行符|`0x00000000lf`|
|float仔细|64位浮点数|无|`4.14` 或 `2.3E+32` 或 `2.3e+32`|
|||换行符|`0x0000000000000000LF`|
|bigint|不限于64位表示形式的整数|I|`9999999999999999999999999999I`|
|Decimal|表示为固定点或有理数的小数|M 或 m|`0.7833M` 或 `0.7833m`|
|Char|Unicode 字符|无|`'a'` 或 `'\u0061'`|
|String|Unicode 字符串|无|`"text\n"`<br /><br />或<br /><br />`@"c:\filename"`<br /><br />或<br /><br />`"""<book title="Paradise Lost">"""`<br /><br />或<br /><br />`"string1" + "string2"`<br /><br />另请参阅 [字符串](Strings.md)。|
|字节|ASCII 字符|B|`'a'B`|
|byte[]|ASCII 字符串|B|`"text"B`|
|String 或 byte []|原义字符串|@ prefix|`@"\\server\share"` (Unicode) <br /><br />`@"\\server\share"B` (ASCII) |

## <a name="named-literals"></a>命名文本

应为常量的值可以用 [文本](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-literalattribute.html) 属性进行标记。 此属性的作用是使值编译为常量。

在模式匹配表达式中，以小写字符开头的标识符始终被视为要绑定的变量而不是文本，因此在定义文本时通常应使用首字母大写。

```fsharp
[<Literal>]
let SomeJson = """{"numbers":[1,2,3,4,5]}"""

[<Literal>]
let Literal1 = "a" + "b"

[<Literal>]
let FileLocation =   __SOURCE_DIRECTORY__ + "/" + __SOURCE_FILE__

[<Literal>]
let Literal2 = 1 ||| 64

[<Literal>]
let Literal3 = System.IO.FileAccess.Read ||| System.IO.FileAccess.Write
```

## <a name="remarks"></a>备注

Unicode 字符串可以包含可使用指定的显式编码， `\u` 后跟16位十六进制代码 (0000-FFFF) 或32编码，可以通过使用 `\U` 后跟表示任何 Unicode 码位的32位十六进制代码 (00000000-0010ffff 之间) 。

不允许使用以外的其他位运算符 `|||` 。

## <a name="integers-in-other-bases"></a>其他基中的整数

使用 `0x` 、或前缀，还可以使用十六进制、八进制或二进制格式指定已签名的32位整数 `0o` `0b` 。

```fsharp
let numbers = (0x9F, 0o77, 0b1010)
// Result: numbers : int * int * int = (159, 63, 10)
```

## <a name="underscores-in-numeric-literals"></a>数值文本中的下划线

可以用下划线字符 () 分隔数字 `_` 。

```fsharp
let value = 0xDEAD_BEEF

let valueAsBits = 0b1101_1110_1010_1101_1011_1110_1110_1111

let exampleSSN = 123_456_7890
```
