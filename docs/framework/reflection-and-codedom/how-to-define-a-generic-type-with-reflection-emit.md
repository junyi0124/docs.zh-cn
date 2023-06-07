---
title: 如何：使用反射发出定义泛型类型
description: 了解如何使用反射发出定义泛型类型。 创建具有两种类型参数的泛型类型、应用类约束、接口约束等。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generics [.NET Framework], reflection emit
- generics [.NET Framework], dynamic types
- reflection emit, generic types
ms.assetid: 07d5f01a-7b5b-40ea-9b15-f21561098fe4
ms.openlocfilehash: 75076eb9ce1b9bfc6b3c8b5a48e200ca5e63cdff
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263459"
---
# <a name="how-to-define-a-generic-type-with-reflection-emit"></a>如何：使用反射发出定义泛型类型

此主题说明如何创建具有两个参数的简单泛型类型、如何对类型参数应用类约束、接口约束和特殊约束，以及如何创建使用类的类型参数作为参数类型和返回类型的成员。  
  
> [!IMPORTANT]
> 某方法只要属于泛型类型，且使用该类型的类型参数，就不是泛型方法。 只有当方法有属于自己的类型参数列表时才是泛型方法。 多数泛型类型上的方法都不是泛型方法，如本示例所示。 有关发出泛型方法的示例，请参阅[如何：使用反射发出定义泛型方法](how-to-define-a-generic-method-with-reflection-emit.md)。  
  
### <a name="to-define-a-generic-type"></a>定义泛型类型  
  
1. 定义名为 `GenericEmitExample1` 的动态程序集。 在此示例中，执行了程序集并将其保存到磁盘中，所以指定了 <xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndSave?displayProperty=nameWithType>。  
  
     [!code-cpp[EmitGenericType#2](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#2)]
     [!code-csharp[EmitGenericType#2](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#2)]
     [!code-vb[EmitGenericType#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#2)]  
  
2. 定义动态模块。 程序集由可执行模块组成。 对于单模块程序集，模块名称与程序集名称相同，文件名为模块名称加上扩展名。  
  
     [!code-cpp[EmitGenericType#3](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#3)]
     [!code-csharp[EmitGenericType#3](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#3)]
     [!code-vb[EmitGenericType#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#3)]  
  
3. 定义类。 在此示例中，类命名为 `Sample`。  
  
     [!code-cpp[EmitGenericType#4](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#4)]
     [!code-csharp[EmitGenericType#4](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#4)]
     [!code-vb[EmitGenericType#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#4)]  
  
4. 通过将包含参数名称的字符串数组传递给 <xref:System.Reflection.Emit.TypeBuilder.DefineGenericParameters%2A?displayProperty=nameWithType> 方法来定义 `Sample` 的泛型类型参数。 这将使该类成为泛型类型。 返回值是表示类型参数的 <xref:System.Reflection.Emit.GenericTypeParameterBuilder> 对象数组，可用于发出的代码。  
  
     在以下代码中，`Sample` 将成为具有类型参数 `TFirst` 和 `TSecond` 的泛型类型。 为了使代码更易于阅读，每个 <xref:System.Reflection.Emit.GenericTypeParameterBuilder> 都将放置在与类型参数同名的变量中。  
  
     [!code-cpp[EmitGenericType#5](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#5)]
     [!code-csharp[EmitGenericType#5](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#5)]
     [!code-vb[EmitGenericType#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#5)]  
  
5. 向类型参数添加特殊约束。 在此示例中，类型参数 `TFirst` 被限制为具有无参数构造函数的类型以及引用类型。  
  
     [!code-cpp[EmitGenericType#6](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#6)]
     [!code-csharp[EmitGenericType#6](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#6)]
     [!code-vb[EmitGenericType#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#6)]  
  
6. 可以选择向类型参数添加类约束和接口约束。 在此示例中，类型参数 `TFirst` 被限制为从 `baseType` 变量包含的 <xref:System.Type> 对象所表示的基类中派生的类型，以及实现接口类型包含在变量 `interfaceA` 和 `interfaceB` 中的接口的类型。 有关这些变量的声明和分配，请参阅代码示例。  
  
     [!code-cpp[EmitGenericType#7](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#7)]
     [!code-csharp[EmitGenericType#7](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#7)]
     [!code-vb[EmitGenericType#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#7)]  
  
7. 定义字段。 在此示例中，该字段的类型由类型参数 `TFirst` 指定。 <xref:System.Reflection.Emit.GenericTypeParameterBuilder> 派生自 <xref:System.Type>，因此可在任何能够使用类型的位置使用泛型类型参数。  
  
     [!code-cpp[EmitGenericType#21](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#21)]
     [!code-csharp[EmitGenericType#21](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#21)]
     [!code-vb[EmitGenericType#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#21)]  
  
8. 定义使用泛型类型的类型参数的方法。 请注意，此类方法不是泛型方法，除非它们具有自己的类型参数列表。 以下代码定义一个 `static` 方法（在 Visual Basic 中为 `Shared`），该方法接收一个 `TFirst` 数组并返回包含该数组所有元素的 `List<TFirst>`（在 Visual Basic 中为 `List(Of TFirst)`）。 若要定义此方法，需要通过调用泛型类型定义 `List<T>` 上的 <xref:System.Type.MakeGenericType%2A> 来创建类型 `List<TFirst>`。 （使用 `typeof` 运算符（在 Visual Basic 中为 `GetType`）获取泛型类型定义时，将忽略 `T`。）通过使用 <xref:System.Type.MakeArrayType%2A> 方法创建参数类型。  
  
     [!code-cpp[EmitGenericType#22](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#22)]
     [!code-csharp[EmitGenericType#22](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#22)]
     [!code-vb[EmitGenericType#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#22)]  
  
9. 发出方法主体。 方法主体由三个操作码组成，这些操作码将输入数组加载到堆栈，调用接收 `IEnumerable<TFirst>` 的 `List<TFirst>` 构造函数（该函数完成将输入元素放入列表的所有工作）并返回（将新的 <xref:System.Collections.Generic.List%601> 对象留在堆栈中）。 发出此代码的难点在于获取构造函数。  
  
     <xref:System.Reflection.Emit.GenericTypeParameterBuilder> 中不支持 <xref:System.Type.GetConstructor%2A> 方法，所以不可能直接获取 `List<TFirst>` 的构造函数。 首先，必须获取泛型类型定义 `List<T>` 的构造函数，然后调用一种可将其转换为 `List<TFirst>` 的对应构造函数的方法。  
  
     用于此代码示例的构造函数接收 `IEnumerable<T>`。 但请注意，这不是 <xref:System.Collections.Generic.IEnumerable%601> 泛型接口的泛型类型定义；相反，必须将 `List<T>` 的类型参数 `T` 替代为 `IEnumerable<T>` 的类型参数 `T`。 （这似乎容易混淆，因为这两个类型都具有名为 `T` 的类型参数。 这就是此代码示例使用名称 `TFirst` 和 `TSecond` 的原因。）若要获取构造函数的参数类型，请从泛型类型定义 `IEnumerable<T>` 开始并使用 `List<T>` 的第一个泛型类型参数调用 <xref:System.Type.MakeGenericType%2A>。 构造函数参数列表必须作为数组进行传递，在本例中只有一个参数。  
  
    > [!NOTE]
    > 在 C# 中使用 `typeof` 运算符时，泛型类型定义将表达为 `IEnumerable<>`；在 Visual Basic 中使用 `GetType` 运算符时，泛型类型定义将表达为 `IEnumerable(Of )`。  
  
     现在，可以通过调用泛型类型定义上的 <xref:System.Type.GetConstructor%2A> 来获取 `List<T>` 的构造函数。 若要将此构造函数转换为 `List<TFirst>` 对应的构造函数，请将 `List<TFirst>` 和 `List<T>` 的构造函数传递给静态 <xref:System.Reflection.Emit.TypeBuilder.GetConstructor%28System.Type%2CSystem.Reflection.ConstructorInfo%29?displayProperty=nameWithType> 方法。  
  
     [!code-cpp[EmitGenericType#23](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#23)]
     [!code-csharp[EmitGenericType#23](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#23)]
     [!code-vb[EmitGenericType#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#23)]  
  
10. 创建类型，并保存文件。  
  
     [!code-cpp[EmitGenericType#8](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#8)]
     [!code-csharp[EmitGenericType#8](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#8)]
     [!code-vb[EmitGenericType#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#8)]  
  
11. 调用方法。 `ExampleMethod` 不是泛型，但它所属的类型为泛型，因此，为了获取可以调用的 <xref:System.Reflection.MethodInfo>，需要从 `Sample` 的类型定义创建一个构造类型。 该构造类型使用 `Example` 类和 `ExampleDerived` 类，前者满足 `TFirst` 上的约束，因为它是引用类型并且具有默认的无参数构造函数，后者满足 `TSecond` 上的约束。 （示例代码部分提供了 `ExampleDerived` 的代码。）将这两个类型传递给 <xref:System.Type.MakeGenericType%2A> 以创建构造类型。 然后使用 <xref:System.Type.GetMethod%2A> 方法获取 <xref:System.Reflection.MethodInfo>。  
  
     [!code-cpp[EmitGenericType#9](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#9)]
     [!code-csharp[EmitGenericType#9](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#9)]
     [!code-vb[EmitGenericType#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#9)]  
  
12. 以下代码将创建一个 `Example` 对象数组，将该数组放在类型为 <xref:System.Object>，表示要调用的方法的参数的数组中，然后将创建的数组传递给 <xref:System.Reflection.MethodBase.Invoke%28System.Object%2CSystem.Object%5B%5D%29> 方法。 <xref:System.Reflection.MethodBase.Invoke%2A> 方法的第一个参数是空引用，因为该方法为 `static`。  
  
     [!code-cpp[EmitGenericType#10](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#10)]
     [!code-csharp[EmitGenericType#10](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#10)]
     [!code-vb[EmitGenericType#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#10)]  
  
## <a name="example"></a>示例  

 以下代码示例定义一个名为 `Sample` 的类，还定义一个基类和两个接口。 该程序可为 `Sample` 定义两个泛型类型参数，使其成为泛型类型。 只有类型参数能使一个类型成为泛型类型。 该程序通过显示定义类型参数前后的测试消息来对此进行演示。  
  
 类型参数 `TSecond` 用于通过基类和接口来演示类和接口约束，类型参数 `TFirst` 用于演示特殊约束。  
  
 此代码示例针对字段类型以及方法的参数和返回类型，通过使用该类的类型参数定义字段和方法。  
  
 创建 `Sample` 类后，调用该方法。  
  
 该程序中包括一个可列出泛型类型信息的方法，和一个可列出类型参数上的特殊约束的方法。 这些方法用于显示有关已完成的 `Sample` 类的信息。  
  
 程序将完成的模块以 `GenericEmitExample1.dll` 的形式保存到磁盘中，所以可以使用 [Ildasm.exe（IL 反汇编程序）](../tools/ildasm-exe-il-disassembler.md)将其打开，并检查 `Sample` 类的 MSIL。  
  
 [!code-cpp[EmitGenericType#1](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#1)]
 [!code-csharp[EmitGenericType#1](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#1)]
 [!code-vb[EmitGenericType#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Reflection.Emit.GenericTypeParameterBuilder>
- [使用反射发出](/previous-versions/dotnet/netframework-4.0/3y322t50(v=vs.100))
- [反射发出动态程序集应用场景](/previous-versions/dotnet/netframework-4.0/tt9483fk(v=vs.100))
