---
description: checked 关键字 - C# 参考
title: checked 关键字 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- checked_CSharpKeyword
- checked
helpviewer_keywords:
- checked keyword [C#]
ms.assetid: 718a1194-988d-48a3-b089-d6ee8bd1608d
ms.openlocfilehash: 4dd8bc02d3af89d6bab3aef55a88cb8b40704da6
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89138275"
---
# <a name="checked-c-reference"></a>checked（C# 参考）

`checked` 关键字用于对整型类型算术运算和转换显式启用溢出检查。

默认情况下，如果表达式仅包含常量值，且产生的值在目标类型范围之外，则会导致编译器错误。 如果表达式包含一个或多个非常量值，则编译器不检测溢出。 在下面的示例中，计算赋给 `i2` 的表达式不会导致编译器错误。

[!code-csharp[csrefKeywordsChecked#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsChecked/CS/csrefKeywordsChecked.cs#3)]

默认情况下，在运行时也不检查这些非常量表达式是否溢出，这些表达式不引发溢出异常。 上面的示例显示 -2,147,483,639 作为两个正整数之和。

可以通过编译器选项、环境配置或使用 `checked` 关键字来启用溢出检查。 下面的示例演示如何使用 `checked` 表达式或 `checked` 块，在运行时检测由前面的求和计算导致的溢出。 两个示例都引发溢出异常。

[!code-csharp[csrefKeywordsChecked#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsChecked/CS/csrefKeywordsChecked.cs#4)]

可以使用 [unchecked](./unchecked.md) 关键字阻止溢出检查。

## <a name="example"></a>示例

此示例演示如何使用 `checked` 启用运行时溢出检查。

[!code-csharp[csrefKeywordsChecked#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsChecked/CS/csrefKeywordsChecked.cs#1)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](./index.md)
- [Checked 和 Unchecked](./checked-and-unchecked.md)
- [unchecked](./unchecked.md)
