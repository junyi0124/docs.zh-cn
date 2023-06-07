---
title: 纯文本格式
description: '了解如何在 F # 应用程序和脚本中使用 printf 和其他纯文本格式。'
ms.date: 07/22/2020
ms.openlocfilehash: 90a861736dae69dfbc199a19e24f587c42404737
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063778"
---
# <a name="plain-text-formatting"></a>纯文本格式

F # 支持使用 `printf` 、 `printfn` 、 `sprintf` 和相关函数的纯文本格式的类型检查格式。
例如，应用于对象的

```console
dotnet fsi

> printfn "Hello %s, %d + %d is %d" "world" 2 2 (2+2);;
```

提供输出

```fsharp
Hello world, 2 + 2 is 4
```

F # 还允许将结构化值设置为纯文本格式。
例如，请考虑以下示例，该示例将输出的格式设置为类似矩阵的元组显示形式。

```console
dotnet fsi

> printfn "%A" [ for i in 1 .. 5 -> [ for j in 1 .. 5 -> (i, j) ] ];;

[[(1, 1); (1, 2); (1, 3); (1, 4); (1, 5)];
 [(2, 1); (2, 2); (2, 3); (2, 4); (2, 5)];
 [(3, 1); (3, 2); (3, 3); (3, 4); (3, 5)];
 [(4, 1); (4, 2); (4, 3); (4, 4); (4, 5)];
 [(5, 1); (5, 2); (5, 3); (5, 4); (5, 5)]]
```

在 `%A` 格式字符串中使用格式时，将激活结构化纯文本格式 `printf` 。
在 F # interactive 中格式化值的输出时，它也会被激活，其中的输出包含额外信息，还可自定义。
纯文本格式设置也可通过对 `x.ToString()` F # 联合和记录值（包括在调试、日志记录和其他工具中隐式执行的）的调用来观察。

## <a name="checking-of-printf-format-strings"></a>检查 `printf` 格式字符串

如果将 `printf` 格式设置函数与格式字符串中的 printf 格式说明符不匹配的参数一起使用，则将报告编译时错误。  例如，应用于对象的

```fsharp
sprintf "Hello %s" (2+2)
```

提供输出

```console
  sprintf "Hello %s" (2+2)
  ----------------------^

stdin(3,25): error FS0001: The type 'string' does not match the type 'int'
```

从技术上讲，使用 `printf` 和其他相关函数时，F # 编译器中的特殊规则检查作为格式字符串传递的字符串文本，确保应用的后续参数是正确的类型，以匹配所使用的格式说明符。

## <a name="format-specifiers-for-printf"></a>的格式说明符`printf`

格式规范格式 `printf` 是带有 `%` 指示格式的标记的字符串。 格式占位符包含以下 `%[flags][width][.precision][type]` 内容：

| 格式说明符   | 键入 (s)         | 备注                      |
|:-------------------|:---------------|:-----------------------------|
| `%b`               | bool      | 格式为 `true` 或`false`                |
| `%s`               | string    | 格式化为未转义的内容         |
| `%c`               | char      | 格式化为字符文本  |
| `%d`, `%i`         | 基本整数类型 | 格式化为十进制整数，如果基本整数类型已签名，则对其进行签名 |
| `%u`               | 基本整数类型 | 格式化为无符号的十进制整数   |
| `%x`, `%X`         | 基本整数类型 | 格式化为一个无符号的十六进制数字，分别 (a-f 或 A-F)   |
|  `%o`              | 基本整数类型 | 格式化为无符号的八进制数 |
| `%e`, `%E`         | 基本浮点类型 | 格式为有符号值 `[-]d.dddde[sign]ddd` ，其中 d 是单个十进制数字，dddd 是一个或多个十进制数字，ddd 只是三个十进制数字，而 sign 为 `+` 或`-` |
| `%f`               | 基本浮点类型 | 格式化为具有形式的带符号值 `[-]dddd.dddd` ，其中 `dddd` 是一个或多个十进制数字。 小数点前的数字位数取决于数字的度量值，小数点后的数字位数取决于所需精度。 |
| `%g`, `%G` | 基本浮点类型 |  使用作为或格式打印的带符号值进行格式化 `%f` `%e` ，以更紧凑的给定值和精度为准。 |
| `%M` | 一个 `System.Decimal` 值  |    使用 `"G"` 格式说明符的格式设置`System.Decimal.ToString(format)` |
| `%O` | 任何值  |   通过装箱对象并调用其方法设置格式 `System.Object.ToString()` |
| `%A` | 任何值  |   使用默认布局设置以[结构化纯文本格式](plaintext-formatting.md)设置格式 |
| `%a` | 任何值  |   需要两个参数：一个格式设置函数，接受上下文参数和值以及要打印的特定值 |
| `%t` | 任何值  |   需要一个参数：一个格式设置函数，它接受输出或返回适当文本的上下文参数 |
| `%%` | （无）  |   不需要任何参数，也不需要打印普通百分号：`%` |

基本整数类型 `byte` (`System.Byte`) ， `sbyte` (`System.SByte`)  () `int16` () `System.Int16` `uint16` `System.UInt16` () `int32` `System.Int32` `uint32` `System.UInt32` `int64` `System.Int64` `uint64` `System.UInt64` `nativeint` `System.IntPtr` `unativeint` `System.UIntPtr` ()  ()  ()  ()  () 。
基本浮点类型 `float` (`System.Double`) 并 `float32` (`System.Single`) 。

可选宽度是指示结果的最小宽度的整数。 例如， `%6d` 打印一个整数，用空格作为前缀，以至少填写六个字符。 如果 width 为 `*` ，则使用额外的整数参数来指定相应的宽度。

有效标志为：

| Flag   | 效果        | 备注                      |
|:-------------------|:---------------|:-----------------------------|
| `0`  | 添加零而不是空格以构成所需宽度 |    |
| `-` |  将结果左对齐指定的宽度 |   |
| `+`  | `+`如果数字为正，则添加一个字符， (以匹配 `-` 负号)  |   |
| 空格字符 | 如果数字为正，则添加一个额外的空格 (以匹配负的 "-" 符号)  |

Printf `#` 标志无效，如果使用，将报告编译时错误。

使用固定区域性设置值的格式。 区域性设置与格式无关， `printf` 除非它们影响和格式的结果 `%O` `%A` 。 有关详细信息，请参阅[结构化纯文本格式](plaintext-formatting.md)。

## <a name="a-formatting"></a>`%A`样式

`%A`格式说明符用于以用户可读的方式设置值的格式，并且还可用于报告诊断信息。

### <a name="primitive-values"></a>基元值

使用说明符设置纯文本格式时 `%A` ，F # 数值使用其后缀和固定区域性进行格式设置。 使用浮点精度的10个位置设置浮点值的格式。 例如，应用于对象的

```fsharp
printfn "%A" (1L, 3n, 5u, 7, 4.03f, 5.000000001, 5.0000000001)
```

得到

```console
(1L, 3n, 5u, 7, 4.03000021f, 5.000000001, 5.0)
```

使用此 `%A` 说明符时，使用引号设置字符串的格式。 不会添加转义码，而是输出原始字符。 例如，应用于对象的

```fsharp
printfn "%A" ("abc", "a\tb\nc\"d")

```

得到

```console
("abc", "a      b
c"d")
```

### <a name="net-values"></a>.NET 值

使用说明符设置纯文本格式时 `%A` ，非 F # .net 对象将使用 `x.ToString()` 和提供的 .net 默认设置进行格式化 `System.Globalization.CultureInfo.CurrentCulture` `System.Globalization.CultureInfo.CurrentUICulture` 。  例如，应用于对象的

```fsharp
open System.Globalization

let date = System.DateTime(1999, 12, 31)

CultureInfo.CurrentCulture <- CultureInfo.GetCultureInfo("de-DE")
printfn "Culture 1: %A" date

CultureInfo.CurrentCulture <- CultureInfo.GetCultureInfo("en-US")
printfn "Culture 2: %A" date
```

得到

```console
Culture 1: 31.12.1999 00:00:00
Culture 2: 12/31/1999 12:00:00 AM
```

### <a name="structured-values"></a>结构化值

使用说明符设置纯文本格式时 `%A` ，块缩进用于 F # 列表和元组。 如上一示例中所示。
还使用数组的结构，包括多维数组。  用语法显示一维数组 `[| ... |]` 。 例如，应用于对象的

```fsharp
printfn "%A" [| for i in 1 .. 20 -> (i, i*i) |]
```

得到

```console
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25); (6, 36); (7, 49); (8, 64); (9, 81);
  (10, 100); (11, 121); (12, 144); (13, 169); (14, 196); (15, 225); (16, 256);
  (17, 289); (18, 324); (19, 361); (20, 400)|]
```

默认打印宽度为80。  可以通过在格式说明符中使用打印宽度自定义此宽度。 例如，应用于对象的

```fsharp
printfn "%10A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%20A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%50A" [| for i in 1 .. 5 -> (i, i*i) |]
```

得到

```console
[|(1, 1);
  (2, 4);
  (3, 9);
  (4, 16);
  (5, 25)|]
[|(1, 1); (2, 4);
  (3, 9); (4, 16);
  (5, 25)|]
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25)|]
```

如果将打印宽度指定为0，则不会使用打印宽度。 将产生单行文本，只不过输出中嵌入的字符串包含分行符。  例如

```fsharp
printfn "%0A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%0A" [| for i in 1 .. 5 -> "abc\ndef" |]
```

得到

```console
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25)|]
[|"abc
def"; "abc
def"; "abc
def"; "abc
def"; "abc
def"|]
```

深度限制为4，用于序列 (`IEnumerable`) 值，这些值显示为 `seq { ...}` 。 对于列表和数组值，深度限制为100。
例如，应用于对象的

```fsharp
printfn "%A" (seq { for i in 1 .. 10 -> (i, i*i) })
```

得到

```console
seq [(1, 1); (2, 4); (3, 9); (4, 16); ...]
```

块缩进还用于公共记录和联合值的结构。 例如，应用于对象的

```fsharp
type R = { X : int list; Y : string list }

printfn "%A" { X =  [ 1;2;3 ]; Y = ["one"; "two"; "three"] }
```

得到

```console
{ X = [1; 2; 3]
  Y = ["one"; "two"; "three"] }
```

如果 `%+A` 使用了，则也可以使用反射来显示记录和联合的专用结构。 例如

```fsharp
type internal R =
    { X : int list; Y : string list }
    override _.ToString() = "R"

let internal data = { X = [ 1;2;3 ]; Y = ["one"; "two"; "three"] }

printfn "external view:\n%A" data

printfn "internal view:\n%+A" data

```

得到

```console
external view:
R

internal view:
{ X = [1; 2; 3]
  Y = ["one"; "two"; "three"] }
```

### <a name="large-cyclic-or-deeply-nested-values"></a>大、循环或深度嵌套的值

大型结构化值的格式设置为最大总体对象节点计数10000。
深度嵌套的值的格式为100。  在这两种情况下，都 `...` 用于 elide 一些输出。  例如，应用于对象的

```fsharp
type Tree =
    | Tip
    | Node of Tree * Tree

let rec make n =
    if n = 0 then
        Tip
    else
        Node(Tip, make (n-1))

printfn "%A" (make 1000)
```

生成包含部分省略的大型输出：

```console
Node(Tip, Node(Tip, ....Node (..., ...)...))
```

在对象图中检测到循环，并在 `...` 检测到循环的位置使用。 例如

```fsharp
type R = { mutable Links: R list }
let r = { Links = [] }
r.Links <- [r]
printfn "%A" r
```

得到

```console
{ Links = [...] }
```

### <a name="lazy-null-and-function-values"></a>迟缓、null 和函数值

如果尚未计算该值，则迟缓值将打印为 `Value is not created` 或等效文本。

空值将输出为， `null` 除非将值的静态类型确定为联合类型，其中 `null` 是允许的表示形式。

F # 函数值打印为其内部生成的结束名称，例如 `<fun:it@43-7>` 。

### <a name="customize-plain-text-formatting-with-structuredformatdisplay"></a>自定义纯文本格式`StructuredFormatDisplay`

使用说明符时 `%A` ，将遵循 `StructuredFormatDisplay` 类型声明的属性。  这可用于指定代理项文本和属性以显示值。 例如：

```fsharp
[<StructuredFormatDisplay("Counts({Clicks})")>]
type Counts = { Clicks:int list}

printfn "%20A" {Clicks=[0..20]}
```

得到

```console
Counts([0; 1; 2; 3;
        4; 5; 6; 7;
        8; 9; 10; 11;
        12; 13; 14;
        15; 16; 17;
        18; 19; 20])
```

### <a name="customize-plain-text-formatting-by-overriding-tostring"></a>通过重写自定义纯文本格式`ToString`

`ToString`在 F # 编程中可观察到的默认实现。 通常，默认结果不适合在面向程序员的信息显示或用户输出中使用，因此，通常会替代默认实现。  

默认情况下，F # 记录和联合类型使用的实现重写的实现 `ToString` `sprintf "%+A"` 。  例如，应用于对象的

```fsharp
type Counts = { Clicks:int list }

printfn "%s" ({Clicks=[0..10]}.ToString())
```

得到

```console
{ Clicks = [0; 1; 2; 3; 4; 5; 6; 7; 8; 9; 10] }
```

对于类类型，不提供的默认实现， `ToString` 并且使用 .net 默认值，该默认值报告类型的名称。 例如，应用于对象的

```fsharp
type MyClassType(clicks: int list) =
   member _.Clicks = clicks

let data = [ MyClassType([1..5]); MyClassType([1..5]) ]
printfn "Default structured print gives this:\n%A" data
printfn "Default ToString gives:\n%s" (data.ToString())
```

得到

```console
Default structured print gives this:
[MyClassType; MyClassType]
Default ToString gives:
[MyClassType; MyClassType]
```

为添加替代 `ToString` 可以提供更好的格式设置。

```fsharp
type MyClassType(clicks: int list) =
   member _.Clicks = clicks
   override _.ToString() = sprintf "MyClassType(%0A)" clicks

let data = [ MyClassType([1..5]); MyClassType([1..5]) ]
printfn "Now structured print gives this:\n%A" data
printfn "Now ToString gives:\n%s" (data.ToString())
```

得到

```console
Now structured print gives this:
[MyClassType([1; 2; 3; 4; 5]); MyClassType([1; 2; 3; 4; 5])]
Now ToString gives:
[MyClassType([1; 2; 3; 4; 5]); MyClassType([1; 2; 3; 4; 5])]
```

## <a name="f-interactive-structured-printing"></a>F# 交互窗口结构化打印

F# 交互窗口 (`dotnet fsi`) 使用结构化纯文本格式的扩展版本来报告值并允许附加可定制性。 有关详细信息，请参阅[F# 交互窗口](fsharp-interactive-options.md)。

## <a name="customize-debug-displays"></a>自定义调试显示

适用于 .NET 的调试器考虑使用和等属性 `DebuggerDisplay` `DebuggerTypeProxy` ，这些属性会影响调试器检查窗口中的对象的结构化显示。
F # 编译器自动为可区分的联合和记录类型生成这些属性，但不自动生成类、接口或结构类型。

F # 纯文本格式中会忽略这些属性，但在调试 F # 类型时，实现这些方法以改进显示会很有用。

## <a name="see-also"></a>请参阅

- [字符串](strings.md)
- [记录](records.md)
- [可区分联合](discriminated-unions.md)
- [F# Interactive](fsharp-interactive-options.md)
