---
title: 如何：定义和执行动态方法
description: 了解如何在 .NET. 中定义和执行动态方法。 查看简单的动态方法和绑定到类实例的动态方法的示例。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reflection emit, dynamic methods
- dynamic methods
ms.assetid: 07d08a99-62c5-4254-bce2-2a75e55a18ab
ms.openlocfilehash: 1ea15149ea72c3c422251abefb5a1008cc496f2c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263446"
---
# <a name="how-to-define-and-execute-dynamic-methods"></a>如何：定义和执行动态方法

以下过程介绍如何定义和执行简单的动态方法和绑定到类实例的动态方法。 有关动态方法的更多信息，请参阅 <xref:System.Reflection.Emit.DynamicMethod> 类和[反射发出动态方法应用场景](/previous-versions/dotnet/netframework-4.0/sfk2s47t(v=vs.100))。  
  
### <a name="to-define-and-execute-a-dynamic-method"></a>定义和执行动态方法  
  
1. 声明用于执行方法的委托类型。 考虑使用泛型委托，将需要声明的委托类型数降到最低。 以下代码声明两种可用于 `SquareIt` 方法的委托类型，其中一个是泛型。  
  
     [!code-cpp[DynamicMethodHowTo#2](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#2)]
     [!code-csharp[DynamicMethodHowTo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#2)]
     [!code-vb[DynamicMethodHowTo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#2)]  
  
2. 创建用于为动态方法指定参数类型的数组。 在此示例中，唯一的参数为 `int`（在 Visual Basic 中为 `Integer`），所以数组只有一个元素。  
  
     [!code-cpp[DynamicMethodHowTo#3](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#3)]
     [!code-csharp[DynamicMethodHowTo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#3)]
     [!code-vb[DynamicMethodHowTo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#3)]  
  
3. 创建 <xref:System.Reflection.Emit.DynamicMethod>。 在此示例中，该方法命名为 `SquareIt`。  
  
    > [!NOTE]
    > 不需要为动态方法命名，并且不能通过名称调用它们。 多个动态方法可以具有相同的名称。 但是，名称将在调用堆栈中显示并且可用于调试。  
  
     返回值的类型指定为 `long`。 该方法与包含 `Example` 类的模块关联，该类包含代码示例。 可以指定任何加载的模块。 动态方法的行为类似于模块级的 `static` 方法（在 Visual Basic 中为 `Shared`）。  
  
     [!code-cpp[DynamicMethodHowTo#4](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#4)]
     [!code-csharp[DynamicMethodHowTo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#4)]
     [!code-vb[DynamicMethodHowTo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#4)]  
  
4. 发出方法主体。 在此示例中，使用 <xref:System.Reflection.Emit.ILGenerator> 对象发出 Microsoft 中间语言 (MSIL)。 也可以结合使用 <xref:System.Reflection.Emit.DynamicILInfo> 对象与非托管代码生成器，发出 <xref:System.Reflection.Emit.DynamicMethod> 的方法主体。  
  
     此示例中的 MSIL 将该参数（一个 `int`）加载到堆栈上，将其转换为 `long`，复制 `long`，然后将这两个数字相乘。 这会将平方结果保留在堆栈中，方法只需返回即可。  
  
     [!code-cpp[DynamicMethodHowTo#5](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#5)]
     [!code-csharp[DynamicMethodHowTo#5](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#5)]
     [!code-vb[DynamicMethodHowTo#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#5)]  
  
5. 通过调用 <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A> 方法创建表示动态方法的委托（在步骤 1 中声明）的实例。 创建委托即完成该方法，任何更改方法的进一步尝试（例如，添加更多 MSIL）都将被忽略。 以下代码使用泛型委托创建委托并调用它。  
  
     [!code-cpp[DynamicMethodHowTo#6](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#6)]
     [!code-csharp[DynamicMethodHowTo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#6)]
     [!code-vb[DynamicMethodHowTo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#6)]  
  
### <a name="to-define-and-execute-a-dynamic-method-that-is-bound-to-an-object"></a>定义和执行绑定到对象的动态方法  
  
1. 声明用于执行方法的委托类型。 考虑使用泛型委托，将需要声明的委托类型数降到最低。 以下代码声明一个泛型委托类型，可使用该类型执行任何具有一个参数和一个返回值的方法，如果委托绑定到对象，则该类型也可以执行具有两个参数和一个返回值的方法。  
  
     [!code-cpp[DynamicMethodHowTo#12](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#12)]
     [!code-csharp[DynamicMethodHowTo#12](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#12)]
     [!code-vb[DynamicMethodHowTo#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#12)]  
  
2. 创建用于为动态方法指定参数类型的数组。 如果表示方法的委托要绑定到对象，则第一个参数必须与委托绑定到的类型相匹配。 在此示例中，存在两个参数，分别属于 `Example` 和 `int`（在 Visual Basic 中为 `Integer`）类型。  
  
     [!code-cpp[DynamicMethodHowTo#13](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#13)]
     [!code-csharp[DynamicMethodHowTo#13](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#13)]
     [!code-vb[DynamicMethodHowTo#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#13)]  
  
3. 创建 <xref:System.Reflection.Emit.DynamicMethod>。 在此示例中，方法没有名称。 返回值的类型指定为 `int`（在 Visual Basic 中为 `Integer`）。 该方法可以访问 `Example` 类的私有成员和受保护成员。  
  
     [!code-cpp[DynamicMethodHowTo#14](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#14)]
     [!code-csharp[DynamicMethodHowTo#14](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#14)]
     [!code-vb[DynamicMethodHowTo#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#14)]  
  
4. 发出方法主体。 在此示例中，使用 <xref:System.Reflection.Emit.ILGenerator> 对象发出 Microsoft 中间语言 (MSIL)。 也可以结合使用 <xref:System.Reflection.Emit.DynamicILInfo> 对象与非托管代码生成器，发出 <xref:System.Reflection.Emit.DynamicMethod> 的方法主体。  
  
     此示例中的 MSIL 加载第一个参数（一个 `Example` 类的实例），然后使用该参数加载类型 `int` 的专用实例字段的值。 然后加载第二个参数，并将两个数相乘。 如果结果大于 `int`，将截断该值且丢弃最高有效位。 该方法返回，返回值保留在堆栈中。  
  
     [!code-cpp[DynamicMethodHowTo#15](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#15)]
     [!code-csharp[DynamicMethodHowTo#15](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#15)]
     [!code-vb[DynamicMethodHowTo#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#15)]  
  
5. 通过调用 <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%28System.Type%2CSystem.Object%29> 方法重载创建表示动态方法的委托（在步骤 1 中声明）的实例。 创建委托即完成该方法，任何更改方法的进一步尝试（例如，添加更多 MSIL）都将被忽略。  
  
    > [!NOTE]
    > 可以多次调用 <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A> 方法，创建绑定到目标类型的其他实例的委托。  
  
     以下代码将该方法绑定到 `Example` 类的一个新实例，该类的专用测试字段设置为 42。 也就是说，每次调用委托时，都会将 `Example` 的实例传递给该方法的第一个参数。  
  
     使用委托 `OneParameter` 的原因是该方法的第一个参数始终会接收 `Example` 的实例。 调用委托时，只需要使用第二个参数。  
  
     [!code-cpp[DynamicMethodHowTo#16](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#16)]
     [!code-csharp[DynamicMethodHowTo#16](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#16)]
     [!code-vb[DynamicMethodHowTo#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#16)]  
  
## <a name="example"></a>示例  

 以下代码示例演示一个简单的动态方法和一个绑定到类实例的动态方法。  
  
 简单动态方法具有一个 32 位整数参数，返回该整数的 64 位平方。 使用泛型委托来调用该方法。  
  
 第二个动态方法有两个参数，分别属于 `Example` 和 `int`（在 Visual Basic 中为 `Integer`）类型。 创建动态方法后，将使用具有一个 `int` 类型参数的泛型委托将其绑定到 `Example` 的实例。 该委托没有 `Example` 类型的参数，因为方法的第一个参数始终接收 `Example` 的绑定实例。 调用委托时，只提供 `int` 参数。 此动态方法访问 `Example` 类的专用字段，并返回专用字段和 `int` 参数的乘积。  
  
 该代码示例定义可用于执行方法的委托。  
  
 [!code-cpp[DynamicMethodHowTo#1](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#1)]
 [!code-csharp[DynamicMethodHowTo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#1)]
 [!code-vb[DynamicMethodHowTo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Reflection.Emit.DynamicMethod>
- [使用反射发出](/previous-versions/dotnet/netframework-4.0/3y322t50(v=vs.100))
- [反射发出动态方法应用场景](/previous-versions/dotnet/netframework-4.0/sfk2s47t(v=vs.100))
