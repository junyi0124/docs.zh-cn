---
title: 如何：将程序集加载到仅反射上下文中
description: 查看有关如何在 .NET 中将程序集加载到仅反射上下文中的示例。 检查为其他平台或 .NET 版本编译的程序集。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reflection, reflection-only loader context
- assemblies [.NET Framework], loading for reflection
- attributes [.NET Framework], retrieving
- assemblies [.NET Framework], reflection-only loader context
- reflection-only loader context
ms.assetid: 9818b660-52f5-423d-a9af-e75163aa7068
ms.openlocfilehash: 92f847f6c61ba39bf8621af6080baccfdabe514a
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865068"
---
# <a name="how-to-load-assemblies-into-the-reflection-only-context"></a>如何：将程序集加载到仅反射上下文中

通过仅反射加载上下文，可检查为其他平台或 .NET Framework 的其他版本编译的程序集。 只能检查，不能执行加载到此上下文中的代码。 这意味着无法创建对象，因为无法执行构造函数。 因为该代码无法执行，所以不会自动加载依赖项。 如果需要对依赖项进行检查，必须自行加载。

## <a name="to-load-an-assembly-into-the-reflection-only-load-context"></a>将程序集加载到仅反射加载上下文中

1. 使用 <xref:System.Reflection.Assembly.ReflectionOnlyLoad%28System.String%29> 方法重载可加载给定了显示名称的程序集，而使用 <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A> 方法可加载给定了路径的程序集。 如果该程序集为二进制文件映像，则使用 <xref:System.Reflection.Assembly.ReflectionOnlyLoad%28System.Byte%5B%5D%29> 方法重载。

    > [!NOTE]
    > 不能使用仅反射上下文从非执行上下文中的 .NET Framework 版本加载 mscorlib.dll 版本。

2. 如果该程序集具有依赖项，<xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> 方法不会加载这些依赖项。 如果需要对依赖项进行检查，必须自行加载。

3. 使用程序集的 <xref:System.Reflection.Assembly.ReflectionOnly%2A> 属性确定是否已将程序集加载到仅反射上下文中。

4. 如果向程序集或程序集中的类型应用了特性，则应使用 <xref:System.Reflection.CustomAttributeData> 类检查这些特性，确保未尝试在仅反射上下文中执行代码。 使用 <xref:System.Reflection.CustomAttributeData.GetCustomAttributes%2A?displayProperty=nameWithType> 方法的适当重载获取表示应用于程序集、成员、模块或参数的特性的 <xref:System.Reflection.CustomAttributeData> 对象。

    > [!NOTE]
    > 应用于程序集或其内容的特性可能是在该程序集中定义的，也可能是在加载到仅反射上下文中的另一个程序集中定义的。 无法事先知悉这些特性是在何处定义的。

## <a name="example"></a>示例

以下代码示例演示如何检查应用于加载到仅反射上下文中的程序集的特性。

本代码示例定义带有两个构造函数和一个属性的自定义特性。 该特性可应用于程序集、在该程序集中声明的类型、该类型的方法以及该方法的参数。 执行时，该程序集将其本身加载到仅反射上下文中，并显示应用到程序集及其所含类型和成员的自定义特性的相关信息。

> [!NOTE]
> 为简化本代码示例，该程序集自行完成加载和检查操作。 通常情况下，不要在执行上下文和仅反射上下文中加载同一程序集。

[!code-cpp[CustomAttributeData#1](../../../samples/snippets/cpp/VS_Snippets_CLR/CustomAttributeData/CPP/source.cpp#1)]
[!code-csharp[CustomAttributeData#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CustomAttributeData/CS/source.cs#1)]
[!code-vb[CustomAttributeData#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CustomAttributeData/VB/source.vb#1)]

## <a name="see-also"></a>请参阅

- <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A>
- <xref:System.Reflection.Assembly.ReflectionOnly%2A>
- <xref:System.Reflection.CustomAttributeData>
