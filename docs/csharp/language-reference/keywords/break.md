---
description: break 语句 - C# 参考
title: break 语句 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- break
- break_CSharpKeyword
helpviewer_keywords:
- break keyword [C#]
ms.assetid: be2571ed-efb0-4965-b122-81e5b09db0b9
ms.openlocfilehash: 7fd05889f684f7a2282de8222e1195898dead5b9
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89134739"
---
# <a name="break-c-reference"></a>break（C# 参考）

`break` 语句将终止其所在位置的最接近封闭循环或 [switch](./switch.md) 语句。 控制权将传递给已终止语句后面的语句（若有）。

## <a name="example"></a>示例

在此示例中，条件语句包含一个应从 1 计数到 100 的计数器；但 `break` 语句在计数器计数到 4 后终止了循环。

[!code-csharp[csrefKeywordsJump#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsJump/CS/csrefKeywordsJump.cs#1)]

## <a name="example"></a>示例

本示例演示 `break` 在 [switch](./switch.md) 语句中的用法。

[!code-csharp[csrefKeywordsJump#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsJump/CS/csrefKeywordsJump.cs#2)]

如果输入 `4`，则输出为：

```console
Enter your selection (1, 2, or 3): 4
Sorry, invalid selection.
```

## <a name="example"></a>示例

在此示例中，`break` 语句用于中断内层嵌套循环，并将控制权返回给外层循环。 控件在嵌套循环中仅向上返回一级__。

[!code-csharp[csrefKeywordsJump#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsJump/CS/csrefKeywordsJump.cs#7)]

## <a name="example"></a>示例

在本例中，`break` 语句仅用于在循环的每次迭代中脱离当前分支。 循环本身不受属于嵌套 [switch](./switch.md) 语句的 `break` 实例的影响。

[!code-csharp[csrefKeywordsJump#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsJump/CS/csrefKeywordsJump.cs#8)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](./index.md)
- [switch](./switch.md)
