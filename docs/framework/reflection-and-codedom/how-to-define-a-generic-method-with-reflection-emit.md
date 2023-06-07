---
title: 如何：用反射发出定义泛型方法
description: 使用反射发出定义泛型方法。 一个示例创建具有两个类型参数的泛型方法。 第二个示例演示如何发出方法主体。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- generics [.NET Framework], reflection emit
- reflection emit, generic methods
- generics [.NET Framework], dynamic types
ms.assetid: 93892fa4-90b3-4ec4-b147-4bec9880de2b
ms.openlocfilehash: 3b85fb480e5862daa3b2800f75392adbe92348f2
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865133"
---
# <a name="how-to-define-a-generic-method-with-reflection-emit"></a>如何：用反射发出定义泛型方法

第一个过程演示如何使用两个类型参数创建简单的泛型方法，以及如何将类约束、接口约束和特殊约束应用于类型参数。

第二个过程演示如何发出方法主体，以及如何使用泛型方法的类型参数创建泛型类型的实例和调用其方法。

第三个过程演示如何调用泛型方法。

> [!IMPORTANT]
> 某方法只要属于泛型类型，且使用该类型的类型参数，就不是泛型方法。 只有当方法有属于自己的类型参数列表时才是泛型方法。 泛型方法可在非泛型类型上出现，如本示例中所示。 有关泛型类型上的非泛型方法示例，请参阅[如何：使用反射发出定义泛型类型](how-to-define-a-generic-type-with-reflection-emit.md)。

### <a name="to-define-a-generic-method"></a>定义泛型方法

1. 开始前，最好先研究下在使用高级语言编写时泛型方法的出现方式。 以下代码与调用泛型方法的代码一起包含在此主题的示例代码中。 该方法具有两个类型参数：`TInput` 和 `TOutput`。其中第二个参数必须具备以下条件：是引用类型 (`class`)；具有无参数构造函数 (`new`)；实现 `ICollection(Of TInput)`（在 C# 中为 `ICollection<TInput>`）。 此接口约束确保可使用 <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> 方法将元素添加到该方法创建的 `TOutput` 集合。 该方法具有一个形参 `input`，它是 `TInput` 的数组。 该方法将创建一个 `TOutput` 类型的集合，并将 `input` 的元素复制到该集合。

    [!code-csharp[GenericMethodHowTo#20](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#20)]
    [!code-vb[GenericMethodHowTo#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#20)]

2. 定义动态程序集和动态模块，以包含泛型方法所属类型。 在这种情况下，程序集仅有一个模块 `DemoMethodBuilder1`，模块名称为该程序集名称加上扩展名。 在此示例中，因为要将程序集保存在磁盘并执行，所以指定了 <xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndSave?displayProperty=nameWithType>。 可以使用 [Ildasm.exe（IL 反汇编程序）](../tools/ildasm-exe-il-disassembler.md)检查 DemoMethodBuilder1.dll，并将其与步骤 1 中所示方法的 Microsoft 中间语言 (MSIL) 比较。

    [!code-csharp[GenericMethodHowTo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#2)]
    [!code-vb[GenericMethodHowTo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#2)]

3. 定义泛型方法所属类型。 该类型不一定是泛型类型。 泛型方法可以属于泛型或非泛型类型。 在此示例中，类型是一个名为 `DemoType` 类，不是泛型类型。

    [!code-csharp[GenericMethodHowTo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#3)]
    [!code-vb[GenericMethodHowTo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#3)]

4. 定义泛型方法。 如果泛型方法的泛型类型参数指定了泛型方法的形参类型，则使用 <xref:System.Reflection.Emit.TypeBuilder.DefineMethod%28System.String%2CSystem.Reflection.MethodAttributes%29> 方法重载来定义该方法。 由于尚未定义该方法的泛型类型参数，因此不能在对 <xref:System.Reflection.Emit.TypeBuilder.DefineMethod%2A> 的调用中指定方法的形参类型。 在此示例中，该方法名为 `Factory`。 该方法是 public 和 `static` 方法（在 Visual Basic 中是 `Shared` 方法）。

    [!code-csharp[GenericMethodHowTo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#4)]
    [!code-vb[GenericMethodHowTo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#4)]

5. 通过将包含参数名称的字符串数组传递给 <xref:System.Reflection.Emit.MethodBuilder.DefineGenericParameters%2A?displayProperty=nameWithType> 方法来定义 `DemoMethod` 的泛型类型参数。 这使该方法成为泛型方法。 以下代码可以将 `Factory` 变为具有类型参数 `TInput` 和 `TOutput` 的泛型方法。 为了更加方便阅读代码，采用以上名称创建变量，以保留表示这两个类型参数的 <xref:System.Reflection.Emit.GenericTypeParameterBuilder> 对象。

    [!code-csharp[GenericMethodHowTo#5](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#5)]
    [!code-vb[GenericMethodHowTo#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#5)]

6. 可以选择向类型参数添加特殊约束。 使用 <xref:System.Reflection.Emit.GenericTypeParameterBuilder.SetGenericParameterAttributes%2A> 方法添加特殊约束。 在此示例中，约束 `TOutput` 作为引用类型并且具有无参数构造函数。

    [!code-csharp[GenericMethodHowTo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#6)]
    [!code-vb[GenericMethodHowTo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#6)]

7. 可以选择向类型参数添加类约束和接口约束。 在此示例中，约束类型参数 `TOutput` 为实现 `ICollection(Of TInput)`（C# 中为 `ICollection<TInput>`）接口的类型。 这样确保了可以使用 <xref:System.Collections.Generic.ICollection%601.Add%2A> 方法来添加元素。

    [!code-csharp[GenericMethodHowTo#7](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#7)]
    [!code-vb[GenericMethodHowTo#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#7)]

8. 使用 <xref:System.Reflection.Emit.MethodBuilder.SetParameters%2A> 方法定义方法的形参。 在此示例中，`Factory` 方法具有一个参数，它是 `TInput` 的数组。 通过调用表示 `TInput` 的 <xref:System.Reflection.Emit.GenericTypeParameterBuilder> 上的 <xref:System.Type.MakeArrayType%2A> 方法创建此类型。 <xref:System.Reflection.Emit.MethodBuilder.SetParameters%2A> 的参数是 <xref:System.Type> 对象的数组。

    [!code-csharp[GenericMethodHowTo#8](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#8)]
    [!code-vb[GenericMethodHowTo#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#8)]

9. 使用 <xref:System.Reflection.Emit.MethodBuilder.SetReturnType%2A> 方法定义该方法的返回类型。 在此示例中，将返回 `TOutput` 的实例。

    [!code-csharp[GenericMethodHowTo#9](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#9)]
    [!code-vb[GenericMethodHowTo#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#9)]

10. 使用 <xref:System.Reflection.Emit.ILGenerator> 发出方法主体。 有关详细信息，请参阅附带的用于发出方法体的过程。

    > [!IMPORTANT]
    > 如果发出对泛型类型方法的调用，并且这些类型的类型参数是泛型方法的类型参数，则必须使用 <xref:System.Reflection.Emit.TypeBuilder> 类的 `static`<xref:System.Reflection.Emit.TypeBuilder.GetConstructor%28System.Type%2CSystem.Reflection.ConstructorInfo%29>、<xref:System.Reflection.Emit.TypeBuilder.GetMethod%28System.Type%2CSystem.Reflection.MethodInfo%29> 和 <xref:System.Reflection.Emit.TypeBuilder.GetField%28System.Type%2CSystem.Reflection.FieldInfo%29> 方法重载来获取方法的构造窗体。 发出方法主体的附带过程提供了相关演示。

11. 完成包含该方法的类型，并保存程序集。 附带的泛型方法调用过程演示了调用完整方法的两种方式。

    [!code-csharp[GenericMethodHowTo#14](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#14)]
    [!code-vb[GenericMethodHowTo#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#14)]

<a name="procedureSection1"></a>

### <a name="to-emit-the-method-body"></a>发出方法主体

1. 获取代码生成器，并声明局部变量和标签。 <xref:System.Reflection.Emit.ILGenerator.DeclareLocal%2A> 方法用于声明局部变量。 `Factory` 方法具有四个局部变量：`retVal`，用于保留该方法返回的新 `TOutput`；`ic`，用于在 `TOutput` 强制转换成 `ICollection(Of TInput)`（在 C# 中为 `ICollection<TInput>`） 时进行保留；`input`，用于保留 `TInput` 对象的输入数组；`index`，用于循环访问数组。 该方法还具有两个标签，一个用于进入循环 (`enterLoop`)，另一个用于循环的顶部 (`loopAgain`)，这两个标签均使用 <xref:System.Reflection.Emit.ILGenerator.DefineLabel%2A> 方法定义。

    该方法执行的第一个操作是使用 <xref:System.Reflection.Emit.OpCodes.Ldarg_0> 操作码加载其参数，并使用 <xref:System.Reflection.Emit.OpCodes.Stloc_S> 操作码将参数存储到局部变量 `input`。

    [!code-csharp[GenericMethodHowTo#10](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#10)]
    [!code-vb[GenericMethodHowTo#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#10)]

2. 使用 <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> 方法的泛型方法重载，发出代码，创建 `TOutput` 的实例。 使用此重载要求指定的类型具有无参数构造函数，因此需向 `TOutput` 添加该约束。 通过将 `TOutput` 传递到 <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A> 来创建构造泛型方法。 发出代码调用方法后，使用 <xref:System.Reflection.Emit.OpCodes.Stloc_S> 发出代码，将其存储在局部变量 `retVal`

    [!code-csharp[GenericMethodHowTo#11](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#11)]
    [!code-vb[GenericMethodHowTo#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#11)]

3. 发出代码，将新的 `TOutput` 对象强制转换为 `ICollection(Of TInput)`，并将其存储在局部变量 `ic`。

    [!code-csharp[GenericMethodHowTo#31](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#31)]
    [!code-vb[GenericMethodHowTo#31](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#31)]

4. 获取表示 <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> 方法的 <xref:System.Reflection.MethodInfo>。 此方法作用于 `ICollection(Of TInput)`（在 C# 中为 `ICollection<TInput>`），因此必须获取特定于该构造类型的 `Add` 方法。 不能使用 <xref:System.Type.GetMethod%2A> 方法直接从 `icollOfTInput` 获取 <xref:System.Reflection.MethodInfo>，因为 <xref:System.Type.GetMethod%2A> 在已使用 <xref:System.Reflection.Emit.GenericTypeParameterBuilder> 构造的类型上不受支持。 而应该在包含 <xref:System.Collections.Generic.ICollection%601> 泛型接口的泛型类型定义的 `icoll` 上调用 <xref:System.Type.GetMethod%2A>。 然后，使用 <xref:System.Reflection.Emit.TypeBuilder.GetMethod%28System.Type%2CSystem.Reflection.MethodInfo%29>`static` 方法生成该构造类型的 <xref:System.Reflection.MethodInfo>。 下面的代码对此进行了演示。

    [!code-csharp[GenericMethodHowTo#12](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#12)]
    [!code-vb[GenericMethodHowTo#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#12)]

5. 通过加载 32 位整数 0 并将其存储在变量中，发出代码，初始化 `index` 变量。 发出代码，分支到标签 `enterLoop`。 因为此标签位于循环内，所以尚未标记。 循环的代码在下一步发出。

    [!code-csharp[GenericMethodHowTo#32](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#32)]
    [!code-vb[GenericMethodHowTo#32](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#32)]

6. 发出该循环的代码。 第一步是通过 `loopAgain` 标签调用 <xref:System.Reflection.Emit.ILGenerator.MarkLabel%2A>，标记循环的顶部。 使用该标签的分支语句现在将分支到代码中的这个点。 下一步是将强制转换为 `ICollection(Of TInput)` 的 `TOutput` 对象推入堆栈。 无需立即进行此操作，但需要在调用 `Add` 方法之前完成。 接下来，依次将输入数组和包含该数组的当前索引的 `index` 变量推入堆栈。 <xref:System.Reflection.Emit.OpCodes.Ldelem> 操作码从堆栈中弹出该索引和数组，然后将索引数组元素推入堆栈。 堆栈现已准备好调用 <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> 方法，该方法从堆栈中弹出集合和新元素，并将该元素添加到该集合中。

    循环中的其余代码会递增索引并测试以查看循环是否已完成：将索引和 32 位整数 1 压入堆栈并添加，将总和留在堆栈上；总和存储在 `index` 中。 调用 <xref:System.Reflection.Emit.ILGenerator.MarkLabel%2A>，将该点设置为循环的入口点。 再次加载该索引。 将输入数组推入堆栈，然后发出 <xref:System.Reflection.Emit.OpCodes.Ldlen>，获取其长度。 索引和长度现位于堆栈中，发出 <xref:System.Reflection.Emit.OpCodes.Clt> 对二者进行比较。 如果索引小于该长度，<xref:System.Reflection.Emit.OpCodes.Brtrue_S> 会从分支返回到循环的起始点。

    [!code-csharp[GenericMethodHowTo#13](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#13)]
    [!code-vb[GenericMethodHowTo#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#13)]

7. 发出代码，将 `TOutput` 对象推入堆栈，并从该方法返回。 局部变量 `retVal` 和 `ic` 均包含对新的 `TOutput` 的引用；`ic` 仅用于访问 <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> 方法。

    [!code-csharp[GenericMethodHowTo#33](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#33)]
    [!code-vb[GenericMethodHowTo#33](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#33)]

<a name="procedureSection2"></a>

### <a name="to-invoke-the-generic-method"></a>调用泛型方法

1. `Factory` 是泛型方法定义。 若要调用泛型方法，必须向其泛型类型参数分配类型。 可使用 <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A> 方法完成此操作。 下面的代码通过为 `TInput` 指定 <xref:System.String> 并为 `TOutput` 指定 `List(Of String)`（在 C# 中为 `List<string>`）创建构造泛型方法，并显示该方法的字符串表示形式。

    [!code-csharp[GenericMethodHowTo#21](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#21)]
    [!code-vb[GenericMethodHowTo#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#21)]

2. 若要以后期绑定的形式调用该方法，请使用 <xref:System.Reflection.MethodBase.Invoke%2A> 方法。 以下代码可创建 <xref:System.Object> 的数组（其中包含的唯一元素为字符串数组），并将其作为泛型方法的参数列表传递。 <xref:System.Reflection.MethodBase.Invoke%2A> 的第一个参数是空引用，因为该方法为 `static`。 将返回值强制转换为 `List(Of String)`，并显示它的第一个元素。

    [!code-csharp[GenericMethodHowTo#22](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#22)]
    [!code-vb[GenericMethodHowTo#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#22)]

3. 若要使用委托调用该方法，必须具有与该构造泛型方法的签名匹配的委托。 实现上述目标的一种简便方式是创建一个泛型委托。 以下代码使用 <xref:System.Delegate.CreateDelegate%28System.Type%2CSystem.Reflection.MethodInfo%29?displayProperty=nameWithType> 方法重载来创建示例代码中定义的泛型委托 `D` 的实例，然后调用该委托。 委托的性能优于后期绑定调用的性能。

    [!code-csharp[GenericMethodHowTo#23](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#23)]
    [!code-vb[GenericMethodHowTo#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#23)]

4. 也可以从引用保存的程序集的程序调用发出的方法。

## <a name="example"></a>示例

下面的代码示例使用泛型方法 `Factory` 创建一个非泛型类型 `DemoType`。 此方法具有两个泛型类型参数：`TInput`，用于指定输入类型；`TOutput`，用于指定输出类型。 `TOutput` 类型参数具有以下约束：实现 `ICollection<TInput>`（在 Visual Basic 中为 `ICollection(Of TInput)`）；作为引用类型；具有无参数构造函数。

该方法具有一个形参，它是 `TInput` 的数组。 该方法返回 `TOutput` 的实例，该实例包含输入数组的所有元素。 `TOutput` 可以是实现 <xref:System.Collections.Generic.ICollection%601> 泛型接口的任意泛型集合类型。

执行代码时，该动态程序集会另存为 DemoGenericMethod1.dll，使用 [Ildasm.exe（IL 反汇编程序）](../tools/ildasm-exe-il-disassembler.md) 可以对其进行检查。

> [!NOTE]
> 一种了解如何发出代码的好方法是编写 Visual Basic、C# 或 Visual C++ 程序，执行尝试发出的任务，然后使用反汇编程序检查编译器生成的 MSIL。

该代码示例包含等效于发出的方法的源代码。 发出的方法也是通过使用代码示例中声明的泛型委托，以后期绑定的形式调用的。

[!code-csharp[GenericMethodHowTo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#1)]
[!code-vb[GenericMethodHowTo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#1)]

## <a name="see-also"></a>请参阅

- <xref:System.Reflection.Emit.MethodBuilder>
- [如何：使用反射发出定义泛型类型](how-to-define-a-generic-type-with-reflection-emit.md)
