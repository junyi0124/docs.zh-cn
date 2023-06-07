---
title: 如何：使用反射检查和实例化泛型类型
description: 了解如何使用反射检查和实例化泛型类型。 使用 IsGenericType、IsGenericParameter 和 GenericParameterPosition 属性。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reflection, generic types
- generics [.NET Framework], reflection
ms.assetid: f93b03b0-1778-43fc-bc6d-35983d210e74
ms.openlocfilehash: 34efca4a26b0ab3739d19b793237532ec9f4f15e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263433"
---
# <a name="how-to-examine-and-instantiate-generic-types-with-reflection"></a>如何：使用反射检查和实例化泛型类型

获取泛型类型信息的方式与获取其他类型信息的方式相同：检查表示泛型类型的 <xref:System.Type> 对象。 主要的差异在于，泛型类型具有表示其泛型类型参数的 <xref:System.Type> 对象列表。 本部分的第一个过程是检查泛型类型。  
  
 通过将类型实参绑定到泛型类型定义的类型形参，可以创建表示构造类型的 <xref:System.Type> 对象。 第二个过程对此进行了演示。  
  
### <a name="to-examine-a-generic-type-and-its-type-parameters"></a>检查泛型类型及其类型参数  
  
1. 获取表示泛型类型的 <xref:System.Type> 实例。 在下面的代码中，使用 C# 的 `typeof` 运算符（在 Visual Basic 中为 `GetType`，在 Visual C++ 中为 `typeid`）获取类型。 有关获取 <xref:System.Type> 对象的其他方法，请参阅 <xref:System.Type> 类主题。 请注意，在余下的过程中，类型包含在名为 `t` 的方法参数中。  
  
     [!code-cpp[HowToGeneric#2](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#2)]
     [!code-csharp[HowToGeneric#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#2)]
     [!code-vb[HowToGeneric#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#2)]  
  
2. 使用 <xref:System.Type.IsGenericType%2A> 属性确定类型是否为泛型，然后使用 <xref:System.Type.IsGenericTypeDefinition%2A> 属性确定类型是否为泛型类型定义。  
  
     [!code-cpp[HowToGeneric#3](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#3)]
     [!code-csharp[HowToGeneric#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#3)]
     [!code-vb[HowToGeneric#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#3)]  
  
3. 使用 <xref:System.Type.GetGenericArguments%2A> 方法获取包含泛型类型参数的数组。  
  
     [!code-cpp[HowToGeneric#4](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#4)]
     [!code-csharp[HowToGeneric#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#4)]
     [!code-vb[HowToGeneric#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#4)]  
  
4. 对于每个类型实参，使用 <xref:System.Type.IsGenericParameter%2A> 属性确定它是类型形参（例如，在泛型类型定义中），还是为类型形参指定的类型（例如，在构造类型中）。  
  
     [!code-cpp[HowToGeneric#5](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#5)]
     [!code-csharp[HowToGeneric#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#5)]
     [!code-vb[HowToGeneric#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#5)]  
  
5. 在类型系统中，和普通类型一样，泛型类型参数由 <xref:System.Type> 实例表示。 以下代码显示表示泛型类型参数的 <xref:System.Type> 对象的名称和参数位置。 此处，参数位置无足轻重；它在用户检查已用作其他泛型类型的类型实参的类型形参时更有价值。  
  
     [!code-cpp[HowToGeneric#6](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#6)]
     [!code-csharp[HowToGeneric#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#6)]
     [!code-vb[HowToGeneric#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#6)]  
  
6. 通过使用 <xref:System.Type.GetGenericParameterConstraints%2A> 方法获取单个数组中的所有约束，确定泛型类型参数的基类型约束和接口约束。 不保证约束以任何特定顺序排列。  
  
     [!code-cpp[HowToGeneric#7](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#7)]
     [!code-csharp[HowToGeneric#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#7)]
     [!code-vb[HowToGeneric#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#7)]  
  
7. 使用 <xref:System.Type.GenericParameterAttributes%2A> 属性发现类型参数的特殊约束，如要求其作为引用类型。 该属性还包含表示方差的值，该值可屏蔽，如下面的代码所示。  
  
     [!code-cpp[HowToGeneric#8](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#8)]
     [!code-csharp[HowToGeneric#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#8)]
     [!code-vb[HowToGeneric#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#8)]  
  
8. 特殊约束属性为标志，表示没有任何特殊约束的相同标志 (<xref:System.Reflection.GenericParameterAttributes.None?displayProperty=nameWithType>) 还表示没有协变或逆变。 因此，若要测试其中一项条件，则必须使用适当的掩码。 在这种情况下，请使用 <xref:System.Reflection.GenericParameterAttributes.SpecialConstraintMask?displayProperty=nameWithType> 隔离特殊约束标志。  
  
     [!code-cpp[HowToGeneric#9](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#9)]
     [!code-csharp[HowToGeneric#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#9)]
     [!code-vb[HowToGeneric#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#9)]  
  
## <a name="constructing-an-instance-of-a-generic-type"></a>构造泛型类型的实例  

 泛型类型和模板类似。 除非指定其泛型类型参数的实际类型，否则不能创建泛型类型的实例。 若要在运行时执行此操作，则需要使用 <xref:System.Type.MakeGenericType%2A> 方法。  
  
#### <a name="to-construct-an-instance-of-a-generic-type"></a>构造泛型类型的实例  
  
1. 获取表示泛型类型的 <xref:System.Type> 对象。 以下代码以两种不同方式获取泛型类型 <xref:System.Collections.Generic.Dictionary%602>：一种是使用 <xref:System.Type.GetType%28System.String%29?displayProperty=nameWithType> 方法重载和描述类型的字符串，另一种是调用构造类型 `Dictionary\<String, Example>`（在 Visual Basic 中为 `Dictionary(Of String, Example)`）的 <xref:System.Type.GetGenericTypeDefinition%2A> 方法。 <xref:System.Type.MakeGenericType%2A> 方法需要泛型类型定义。  
  
     [!code-cpp[HowToGeneric#10](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#10)]
     [!code-csharp[HowToGeneric#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#10)]
     [!code-vb[HowToGeneric#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#10)]  
  
2. 构造用于替代类型形参的类型实参数组。 该数组必须包含正确数目的 <xref:System.Type> 对象，并且顺序和它们在类型形参列表中的顺序相同。 在这种情况下，键（第一个类型形参）的类型为 <xref:System.String>，字典中的值是名为 `Example` 的类的实例。  
  
     [!code-cpp[HowToGeneric#11](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#11)]
     [!code-csharp[HowToGeneric#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#11)]
     [!code-vb[HowToGeneric#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#11)]  
  
3. 调用 <xref:System.Type.MakeGenericType%2A> 方法，将类型实参绑定到类型形参，然后构造类型。  
  
     [!code-cpp[HowToGeneric#12](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#12)]
     [!code-csharp[HowToGeneric#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#12)]
     [!code-vb[HowToGeneric#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#12)]  
  
4. 使用 <xref:System.Activator.CreateInstance%28System.Type%29> 方法重载来创建构造类型的对象。 以下代码在生成的 `Dictionary<String, Example>` 对象中存储 `Example` 类的两个实例。  
  
     [!code-cpp[HowToGeneric#13](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#13)]
     [!code-csharp[HowToGeneric#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#13)]
     [!code-vb[HowToGeneric#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#13)]  
  
## <a name="example"></a>示例  

 下面的代码示例定义 `DisplayGenericType` 方法来检查代码中使用的泛型类型定义和构造类型，并显示它们的信息。 `DisplayGenericType` 方法演示如何使用 <xref:System.Type.IsGenericType%2A>、<xref:System.Type.IsGenericParameter%2A> 和 <xref:System.Type.GenericParameterPosition%2A> 属性以及 <xref:System.Type.GetGenericArguments%2A> 方法。  
  
 该示例还定义 `DisplayGenericParameter` 方法来检查泛型类型参数，并显示其约束。  
  
 代码示例定义一组测试类型，包括说明类型参数约束的泛型类型，并演示如何显示这些类型的信息。  
  
 示例通过创建类型参数数组并调用 <xref:System.Type.MakeGenericType%2A> 方法，从 <xref:System.Collections.Generic.Dictionary%602> 类构造类型。 程序将使用 <xref:System.Type.MakeGenericType%2A> 构造的 <xref:System.Type> 对象和使用 `typeof`（在 Visual Basic 中为 `GetType`）获取的 <xref:System.Type> 对象进行比较，演示这两个对象是相同的。 类似地，程序使用 <xref:System.Type.GetGenericTypeDefinition%2A> 方法获取构造类型的泛型类型定义，并将其与表示 <xref:System.Collections.Generic.Dictionary%602> 类的 <xref:System.Type> 对象进行比较。  
  
 [!code-cpp[HowToGeneric#1](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#1)]
 [!code-csharp[HowToGeneric#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#1)]
 [!code-vb[HowToGeneric#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#1)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Type>
- <xref:System.Reflection.MethodInfo>
- [反射类型和泛型类型](reflection-and-generic-types.md)
- [查看类型信息](viewing-type-information.md)
- [泛型](../../standard/generics/index.md)
