---
title: 属性概述
ms.date: 07/20/2015
ms.assetid: 1449f69b-c063-41de-8d89-f0bbdcf96ac6
ms.openlocfilehash: a0a080d44796289cc3562803c84ec915dcedd314
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400702"
---
# <a name="attributes-overview-visual-basic"></a>特性概述 (Visual Basic)

使用特性，可以有效地将元数据或声明性信息与代码（程序集、类型、方法、属性等）相关联。 将特性与程序实体相关联后，可以在运行时使用*反射*这项技术查询特性。 有关详细信息，请参阅[反射 (Visual Basic)](../reflection.md)。

特性具有以下属性：

- 特性向程序添加元数据。 *元数据*是程序中定义的类型的相关信息。 所有 .NET 程序集都包含一组指定的元数据，用于描述程序集中定义的类型和类型成员。 可以添加自定义特性来指定所需的其他任何信息。 有关详细信息，请参阅[创建自定义特性 (Visual Basic)](creating-custom-attributes.md)。

- 可以将一个或多个特性应用于整个程序集、模块或较小的程序元素（如类和属性）。

- 特性可以像方法和属性一样接受自变量。

- 程序可使用反射来检查自己的元数据或其他程序中的元数据。 有关详细信息，请参阅[使用反射访问特性 (Visual Basic)](accessing-attributes-by-using-reflection.md)。

## <a name="using-attributes"></a>使用属性

可以将特性附加到大多数的声明中，尽管特定特性可能会限制可有效附加到的声明的类型。 在 Visual Basic 中，特性括在尖括号（）中 \< > 。 特性的后面必须紧接着应用它的元素，且两者必须位于同一代码行。

在此示例中，<xref:System.SerializableAttribute> 特性用于将具体特征应用于类：

```vb
<System.Serializable()> Public Class SampleClass
    ' Objects of this type can be serialized.
End Class
```

 下方声明了一个具有特性 <xref:System.Runtime.InteropServices.DllImportAttribute> 的方法：

```vb
Imports System.Runtime.InteropServices
```

```vb
<System.Runtime.InteropServices.DllImport("user32.dll")>
Sub SampleMethod()
End Sub
```

可以将多个特性附加到声明中：

```vb
Imports System.Runtime.InteropServices
```

```vb
Sub MethodA(<[In](), Out()> ByVal x As Double)
End Sub
Sub MethodB(<Out(), [In]()> ByVal x As Double)
End Sub
```

对于给定实体，一些特性可以指定多次。 <xref:System.Diagnostics.ConditionalAttribute> 便属于此类多用途特性：

```vb
<Conditional("DEBUG"), Conditional("TEST1")>
Sub TraceMethod()
End Sub
```

> [!NOTE]
> 按照约定，所有特性名称均以“Attribute”一词结尾，以便与 .NET Framework 中的其他项区分开来。 不过，在代码中使用特性时，无需指定特性后缀。 例如，`[DllImport]` 等同于 `[DllImportAttribute]`，但 `DllImportAttribute` 是此特性在 .NET Framework 中的实际名称。

### <a name="attribute-parameters"></a>特性参数

许多属性都有参数，可以是位置参数、未命名参数或已命名参数。 所有位置参数都必须按特定的顺序指定，不能省略；已命名参数是可选的，可以按任意顺序指定。 首先指定的是位置参数。 例如，下面这三个特性是等同的：

```vb
<DllImport("user32.dll")>
<DllImport("user32.dll", SetLastError:=False, ExactSpelling:=False)>
<DllImport("user32.dll", ExactSpelling:=False, SetLastError:=False)>
```

第一个参数（DLL 名称）是位置参数，始终第一个出现；其他是已命名参数。 在此示例中，两个已命名参数的默认值均为 false，因此可以省略。 若要了解默认参数值，请参阅各个特性文档。

### <a name="attribute-targets"></a>特性目标

特性的*目标*是指应用特性的实体。 例如，特性可应用于类、特定方法或整个程序集。 默认情况下，特性应用于它后面紧接着的元素。 不过，还可以进行显式标识。例如，可以标识为将特性应用于方法，还是应用于其参数或返回值。

若要显式标识特性目标，请使用以下语法：

```vb
<target : attribute-list>
```

下表列出了可能的 `target` 值。

|目标值|适用对象|
|------------------|----------------|
|`assembly`|整个程序集|
|`module`|当前的程序集模块（不同于 Visual Basic 模块）|

 下面的示例展示了如何将特性应用于程序集和模块。 有关详细信息，请参阅[常见特性 (Visual Basic)](common-attributes.md)。

```vb
Imports System.Reflection
<Assembly: AssemblyTitleAttribute("Production assembly 4"),
Module: CLSCompliant(True)>
```

## <a name="common-uses-for-attributes"></a>特性的常见用途

下面列出了代码中特性的一些常见用途：

- 在 Web 服务中使用 `WebMethod` 特性标记方法，以指明方法应可通过 SOAP 协议进行调用。 有关详细信息，请参阅 <xref:System.Web.Services.WebMethodAttribute>。

- 描述在与本机代码互操作时如何封送方法参数。 有关详细信息，请参阅 <xref:System.Runtime.InteropServices.MarshalAsAttribute>。

- 描述类、方法和接口的 COM 属性。

- 使用 <xref:System.Runtime.InteropServices.DllImportAttribute> 类调用非托管代码。

- 从标题、版本、说明或商标方面描述程序集。

- 描述要序列化并暂留类的哪些成员。

- 描述如何为了执行 XML 序列化在类成员和 XML 节点之间进行映射。

- 描述的方法的安全要求。

- 指定用于强制实施安全规范的特征。

- 通过实时 (JIT) 编译器控制优化，这样代码就一直都易于调试。

- 获取方法调用方的相关信息。

## <a name="related-sections"></a>相关章节

有关详细信息，请参见:

- [创建自定义特性 (Visual Basic)](creating-custom-attributes.md)

- [使用反射访问特性 (Visual Basic)](accessing-attributes-by-using-reflection.md)

- [如何：使用特性创建 C/C++ 联合 (Visual Basic)](how-to-create-a-c-cpp-union-by-using-attributes.md)

- [常见特性 (Visual Basic)](common-attributes.md)

- [调用方信息 (Visual Basic)](../caller-information.md)

## <a name="see-also"></a>另请参阅

- [Visual Basic 编程指南](../../index.md)
- [反射 (Visual Basic)](../reflection.md)
- [特性](../../../../standard/attributes/index.md)
