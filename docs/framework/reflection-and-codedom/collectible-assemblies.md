---
title: 动态类型生成的可回收程序集
description: 开始在 .NET 中使用动态类型生成的可回收程序集。 了解可回收程序集的生存期和限制。
ms.date: 08/29/2017
helpviewer_keywords:
- reflection, dynamic assembly
- assemblies, collectible
- collectible assemblies, retrieving
ms.openlocfilehash: 4981b93dbd49a6da96740bebed0f2ed7b89036c8
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475120"
---
# <a name="collectible-assemblies-for-dynamic-type-generation"></a>动态类型生成的可回收程序集

可回收程序集  是一个动态程序集，卸载该程序集时，无需卸载在其中创建了该程序集的应用程序域。 可回收程序集使用的所有托管和非托管内存及其包含的类型都是可回收的。 从内部表中删除了程序集名称等信息。

要启用卸载，请在创建动态程序集时使用 <xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndCollect?displayProperty=nameWithType> 标志。 程序集是瞬态程序集（即无法保存），并受到[可回收程序集限制](#restrictions-on-collectible-assemblies)部分中所述的限制。 释放与程序集关联的所有对象时，公共语言运行时 (CLR) 会自动卸载可回收程序集。 在所有其他方面，可回收程序集的创建和使用方法与其他动态程序集相同。

## <a name="lifetime-of-collectible-assemblies"></a>可回收程序集的生存期

可回收程序集的生存期受控于是否存在对其包含的类型和从这些类型创建的对象的引用。 只要存在以下一项或多项（`T` 是程序集中定义的任何类型），公共语言运行时就不会卸载程序集：

- `T` 的一个实例。

- `T` 数组的一个实例。

- 使用 `T` 作为其类型参数的一个泛型类型实例。 这包括 `T` 的泛型集合，即使该集合为空。

- <xref:System.Type> 或 <xref:System.Reflection.Emit.TypeBuilder> 的一个实例，表示 `T`。

   > [!IMPORTANT]
   > 必须释放表示部分程序集的所有对象。 定义 `T` 的 <xref:System.Reflection.Emit.ModuleBuilder> 保持对 <xref:System.Reflection.Emit.TypeBuilder> 的引用，<xref:System.Reflection.Emit.AssemblyBuilder> 对象保持对 <xref:System.Reflection.Emit.ModuleBuilder> 的引用，因此必须释放对这些对象的引用。 即使是存在 `T` 的构造中使用的 <xref:System.Reflection.Emit.LocalBuilder> 或 <xref:System.Reflection.Emit.ILGenerator>，也会阻止卸载。

- 仍可供执行代码访问的另一动态定义类型 `T1` 对 `T` 的静态引用。 例如，`T1` 可能派生自 `T`，或 `T` 可能为 `T1` 的方法中的参数类型。

- 属于 `T` 的静态字段的 ByRef  。

- 引用 `T` 或 `T` 的组件的 <xref:System.RuntimeTypeHandle>、<xref:System.RuntimeFieldHandle>、<xref:System.RuntimeMethodHandle>。

- 可直接或间接用于访问表示 `T` 的 <xref:System.Type> 对象的任何反射对象的实例。 例如，可从其元素类型为 `T` 的任意数组类型，或类型参数为 `T` 的泛型类型获取 `T` 的 <xref:System.Type> 对象。

- 任何线程的调用堆栈上的方法 `M`，其中 `M` 是 `T` 的一种方法，或程序集中定义的模块级方法。

- 对程序集模块中定义的静态方法的委托。

对于程序集中的唯一类型或唯一方法，如果列表中仅存在一个项，则运行时无法卸载程序集。

> [!NOTE]
> 终结器针对列表中的所有项运行前，运行时实际上不会卸载程序集。

为了跟踪生存期，在生成可回收程序集时创建和使用的构造泛型类型（如 `List<int>` (C#) 或 `List(Of Integer)` (Visual Basic)）被视为在包含泛型类型定义的程序集中定义，或在包含其类型参数之一的程序集中定义。 使用的具体程序集是实现详细信息，并且可能会有所更改。

## <a name="restrictions-on-collectible-assemblies"></a>对可回收程序集的限制

对可回收程序集有以下限制：

- 静态引用  ：普通动态程序集中的类型不能具有对可回收程序集中定义的类型的静态引用。 例如，如果定义了普通类型，且该类型继承自可回收程序集中的类型，则会引发 <xref:System.NotSupportedException> 异常。 可回收程序集中的类型可具有对另一可回收程序集中的类型的引用，但这可将引用程序集的生存期延长至引用的程序集的生存期。

- COM 互操作  ：任何 COM 接口都不可在可回收程序集中定义，并且可回收程序集内的任何类型实例都不可转换为 COM 对象。 可回收程序集中的类型不可用作 COM 可调用包装器 (CCW) 或运行时可调用包装器 (RCW)。 但是，可回收程序集中的类型可以使用实现 COM 接口的对象。

- 平台调用  ：在可回收程序集中声明具有 <xref:System.Runtime.InteropServices.DllImportAttribute> 属性的方法时，该方法不会进行编译。 <xref:System.Reflection.Emit.OpCodes.Calli?displayProperty=nameWithType> 指令不能用于可回收程序集中类型的实现，并且不可将此类类型封送到非托管代码。 但是，仍可使用非可回收程序集中声明的入口点调入本机代码。

- 封送  ：无法封送可回收程序集中定义的对象（尤其是委托）。 这是对所有暂时性发出的类型的限制。

- 程序集加载  ：对于加载可回收程序集，反射发出是支持的唯一机制。 使用任何其他形式的程序集加载机制加载的程序集无法卸载。

- 上下文绑定对象  ：不支持上下文静态变量。 可回收程序集中的类型无法扩展 <xref:System.ContextBoundObject>。 但是，可回收程序集中的代码可使用在其他位置定义的上下文绑定对象。

- 线程静态数据  ：不支持线程静态变量。

## <a name="see-also"></a>请参阅

- [发出动态方法和程序集](emitting-dynamic-methods-and-assemblies.md)
