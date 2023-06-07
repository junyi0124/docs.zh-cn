---
title: 命名空间 - C# 编程指南
description: 了解 C# 编程中的命名空间。 请参阅命名空间属性概述，并查看其他资源。
ms.date: 08/21/2018
helpviewer_keywords:
- C# language, namespaces
- namespaces [C#]
ms.assetid: b1c4ab46-3fad-4ffa-9deb-dd50a2d8c65a
ms.openlocfilehash: 41a666fd5f368e6990e08a36700e18f648939213
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94440336"
---
# <a name="namespaces-c-programming-guide"></a>命名空间（C# 编程指南）

在 C# 编程中，命名空间在两个方面被大量使用。 首先，.NET 使用命名空间来组织它的许多类，如下所示：  

[!code-csharp[csProgGuide#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#22)]

<xref:System> 是一个命名空间，<xref:System.Console> 是该命名空间中的一个类。 可以使用 `using` 关键字，如此则不必使用完整的名称，如下例所示：

[!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]

[!code-csharp[csProgGuide#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#23)]

有关详细信息，请参阅 [using 指令](../../language-reference/keywords/using-directive.md)。

其次，在较大的编程项目中，声明自己的命名空间可以帮助控制类和方法名称的范围。 使用 [namespace](../../language-reference/keywords/namespace.md) 关键字可声明命名空间，如下例所示：

[!code-csharp[csProgGuideNamespaces#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces.cs#6)]

命名空间的名称必须是有效的 C# [标识符名称](../inside-a-program/identifier-names.md)。

## <a name="namespaces-overview"></a>命名空间概述

命名空间具有以下属性：

- 它们组织大型代码项目。
- 通过使用 `.` 运算符分隔它们。
- `using` 指令可免去为每个类指定命名空间的名称。
- `global` 命名空间是“根”命名空间：`global::System` 始终引用 .NET <xref:System> 命名空间。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[命名空间](~/_csharplang/spec/namespaces.md)部分。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [使用命名空间](using-namespaces.md)
- [如何使用 My 命名空间](how-to-use-the-my-namespace.md)
- [标识符名称](../inside-a-program/identifier-names.md)
- [using 指令](../../language-reference/keywords/using-directive.md)
- [::运算符](../../language-reference/operators/namespace-alias-qualifier.md)
