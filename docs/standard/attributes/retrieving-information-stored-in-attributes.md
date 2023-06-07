---
title: 检索存储在特性中的信息
description: 了解如何检索存储在特性中的信息，例如，对于同一范围内的特性实例、多个实例，以及不同范围内的多个实例。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- retrieving attributes
- multiple attribute instances
- attributes [.NET], retrieving
ms.assetid: 37dfe4e3-7da0-48b6-a3d9-398981524e1c
ms.openlocfilehash: 6ba01fcd23e626354e5f9a2baa914815b61c8332
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701557"
---
# <a name="retrieving-information-stored-in-attributes"></a>检索存储在特性中的信息

检索自定义属性的过程非常简单。 首先，声明要检索的属性实例。 然后，使用 <xref:System.Attribute.GetCustomAttribute%2A?displayProperty=nameWithType> 方法，用要检索的属性的值初始化新属性。 在初始化新属性后，只需使用它的属性即可获取值。  
  
> [!IMPORTANT]
> 本主题介绍了如何为执行上下文中加载的代码检索属性。 若要为仅反射上下文中加载的代码检索属性，必须使用 <xref:System.Reflection.CustomAttributeData> 类，如以下所述：[如何：将程序集加载到仅反射上下文中](../../framework/reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)。  
  
 此部分介绍了如何通过以下方式检索属性：  
  
- [检索一个属性实例](#cpconretrievingsingleinstanceofattribute)  
  
- [检索应用于同一范围的多个属性实例](#cpconretrievingmultipleinstancesofattributeappliedtosamescope)  
  
- [检索应用于不同范围的多个属性实例](#cpconretrievingmultipleinstancesofattributeappliedtodifferentscopes)  
  
<a name="cpconretrievingsingleinstanceofattribute"></a>

## <a name="retrieving-a-single-instance-of-an-attribute"></a>检索一个属性实例  

 在下面的示例中，`DeveloperAttribute`（如上一部分所述）在类一级适用于 `MainApp` 类。 `GetAttribute` 方法使用 GetCustomAttribute 在类一级检索 `DeveloperAttribute` 中存储的值，再在控制台中显示它们。  
  
 [!code-cpp[Conceptual.Attributes.Usage#18](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#18)]
 [!code-csharp[Conceptual.Attributes.Usage#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#18)]
 [!code-vb[Conceptual.Attributes.Usage#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#18)]  
  
 此程序在执行时显示以下文本。  
  
```console  
The Name Attribute is: Joan Smith.  
The Level Attribute is: 42.  
The Reviewed Attribute is: True.  
```  
  
 如果找不到属性，GetCustomAttribute 方法会将 `MyAttribute` 初始化为 NULL 值。 此示例在 `MyAttribute` 中查找此类实例，并在找不到属性时通知用户。 如果在类范围中找不到 `DeveloperAttribute`，控制台中显示以下消息。  
  
```console  
The attribute was not found.
```  
  
 此示例假定属性定义位于当前的命名空间中。 请注意，如果属性定义不在当前的命名空间中，请导入属性定义所在的命名空间。  
  
<a name="cpconretrievingmultipleinstancesofattributeappliedtosamescope"></a>

## <a name="retrieving-multiple-instances-of-an-attribute-applied-to-the-same-scope"></a>检索应用于同一范围的多个属性实例  

 在上一示例中，要检查的类和要查找的特定属性都传递给 <xref:System.Attribute.GetCustomAttribute%2A>。 此代码非常适用于只有一个属性实例在类一级应用的情况。 不过，如果在相同的类一级应用多个属性实例，GetCustomAttribute 方法不会检索所有信息。 如果同一属性的多个实例应用于相同范围，可以使用 <xref:System.Attribute.GetCustomAttributes%2A?displayProperty=nameWithType> 将所有属性实例添加到数组中。 例如，如果在相同的类一级应用两个 `DeveloperAttribute` 实例，可以将 `GetAttribute` 方法修改为显示在这两个属性中找到的信息。 请注意，若要在同一级别应用多个属性，必须在 <xref:System.AttributeUsageAttribute> 中定义属性，并将 AllowMultiple 属性设为 true。  
  
 下面的代码示例展示了如何使用 GetCustomAttributes 方法来创建数组，以引用任何给定类中的所有 `DeveloperAttribute` 实例。 然后，所有属性的值都显示在控制台中。  
  
 [!code-cpp[Conceptual.Attributes.Usage#19](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#19)]
 [!code-csharp[Conceptual.Attributes.Usage#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#19)]
 [!code-vb[Conceptual.Attributes.Usage#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#19)]  
  
 如果找不到任何属性，此代码会向用户发出警报。 如果找到，就会显示两个 `DeveloperAttribute` 实例中包含的信息。  
  
<a name="cpconretrievingmultipleinstancesofattributeappliedtodifferentscopes"></a>

## <a name="retrieving-multiple-instances-of-an-attribute-applied-to-different-scopes"></a>检索应用于不同范围的多个属性实例  

 <xref:System.Attribute.GetCustomAttributes%2A> 和 <xref:System.Attribute.GetCustomAttribute%2A> 方法不会搜索整个类，也不会返回相应类中的所有属性实例。 而是一次只搜索一个指定方法或成员。 如果类中的相同属性应用于每个成员，且要检索应用于这些成员的所有属性的值，必须将各个方法或成员单独提供给 GetCustomAttributes 和 GetCustomAttribute。  
  
 下面的代码示例将类用作参数，并在类一级以及相应类的所有方法一级搜索 `DeveloperAttribute`（如前面所定义）。  
  
 [!code-cpp[Conceptual.Attributes.Usage#20](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#20)]
 [!code-csharp[Conceptual.Attributes.Usage#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#20)]
 [!code-vb[Conceptual.Attributes.Usage#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#20)]  
  
 如果在方法或类一级找不到 `DeveloperAttribute` 实例，`GetAttribute` 方法会通知用户找不到属性，并显示不包含属性的方法名称或类名称。 如果找到属性，控制台中会显示 `Name`、`Level` 和 `Reviewed` 字段。  
  
 可以使用 <xref:System.Type> 类的成员，在传递的类中获取各个方法和成员。 此示例先查询 Type 对象，以获取类一级的属性信息。 接下来，它使用 <xref:System.Type.GetMethods%2A?displayProperty=nameWithType> 将所有方法实例都放入 <xref:System.Reflection.MemberInfo?displayProperty=nameWithType> 对象数组，以检索方法一级的属性信息。 还可以使用 <xref:System.Type.GetProperties%2A?displayProperty=nameWithType> 方法检查属性一级的属性，或使用 <xref:System.Type.GetConstructors%2A?displayProperty=nameWithType>方法检查构造函数一级的属性。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Type?displayProperty=nameWithType>
- <xref:System.Attribute.GetCustomAttribute%2A?displayProperty=nameWithType>
- <xref:System.Attribute.GetCustomAttributes%2A?displayProperty=nameWithType>
- [特性](index.md)
