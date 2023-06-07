---
title: 泛型和反射 - C# 编程指南
description: 了解如何使用反射获取有关泛型类型的信息。 查看泛型反射的条款和条件的列表。
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], reflection
- reflection [C#], generic types
ms.assetid: 162fd9b4-dd5b-4abb-8c9b-e44e21e2f451
ms.openlocfilehash: 5439412231ab1bf9ed523d6786af67984ab2d0c3
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172965"
---
# <a name="generics-and-reflection-c-programming-guide"></a>泛型和反射（C# 编程指南）

因为公共语言运行时 (CLR) 能够在运行时访问泛型类型信息，所以可以使用反射获取关于泛型类型的信息，方法与用于非泛型类型的方法相同。 有关详细信息，请参阅[运行时中的泛型](./generics-in-the-run-time.md)。  
  
 在 .NET Framework 2.0 中，向 <xref:System.Type> 类添加了多个新成员来启用泛型类型的运行时信息。 有关如何使用这些方法和属性的详细信息，请参阅这些类的文档。 <xref:System.Reflection.Emit> 命名空间还包含支持泛型的新成员。 请参阅[如何：使用反射发出定义泛型类型](../../../framework/reflection-and-codedom/how-to-define-a-generic-type-with-reflection-emit.md)。  
  
 有关泛型反射中使用的术语的固定条件列表，请参阅 <xref:System.Type.IsGenericType%2A> 属性注解。  
  
|System.Type 成员名称|描述|  
|-----------------------------|-----------------|  
|<xref:System.Type.IsGenericType%2A>|如果类型是泛型，则返回 true。|  
|<xref:System.Type.GetGenericArguments%2A>|返回 `Type` 对象的数组，这些对象表示为构造类型提供的类型实参或泛型类型定义的类型形参。|  
|<xref:System.Type.GetGenericTypeDefinition%2A>|返回当前构造类型的基础泛型类型定义。|  
|<xref:System.Type.GetGenericParameterConstraints%2A>|返回表示当前泛型类型参数约束的 `Type` 对象的数组。|  
|<xref:System.Type.ContainsGenericParameters%2A>|如果类型或任何其封闭类型或方法包含未提供特定类型的类型参数，则返回 true。|  
|<xref:System.Type.GenericParameterAttributes%2A>|获取描述当前泛型类型参数的特殊约束的 `GenericParameterAttributes` 标志组合。|  
|<xref:System.Type.GenericParameterPosition%2A>|对于表示类型参数的 `Type` 对象，获取类型参数在声明其类型参数的泛型类型定义或泛型方法定义的类型参数列表中的位置。|  
|<xref:System.Type.IsGenericParameter%2A>|获取一个值，该值指示当前 `Type` 是否表示泛型类型或方法定义中的类型参数。|  
|<xref:System.Type.IsGenericTypeDefinition%2A>|获取一个值，该值指示当前 <xref:System.Type> 是否表示可以用来构造其他泛型类型的泛型类型定义。 如果该类型表示泛型类型的定义，则返回 true。|  
|<xref:System.Type.DeclaringMethod%2A>|返回定义当前泛型类型参数的泛型方法，如果类型参数未由泛型方法定义，则返回 null。|  
|<xref:System.Type.MakeGenericType%2A>|替代由当前泛型类型定义的类型参数组成的类型数组的元素，并返回表示结果构造类型的 <xref:System.Type> 对象。|  
  
 此外，<xref:System.Reflection.MethodInfo> 类的成员还为泛型方法启用运行时信息。 有关用于反射泛型方法的术语的固定条件列表，请参阅 <xref:System.Reflection.MethodBase.IsGenericMethod%2A> 属性注解。  
  
|System.Reflection.MemberInfo 成员名称|描述|  
|----------------------------------------------|-----------------|  
|<xref:System.Reflection.MethodBase.IsGenericMethod%2A>|如果方法是泛型，则返回 true。|  
|<xref:System.Reflection.MethodInfo.GetGenericArguments%2A>|返回类型对象的数组，这些对象表示构造泛型方法的类型实参或泛型方法定义的类型形参。|  
|<xref:System.Reflection.MethodInfo.GetGenericMethodDefinition%2A>|返回当前构造方法的基础泛型方法定义。|  
|<xref:System.Reflection.MethodBase.ContainsGenericParameters%2A>|如果方法或任何其封闭类型包含未提供特定类型的任何类型参数，则返回 true。|  
|<xref:System.Reflection.MethodBase.IsGenericMethodDefinition%2A>|如果当前 <xref:System.Reflection.MethodInfo> 表示泛型方法的定义，则返回 true。|  
|<xref:System.Reflection.MethodInfo.MakeGenericMethod%2A>|用类型数组的元素替代当前泛型方法定义的类型参数，并返回表示结果构造方法的 <xref:System.Reflection.MethodInfo> 对象。|  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [泛型](./index.md)
- [反射类型和泛型类型](../../../framework/reflection-and-codedom/reflection-and-generic-types.md)
- [泛型](../../../standard/generics/index.md)
