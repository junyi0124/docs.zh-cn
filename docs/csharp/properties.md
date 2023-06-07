---
title: 属性
description: 了解 C# 属性，包括验证功能、计算值、迟缓计算及属性更改通知。
ms.technology: csharp-fundamentals
ms.date: 04/25/2018
ms.openlocfilehash: 5306650a10a3de0ff208f7c72c0ba520a527e746
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96437944"
---
# <a name="properties"></a>属性

属性是 C# 中的一等公民。 借助该语言所定义的语法，开发人员能够编写出准确表达其设计意图的代码。

访问属性时，其行为类似于字段。
但与字段不同的是，属性通过访问器实现；访问器用于定义访问属性或为属性赋值时执行的语句。

## <a name="property-syntax"></a>属性语法

属性语法是字段的自然延伸。 字段定义存储位置：

[!code-csharp[Person class with public fields](../../samples/snippets/csharp/properties/Person.cs#1)]

属性定义包含 `get` 和 `set` 访问器的声明，这两个访问器用于检索该属性的值以及对其赋值：

[!code-csharp[Person class with public properties](../../samples/snippets/csharp/properties/Person.cs#2)]

上述语法是 *自动属性* 语法。 编译器生成支持该属性的字段的存储位置。 编译器还实现 `get` 和 `set` 访问器的正文。

有时，需要将属性初始化为其类型默认值以外的值。  C# 通过在属性的右括号后设置值达到此目的。 对于 `FirstName` 属性的的初始值，你可能更希望设置为空字符串而非 `null`。 可按如下所示进行指定：

[!code-csharp[Person class with properties and initializer](../../samples/snippets/csharp/properties/Person.cs#3)]

特定初始化对于只读属性最有用，本文后面部分将进行介绍。

你也可以自行定义存储，如下所示：

[!code-csharp[Person class with properties and backing field](../../samples/snippets/csharp/properties/Person.cs#4)]

属性实现是单个表达式时，可为 getter 或 setter 使用 expression-bodied 成员：

[!code-csharp[Person class with properties and expression bodied getters and setters](../../samples/snippets/csharp/properties/Person.cs#5)]

在本文中，将在所有适用之处使用此简化的语法。

上述属性定义是读-写属性。 注意 set 访问器中的关键字 `value`。 `set` 访问器始终具有一个名为 `value` 的参数。 `get` 访问器必须返回一个值，该值可转换为该属性的类型（本例中为 `string`）。

这就是该语法的基础知识。 有许多不同的语法变体，支持着各种不同的设计习惯。 接下来我们将了解这些变体，以及每个变体的语法选项。

## <a name="scenarios"></a>方案

上述示例介绍了属性定义中最简单的一种情况：不进行验证的读-写属性。 通过在 `get` 和 `set` 访问器中编写所需的代码，可以创建多种不同的方案。

### <a name="validation"></a>验证

可以在 `set` 访问器中编写代码，确保由某个属性表示的值始终有效。 例如，假设 `Person` 类的一个规则是姓名不得为空白或空白符。 可按如下方式编写：

[!code-csharp[Validating property setters](../../samples/snippets/csharp/properties/Person.cs#6)]

可通过将 `throw` 表达式用作属性资源库验证的一部分来简化前面的示例：

[!code-csharp[Validating property setters](../../samples/snippets/csharp/properties/Person.cs#7)]

上面的示例强制执行名字不得为空白或空白符的规则。 如果开发人员编写

```csharp
hero.FirstName = "";
```

该赋值会引发 `ArgumentException`。 由于属性 set 访问器必须具有 void 返回类型，因此将通过引发异常来报告 set 访问器中的错误。

你可以根据自己的情况随意扩展此语法。 可以检查不同属性之间的关系，或根据任何外部条件进行验证。 任何有效的 C# 语句在属性访问器中都是有效的。

### <a name="read-only"></a>只读

到目前为止，你了解的所有属性定义都是具有公共访问器的读/写属性。 但这不是属性唯一有效的可访问性。
你可以创建只读属性，或者对 set 和 get 访问器提供不同的可访问性。 假设 `Person` 类只能从该类的其他方法中启用 `FirstName` 属性值更改。 可以为 set 访问器提供 `private` 可访问性，而不是 `public` 可访问性：

[!code-csharp[Using a private setter for a publicly readonly property](../../samples/snippets/csharp/properties/Person.cs#8)]

现在，可以从任意代码访问 `FirstName` 属性，但只能从 `Person` 类中的其他代码对其赋值。

可以向 set 和 get 访问器添加任何严格访问修饰符。 在单个访问器上放置的任何访问修饰符都必须比属性定义上的访问修饰符提供更严格的限制。 上述做法是合法的，因为 `FirstName` 属性为 `public`，但 set 访问器为 `private`。 不能声明具有 `public` 访问器的 `private` 属性。 属性声明还可以声明为 `protected`、`internal`、`protected internal`，甚至 `private`。

在 `get` 访问器上放置限制性更强的修饰符也是合法的。 例如，可以有一个 `public` 属性，但将 `get` 访问器限制为 `private`。 不过实际情况下很少这么做。

还可以限制对属性的修改，以便只能在构造函数或属性初始化表达式中设置属性。 可按照这种方式修改 `Person` 类，如下所示：

[!code-csharp[A readonly auto implemented property](../../samples/snippets/csharp/properties/Person.cs#9)]

此功能通常用于初始化作为只读属性公开的集合：

```csharp
public class Measurements
{
    public ICollection<DataPoint> points { get; } = new List<DataPoint>();
}
```

### <a name="computed-properties"></a>计算属性

属性无需只返回某个成员字段的值。 可以创建返回计算值的属性。 让我们展开 `Person` 对象，返回通过串联名字和姓氏计算得出的全名：

[!code-csharp[A computed property](../../samples/snippets/csharp/properties/Person.cs#10)]

上面的示例使用[字符串内插](./language-reference/tokens/interpolated.md)功能来创建全名的格式化字符串。

也可以使用 expression-bodied 成员，以更简洁的方式来创建 `FullName` 计算属性：

[!code-csharp[A computed property using an expression bodied member](../../samples/snippets/csharp/properties/Person.cs#11)]

expression-bodied 成员使用 lambda 表达式语法来定义包含单个表达式的方法。 在这里，该表达式返回 person 对象的全名。

### <a name="cached-evaluated-properties"></a>缓存的计算属性

可以将计算属性和存储的概念混合起来，创建“缓存的计算属性”。  例如，可以更新 `FullName` 属性，以便仅在第一次访问该属性时进行字符串格式设置：

[!code-csharp[Caching the value of a computed property](../../samples/snippets/csharp/properties/Person.cs#12)]

不过，上面的代码含有 bug。 如果代码更新 `FirstName` 或 `LastName` 属性的值，那么，以前计算的 `fullName` 字段将无效。 修改 `FirstName` 和 `LastName` 属性的 `set` 访问器，以便重新计算 `fullName` 字段：

[!code-csharp[Invalidating the cache correctly](../../samples/snippets/csharp/properties/Person.cs#13)]

此最终版本仅在必要时计算 `FullName` 属性。
如果以前计算的版本有效，则使用它。 如果另一个状态更改使以前计算的版本失效，则重新计算。 使用此类的开发人员无需了解实现的细枝末节。 这些内部更改不会影响 Person 对象的使用。 这是使用属性公开对象的数据成员的关键原因。

### <a name="attaching-attributes-to-auto-implemented-properties"></a>将特性附加到自动实现的属性

从 C# 7.3 开始，可在自动实现的属性中将字段特性附加到编译器生成的支持字段。 例如，可考虑添加唯一整数 `Person` 属性的 `Id` 类的修订。
使用自动实现的属性编写 `Id` 属性，但是该设计不需要保留 `Id` 属性。 <xref:System.NonSerializedAttribute> 只能附加到字段，不能附加到属性。 可使用特性上的 `field:` 说明符将 <xref:System.NonSerializedAttribute> 附加到 `Id` 属性的支持字段，如下例所示：

[!code-csharp[Attaching attributes to a backing field](../../samples/snippets/csharp/properties/Person.cs#14)]

该技术适用于附加到自动实现的属性上的支持字段的所有特性。

### <a name="implementing-inotifypropertychanged"></a>实现 INotifyPropertyChanged

需要在属性访问器中编写代码的最后一种情形是为了支持 <xref:System.ComponentModel.INotifyPropertyChanged> 接口，该接口用于通知数据绑定客户端值已更改。 当属性值发生更改时，该对象引发 <xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged?displayProperty=nameWithType> 事件来指示更改。 数据绑定库则基于该更改来更新显示元素。 下面的代码演示如何为此 person 类的 `FirstName` 属性实现 `INotifyPropertyChanged`。

[!code-csharp[invalidating the cache correctly](../../samples/snippets/csharp/properties/Person.cs#15)]

`?.` 运算符称作 *null 条件运算符*。 它在计算运算符右侧之前会检查是否存在空引用。 最终结果为：如果 `PropertyChanged` 事件没有订阅者，则不执行用于引发该事件的代码。 在这种情况下，如果不执行此检查，则会引发 `NullReferenceException`。 有关详细信息，请参阅 [`events`](events-overview.md)。 此示例还使用新的 `nameof` 运算符将属性名称符号转换为其文本表示形式。
使用 `nameof` 可以减少输错属性名称这样的错误。

再次说明，实现 <xref:System.ComponentModel.INotifyPropertyChanged> 是可以在访问器中编写代码以支持所需方案的情况的示例。

## <a name="summing-up"></a>总结

属性是类或对象中的一种智能字段形式。 从对象外部，它们看起来像对象中的字段。 但是，属性可以通过丰富的 C# 功能来实现。
你可以提供验证、不同的可访问性、迟缓计算或方案所需的任何要求。
