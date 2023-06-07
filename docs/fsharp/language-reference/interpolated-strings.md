---
title: 内插字符串
description: '了解内插字符串，它是一种特殊形式的字符串，可用于在其中直接嵌入 F # 表达式。'
ms.date: 11/12/2020
ms.openlocfilehash: 8c552b44cea7d6c51ec333b6bdd4d407c6f10da7
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829682"
---
# <a name="interpolated-strings"></a>内插字符串

内插字符串是允许您向其中嵌入 F # 表达式的 [字符串](strings.md) 。 它们在各种情况下非常有用，其中字符串的值可能根据值或表达式的结果而变化。

## <a name="syntax"></a>语法

```fsharp
$"string-text {expr}"
$"string-text %format-specifier{expr}"
$"""string-text {"embedded string literal"}"""
```

## <a name="remarks"></a>备注

内插字符串使你可以在字符串文本中的 "洞" 中编写代码。 下面是一个基本示例：

```fsharp
let name = "Phillip"
let age = 30
printfn $"Name: {name}, Age: {age}"

printfn $"I think {3.0 + 0.14} is close to {System.Math.PI}!"
```

每个 `{}` 大括号对之间的内容可以是任何 F # 表达式。

若要转义 `{}` 大括号对，请编写两个，如下所示：

```fsharp
let str = $"A pair of braces: {{}}"
// "A pair of braces: {}"
```

## <a name="typed-interpolated-strings"></a>类型化内插字符串

内插字符串还可以具有 F # 格式说明符来强制类型安全。

```fsharp
let name = "Phillip"
let age = 30

printfn $"Name: %s{name}, Age: %d{age}"

// Error: type mismatch
printfn $"Name: %s{age}, Age: %d{name}"
```

在上面的示例中，代码错误地传递了 `age` `name` 应为的值，反之亦然。 因为内插字符串使用格式说明符，所以这是一个编译错误，而不是微妙的运行时错误。

[纯文本格式](plaintext-formatting.md)中涵盖的所有格式说明符在内插字符串中都有效。

## <a name="verbatim-interpolated-strings"></a>逐字内插字符串

F # 支持带有三个引号的逐字内插字符串，以便可以嵌入字符串文本。

```fsharp
let age = 30

printfn $"""Name: {"Phillip"}, Age: %d{age}"""
```

## <a name="aligning-expressions-in-interpolated-strings"></a>对齐内插字符串中的表达式

您可以使用左对齐或右对齐内插字符串中的表达式 `|` ，并指定多少个空格。 下面的内插字符串分别向左和向右对齐7个空格。

```fsharp
printfn $"""|{"Left",-7}|{"Right",7}|"""
// |Left   |  Right|
```

## <a name="interpolated-strings-and-formattablestring-formatting"></a>内插字符串和 `FormattableString` 格式设置

还可以应用符合以下规则的格式 <xref:System.FormattableString> ：

```fsharp
let speedOfLight = 299792.458
printfn $"The speed of light is {speedOfLight:N3} km/s."
// "The speed of light is 299,792.458 km/s."
```

此外，还可以通过类型注释将内插字符串作为类型检查 <xref:System.FormattableString> ：

```fsharp
let frmtStr = $"The speed of light is {speedOfLight:N3} km/s." : FormattableString
// Type: FormattableString
// The speed of light is 299,792.458 km/s.
```

请注意，类型批注必须位于内插字符串表达式本身。 F # 不会将内插字符串隐式转换为 <xref:System.FormattableString> 。

## <a name="see-also"></a>请参阅

* [字符串](strings.md)
