---
description: '@ - C# 参考'
title: '@ - C# 参考'
ms.date: 02/09/2017
f1_keywords:
- '@_CSharpKeyword'
- '@'
helpviewer_keywords:
- '@ special character [C#]'
- '@ language element [C#]'
ms.assetid: 89bc7e53-85f5-478a-866d-1cca003c4e8c
ms.openlocfilehash: 7d78b28479ed6128321207073dc94976710f10b6
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89138899"
---
# <a name="-c-reference"></a>@（C# 参考）

`@` 特殊字符用作原义标识符。 它具有以下用途：

1. 使 C# 关键字用作标识符。 `@` 字符可作为代码元素的前缀，编译器将把此代码元素解释为标识符而非 C# 关键字。 下面的示例使用 `@` 字符定义其在 `for` 循环中使用的名为 `for` 的标识符。

   [!code-csharp[verbatim1](../../../../samples/snippets/csharp/language-reference/keywords/verbatim1.cs#1)]

1. 指示将原义解释字符串。 `@` 字符在此实例中定义原义标识符  。 简单转义序列（如代表反斜杠的 `"\\"`）、十六进制转义序列（如代表大写字母 A 的 `"\x0041"`）和 Unicode 转义序列（如代表大写字母 A 的 `"\u0041"`）都将按字面解释。 只有引号转义序列 (`""`) 不会按字面解释；因为它生成一个双引号。 此外，如果是逐字[内插字符串](interpolated.md)，大括号转义序列（`{{` 和 `}}`）不按字面解释；它们会生成单个大括号字符。 下面的示例分别使用常规字符串和原义字符串定义两个相同的文件路径。 这是原义字符串的较常见用法之一。

   [!code-csharp[verbatim2](../../../../samples/snippets/csharp/language-reference/keywords/verbatim1.cs#2)]

   下面的示例演示定义包含相同字符序列的常规字符串和原义字符串的效果。

   [!code-csharp[verbatim3](../../../../samples/snippets/csharp/language-reference/keywords/verbatim1.cs#3)]

1. 使编译器在命名冲突的情况下区分两种属性。 属性是派生自 <xref:System.Attribute> 的类。 其类型名称通常包含后缀 **Attribute**，但编译器不会强制进行此转换。 随后可在代码中按其完整类型名称（例如 `[InfoAttribute]`）或短名称（例如 `[Info]`）引用此属性。 但是，如果两个短名称相同，并且一个类型名称包含 **Attribute** 后缀而另一类型名称不包含，则会出现命名冲突。 例如，由于编译器无法确定将 `Info` 还是 `InfoAttribute` 属性应用于 `Example` 类，因此下面的代码无法编译。 有关详细信息，请参阅 [CS1614](../compiler-messages/cs1614.md)。

   [!code-csharp[verbatim4](../../../../samples/snippets/csharp/language-reference/keywords/verbatim2.cs#1)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 特殊字符](./index.md)
