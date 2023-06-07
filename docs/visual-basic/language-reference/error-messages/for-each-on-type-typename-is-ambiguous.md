---
title: 类型“<typename>”的“For Each”不明确，因为此类型实现了“System.Collections.Generic.IEnumerable(Of T)”的多个实例化
ms.date: 07/20/2015
f1_keywords:
- vbc32096
- bc32096
helpviewer_keywords:
- BC32096
ms.assetid: ed20d09c-913f-482e-89f8-c0a596c3ec24
ms.openlocfilehash: 0f19836efeabcf1d9e5097667c719c1f7d99cbbb
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163462"
---
# <a name="bc32096-for-each-on-type-typename-is-ambiguous-because-the-type-implements-multiple-instantiations-of-systemcollectionsgenericienumerableof-t"></a>类型 "" 的 BC32096： "" \<typename> 不明确，因为该类型实现 ("的多个实例化，而 T) "

`For Each`语句指定具有多个方法的迭代器变量 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 。

 迭代器变量必须是在 <xref:System.Collections.IEnumerable?displayProperty=nameWithType> <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> .NET Framework 的命名空间之一中实现或接口的类型 `Collections` 。 类可以实现多个构造泛型接口，每个构造使用不同的类型参数。 如果用于执行此的类用于迭代器变量，则该变量有多种 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 方法。 在这种情况下，Visual Basic 无法选择要调用的方法。

 **错误 ID：** BC32096

## <a name="to-correct-this-error"></a>更正此错误

- 使用 [DirectCast 运算符](../operators/directcast-operator.md) 或 [TryCast 运算符](../operators/trycast-operator.md) 将迭代器变量类型强制转换为定义 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 要使用的方法的接口。

## <a name="see-also"></a>另请参阅

- [For Each...Next 语句](../statements/for-each-next-statement.md)
- [接口](../../programming-guide/language-features/interfaces/index.md)
