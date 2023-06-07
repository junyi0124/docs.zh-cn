---
title: C# 保留的特性：Conditional, Obsolete, AttributeUsage
ms.date: 04/09/2020
description: 这些特性由编译器解释，以影响编译器生成的代码
ms.openlocfilehash: c6d697dd08233ffc88900949998047137ee170a9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2020
ms.locfileid: "82021761"
---
# <a name="reserved-attributes-conditionalattribute-obsoleteattribute-attributeusageattribute"></a>保留的特性：ConditionalAttribute, ObsoleteAttribute, AttributeUsageAttribute

这些特性可应用于代码中的元素。 它们为这些元素添加语义。 编译器使用这些语义来更改其输出，并报告使用你的代码的开发人员可能犯的错误。

## <a name="conditional-attribute"></a>`Conditional` 特性

`Conditional` 特性使得方法执行依赖于预处理标识符。 `Conditional` 属性是 <xref:System.Diagnostics.ConditionalAttribute> 的别名，可以应用于方法或特性类。

在以下示例中，`Conditional` 应用于启用或禁用显示特定于程序的诊断信息的方法：

:::code language="csharp" source="snippets/trace.cs" interactive="try-dotnet" :::

如果未定义 `TRACE_ON` 标识符，则不会显示跟踪输出。 在交互式窗口中自己探索。

`Conditional` 特性通常与 `DEBUG` 标识符一起使用，以启用调试生成（而非发布生成）中的跟踪和日志记录功能，如下例所示：

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetConditional" :::

当调用标记为条件的方法时，指定的预处理符号是否存在将决定是包含还是省略该调用。 如果定义了符号，则将包括调用；否则，将忽略该调用。 条件方法必须是类或结构声明中的方法，而且必须具有 `void` 返回类型。 与将方法封闭在 `#if…#endif` 块内相比，`Conditional` 更简洁且较不容易出错。

如果某个方法具有多个 `Conditional` 特性，则如果定义了一个或多个条件符号（通过使用 OR 运算符将这些符号逻辑链接在一起），则包含对该方法的调用。 在以下示例中，存在 `A` 或 `B` 将导致方法调用：

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetMultipleConditions" :::

### <a name="using-conditional-with-attribute-classes"></a>使用带有特性类的 `Conditional`

`Conditional` 特性还可应用于特性类定义。 在以下示例中，如果定义了 `DEBUG`，则自定义特性 `Documentation` 将仅向元数据添加信息。

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetConditionalConditionalAttribute" :::

## <a name="obsolete-attribute"></a>`Obsolete` 特性

`Obsolete` 特性将代码元素标记为不再推荐使用。 使用标记为已过时的实体会生成警告或错误。 `Obsolete` 特性是一次性特性，可以应用于任何允许特性的实体。 `Obsolete` 是 <xref:System.ObsoleteAttribute> 的别名。

在以下示例中，`Obsolete` 特性应用于类 `A` 和方法 `B.OldMethod`。 因为应用于 `B.OldMethod` 的特性构造函数的第二个参数设置为 `true`，所以此方法将导致编译器错误，而使用类 `A` 只会生成警告。 但是，调用 `B.NewMethod` 不会生成任何警告或错误。 例如，将其与先前的定义一起使用时，以下代码会生成两个警告和一个错误：

:::code language="csharp" source="snippets/ObsoleteExample.cs" interactive="try-dotnet" :::

作为特性构造函数的第一个参数提供的字符串将作为警告或错误的一部分显示。 将生成类 `A` 的两个警告：一个用于声明类引用，另一个用于类构造函数。 `Obsolete` 特性可以在不带参数的情况下使用，但建议说明改为使用哪个项目。

## <a name="attributeusage-attribute"></a>`AttributeUsage` 特性

`AttributeUsage` 特性确定自定义特性类的使用方式。 <xref:System.AttributeUsageAttribute> 是应用到自定义特性定义的特性。 `AttributeUsage` 特性帮助控制：

- 可能应用到的具体程序元素特性。 除非使用限制，否则特性可能应用到以下任意程序元素：
  - 程序集 (assembly)
  - name
  - field
  - event
  - method
  - param
  - property
  - return
  - type
- 某特性是否可多次应用于单个程序元素。
- 特性是否由派生类继承。

显式应用时，默认设置如以下示例所示：

:::code language="csharp" source="snippets/NewAttribute.cs" id="SnippetUsageFirst" :::

在此示例中，`NewAttribute` 类可应用于任何受支持的程序元素。 但是它对每个实体仅能应用一次。 特性应用于基类时，它由派生类继承。

<xref:System.AttributeUsageAttribute.AllowMultiple> 和 <xref:System.AttributeUsageAttribute.Inherited> 参数是可选的，因此以下代码具有相同效果：

:::code language="csharp" source="snippets/NewAttribute.cs" id="SnippetUsageSecond" :::

第一个 <xref:System.AttributeUsageAttribute> 参数必须是 <xref:System.AttributeTargets> 枚举的一个或多个元素。 可将多个目标类型与 OR 运算符链接在一起，如下例所示：

:::code language="csharp" source="snippets/NewPropertyOrFieldAttribute.cs" id="SnippetDefinePropertyAttribute" :::

从 C# 7.3 开始，特性可应用于自动实现的属性的属性或支持字段。 特性应用于属性，除非在特性上指定 `field` 说明符。 都在以下示例中进行了演示：

:::code language="csharp" source="snippets/NewPropertyOrFieldAttribute.cs" id="SnippetUsePropertyAttribute" :::

如果 <xref:System.AttributeUsageAttribute.AllowMultiple> 参数为 `true`，那么结果特性可多次应用于单个实体，如以下示例所示：

:::code language="csharp" source="snippets/MultiUseAttribute.cs" id="SnippetMultiUse" :::

在本例中，`MultiUseAttribute` 可重复应用，因为 `AllowMultiple` 设置为 `true`。 所显示的两种用于应用多个特性的格式均有效。

如果 <xref:System.AttributeUsageAttribute.Inherited> 为 `false`，那么该特性不是由特性类派生的类继承。 例如：

:::code language="csharp" source="snippets/NonInheritedAttribute.cs" id="SnippetNonInherited" :::

在本例中，`NonInheritedAttribute` 不会通过继承应用于 `DClass`。

## <a name="see-also"></a>请参阅

- <xref:System.Attribute>
- <xref:System.Reflection>
- [特性](../../../standard/attributes/index.md)
- [反射](../../programming-guide/concepts/reflection.md)
