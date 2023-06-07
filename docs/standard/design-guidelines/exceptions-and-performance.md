---
title: 异常和性能
ms.date: 10/22/2008
helpviewer_keywords:
- tester-doer pattern
- TryParse pattern
- exceptions, throwing
- exceptions, performance
- throwing exceptions, performance
ms.assetid: 3ad6aad9-08e6-4232-b336-0e301f2493e6
ms.openlocfilehash: babe378e0d61357709006e08f71ff578492f116c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734746"
---
# <a name="exceptions-and-performance"></a>异常和性能

与异常相关的一个常见问题是，如果异常用于经常失败的代码，则实现的性能不可接受。 这是一个合理的担忧。 当成员引发异常时，其性能可以是较慢的顺序。 但是，可以获得良好的性能，同时严格遵守禁止使用错误代码的异常指导原则。 本节中所述的两种模式提供了执行此操作的方法。

 ❌ 不要使用错误代码，原因是异常可能会对性能产生负面影响。

 若要提高性能，可以使用以下两节中所述的 Tester-Doer 模式或 Try-Parse 模式。

## <a name="tester-doer-pattern"></a>Tester-Doer 模式

 有时异常引发成员的性能可以通过将成员分为两个来改进。 让我们看看 <xref:System.Collections.Generic.ICollection%601.Add%2A> 接口的方法 <xref:System.Collections.Generic.ICollection%601> 。

```csharp
ICollection<int> numbers = ...
numbers.Add(1);
```

 如果集合是只读的，则该方法将 `Add` 引发。 这可能会导致方法调用经常失败的情况下出现性能问题。 缓解此问题的一种方法是在尝试添加值之前测试集合是否可写。

```csharp
ICollection<int> numbers = ...
...
if (!numbers.IsReadOnly)
{
    numbers.Add(1);
}
```

 用于测试条件的成员（在我们的示例中为属性 `IsReadOnly` ）称为 "测试人员"。 用于执行可能引发的操作的成员（我们的 `Add` 示例中的方法）称为 doer。

 ✔️考虑在常见方案中可能会引发异常的成员的 Tester-Doer 模式，以避免与异常相关的性能问题。

## <a name="try-parse-pattern"></a>Try-Parse 模式

 对于极高性能的 Api，应使用比上一部分中所述的 Tester-Doer 模式更为快速的模式。 模式调用以调整成员名称，从而使定义完善的测试用例成为成员语义的一部分。 例如， <xref:System.DateTime> 定义在对 <xref:System.DateTime.Parse%2A> 字符串进行分析失败时引发异常的方法。 它还定义了一个 <xref:System.DateTime.TryParse%2A> 尝试进行分析的相应方法，但如果分析不成功，则返回 false，并使用参数返回成功分析的结果 `out` 。

```csharp
public struct DateTime
{
    public static DateTime Parse(string dateTime)
    {
        ...
    }
    public static bool TryParse(string dateTime, out DateTime result)
    {
        ...
    }
}
```

 使用此模式时，务必按严格的条款定义尝试功能。 如果成员由于任何原因而失败，而不是明确定义的尝试，则成员仍必须引发相应的异常。

 ✔️考虑在常见方案中可能会引发异常的成员的 Try-Parse 模式，以避免与异常相关的性能问题。

 ✔️对实现此模式的方法使用前缀 "Try" 和 Boolean 返回类型。

 ✔️使用 Try-Parse 模式为每个成员提供异常引发成员。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [异常设计准则](exceptions.md)
