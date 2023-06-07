---
title: 参考返回值
ms.date: 04/28/2017
helpviewer_keywords:
- variables [Visual Basic]
- ref return values [Visual Basic]
- ref returns [Visual Basic]
ms.assetid: 5ef0cc69-eb3a-4a67-92a2-78585f223cb5
ms.openlocfilehash: f2a92c584dbb12a322e28435d797fa4d7c2f6dbb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79186926"
---
# <a name="support-for-reference-return-values-visual-basic"></a>支持参考返回值（可视基本值）

从 C# 7.0 开始，C# 语言支持*参考返回值*。 理解引用返回值的一种方法是，它们是通过引用传递给方法的参数的相反。 修改引用传递的参数时，更改将反映在调用方上的变量的值中。 当方法向调用方提供引用返回值时，调用方对引用返回值所做的修改将反映在被调用方法的数据中。

Visual Basic 不允许使用引用返回值编写方法，但它确实允许您使用引用返回值。 换句话说，可以调用具有引用返回值的方法并修改该返回值，对引用返回值的更改将反映在被调用的方法的数据中。

## <a name="modifying-the-ref-return-value-directly"></a>直接修改 ref 返回值

对于始终成功且没有`ByRef`参数的方法，可以直接修改引用返回值。 为此，通过将新值分配给返回引用返回值的表达式。

下面的 C# 示例定义了一`NumericValue.IncrementValue`个方法，该方法将内部值递增并返回为引用返回值。

[!code-csharp[Ref-Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/procedures/ref-returns1.cs)]

然后，调用方在以下 Visual Basic 示例中修改引用返回值。 请注意，使用 方法调用`NumericValue.IncrementValue`的行不会为 方法分配值。 相反，它将值分配给方法返回的引用返回值。

[!code-vb[Ref-Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/procedures/use-ref-returns1.vb)]

## <a name="using-a-helper-method"></a>使用帮助器方法

在其他情况下，直接修改方法调用的引用返回值可能并不总是可取的。 例如，返回字符串的搜索方法可能并不总是找到匹配项。 在这种情况下，仅当搜索成功时，才要修改引用返回值。

以下 C# 示例说明了此方案。 它定义用`Sentence`C# 编写的类包括一`FindNext`个方法，该方法在以指定的子字符串开头的句子中查找下一个单词。 该字符串作为引用返回值返回，方法引用传递的 `Boolean` 变量指示搜索是否成功。 引用返回值指示除了读取返回的值外，调用方还可以修改它，并且该修改反映在`Sentence`类内部包含的数据中。

[!code-csharp[Ref-Return](../../../../../samples/snippets/visualbasic/getting-started/ref-returns.cs)]

在这种情况下，直接修改引用返回值不可靠，因为方法调用可能无法找到匹配项并返回句子中的第一个单词。 在这种情况下，调用方将无意中修改句子的第一个单词。 调用方返回 （`null`或`Nothing`视觉基本值） 可以防止这种情况。 但是在这种情况下，尝试修改其值为`Nothing`的字符串将引发 。 <xref:System.NullReferenceException> 如果调用方也可以阻止 返回<xref:System.String.Empty?displayProperty=nameWithType>，但这需要调用方定义其值为<xref:System.String.Empty?displayProperty=nameWithType>的字符串变量。 虽然调用方可以修改该字符串，但修改本身没有用处，因为修改后的字符串与`Sentence`类存储的句子中的单词没有关系。

处理此方案的最佳方法是通过引用帮助器方法传递引用返回值。 然后，帮助器方法包含逻辑，以确定方法调用是否成功，如果成功，则修改引用返回值。 下面的示例提供了一个可能的实现。

[!code-vb[Ref-Return](../../../../../samples/snippets/visualbasic/getting-started/ref-return-helper.vb#1)]

## <a name="see-also"></a>另请参阅

- [按值和引用传递参数](passing-arguments-by-value-and-by-reference.md)
- [Visual Basic 中的过程](index.md)
