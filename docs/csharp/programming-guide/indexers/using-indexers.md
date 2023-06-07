---
title: 使用索引器 - C# 编程指南
description: 了解如何使用 C# 声明和使用类、结构或接口的索引器。 本文包含示例代码。
ms.date: 07/15/2020
helpviewer_keywords:
- indexers [C#], about indexers
ms.assetid: df70e1a2-3ce3-4aba-ad80-4b2f3538699f
ms.openlocfilehash: a8a9e05c1d2e44841177a924c6ff51c6c9e6a05a
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301861"
---
# <a name="using-indexers-c-programming-guide"></a>使用索引器（C# 编程指南）

索引器使你可从语法上方便地创建[类](../../language-reference/keywords/class.md)、[结构](../../language-reference/builtin-types/struct.md)或[接口](../../language-reference/keywords/interface.md)，以便客户端应用程序可以像访问数组一样访问它们。 编译器将生成一个 `Item` 属性（或者如果存在 <xref:System.Runtime.CompilerServices.IndexerNameAttribute>，也可以生成一个命名属性）和适当的访问器方法。 在主要目标是封装内部集合或数组的类型中，常常要实现索引器。 例如，假设有一个类 `TempRecord`，它表示 24 小时的周期内在 10 个不同时间点所记录的温度（单位为华氏度）。 此类包含一个 `float[]` 类型的数组 `temps`，用于存储温度值。 通过在此类中实现索引器，客户端可采用 `float temp = tempRecord[4]` 的形式（而非 `float temp = tempRecord.temps[4]`）访问 `TempRecord` 实例中的温度。 索引器表示法不但简化了客户端应用程序的语法；还使类及其目标更容易直观地为其它开发者所理解。

若要在类或结构上声明索引器，请使用 [this](../../language-reference/keywords/this.md) 关键字，如以下示例所示：

```csharp
// Indexer declaration
public int this[int index]
{
    // get and set accessors
}
```

> [!IMPORTANT]
> 通过声明索引器，可自动在对象上生成一个名为 `Item` 的属性。 无法从实例[成员访问表达式](../../language-reference/operators/member-access-operators.md#member-access-expression-)直接访问 `Item` 属性。 此外，如果通过索引器向对象添加自己的 `Item` 属性，则将收到 [CS0102 编译器错误](../../misc/cs0102.md)。 要避免此错误，请使用 <xref:System.Runtime.CompilerServices.IndexerNameAttribute> 来重命名索引器，详细信息如下所示。

## <a name="remarks"></a>备注

索引器及其参数的类型必须至少具有和索引器相同的可访问性。 有关可访问性级别的详细信息，请参阅[访问修饰符](../../language-reference/keywords/access-modifiers.md)。

有关如何在接口上使用索引器的详细信息，请参阅[接口索引器](./indexers-in-interfaces.md)。

索引器的签名由其形参的数目和类型所组成。 它不包含索引器类型或形参的名称。 如果要在相同类中声明多个索引器，则它们的签名必须不同。

索引器值不分类为变量；因此，无法将索引器值作为 [ref](../../language-reference/keywords/ref.md) 或 [out](../../language-reference/keywords/out-parameter-modifier.md) 参数来传递。

若要使索引器的名称可为其他语言所用，请使用 <xref:System.Runtime.CompilerServices.IndexerNameAttribute?displayProperty=nameWithType>，如以下示例所示：

```csharp
// Indexer declaration
[System.Runtime.CompilerServices.IndexerName("TheItem")]
public int this[int index]
{
    // get and set accessors
}
```

此索引器被索引器名称属性重写，因此其名称将为 `TheItem`。 默认情况下，默认名称为 `Item`。

## <a name="example-1"></a>示例 1

下列示例演示如何声明专用数组字段 `temps` 和索引器。 索引器可以实现对实例 `tempRecord[i]` 的直接访问。 若不使用索引器，则将数组声明为[公共](../../language-reference/keywords/public.md)成员，并直接访问其成员 `tempRecord.temps[i]`。

:::code language="csharp" source="snippets/Temperatures/TempRecord.cs":::

请注意，当评估索引器访问时（例如在 `Console.Write` 语句中），将调用 [get](../../language-reference/keywords/get.md) 访问器。 因此，如果不存在 `get` 访问器，则会发生编译时错误。

:::code language="csharp" source="snippets/Temperatures/Program.cs":::

## <a name="indexing-using-other-values"></a>使用其他值进行索引

C# 不将索引参数类型限制为整数。 例如，对索引器使用字符串可能有用。 通过搜索集合内的字符串并返回相应的值，可以实现此类索引器。 访问器可被重载，因此字符串和整数版本可以共存。

## <a name="example-2"></a>示例 2

下面的示例声明了存储星期几的类。 `get` 访问器采用字符串（星期几）并返回对应的整数。 例如，“Sunday”返回 0，“Monday”返回 1，依此类推。

:::code language="csharp" source="snippets/StringIndexers/DayCollection.cs":::

### <a name="consuming-example-2"></a>使用示例 2

:::code language="csharp" source="snippets/StringIndexers/Program.cs":::

## <a name="example-3"></a>示例 3

下面的示例声明了使用 <xref:System.DayOfWeek?displayProperty=fullName> 存储星期几的类。 `get` 访问器采用 `DayOfWeek`（表示星期几的值）并返回对应的整数。 例如，`DayOfWeek.Sunday` 返回 0，`DayOfWeek.Monday` 返回 1，依此类推。

:::code language="csharp" source="snippets/DayOfWeekIndexers/DayOfWeekCollection.cs":::

### <a name="consuming-example-3"></a>使用示例 3

:::code language="csharp" source="snippets/DayOfWeekIndexers/Program.cs":::

## <a name="robust-programming"></a>可靠编程

提高索引器的安全性和可靠性有两种主要方法：

- 请确保结合某一类型的错误处理策略，以处理万一客户端代码传入无效索引值的情况。 在本主题前面的第一个示例中，TempRecord 类提供了 Length 属性，使客户端代码能在将输入传递给索引器之前对其进行验证。 也可将错误处理代码放入索引器自身内部。 请确保为用户记录在索引器的访问器中引发的任何异常。

- 在可接受的程度内，为 [get](../../language-reference/keywords/get.md) 和 [set](../../language-reference/keywords/set.md) 访问器的可访问性设置尽可能多的限制。 这一点对 `set` 访问器尤为重要。 有关详细信息，请参阅[限制访问器可访问性](../classes-and-structs/restricting-accessor-accessibility.md)。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [索引器](./index.md)
- [属性](../classes-and-structs/properties.md)
