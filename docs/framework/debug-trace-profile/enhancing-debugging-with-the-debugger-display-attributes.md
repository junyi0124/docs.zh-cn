---
title: 使用调试器显示特性增强调试
description: 开始在 .NET 中使用调试器显示属性，使类型开发人员还可以指定该类型在调试器中显示时的外观。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- debugger, display attributes
- DebuggerTypeProxyAttribute attribute
- debugging [.NET Framework], debugger display attributes
- DebuggerDisplayAttribute attribute
- display attributes for debugger
- DebuggerBrowsableAttribute attribute
ms.assetid: 72bb7aa9-459b-42c4-9163-9312fab4c410
ms.openlocfilehash: 2e556358490409a0fa7b345c4454eb43cf607e32
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244361"
---
# <a name="enhancing-debugging-with-the-debugger-display-attributes"></a>使用调试器显示特性增强调试

最了解且可指定类型运行时行为的类型开发人员还可以使用调试器显示属性指定类型在调试器中的显示外观。 此外，即使不了解源代码，用户也可将提供 `Target` 属性的调试器显示属性应用于程序集级别。 <xref:System.Diagnostics.DebuggerDisplayAttribute> 属性控制类型或成员在调试器变量窗口中的显示方式。 <xref:System.Diagnostics.DebuggerBrowsableAttribute> 属性决定是否在调试器变量窗口中显示字段或属性，若要显示，则决定其显示方式。 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 属性指定类型的替代类型或代理，并更改类型在调试器窗口中的显示方式。 查看具有代理或替代类型的变量时，代理将代替调试器显示窗口中的原始类型。 调试器变量窗口仅显示代理类型的公共成员。 不会显示私有成员。  
  
## <a name="using-the-debuggerdisplayattribute"></a>使用 DebuggerDisplayAttribute  

<xref:System.Diagnostics.DebuggerDisplayAttribute.%23ctor%2A> 构造函数有一个参数，此参数是一个字符串，将在类型实例的值列中显示。 此字符串可以包含大括号（{ 和 }）。 一对大括号之间的文本将作为表达式进行计算。 例如，选择加号 (+) 以展开 `MyHashtable` 实例的调试器显示时，以下 C# 代码将显示“Count = 4”。  

```csharp
[DebuggerDisplay("Count = {count}")]
class MyHashtable
{
    public int count = 4;
}
```

不会处理应用于表达式中所引用属性的特性。 对于 C# 编译器，允许只能隐式访问当前目标类型实例引用的常规表达式。 该表达式受限，不能访问别名、局部变量或指针。 在 C# 代码中，可在大括号中使用只能隐式访问当前目标类型实例的 `this` 指针的常规表达式。

例如，如果 C# 对象有重写的 `ToString()`，则调试器将调用该重写并显示重写的结果而不是标准 `{<typeName>}.`。因此，如果有重写的 `ToString()`，用户无需使用 <xref:System.Diagnostics.DebuggerDisplayAttribute>。 在同时使用这两者时，<xref:System.Diagnostics.DebuggerDisplayAttribute> 特性将优先于 `ToString()` 重写。

## <a name="using-the-debuggerbrowsableattribute"></a>使用 DebuggerBrowsableAttribute

 将 <xref:System.Diagnostics.DebuggerBrowsableAttribute> 应用于字段或属性，指定字段或属性在调试器窗口中的显示方式。 此属性的构造函数采用一个 <xref:System.Diagnostics.DebuggerBrowsableState> 枚举值，指定以下任一状态：

- <xref:System.Diagnostics.DebuggerBrowsableState.Never> 表示未在数据窗口中显示成员。  例如，将此值用于字段上的 <xref:System.Diagnostics.DebuggerBrowsableAttribute>，则会从层次结构中删除该字段，单击类型实例的加号 (+) 展开封闭类型时，不会显示该字段。

- <xref:System.Diagnostics.DebuggerBrowsableState.Collapsed> 表示显示成员，但默认情况下不展开。  此选项为默认行为。

- <xref:System.Diagnostics.DebuggerBrowsableState.RootHidden> 表示不显示成员本身，但如果成员是一个数组或集合，则会显示其组成对象。

> [!NOTE]
> 在 .NET Framework 2.0 中，Visual Basic 不支持 <xref:System.Diagnostics.DebuggerBrowsableAttribute>。

以下代码示例显示如何使用 <xref:System.Diagnostics.DebuggerBrowsableAttribute> 防止其后面的属性出现在该类的调试窗口中。

```csharp
[DebuggerBrowsable(DebuggerBrowsableState.Never)]
public static string y = "Test String";
```

## <a name="using-the-debuggertypeproxy"></a>使用 DebuggerTypeProxy

 如需从根本上大幅更改某个类型的调试视图，但不改变类型本身，请使用 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 属性。 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 属性用于为类型指定显示代理，允许开发人员为类型定制视图。  该属性与 <xref:System.Diagnostics.DebuggerDisplayAttribute> 类似，可以在程序集级别使用，在这种情况下，<xref:System.Diagnostics.DebuggerTypeProxyAttribute.Target%2A> 属性指定将使用代理的类型。 建议使用该属性指定在应用属性的类型内发生的私有嵌套类型。  显示类型时，支持类型查看器的表达式计算器将检查此属性。 如果找到该属性，表达式计算器会将显示代理类型替换为应用该属性的类型。

 显示 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 时，调试器变量窗口仅显示代理类型的公共成员。 不会显示私有成员。 属性增强视图不会改变数据窗口的行为。

 为避免不必要的性能损失，不会处理显示代理的属性，直到对象展开，无论是用户单击数据窗口中类型旁的加号 (+) 展开，还是通过应用 <xref:System.Diagnostics.DebuggerBrowsableAttribute> 属性展开。 因此，建议不要对显示类型应用任何属性。 可以且应该在显示类型的正文中应用属性。

 以下代码示例显示了如何使用 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 指定要用作调试器显示代理的类型。

```csharp
[DebuggerTypeProxy(typeof(HashtableDebugView))]
class MyHashtable : Hashtable
{
    private const string TestString =
        "This should not appear in the debug window.";

    internal class HashtableDebugView
    {
        private Hashtable hashtable;
        public const string TestStringProxy =
            "This should appear in the debug window.";

        // The constructor for the type proxy class must have a
        // constructor that takes the target type as a parameter.
        public HashtableDebugView(Hashtable hashtable)
        {
            this.hashtable = hashtable;
        }
    }
}
```

## <a name="example"></a>示例

### <a name="description"></a>描述

可在 Visual Studio 中查看下面的代码示例，以查看应用 <xref:System.Diagnostics.DebuggerDisplayAttribute> 、 <xref:System.Diagnostics.DebuggerBrowsableAttribute> 和特性的结果 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 。

### <a name="code"></a>代码

[!code-cpp[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/cpp/program.cpp#1)]
[!code-csharp[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/CS/program.cs#1)]
[!code-vb[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/VB/module1.vb#1)]

## <a name="see-also"></a>请参阅

- <xref:System.Diagnostics.DebuggerDisplayAttribute>
- <xref:System.Diagnostics.DebuggerBrowsableAttribute>
- <xref:System.Diagnostics.DebuggerTypeProxyAttribute>
