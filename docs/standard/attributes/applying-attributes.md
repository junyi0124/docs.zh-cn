---
title: 应用特性
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- assemblies [.NET], attributes
- attributes [.NET], applying
ms.assetid: dd7604eb-9fa3-4b60-b2dd-b47739fa3148
ms.openlocfilehash: 83cfb1d5b5aa3ebc8651406850a758146fd329d4
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829097"
---
# <a name="apply-attributes"></a>应用属性

使用以下过程将特性应用于代码的元素。

1. 定义新特性或使用现有的 .NET 特性。

2. 通过将特性置于紧邻元素之前，将该特性应用于代码元素。

     每种语言都有其自己的特性语法。 在 C++ 和 C# 中，特性是用方括号括起来的，并通过空格与元素分开（可能包括换行符）。 在 Visual Basic 中，特性是用尖括号括起来的，且必须位于同一逻辑线上；如果需要换行符，可使用行继续字符。

3. 指定特性的位置参数和命名参数。

     位置参数是必需的，且必须位于所有命名参数之前；它们对应于特性的其中一个构造函数的参数。 命名参数是可选的且对应于特性的读/写属性。 在 C++ 和 C# 中，为每个可选参数指定 `name=value`，其中 `name` 是属性名。 在 Visual Basic 中，指定 `name:=value`。

 编译代码时，特性将被发到元数据中，并且通过运行时反射服务可用于公共语言运行时和任何自定义工具或应用程序。

 按照惯例，所有特性名称都以“Attribute”结尾。 但是，面向运行时的几种语言（如 Visual Basic 和 C#）无需指定特性的全名。 例如，若要初始化 <xref:System.ObsoleteAttribute?displayProperty=nameWithType>，只需将它引用为 Obsolete 即可。

## <a name="apply-an-attribute-to-a-method"></a>将特性应用于方法

 以下代码示例显示如何使用 System.ObsoleteAttribute（其将代码标记为已过时）。 将字符串 `"Will be removed in next version"` 传递给特性。 当特性描述的代码被调用时，此特性会导致产生编译器警告，显示传递的字符串。

 [!code-cpp[Conceptual.Attributes.Usage#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source1.cpp#3)]
 [!code-csharp[Conceptual.Attributes.Usage#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source1.cs#3)]
 [!code-vb[Conceptual.Attributes.Usage#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source1.vb#3)]

## <a name="apply-attributes-at-the-assembly-level"></a>在程序集级别应用特性

 如果要在程序集级别应用属性，请使用 assembly`Assembly`（Visual Basic 中用`assembly` ）关键字。 下列代码显示在程序集级别应用的 **AssemblyTitleAttribute**。

 [!code-cpp[Conceptual.Attributes.Usage#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source1.cpp#2)]
 [!code-csharp[Conceptual.Attributes.Usage#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source1.cs#2)]
 [!code-vb[Conceptual.Attributes.Usage#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source1.vb#2)]

 应用此特性时，字符串 `"My Assembly"` 将被放置在文件元数据部分的程序集清单中。 可通过后列方法查看特性：使用 [MSIL 反汇编程序 (Ildasm.exe)](../../framework/tools/ildasm-exe-il-disassembler.md)，或创建一个自定义程序来检索特性。

## <a name="see-also"></a>请参阅

- [特性](index.md)
- [检索存储在特性中的信息](retrieving-information-stored-in-attributes.md)
- [概念](/cpp/windows/attributed-programming-concepts)
- [特性 (C#)](../../csharp/programming-guide/concepts/attributes/index.md)
- [特性概述 (Visual Basic)](../../visual-basic/programming-guide/concepts/attributes/index.md)
