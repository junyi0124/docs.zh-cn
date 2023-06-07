---
description: using 语句 - C# 参考
title: using 语句 - C# 参考
ms.date: 05/29/2020
f1_keywords:
- using-statement_CSharpKeyword
helpviewer_keywords:
- using statement [C#]
ms.assetid: afc355e6-f0b9-4240-94dd-0d93f17d9fc3
ms.openlocfilehash: 451235d9d41565c35369700eeec75cfd2f5bfc06
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599325"
---
# <a name="using-statement-c-reference"></a>using 语句（C# 参考）

提供可确保正确使用 <xref:System.IDisposable> 对象的方便语法。 从 C#8.0 开始，`using` 语句可确保正确使用 <xref:System.IAsyncDisposable> 对象。

## <a name="example"></a>示例

下面的示例演示如何使用 `using` 语句。

:::code language="csharp" source="snippets/usings.cs" id="SnippetFirstExample":::

从 C# 8.0 开始，可以对不需要使用大括号的 `using` 语句使用以下替代语法：

:::code language="csharp" source="snippets/usings.cs" id="SnippetModernUsing":::

## <a name="remarks"></a>备注

<xref:System.IO.File> 和 <xref:System.Drawing.Font> 是访问非托管资源（本例中为文件句柄和设备上下文）的托管类型的示例。 有许多其他类别的非托管资源和封装这些资源的类库类型。 所有此类类型都必须实现 <xref:System.IDisposable> 接口或 <xref:System.IAsyncDisposable> 接口。

`IDisposable` 对象的生存期限于单个方法时，应在 `using` 语句中声明并实例化它。 `using` 语句按照正确的方式调用对象上的 <xref:System.IDisposable.Dispose%2A> 方法，并（在按照前面所示方式使用它时）会导致在调用 <xref:System.IDisposable.Dispose%2A> 时对象自身处于范围之外。 在 `using` 块中，对象是只读的并且无法进行修改或重新分配。 如果对象实现 `IAsyncDisposable` 而不是 `IDisposable`，`using` 语句将调用 <xref:System.IAsyncDisposable.DisposeAsync%2A> 和 `awaits` 返回的 <xref:System.Threading.Tasks.ValueTask>。 有关 <xref:System.IAsyncDisposable> 的详细信息，请参阅[实现 DisposeAsync 方法](../../../standard/garbage-collection/implementing-disposeasync.md)。

`using` 语句可确保调用 <xref:System.IDisposable.Dispose%2A> 或 <xref:System.IAsyncDisposable.DisposeAsync%2A>，即使 `using` 块中发生异常也是如此。 通过将对象放入 `try` 块中，然后调用 `finally` 块中的 <xref:System.IDisposable.Dispose%2A>（或 <xref:System.IAsyncDisposable.DisposeAsync%2A>），可以实现相同的结果；实际上，这就是编译器转换 `using` 语句的方式。 前面的代码示例在编译时将扩展到以下代码（请注意，使用额外的大括号为对象创建有限范围）：

:::code language="csharp" source="snippets/usings.cs" id="SnippetTryFinallyExample":::

较新的 `using` 语句语法转换为类似的代码。 `try` 块在声明变量的位置打开。 `finally` 块添加在封闭块的末尾，通常是在方法的末尾。

有关 `try`-`finally` 语句的详细信息，请参阅 [try-finally](try-finally.md) 一文。

可在单个 `using` 语句中声明一个类型的多个实例，如下面的示例中所示。 注意，在单个语句中声明多个变量时，不能使用隐式类型的变量 (`var`)：

:::code language="csharp" source="snippets/usings.cs" id="SnippetDeclareMultipleVariables":::

可以使用与 C# 8 一起引入的新语法，合并同一类型的多个声明，如下面的示例中所示：

:::code language="csharp" source="snippets/usings.cs" id="SnippetModernMultipleVariables":::

可以实例化资源对象，然后将变量传递到 `using` 语句，但这不是最佳做法。 在这种情况下，控件退出 `using` 块以后，对象保留在作用域中，但是可能没有访问其未托管资源的权限。 换而言之，它不再是完全初始化的。 如果尝试在 `using` 块外部使用该对象，则可能导致引发异常。 因此，最好在 `using` 语句中实例化该对象并将其范围限制在 `using` 块中。

:::code language="csharp" source="snippets/usings.cs" id="SnippetDeclareBeforeUsing":::

有关释放 `IDisposable` 对象的详细信息，请参阅[使用实现 IDisposable 的对象](../../../standard/garbage-collection/using-objects.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的 [using 语句](~/_csharplang/spec/statements.md#the-using-statement)。 该语言规范是 C# 语法和用法的权威资料。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [using 指令](using-directive.md)
- [垃圾回收](../../../standard/garbage-collection/index.md)
- [使用实现 IDisposable 的对象](../../../standard/garbage-collection/using-objects.md)
- [IDisposable 接口](xref:System.IDisposable)
- [C# 8.0 中的 using 语句](~/_csharplang/proposals/csharp-8.0/using.md)
