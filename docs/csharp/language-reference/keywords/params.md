---
description: 参数数组的 params 关键字 - C# 参考
title: 参数数组的 params 关键字 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- params_CSharpKeyword
- params
helpviewer_keywords:
- parameters [C#], params
- params keyword [C#]
- parameter array
ms.assetid: 1690815e-b52b-4967-8380-5780aff08012
ms.openlocfilehash: a2726c725508cd297001aaabddeff414704d1115
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89134463"
---
# <a name="params-c-reference"></a>params（C# 参考）

使用 `params` 关键字可以指定采用数目可变的参数的[方法参数](method-parameters.md)。 参数类型必须是一维数组。

在方法声明中的 `params` 关键字之后不允许有任何其他参数，并且在方法声明中只允许有一个 `params` 关键字。

如果 `params` 参数的声明类型不是一维数组，则会发生编译器错误 [CS0225](../../misc/cs0225.md)。

使用 `params` 参数调用方法时，可以传入：

- 数组元素类型的参数的逗号分隔列表。
- 指定类型的参数的数组。
- 无参数。 如果未发送任何参数，则 `params` 列表的长度为零。

## <a name="example"></a>示例

下面的示例演示可向 `params` 形参发送实参的各种方法。

[!code-csharp[csrefKeywordsMethodParams#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsMethodParams/CS/csrefKeywordsMethodParams.cs#5)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [方法参数](method-parameters.md)
