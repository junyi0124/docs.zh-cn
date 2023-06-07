---
title: 查看类型信息
description: 使用 System.Type 查看类型信息，这是 .NET 中反射的中心。 查看 ConstructorInfo、MemberInfo、MethodInfo、FieldInfo 和 PropertyInfo。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- types, viewing type information
- Type object
- viewing type information
- reflection, viewing type information
ms.assetid: 7e7303a9-4064-4738-b4e7-b75974ed70d2
ms.openlocfilehash: 3baacbeca7f5cc50fbb720849aec273f996f86e7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96259136"
---
# <a name="viewing-type-information"></a>查看类型信息

<xref:System.Type?displayProperty=nameWithType> 类是反射的中心。 当反射提出请求时，公共语言运行时为已加载的类型创建 Type  。 可使用 Type  对象的方法、字段、属性和嵌套类来查找该类型的任何信息。  
  
 使用 <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> 或 <xref:System.Reflection.Assembly.GetTypes%2A?displayProperty=nameWithType> 从尚未加载的程序集中获取 Type  对象，传入所需类型的名称。 使用 <xref:System.Type.GetType%2A?displayProperty=nameWithType> 从已加载的程序集中获取 Type 对象  。 使用 <xref:System.Reflection.Module.GetType%2A?displayProperty=nameWithType> 和 <xref:System.Reflection.Module.GetTypes%2A?displayProperty=nameWithType> 获取模块 Type 对象  。  
  
> [!NOTE]
> 要检查和操纵泛型类型及方法，请参阅[反射类型和泛型类型](reflection-and-generic-types.md)和[如何：使用反射检查和实例化泛型类型](how-to-examine-and-instantiate-generic-types-with-reflection.md)中提供的其他信息。  
  
 下列示例演示获取程序集的 <xref:System.Reflection.Assembly> 对象和模块所必需的语法。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source5.cpp#6)]
 [!code-csharp[Conceptual.Types.ViewInfo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source5.cs#6)]
 [!code-vb[Conceptual.Types.ViewInfo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source5.vb#6)]  
  
 下列示例演示如何从已加载的程序集获取 Type 对象  。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source5.cpp#7)]
 [!code-csharp[Conceptual.Types.ViewInfo#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source5.cs#7)]
 [!code-vb[Conceptual.Types.ViewInfo#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source5.vb#7)]  
  
 一旦获取 Type 对象，可通过多种方式来查看该类型成员的相关信息  。 例如，可调用 <xref:System.Type.GetMembers%2A?displayProperty=nameWithType> 方法查找所有类型的成员，获取一组描述当前类型各成员的 <xref:System.Reflection.MemberInfo> 对象。  
  
 还可以使用 Type 类上的方法来检索按名称指定的一个或多个构造函数、方法、事件、字段或属性的相关信息  。 例如，<xref:System.Type.GetConstructor%2A?displayProperty=nameWithType> 封装当前类的特定构造函数。  
  
 如果有 Type，可使用 <xref:System.Type.Module%2A?displayProperty=nameWithType> 属性获取一个封装含该类型的模块的对象  。 使用 <xref:System.Reflection.Module.Assembly%2A?displayProperty=nameWithType> 属性查找一个封装含该模块的程序集的对象。 可以获取直接使用 <xref:System.Type.Assembly%2A?displayProperty=nameWithType> 属性封装类型的程序集。  
  
## <a name="systemtype-and-constructorinfo"></a>System.Type 和 ConstructorInfo  

 下列示例演示如何列出类的构造函数，在本例中即指 <xref:System.String> 类。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#1](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source1.cpp#1)]
 [!code-csharp[Conceptual.Types.ViewInfo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source1.cs#1)]
 [!code-vb[Conceptual.Types.ViewInfo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source1.vb#1)]  
  
## <a name="memberinfo-methodinfo-fieldinfo-and-propertyinfo"></a>MemberInfo、MethodInfo、FieldInfo 和 PropertyInfo  

 使用 <xref:System.Reflection.MemberInfo>、<xref:System.Reflection.MethodInfo>、<xref:System.Reflection.FieldInfo> 或 <xref:System.Reflection.PropertyInfo> 对象获取类型的方法、属性、事件和字段的相关信息。  
  
 下列示例使用 MemberInfo 列出 System.IO.File 类中成员的数量，并使用 <xref:System.Type.IsPublic%2A> 属性确定类的可见性   。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source2.cpp#2)]
 [!code-csharp[Conceptual.Types.ViewInfo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source2.cs#2)]
 [!code-vb[Conceptual.Types.ViewInfo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source2.vb#2)]  
  
 下列示例调查指定成员的类型。 它在 MemberInfo 类的成员上执行反射，并列出该成员的类型  。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source3.cpp#3)]
 [!code-csharp[Conceptual.Types.ViewInfo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source3.cs#3)]
 [!code-vb[Conceptual.Types.ViewInfo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source3.vb#3)]  
  
 下列示例使用所有反射 \*Info 类和 <xref:System.Reflection.BindingFlags> 来列出指定类的全体成员（构造函数、字段、属性、事件和方法），并将成员区分为静态和实例类别  。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source4.cpp#4)]
 [!code-csharp[Conceptual.Types.ViewInfo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source4.cs#4)]
 [!code-vb[Conceptual.Types.ViewInfo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source4.vb#4)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Reflection.BindingFlags>
- <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.GetTypes%2A?displayProperty=nameWithType>
- <xref:System.Type.GetType%2A?displayProperty=nameWithType>
- <xref:System.Type.GetMembers%2A?displayProperty=nameWithType>
- <xref:System.Type.GetFields%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Module.GetType%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Module.GetTypes%2A?displayProperty=nameWithType>
- <xref:System.Reflection.MemberInfo>
- <xref:System.Reflection.ConstructorInfo>
- <xref:System.Reflection.MethodInfo>
- <xref:System.Reflection.FieldInfo>
- <xref:System.Reflection.EventInfo>
- <xref:System.Reflection.ParameterInfo>
- [反射类型和泛型类型](reflection-and-generic-types.md)
