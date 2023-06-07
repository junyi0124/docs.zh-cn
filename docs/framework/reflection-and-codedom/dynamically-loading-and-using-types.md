---
title: 动态加载和使用类型
description: .NET 中的动态加载和使用类型。 使用反射提供语言编译器为实现隐式后期绑定而使用的基础结构。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- late binding, about late binding
- early binding
- dynamically loading and using types
- implicit late binding
- reflection, dynamically using types
ms.assetid: db985bec-5942-40ec-b13a-771ae98623dc
ms.openlocfilehash: be9991a4df866f65aabe063be3cc2b374f4d124d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266787"
---
# <a name="dynamically-loading-and-using-types"></a>动态加载和使用类型

反射提供语言编译器为实现隐式后期绑定而使用的基础结构。 声明与唯一指定的类型相对应，绑定是查找声明（即实现）的过程。 运行时（而非编译时）发生此进程就称为后期绑定。 Visual Basic 允许在代码中使用隐式后期绑定；Visual Basic 编译器会调用使用反射来获取对象类型的帮助程序方法。 传递给帮助程序方法的参数会导致在运行时调用相应方法。 这些参数是在其上调用方法的实例（对象）、被调用方法的名称（字符串）和传递给被调用方法的参数（对象数组）。  
  
 在以下示例中，Visual Basic 编译器以隐式方式使用反射对某个对象调用一种方法，该对象的类型在编译时未知。 HelloWorld 类具有 PrintHello 方法，该方法可以打印输出“Hello World”，且该文本与传递给 PrintHello 方法的一些文本相关联    。 本示例中调用的 PrintHello 方法实际上是 <xref:System.Type.InvokeMember%2A?displayProperty=nameWithType>；如果在编译时（早期绑定）而不是在运行时（后期绑定）知道对象的类型 (helloObj)，则 Visual Basic 代码允许调用 PrintHello 方法   。  
  
```vb
Module Hello  
    Sub Main()  
        ' Sets up the variable.  
        Dim helloObj As Object  
        ' Creates the object.  
        helloObj = new HelloWorld()  
        ' Invokes the print method as if it was early bound  
        ' even though it is really late bound.  
        helloObj.PrintHello("Visual Basic Late Bound")  
    End Sub  
End Module  
```  
  
## <a name="custom-binding"></a>自定义绑定  

 除了被编译器隐式用于后期绑定之外，反射还可在代码中显式用于完成后期绑定。  
  
 [公共语言运行时](../../standard/clr.md)支持多种编程语言，而且这些语言的绑定规则各不相同。 在早期绑定的情况下，代码生成器可以完全控制此绑定。 但是在通过反射实现的后期绑定中，必须由自定义绑定来控制该绑定。 <xref:System.Reflection.Binder> 类提供对成员选择和调用的自定义控制。  
  
 使用自定义绑定可以在运行时加载程序集、获取该程序集中有关类型的信息、指定所需类型，并在之后调用该类型上的方法或访问该类型上的字段或属性。 如果在编译时不知道对象的类型，则此方法非常有用，例如当对象类型取决于用户输入时。  
  
 下面的示例演示了一个简单的自定义联编程序，其中不提供参数类型转换。 `Simple_Type.dll` 的代码位于示例主体之前。 务必生成 `Simple_Type.dll`，然后运行时在项目中包含对它的引用。  
  
 [!code-cpp[Conceptual.Types.Dynamic#1](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.dynamic/cpp/source1.cpp#1)]
 [!code-csharp[Conceptual.Types.Dynamic#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.dynamic/cs/source1.cs#1)]
 [!code-vb[Conceptual.Types.Dynamic#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.dynamic/vb/source1.vb#1)]  
  
### <a name="invokemember-and-createinstance"></a>InvokeMember 和 CreateInstance  

 使用 <xref:System.Type.InvokeMember%2A?displayProperty=nameWithType> 调用类型的成员。 InvokeMember 可以创建指定类型的新实例，而各种类的 CreateInstance 方法（例如 <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> 和 <xref:System.Reflection.Assembly.CreateInstance%2A?displayProperty=nameWithType>）是 InvokeMember 的专用形式   。 Binder 类可在这些方法中用于重载解析和参数强制转换  。  
  
 下面的示例演示了参数强制转换（类型转换）和成员选择的三种可能的组合方式。 在第 1 个例子中，无需进行参数强制转换或成员选择。 在第 2 个例子中，只需进行成员选择。 在第 3 种情况下，只需进行参数强制转换。  
  
 [!code-cpp[Conceptual.Types.Dynamic#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.dynamic/cpp/source2.cpp#2)]
 [!code-csharp[Conceptual.Types.Dynamic#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.dynamic/cs/source2.cs#2)]
 [!code-vb[Conceptual.Types.Dynamic#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.dynamic/vb/source2.vb#2)]  
  
 当有多个具有相同名称的成员可用时，需进行重载解析。 <xref:System.Reflection.Binder.BindToMethod%2A?displayProperty=nameWithType> 和 <xref:System.Reflection.Binder.BindToField%2A?displayProperty=nameWithType> 方法可用于解析对单个成员的绑定。 Binder.BindToMethod 还通过 get 和 set 属性访问器提供属性解析    。  
  
 BindToMethod 返回 <xref:System.Reflection.MethodBase> 供调用，如果不可能发生此类调用，则返回空引用（在 Visual Basic 中则返回 Nothing）   。 MethodBase 返回值不必是 match 参数中包含的值之一，虽然通常情况下都是这样   。  
  
 存在 ByRef 参数时，调用方可能想将其恢复。 因此，如果 BindToMethod 已操作参数数组，联编程序允许客户端将参数数组映射回其原始形式   。 为此，调用方必须确保参数顺序不变。 按名称传递参数时，联编程序会对参数数组重新排序，该顺序即调用方所见顺序  。 有关详细信息，请参阅 <xref:System.Reflection.Binder.ReorderArgumentArray%2A?displayProperty=nameWithType>。  
  
 可用成员集是在类型或任何基类型中定义的成员。 如果指定了 <xref:System.Reflection.BindingFlags>，则会将所有可访问的成员返回到该集中。 如果未指定 BindingFlags.NonPublic，则联编程序必须强制实施可访问性规则  。 指定公共或非公共绑定标志时，必须同时指定实例或静态绑定标志，否则不会返回任何成员     。  
  
 如果给定名称只有一个成员，则无需回调，已在该方法上完成绑定。 第 1 个代码示例演示了这种情况：只有一种 PrintBob 方法可用，因此无需回调  。  
  
 如果可用集中存在多个成员，则所有方法都会传递给 BindToMethod，让它来选择适当的方法并返回结果  。 第 2 个代码示例中，有两个名为 PrintValue 的方法  。 通过调用 BindToMethod 选择适当的方法  。  
  
 <xref:System.Reflection.Binder.ChangeType%2A> 执行参数强制转换（类型转换），将实参转换为所选方法的形参类型。 会为每个参数调用 ChangeType，即使类型完全匹配  。  
  
 第 3 个代码示例中，将值为“5.5”的字符串类型的实参传递给了具有 Double 类型形参的方法   。 字符串值“5.5”必须转换成 double 值，调用才能成功。 ChangeType 执行此转换  。  
  
 ChangeType 只执行无损转换或[扩大强制转换](../../standard/base-types/type-conversion.md)，如下表所示  。  
  
|源类型|目标类型|  
|-----------------|-----------------|  
|任何类型|其基类型|  
|任何类型|实现的接口|  
|Char|UInt16、UInt32、Int32、UInt64、Int64、Single、Double|  
|Byte|Char、UInt16、Int16、UInt32、Int32、UInt64、Int64、Single、Double|  
|SByte|Int16、Int32、Int64、Single、Double|  
|UInt16|UInt32、Int32、UInt64、Int64、Single、Double|  
|Int16|Int32、Int64、Single、Double|  
|UInt32|UInt64、Int64、Single、Double|  
|Int32|Int64、Single、Double|  
|UInt64|Single、Double|  
|Int64|Single、Double|  
|Single|Double|  
|非引用类型|引用类型|  
  
 <xref:System.Type> 类具有 Get 方法，该方法使用联编程序类型的参数来解析对特定成员的引用   。 <xref:System.Type.GetConstructor%2A?displayProperty=nameWithType>、<xref:System.Type.GetMethod%2A?displayProperty=nameWithType> 和 <xref:System.Type.GetProperty%2A?displayProperty=nameWithType> 通过提供成员的签名信息来搜索当前类型的特定成员。 回调 <xref:System.Reflection.Binder.SelectMethod%2A?displayProperty=nameWithType> 和 <xref:System.Reflection.Binder.SelectProperty%2A?displayProperty=nameWithType> 以选择适当方法的给定签名信息。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Type.InvokeMember%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>
- [查看类型信息](viewing-type-information.md)
- [.NET Framework 中的类型转换](../../standard/base-types/type-conversion.md)
