---
title: 如何比较字符串 - C# 指南
description: 了解在区分或不区分大小写以及使用或不使用区域性特定的排序情况下，如何对字符串值进行比较和排序
ms.date: 10/03/2018
helpviewer_keywords:
- strings [C#], comparison
- comparing strings [C#]
ms.openlocfilehash: d1ea0fc3573714347580a2aaded2d0f3118681a8
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324176"
---
# <a name="how-to-compare-strings-in-c"></a>如何：比较 C\# 中的字符串

通过比较字符串可以回答两个问题，一个是：“这两个字符串相等吗？” 另一个是“排序时，应该按什么顺序排列这些字符串？”

这两个问题非常复杂，因为字符串比较受很多因素的影响：

- 可以选择序号比较或语义比较。
- 可以选择是否区分大小写。
- 可以选择区域性特定的比较。
- 语义比较取决于区域性和平台。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

在比较字符串时定义它们的顺序。 通过比较决定字符串的序列。 确定序列顺序后，软件和人工都可以更轻松地进行搜索。 其他比较可能会检查字符串是否相同。 这种一致性检查与相等性检查类似，但是也有一些不同之处，例如可能会忽略大小写的差异。

## <a name="default-ordinal-comparisons"></a>默认的序号比较

默认情况下，最常见的操作：

- <xref:System.String.CompareTo%2A?displayProperty=nameWithType>
- <xref:System.String.Equals%2A?displayProperty=nameWithType>
- <xref:System.String.op_Equality%2A?displayProperty=nameWithType> 和 <xref:System.String.op_Inequality%2A?displayProperty=nameWithType>（即，分别为[相等运算符 `==` 和 `!=`](../language-reference/operators/equality-operators.md#string-equality)）

执行区分大小写的序号比较，并在需要的情况下使用当前区域性。 下面的示例演示了这一操作：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet1":::

默认序号比较在比较字符串时不会考虑语义规则。 它会比较两个字符串中每个 <xref:System.Char> 对象的二进制值。 因此，默认的序号比较也是区分大小写的。

使用 <xref:System.String.Equals%2A?displayProperty=nameWithType> 以及 `==` 和 `!=` 运算符的相等性测试不同于使用 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 和 <xref:System.String.Compare(System.String,System.String)?displayProperty=nameWithType)> 方法的字符串比较。 虽然相等性测试执行区分大小写的序号比较，但比较方法使用当前区域性执行区分大小写、区分区域性的比较。 由于默认比较方法通常执行不同类型的比较，因此建议始终调用显式指定要执行的比较类型的重载，确保代码意图明确。

## <a name="case-insensitive-ordinal-comparisons"></a>不区分大小写的序号比较

<xref:System.String.Equals(System.String,System.StringComparison)?displayProperty=nameWithType> 方法允许指定 <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 的 <xref:System.StringComparison> 值
对于不区分大小写的序号比较。 此外还有静态 <xref:System.String.Compare(System.String,System.String,System.StringComparison)?displayProperty=nameWithType> 方法，该方法执行不区分大小写的序号比较，前提是你指定 <xref:System.StringComparison> 参数的 <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 值。 如以下代码所示：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet2":::

执行不区分大小写的序号比较时，这些方法使用[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)的大小写约定。

## <a name="linguistic-comparisons"></a>语义比较

可以使用当前区域性的语义规则来对字符串进行排序。
这有时被称为“文字排序顺序”。 在执行语义比较时，一些非字母数字的 Unicode 字符可能分配有特殊的权重。 例如，连字符“-”分配的权重可能很小，所以“co-op”和“coop”在排序顺序中会彼此相邻。 此外，一些 Unicode 字符可能与一系列 <xref:System.Char> 实例等效。 下面以德语短句“他们在街上跳舞。”为例， 在德语中，一个字符串中有“ss”(U+0073 U+0073)，另一个字符串中有“ß”(U+00DF)。 （在 Windows 系统中）从语义上说，“ss”在“en-US”和“de-DE”区域性中都等同于德语中的“'ß”。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet3":::

此示例说明了语义比较是依赖于操作系统的。 交互式窗口的主机为 Linux 主机。 语义比较和序号比较的结果是一样的。 如果在 Windows 主机上运行同样的示例，会看到如下输出内容：

```console
<coop> is less than <co-op> using invariant culture
<coop> is greater than <co-op> using ordinal comparison
<coop> is less than <cop> using invariant culture
<coop> is less than <cop> using ordinal comparison
<co-op> is less than <cop> using invariant culture
<co-op> is less than <cop> using ordinal comparison
```

在 Windows 上，从语义比较改为序号比较时，“cop”、“coop”和“co-op”的排序顺序产生了变化。 使用不同的比较类型时，这两个德语句子的比较结果也就不同了。

## <a name="comparisons-using-specific-cultures"></a>使用特定区域性的比较

此示例为 en-US 和 de-DE 区域性存储 <xref:System.Globalization.CultureInfo> 对象。
使用 <xref:System.Globalization.CultureInfo> 对象执行比较以确保执行的是特定于区域性的比较。

所用的区域性会对语义比较产生影响。 以下示例展示使用“en-US”区域性和“de-DE”区域性对两个德语句子进行比较的结果：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet4":::

区分区域性的比较通常用于对用户输入的字符串以及用户输入的其他字符串进行比较和排序。 字符和这些字符的排序约定可能会根据用户计算机的区域设置而有所不同。 即使是包含相同字符的字符串，也可能因当前线程的区域性而具有不同的排序。 此外，在本地 Windows 计算机上尝试下列示例代码，将得到下列结果：

```console
<coop> is less than <co-op> using en-US culture
<coop> is greater than <co-op> using ordinal comparison
<coop> is less than <cop> using en-US culture
<coop> is less than <cop> using ordinal comparison
<co-op> is less than <cop> using en-US culture
<co-op> is less than <cop> using ordinal comparison
```

语义比较取决于当前区域性以及操作系统。 在进行字符串比较时，请考虑这些内容。

## <a name="linguistic-sorting-and-searching-strings-in-arrays"></a>数组中的语义排序和字符串搜索

以下示例演示如何在数组中使用依赖当前区域性的语义比较对字符串进行排序和搜索。 使用采用 <xref:System.StringComparer?displayProperty=nameWithType> 参数的静态 <xref:System.Array> 方法。

此示例演示如何使用当前区域性对字符串数组进行排序：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet5":::

对数组进行排序后，可以使用二分搜索法搜索条目。 二分搜索法从集合的中间开始搜索，判断集合的哪一半包含所找字符串。 后续的每个比较都将集合的剩余部分再次对半分开。  使用 <xref:System.StringComparer.CurrentCulture?displayProperty=nameWithType> 存储数组。 本地函数 `ShowWhere` 显示发现字符串所在位置的信息。 如果未找到字符串，返回的值会指示可以在其中找到字符串的位置。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet6":::

## <a name="ordinal-sorting-and-searching-in-collections"></a>集合中的序号排序和搜索

以下代码使用 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> 集合类存储字符串。 字符串是通过 <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType> 方法排序的。 此方法需要对两个字符串进行比较和排序的委托。 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 方法提供该比较函数。 请运行示例并观察顺序。 此排序操作使用区分大小写的序号排序。 你要使用静态 <xref:System.String.Compare%2A?displayProperty=nameWithType> 方法指定不同的比较规则。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet7":::

排序后，可以使用二分搜索法对字符串列表进行搜索。 以下示例演示如何使用相同的比较函数搜索排序列表。 本地函数 `ShowWhere` 显示所查找的文本所在的位置或可能会在位置：

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet8":::

在排序和搜索过程中，请始终确保使用相同的比较类型。 使用不同的比较类型进行排序和搜索会产生意外的结果。

元素或键的类型为 `string` 时，<xref:System.Collections.Hashtable?displayProperty=nameWithType>、<xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> 和 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> 等集合类的构造函数具有 <xref:System.StringComparer?displayProperty=nameWithType> 参数。 通常，应尽可能使用这些构造函数，并指定 <xref:System.StringComparer.Ordinal?displayProperty=nameWithType> 或 <xref:System.StringComparer.OrdinalIgnoreCase?displayProperty=nameWithType>。

## <a name="reference-equality-and-string-interning"></a>引用相等性和字符串集中

这些示例都没有使用 <xref:System.Object.ReferenceEquals%2A>。 此方法确定两个字符串是否为同一对象，这可能导致字符串比较中出现不一致的结果。 以下示例演示了 C# 中的字符串集中功能。 如果程序声明了 2 个或多个相同的字符串变量，则编译器会将其存储在同一位置。 通过调用 <xref:System.Object.ReferenceEquals%2A> 方法，可以看到这两个字符串实际上引用的是内存中的同一对象。 使用 <xref:System.String.Copy%2A?displayProperty=nameWithType> 方法可避免集中。 创建副本后，两个字符串存储在不同位置（即使它们具有相同的值）。 运行下列示例以显示字符串 `a` 和 `b` 是集中的，也就是说它们共享相同的存储。 字符串 `a` 和 `c` 不是。

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet9":::

> [!NOTE]
> 测试字符串是否相等时，使用的方法应显式指定要执行的比较类型。 你的代码具备更强的可维护性和可读性。 重载采用了 <xref:System.StringComparison> 枚举参数的 <xref:System.String?displayProperty=nameWithType> 和 <xref:System.Array?displayProperty=nameWithType> 类的方法。 指定要执行的比较类型。 在测试相等性时，请避免使用 `==` 和 `!=` 运算符。 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> 实例方法始终执行区分大小写的序号比较。 它们主要适用于按字母顺序进行的字符串排序。

可以通过调用 <xref:System.String.Intern%2A?displayProperty=nameWithType> 方法暂存字符串或检索对现有暂存字符串的引用。 要确定字符串是否暂存，请调用 <xref:System.String.IsInterned%2A?displayProperty=nameWithType> 方法。

## <a name="see-also"></a>请参阅

- <xref:System.Globalization.CultureInfo?displayProperty=nameWithType>
- <xref:System.StringComparer?displayProperty=nameWithType>
- [字符串](../programming-guide/strings/index.md)
- [比较字符串](../../standard/base-types/comparing.md)
- [对应用程序进行全球化和本地化](/visualstudio/ide/globalizing-and-localizing-applications)
