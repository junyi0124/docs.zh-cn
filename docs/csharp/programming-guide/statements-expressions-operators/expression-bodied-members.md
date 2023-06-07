---
title: 表达式主体定义的成员 - C# 编程指南
description: 了解 expression-bodied 成员。 查看使用属性、构造函数、终结器等的表达式主体定义的代码示例。
ms.date: 02/06/2019
helpviewer_keywords:
- expression-bodied members[C#]
- C# language, expresion-bodied members
ms.openlocfilehash: e68e96e4aa3ff6a64590459a7197da1833e1a275
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381653"
---
# <a name="expression-bodied-members-c-programming-guide"></a>Expression-bodied 成员（C# 编程指南）

通过表达式主体定义，可采用非常简洁的可读形式提供成员的实现。 只要任何支持的成员（如方法或属性）的逻辑包含单个表达式，就可以使用表达式主体定义。 表达式主体定义具有下列常规语法：

```csharp
member => expression;
```

其中“expression”是有效的表达式。

C# 6 中引入了针对方法和只读属性的表达式主体定义支持，并在 C# 7.0 中进行了扩展。 表达式主体定义可用于下表列出的类型成员：

|成员  |开始提供支持的版本 |
|---------|---------|
|[方法](#methods)  |C# 6 |
|[只读属性](#read-only-properties)   |C# 6  |
|[Property](#properties)  |C# 7.0 |
|[构造函数](#constructors)   |C# 7.0 |
|[终结器](#finalizers)     |C# 7.0 |
|[索引器](#indexers)       |C# 7.0 |

## <a name="methods"></a>方法

expression-bodied 方法包含单个表达式，它返回的值的类型与方法的返回类型匹配；或者，对于返回 `void` 的方法，其表达式则执行某些操作。 例如，替代 <xref:System.Object.ToString%2A> 方法的类型通常包含单个表达式，该表达式返回当前对象的字符串表示形式。

下面的示例定义 `Person` 类，该类通过表达式主体定义替代 <xref:System.Object.ToString%2A>。 它还定义向控制台显示名称的 `DisplayName` 方法。 请注意，`ToString` 表达式主体定义中未使用 `return` 关键字。

[!code-csharp[expression-bodied-methods](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-methods.cs)]  

有关详细信息，请参阅[方法（C# 编程指南）](../classes-and-structs/methods.md)。

## <a name="read-only-properties"></a>只读属性

从 C# 6 开始，可以使用表达式主体定义来实现只读属性。 为此，请使用以下语法：

```csharp
PropertyType PropertyName => expression;
```

下面的示例定义 `Location` 类，其只读 `Name` 属性以表达式主体定义的形式实现，该表达式主体定义返回私有 `locationName` 字段值：

[!code-csharp[expression-bodied-read-only-property](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-readonly.cs#1)]  

有关属性的详细信息，请参阅[属性（C# 编程指南）](../classes-and-structs/properties.md)。

## <a name="properties"></a>属性

从 C# 7.0 开始，可以使用表达式主体定义来实现属性 `get` 和 `set` 访问器。 下面的示例演示其实现方法：

[!code-csharp[expression-bodied-property-get-set](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-ctor.cs#1)]

有关属性的详细信息，请参阅[属性（C# 编程指南）](../classes-and-structs/properties.md)。

## <a name="constructors"></a>构造函数

构造函数的表达式主体定义通常包含单个赋值表达式或一个方法调用，该方法调用可处理构造函数的参数，也可初始化实例状态。

以下示例定义 `Location` 类，其构造函数具有一个名为“name”的字符串参数。 表达式主体定义向 `Name` 属性分配参数。

[!code-csharp[expression-bodied-constructor](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-ctor.cs#1)]  

有关详细信息，请参阅[构造函数（C# 编程指南）](../classes-and-structs/constructors.md)。

## <a name="finalizers"></a>终结器

终结器的表达式主体定义通常包含清理语句，例如释放非托管资源的语句。

下面的示例定义了一个终结器，该终结器使用表达式主体定义来指示已调用该终结器。

[!code-csharp[expression-bodied-finalizer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-destructor.cs#1)]  

有关详细信息，请参阅[终结器（C# 编程指南）](../classes-and-structs/destructors.md)。

## <a name="indexers"></a>索引器

与使用属性一样，如果 `get` 访问器包含返回值的单个表达式或 `set` 访问器执行简单的赋值，则索引器 `get` 和 `set` 访问器包含表达式主体定义。

下面的示例定义名为 `Sports` 的类，其中包含一个内部 <xref:System.String> 数组，该数组包含大量体育运动的名称。 索引器的 `get` 和 `set` 访问器都以表达式主体定义的形式实现。

[!code-csharp[expression-bodied-indexer](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-indexers.cs#1)]

有关详细信息，请参阅[索引器（C# 编程指南）](../indexers/index.md)。
