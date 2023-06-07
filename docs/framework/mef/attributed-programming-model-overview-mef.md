---
title: 特性化编程模型概述 (MEF)
description: 开始使用特性化编程模型，该模型是 .NET 中 Managed Extensibility Framework (MEF) 的默认编程模型。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- MEF, attributed programming model
- attributed programming model [MEF]
ms.assetid: 49b787ff-2741-4836-ad51-c3017dc592d4
ms.openlocfilehash: aea3a19ffe6f177901e5c0839b618bb36f573beb
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281363"
---
# <a name="attributed-programming-model-overview-mef"></a>特性化编程模型概述 (MEF)

在 Managed Extensibility Framework (MEF) 中， *编程模型* 是定义 MEF 所操作的概念性对象集的特定方法。 这些概念对象包括部件、导入和导出。 MEF 使用这些对象，但未指定应如何表示这些对象。 因此，将可能有各种各样的编程模型，其中包括自定义编程模型。

在 MEF 中使用的默认编程模型是 *特性化编程模型*。 在特性化编程模型中，部件、导入、导出和其他对象是用修饰普通 .NET Framework 类的特性定义的。 本主题介绍如何使用特性化编程模型提供的特性来创建 MEF 应用程序。

<a name="import_and_export_basics"></a>

## <a name="import-and-export-basics"></a>导入和导出基础知识

*导出* 是部件向容器中的其他部件提供的一个值，而 *导入* 是部件向要通过可用导出填充的容器提出的要求。 在特性化编程模型中，导入和导出由使用 `Import` 和 `Export` 特性的修饰类或成员声明。 `Export` 特性可修饰类、字段、属性或方法，而 `Import` 特性可修饰字段、属性或构造函数参数。

为了使导入与导出相匹配，该导入和该导出必须具有相同的 *协定*。 协定由一个字符串（称为 *协定名称*）和已导出或导入对象的类型（称为 *协定类型*）组成。 只有在协定名称和协定类型均匹配时，才会认为导出能够满足特定导入。

协定参数中的其中任意一个或两者可能为隐式也可能为显式。 下面的代码示例演示一个声明基础导入的类。

```vb
Public Class MyClass1
    <Import()>
    Public Property MyAddin As IMyAddin
End Class
```

```csharp
public class MyClass
{
    [Import]
    public IMyAddin MyAddin { get; set; }
}
```

在此导入中， `Import` 特性既未附加协定类型参数，也未附加协定名称参数。 因此，这两者均将从修饰的属性推断而出。 在这种情况下，协定类型将为 `IMyAddin`，而协定名称将为依据该协定类型创建的唯一字符串。 （换言之，协定名称将仅与其名称同样从类型 `IMyAddin`推断而出的导出匹配。）

下面的示例演示与前面的导入匹配的导出。

```vb
<Export(GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export(typeof(IMyAddin))]
public class MyLogger : IMyAddin { }
```

在此导出中，由于协定类型指定为 `IMyAddin` 特性的参数，因此协定类型为 `Export` 。 导出的类型必须与协定类型相同、派生自协定类型，或者实现协定类型（如果导出的类型为接口）。 在此导出中，实际类型 `MyLogger` 实现接口 `IMyAddin`。 协定名称是从协定类型推断而出的，这意味着此导出将与前面的导入匹配。

> [!NOTE]
> 通常应对公共类或成员声明导出和导入。 其他声明也受支持，但如果导出或导入私有成员、受保护成员或内部成员，将会损坏部件的隔离模型，因此建议不要这样做。

协定类型对于要视为匹配的导出和导入必须完全匹配。 请看下面的导出。

```vb
<Export()> 'WILL NOT match the previous import!
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export] //WILL NOT match the previous import!
public class MyLogger : IMyAddin { }
```

在此导出中，协定类型为 `MyLogger` ，而不是 `IMyAddin`。 即使 `MyLogger` 实现 `IMyAddin`并因此可强制转换为 `IMyAddin` 对象，此导出也将由于协定类型不匹配而与前面的导入不匹配。

通常不必指定协定名称，并且应依据协定类型和元数据定义大多数协定。 但是，在某些情况下，务必要直接指定协定名称。 最常见的情况是：类导出共享通用类型的若干值（例如基元）。 可将协定名称指定为 `Import` 或 `Export` 特性的第一个参数。 下面的代码演示具有指定协定名称 `MajorRevision`的导入和导出。

```vb
Public Class MyExportClass

    'This one will match
    <Export("MajorRevision")>
    Public ReadOnly Property MajorRevision As Integer
        Get
            Return 4
        End Get
    End Property

    <Export("MinorRevision")>
    Public ReadOnly Property MinorRevision As Integer
        Get
            Return 16
        End Get
    End Property
End Class
```

```csharp
public class MyClass
{
    [Import("MajorRevision")]
    public int MajorRevision { get; set; }
}

public class MyExportClass
{
    [Export("MajorRevision")] //This one will match.
    public int MajorRevision = 4;

    [Export("MinorRevision")]
    public int MinorRevision = 16;
}
```

如果未指定协定类型，将仍然会从导入或导出的类型推断出协定类型。 但是，即使显式指定了协定名称，协定类型对于要视为匹配的导入和导出也仍然必须完全匹配。 例如，如果 `MajorRevision` 字段为字符串，则推断出的协定类型将不匹配，并且导出将与导入不匹配（尽管具有相同的协定名称）。

### <a name="importing-and-exporting-a-method"></a>导入和导出方法

`Export` 特性还可以按照与类、属性或函数相同的方式声明方法。 方法导出必须将协定类型或协定名称指定为类型，并且无法推断。 指定的类型可以为自定义委托或泛型类型，例如 `Func`。 下面的类导出一个名为 `DoSomething`的方法。

```vb
Public Class MyAddin

    'Explicitly specifying a generic type
    <Export(GetType(Func(Of Integer, String)))>
    Public Function DoSomething(ByVal TheParam As Integer) As String
        Return Nothing 'Function body goes here
    End Function

End Class
```

```csharp
public class MyAddin
{
    //Explicitly specifying a generic type.
    [Export(typeof(Func<int, string>))]
    public string DoSomething(int TheParam);
}
```

在此类中， `DoSomething` 方法采用单个 `int` 参数，并返回 `string`。 若要匹配此导出，导入部件必须适当的成员。 下面的类导入 `DoSomething` 方法。

```vb
Public Class MyClass1

    'Contract name must match!
    <Import()>
    Public Property MajorRevision As Func(Of Integer, String)
End Class
```

```csharp
public class MyClass
{
    [Import] //Contract name must match!
    public Func<int, string> DoSomething { get; set; }
}
```

有关如何使用 `Func<T, T>` 对象的更多信息，请参见 <xref:System.Func%602>。

<a name="types_of_imports"></a>

## <a name="types-of-imports"></a>导入的类型

MEF 支持若干导入类型，其中包括动态导入、延迟导入、必备导入和可选导入。

### <a name="dynamic-imports"></a>动态导入

在某些情况下，导入类可能需要与具有特定协定名称的任何类型的导出匹配。 在这种情况下，类可以声明 *动态导入*。 下面的导入与具有协定名称“TheString”的任何导出匹配。

```vb
Public Class MyClass1

    <Import("TheString")>
    Public Property MyAddin

End Class
```

```csharp
public class MyClass
{
    [Import("TheString")]
    public dynamic MyAddin { get; set; }
}
```

如果协定类型从 `dynamic` 关键字推断而出，则它将与任何协定类型匹配。 在这种情况下，导入应 **始终** 指定要依据其进行匹配的协定名称。 （如果未指定协定名称，则会将导入视为未与导出匹配。）下面的两个导出均将与前面的导入匹配。

```vb
<Export("TheString", GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class

<Export("TheString")>
Public Class MyToolbar

End Class
```

```csharp
[Export("TheString", typeof(IMyAddin))]
public class MyLogger : IMyAddin { }

[Export("TheString")]
public class MyToolbar { }
```

显然，导入类必须准备处理任意类型的对象。

### <a name="lazy-imports"></a>延迟导入

在某些情况下，导入类可能需要间接引用导入的对象，因此不会立即实例化该对象。 在这种情况下，类可以通过使用协定类型 *来声明* 延迟导入 `Lazy<T>`。 下面的导入属性声明一个延迟导入。

```vb
Public Class MyClass1

    <Import()>
    Public Property MyAddin As Lazy(Of IMyAddin)

End Class
```

```csharp
public class MyClass
{
    [Import]
    public Lazy<IMyAddin> MyAddin { get; set; }
}
```

从组合引擎的角度来看，协定类型 `Lazy<T>` 将被视为与协定类型 `T`相同。 因此，前面的导入将与下面的导出匹配。

```vb
<Export(GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export(typeof(IMyAddin))]
public class MyLogger : IMyAddin { }
```

可在 `Import` 特性中为延迟导入指定协定名称和协定类型，如前面的“导入和导出基础知识”一节中所述。

### <a name="prerequisite-imports"></a>必备导入

导出的 MEF 部件通常由组合引擎创建，以响应填写匹配的导入的直接请求或需求。 默认情况下，在创建部件时，组合引擎将使用无参数的构造函数。 若要使该引擎使用其他构造函数，你可以用 `ImportingConstructor` 特性标记它。

每个部件都可能只有一个供组合引擎使用的构造函数。 如果不提供无参数构造函数和 `ImportingConstructor` 特性，或者提供多个 `ImportingConstructor` 特性，将会产生错误。

为了填充用 `ImportingConstructor` 特性标记的构造函数的参数，所有这些参数均将自动声明为导入。 这样可以方便地声明在部件初始化过程中使用的导入。 下面的类使用 `ImportingConstructor` 来声明导入。

```vb
Public Class MyClass1

    Private _theAddin As IMyAddin

    'Parameterless constructor will NOT be used
    'because the ImportingConstructor
    'attribute is present.
    Public Sub New()

    End Sub

    'This constructor will be used.
    'An import with contract type IMyAddin
    'is declared automatically.
    <ImportingConstructor()>
    Public Sub New(ByVal MyAddin As IMyAddin)
        _theAddin = MyAddin
    End Sub

End Class
```

```csharp
public class MyClass
{
    private IMyAddin _theAddin;

    //Parameterless constructor will NOT be
    //used because the ImportingConstructor
    //attribute is present.
    public MyClass() { }

    //This constructor will be used.
    //An import with contract type IMyAddin is
    //declared automatically.
    [ImportingConstructor]
    public MyClass(IMyAddin MyAddin)
    {
        _theAddin = MyAddin;
    }
}
```

默认情况下， `ImportingConstructor` 特性为所有参数导入使用推断出的协定类型和协定名称。 可以通过用 `Import` 特性修饰参数来重写此行为，这些特性随后可显式定义协定类型和协定名称。 下面的代码演示一个使用此语法来导入派生类（而不是父类）的构造函数。

```vb
<ImportingConstructor()>
Public Sub New(<Import(GetType(IMySubAddin))> ByVal MyAddin As IMyAddin)

End Sub
```

```csharp
[ImportingConstructor]
public MyClass([Import(typeof(IMySubAddin))]IMyAddin MyAddin)
{
    _theAddin = MyAddin;
}
```

在使用集合参数时应特别小心。 例如，如果你使用类型为 `ImportingConstructor` 的参数在构造函数上指定 `IEnumerable<int>`，则导入将与类型为 `IEnumerable<int>`的单个导出（而不是类型为 `int`的一组导入）匹配。 若要与类型为 `int`的一组导出匹配，你必须用 `ImportMany` 特性修饰参数。

由 `ImportingConstructor` 特性声明为导入的参数也标记为 *必备导入*。 MEF 通常允许导出和导入形成 *循环*。 例如，循环是指对象 A 导入对象 B，后者反过来又导入对象 A。在一般情况下，循环不会成为问题，并且组合容器将正常构造两个对象。

当部件的构造函数需要导入的值时，该对象将无法参与循环。 如果对象 A 要求在其自身可构造之前构造对象 B，并且对象 B 导入对象 A，则循环将无法解决，并将发生组合错误。 因此，在构造函数参数上声明的导入为必备导入，必须先满足这些导入，之后才能使用需要这些导入的对象中的任何导出。

### <a name="optional-imports"></a>可选导入

`Import` 特性指定部件正常运行的要求。 如果导入无法得到满足，则该部件的组合将失败，并且部件将不可用。

通过使用 *属性指定导入为* 可选 `AllowDefault` 。 在这种情况下，即使导入未与任何可用的导出匹配，组合也将成功，并且导入属性将设置为其属性类型的默认值（对于对象属性则为 `null`，对于布尔值则为 `false`，对于数值属性则为零。）下面的类使用可选导入。

```vb
Public Class MyClass1

    <Import(AllowDefault:=True)>
    Public Property thePlugin As Plugin

    'If no matching export is available,
    'thePlugin will be set to null.
End Class
```

```csharp
public class MyClass
{
    [Import(AllowDefault = true)]
    public Plugin thePlugin { get; set; }

    //If no matching export is available,
    //thePlugin will be set to null.
}
```

### <a name="importing-multiple-objects"></a>导入多个对象

只有当 `Import` 特性与一个导出匹配并且仅与一个导出匹配时，才能成功构成该特性。 其他类将生成组合错误。 若要导入与同一协定匹配的多个导出，请使用 `ImportMany` 特性。 用此特性标记的导入始终是可选的。 例如，如果不存在匹配导出，组合将失败。 下面的类将导入任意数量类型为 `IMyAddin`的导出。

```vb
Public Class MyClass1

    <ImportMany()>
    Public Property MyAddin As IEnumerable(Of IMyAddin)

End Class
```

```csharp
public class MyClass
{
    [ImportMany]
    public IEnumerable<IMyAddin> MyAddin { get; set; }
}
```

可以使用普通 `IEnumerable<T>` 语法和方法访问导入的数组。 也可以改用普通数组 (`IMyAddin[]`)。

当你将此模式与 `Lazy<T>` 语法结合使用时，它可能非常重要。 例如，通过使用 `ImportMany`、 `IEnumerable<T>`和 `Lazy<T>`，你可以导入对任意数量的对象的间接引用，并仅实例化必要的对象。 下面的类演示此模式。

```vb
Public Class MyClass1

    <ImportMany()>
    Public Property MyAddin As IEnumerable(Of Lazy(Of IMyAddin))

End Class
```

```csharp
public class MyClass
{
    [ImportMany]
    public IEnumerable<Lazy<IMyAddin>> MyAddin { get; set; }
}
```

<a name="avoiding_discovery"></a>

## <a name="avoiding-discovery"></a>避免发现

在某些情况下，你可能需要防止部件作为目录的一部分被发现。 例如，部件可能是应从中继承（而不是使用）的基类。 可通过两种方式来实现此目的。 首先，可以对部件类使用 `abstract` 关键字。 尽管抽象类能够向派生自抽象类的类提供继承的导出，但抽象类从不提供导出。

如果无法使类成为抽象类，你可以使用 `PartNotDiscoverable` 特性来修饰它。 用此特性修饰的部件将不会包括在任何目录中。 下面的示例演示这些模式。 `DataOne` 将被目录发现。 由于 `DataTwo` 是抽象的，因此它将不会被发现。 由于 `DataThree` 使用了 `PartNotDiscoverable` 特性，因此它将不会被发现。

```vb
<Export()>
Public Class DataOne
    'This part will be discovered
    'as normal by the catalog.
End Class

<Export()>
Public MustInherit Class DataTwo
    'This part will not be discovered
    'by the catalog.
End Class

<PartNotDiscoverable()>
<Export()>
Public Class DataThree
    'This part will also not be discovered
    'by the catalog.
End Class
```

```csharp
[Export]
public class DataOne
{
    //This part will be discovered
    //as normal by the catalog.
}

[Export]
public abstract class DataTwo
{
    //This part will not be discovered
    //by the catalog.
}

[PartNotDiscoverable]
[Export]
public class DataThree
{
    //This part will also not be discovered
    //by the catalog.
}
```

<a name="metadata_and_metadata_views"></a>

## <a name="metadata-and-metadata-views"></a>元数据和元数据视图

导出可提供有关自身的附加信息（称为“元数据” ）。 元数据可用于将导出的对象的属性传递到导入部件。 导入部件可以使用此数据来决定要使用哪些导出，或收集有关导出的信息而不必构造导出。 因此，导入必须为延迟导入才能使用元数据。

为了使用元数据，你通常会声明一个称为 *元数据视图*的接口，该接口声明哪些元数据将可用。 元数据视图接口必须只有属性，并且这些属性必须具有 `get` 访问器。 下面的接口是一个示例元数据视图。

```vb
Public Interface IPluginMetadata

    ReadOnly Property Name As String

    <DefaultValue(1)>
    ReadOnly Property Version As Integer

End Interface
```

```csharp
public interface IPluginMetadata
{
    string Name { get; }

    [DefaultValue(1)]
    int Version { get; }
}
```

也可以使用泛型集合 `IDictionary<string, object>`作为元数据视图，但这样将会丧失类型检查的优点，因此应避免这样做。

通常，在元数据视图中命名的所有属性都是必需的，并且不会将未提供这些属性的任何导出视为匹配。 `DefaultValue` 特性指定属性是可选的。 如果未包括属性，则将为其分配指定为 `DefaultValue`的参数的默认值。 下面是用元数据修饰的两个不同的类。 这两个类都将与前面的元数据视图匹配。

```vb
<Export(GetType(IPlugin))>
<ExportMetadata("Name", "Logger")>
<ExportMetadata("Version", 4)>
Public Class MyLogger
    Implements IPlugin

End Class

'Version is not required because of the DefaultValue
<Export(GetType(IPlugin))>
<ExportMetadata("Name", "Disk Writer")>
Public Class DWriter
    Implements IPlugin

End Class
```

```csharp
[Export(typeof(IPlugin)),
    ExportMetadata("Name", "Logger"),
    ExportMetadata("Version", 4)]
public class Logger : IPlugin
{
}

[Export(typeof(IPlugin)),
    ExportMetadata("Name", "Disk Writer")]
    //Version is not required because of the DefaultValue
public class DWriter : IPlugin
{
}
```

元数据是通过使用 `Export` 特性在 `ExportMetadata` 特性之后表示的。 每一段元数据都由一个名称/值对组成。 元数据的名称部分必须与元数据视图中相应属性的名称匹配，并且值将分配给该属性。

导入程序负责指定将使用的元数据视图（如果有）。 包含元数据的导入将声明为延迟导入，其元数据接口作为 `Lazy<T,T>`的第二个类型参数。 下面的类导入前面的部件以及元数据。

```vb
Public Class Addin

    <Import()>
    Public Property plugin As Lazy(Of IPlugin, IPluginMetadata)
End Class
```

```csharp
public class Addin
{
    [Import]
    public Lazy<IPlugin, IPluginMetadata> plugin;
}
```

在许多情况下，你需要将元数据与 `ImportMany` 特性结合，以便分析各个可用的导入并选择仅实例化一个导入，或者筛选集合以匹配特定条件。 下面的类仅实例化具有 `IPlugin` 值“Logger”的 `Name` 对象。

```vb
Public Class User

    <ImportMany()>
    Public Property plugins As IEnumerable(Of Lazy(Of IPlugin, IPluginMetadata))

    Public Function InstantiateLogger() As IPlugin

        Dim logger As IPlugin
        logger = Nothing

        For Each Plugin As Lazy(Of IPlugin, IPluginMetadata) In plugins
            If Plugin.Metadata.Name = "Logger" Then
                logger = Plugin.Value
            End If
        Next
        Return logger
    End Function

End Class
```

```csharp
public class User
{
    [ImportMany]
    public IEnumerable<Lazy<IPlugin, IPluginMetadata>> plugins;

    public IPlugin InstantiateLogger()
    {
        IPlugin logger = null;

        foreach (Lazy<IPlugin, IPluginMetadata> plugin in plugins)
        {
            if (plugin.Metadata.Name == "Logger")
                logger = plugin.Value;
        }
        return logger;
    }
}
```

<a name="import_and_export_inheritance"></a>

## <a name="import-and-export-inheritance"></a>导入和导出继承

如果某个类继承自部件，则该类也可能会成为部件。 导入始终由子类继承。 因此，部件的子类将始终为部件，并具有与其父类相同的导入。

通过使用 `Export` 特性的声明的导出不会由子类继承。 但是，部件可通过使用 `InheritedExport` 特性继承自身。 部件的子类将继承并提供相同的导出，其中包括协定名称和协定类型。 与 `Export` 特性不同， `InheritedExport` 只能在类级别（而不是成员级别）应用。 因此，成员级别导出永远不能被继承。

下面四个类演示了导入和导出继承的原则。 `NumTwo` 继承自 `NumOne`，因此 `NumTwo` 将导入 `IMyData`。 普通导出不会被继承，因此 `NumTwo` 将不会导出任何内容。 `NumFour` 继承自 `NumThree`。 由于 `NumThree` 使用了 `InheritedExport`，因此 `NumFour` 具有一个协定类型为 `NumThree`的导出。 成员级别导出从不会被继承，因此不会导出 `IMyData` 。

```vb
<Export()>
Public Class NumOne
    <Import()>
    Public Property MyData As IMyData
End Class

Public Class NumTwo
    Inherits NumOne

    'Imports are always inherited, so NumTwo will
    'Import IMyData

    'Ordinary exports are not inherited, so
    'NumTwo will NOT export anything.  As a result it
    'will not be discovered by the catalog!

End Class

<InheritedExport()>
Public Class NumThree

    <Export()>
    Public Property MyData As IMyData

    'This part provides two exports, one of
    'contract type NumThree, and one of
    'contract type IMyData.

End Class

Public Class NumFour
    Inherits NumThree

    'Because NumThree used InheritedExport,
    'this part has one export with contract
    'type NumThree.

    'Member-level exports are never inherited,
    'so IMyData is not exported.

End Class
```

```csharp
[Export]
public class NumOne
{
    [Import]
    public IMyData MyData { get; set; }
}

public class NumTwo : NumOne
{
    //Imports are always inherited, so NumTwo will
    //import IMyData.

    //Ordinary exports are not inherited, so
    //NumTwo will NOT export anything. As a result it
    //will not be discovered by the catalog!
}

[InheritedExport]
public class NumThree
{
    [Export]
    Public IMyData MyData { get; set; }

    //This part provides two exports, one of
    //contract type NumThree, and one of
    //contract type IMyData.
}

public class NumFour : NumThree
{
    //Because NumThree used InheritedExport,
    //this part has one export with contract
    //type NumThree.

    //Member-level exports are never inherited,
    //so IMyData is not exported.
}
```

如果存在与 `InheritedExport` 特性关联的元数据，该元数据也将被继承。 （有关详细信息，请参阅前面的“元数据和元数据视图”一节。）子类无法修改继承的元数据。 但是，通过使用相同协定名称和协定类型但使用新元数据重新声明 `InheritedExport` 特性，子类可以将继承的元数据替换为新元数据。 下面的类演示此原则。 `MegaLogger` 部件继承自 `Logger` 并包括 `InheritedExport` 特性。 由于 `MegaLogger` 重新声明名为 Status 的新元数据，因此它不会从 `Logger`中继承 Name 和 Version 元数据。

```vb
<InheritedExport(GetType(IPlugin))>
<ExportMetadata("Name", "Logger")>
<ExportMetadata("Version", 4)>
Public Class Logger
    Implements IPlugin

    'Exports with contract type IPlugin
    'and metadata "Name" and "Version".
End Class

Public Class SuperLogger
    Inherits Logger

    'Exports with contract type IPlugin and
    'metadata "Name" and "Version", exactly the same
    'as the Logger class.

End Class

<InheritedExport(GetType(IPlugin))>
<ExportMetadata("Status", "Green")>
Public Class MegaLogger
    Inherits Logger

    'Exports with contract type IPlugin and
    'metadata "Status" only. Re-declaring
    'the attribute replaces all metadata.

End Class
```

```csharp
[InheritedExport(typeof(IPlugin)),
    ExportMetadata("Name", "Logger"),
    ExportMetadata("Version", 4)]
public class Logger : IPlugin
{
    //Exports with contract type IPlugin and
    //metadata "Name" and "Version".
}

public class SuperLogger : Logger
{
    //Exports with contract type IPlugin and
    //metadata "Name" and "Version", exactly the same
    //as the Logger class.
}

[InheritedExport(typeof(IPlugin)),
    ExportMetadata("Status", "Green")]
public class MegaLogger : Logger        {
    //Exports with contract type IPlugin and
    //metadata "Status" only. Re-declaring
    //the attribute replaces all metadata.
}
```

在重新声明 `InheritedExport` 特性以重写元数据时，请确保协定类型相同。 （在前面的示例中，`IPlugin` 为协定类型。）如果协定类型不同，则第二个特性将创建另一个独立于部件的导出（而不是重写）。 通常，这意味着你必须在重写 `InheritedExport` 特性时显式指定协定类型，如前面的示例所示。

由于无法直接实例化接口，因此通常无法用 `Export` 或 `Import` 特性来修饰接口。 不过，可以在接口级别用 `InheritedExport` 特性修饰接口，并且任何实现类将随任何关联的元数据一起继承该导出。 但是，接口本身将不可用作部件。

<a name="custom_export_attributes"></a>

## <a name="custom-export-attributes"></a>自定义导出特性

可以对基本导出特性 `Export` 和 `InheritedExport`进行扩展，以包括元数据作为特性属性。 在将类似的元数据应用于多个部件或创建元数据特性的继承树时，此方法十分有用。

自定义特性可以指定协定类型、协定名称或任何其他元数据。 为了定义自定义特性，必须使用 `ExportAttribute` 特性来修饰继承自 `InheritedExportAttribute`（或 `MetadataAttribute` ）的类。 下面的类定义一个自定义特性。

```vb
<MetadataAttribute()>
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=false)>
Public Class MyAttribute
    Inherits ExportAttribute

    Public Property MyMetadata As String

    Public Sub New(ByVal myMetadata As String)
        MyBase.New(GetType(IMyAddin))

        myMetadata = myMetadata
    End Sub

End Class
```

```csharp
[MetadataAttribute]
[AttributeUsage(AttributeTargets.Class, AllowMultiple=false)]
public class MyAttribute : ExportAttribute
{
    public MyAttribute(string myMetadata)
        : base(typeof(IMyAddin))
    {
        MyMetadata = myMetadata;
    }

    public string MyMetadata { get; private set; }
}
```

此类定义一个名为 `MyAttribute` 的自定义特性，具有协定类型 `IMyAddin` 和某些名为 `MyMetadata`的元数据。 用 `MetadataAttribute` 特性标记的类中的所有属性都将被视为自定义特性中定义的元数据。 下面两个声明等效。

```vb
<Export(GetType(IMyAddin))>
<ExportMetadata("MyMetadata", "theData")>
Public Property myAddin As MyAddin
```

```vb
<MyAttribute("theData")>
Public Property myAddin As MyAddin
```

```csharp
[Export(typeof(IMyAddin)),
    ExportMetadata("MyMetadata", "theData")]
public MyAddin myAddin { get; set; }
```

```csharp
[MyAttribute("theData")]
public MyAddin myAddin { get; set; }
```

在第一个声明中，协定类型和元数据是显式定义的。 在第二个声明中，协定类型和元数据在自定义特性中是隐式的。 特别是，在必须将大量的相同元数据（例如，作者或版权信息）应用于多个部件的情况下，使用自定义特性可以节约大量的时间和重复工作。 此外，可以创建自定义特性的继承树来为变体留出余地。

若要在自定义特性中创建可选元数据，你可以使用 `DefaultValue` 特性。 如果此特性应用于自定义特性类中的属性，它将指定修饰的属性是可选的，并且不必由导出程序提供。 如果未提供属性的值，则将为属性分配其属性类型的默认值（通常为 `null`、 `false`或 0。）

<a name="creation_policies"></a>

## <a name="creation-policies"></a>创建策略

当部件指定执行导入和组合时，组合容器将尝试查找匹配的导出。 如果它将导入与导出成功匹配，则导入成员将设置为导出的对象的实例。 导出部件的 *创建策略*控制此实例来源于何处。

两个可能的创建策略为： *共享* 和 *非共享*。 在具有该协定的部件的容器中，创建策略为“共享”的部件将在每个导入之间共享。 当组合引擎找到匹配项并且必须设置导入属性时，它只有在部件尚不存在时才会实例化部件的新副本；否则它将提供现有副本。 这意味着许多对象可能会引用相同部件。 此类部件不应依赖于可能会从许多地方更改的内部状态。 此策略适用于静态部件、提供服务的部件，以及消耗大量内存或其他资源的部件。

在每次找到部件的其中一个导出的匹配导入时，将会创建创建策略为“非共享”的部件。 因此，将为与部件的其中一个已导出协定匹配的容器中的每个导入实例化一个新副本。 这些副本的内部状态将不会共享。 此策略适用于每个导入需要其自己的内部状态的部件。

导入和导出都可从值 `Shared`、 `NonShared`或 `Any`中指定部件的创建策略。 导入和导出的默认值均为 `Any` 。 指定 `Shared` 或 `NonShared` 的导出将仅与指定相同值或指定 `Any`的导入匹配。 同样，指定 `Shared` 或 `NonShared` 的导入将仅与指定相同值或指定 `Any`的导出匹配。 就像其协定名称或协定类型不匹配的导入和导出一样，创建策略不兼容的导入和导出也不会被视为匹配。 如果导入和导出均指定 `Any`，或者未指定创建策略并默认为 `Any`，则创建策略将默认为“共享”。

以下示例说明了导入和导出均指定创建策略。 `PartOne` 未指定创建策略，因此默认值为 `Any`。 `PartTwo` 未指定创建策略，因此默认值为 `Any`。 由于导入和导出均默认为 `Any`，因此将共享 `PartOne` 。 `PartThree` 指定 `Shared` 创建策略，因此 `PartTwo` 和 `PartThree` 将共享 `PartOne`的同一副本。 `PartFour` 指定 `NonShared` 创建策略，因此 `PartFour` 在 `PartFive`中将为非共享。 `PartSix` 指定一个 `NonShared` 创建策略。 `PartFive` 和 `PartSix` 将各自收到 `PartFour`的单独副本。 `PartSeven` 指定一个 `Shared` 创建策略。 由于没有创建策略为 `PartFour` 的已导出 `Shared`，因此 `PartSeven` 导入不与任何内容匹配，并且将不会得到满足。

```vb
<Export()>
Public Class PartOne
    'The default creation policy for an export is Any.
End Class

Public Class PartTwo

    <Import()>
    Public Property partOne As PartOne

    'The default creation policy for an import is Any.
    'If both policies are Any, the part will be shared.

End Class

Public Class PartThree

    <Import(RequiredCreationPolicy:=CreationPolicy.Shared)>
    Public Property partOne As PartOne

    'The Shared creation policy is explicitly specified.
    'PartTwo and PartThree will receive references to the
    'SAME copy of PartOne.

End Class

<Export()>
<PartCreationPolicy(CreationPolicy.NonShared)>
Public Class PartFour
    'The NonShared creation policy is explicitly specified.
End Class

Public Class PartFive

    <Import()>
    Public Property partFour As PartFour

    'The default creation policy for an import is Any.
    'Since the export's creation policy was explicitly
    'defined, the creation policy for this property will
    'be non-shared.

End Class

Public Class PartSix

    <Import(RequiredCreationPolicy:=CreationPolicy.NonShared)>
    Public Property partFour As PartFour

    'Both import and export specify matching creation
    'policies.  PartFive and PartSix will each receive
    'SEPARATE copies of PartFour, each with its own
    'internal state.

End Class

Public Class PartSeven

    <Import(RequiredCreationPolicy:=CreationPolicy.Shared)>
    Public Property partFour As PartFour

    'A creation policy mismatch.  Because there is no
    'exported PartFour with a creation policy of Shared,
    'this import does not match anything and will not be
    'filled.

End Class
```

```csharp
[Export]
public class PartOne
{
    //The default creation policy for an export is Any.
}

public class PartTwo
{
    [Import]
    public PartOne partOne { get; set; }

    //The default creation policy for an import is Any.
    //If both policies are Any, the part will be shared.
}

public class PartThree
{
    [Import(RequiredCreationPolicy = CreationPolicy.Shared)]
    public PartOne partOne { get; set; }

    //The Shared creation policy is explicitly specified.
    //PartTwo and PartThree will receive references to the
    //SAME copy of PartOne.
}

[Export]
[PartCreationPolicy(CreationPolicy.NonShared)]
public class PartFour
{
    //The NonShared creation policy is explicitly specified.
}

public class PartFive
{
    [Import]
    public PartFour partFour { get; set; }

    //The default creation policy for an import is Any.
    //Since the export's creation policy was explicitly
    //defined, the creation policy for this property will
    //be non-shared.
}

public class PartSix
{
    [Import(RequiredCreationPolicy = CreationPolicy.NonShared)]
    public PartFour partFour { get; set; }

    //Both import and export specify matching creation
    //policies.  PartFive and PartSix will each receive
    //SEPARATE copies of PartFour, each with its own
    //internal state.
}

public class PartSeven
{
    [Import(RequiredCreationPolicy = CreationPolicy.Shared)]
    public PartFour partFour { get; set; }

    //A creation policy mismatch.  Because there is no
    //exported PartFour with a creation policy of Shared,
    //this import does not match anything and will not be
    //filled.
}
```

<a name="life_cycle_and_disposing"></a>

## <a name="life-cycle-and-disposing"></a>生命周期和释放

由于部件承载于组合容器中，因此其生命周期可能比普通对象更复杂。 部件可实现两个重要的生命周期相关接口： `IDisposable` 和 `IPartImportsSatisfiedNotification`。

需要在关闭时执行工作的部件和需要释放资源的部件应照常为 .NET Framework 对象实现 `IDisposable`。 但是，由于容器创建并维护对部件的引用，因此只有拥有部件的容器才应对其调用 `Dispose` 方法。 容器本身实现 `IDisposable`，并且作为 `Dispose` 中其清理的一部分，它将对拥有的所有部件调用 `Dispose` 。 因此，当不再需要组合容器及其拥有的任何部件时，你应始终释放该组合容器。

对于生存期很长的组合容器，创建策略为“非共享”的部件的内存消耗可能会成为问题。 这些非共享部件可以多次创建，并且在容器本身被释放之前将不会得到释放。 为了应对这种情况，容器提供了 `ReleaseExport` 方法。 如果对非共享导出调用此方法，将会从组合容器中移除该导出并将其释放。 仅由移除的导出使用的部件以及树中更深层的诸如此类部件将也会被移除并得到释放。 通过这种方式，不必释放组合窗口本身即可回收资源。

`IPartImportsSatisfiedNotification` 包含一个名为 `OnImportsSatisfied`的方法。 当组合已完成并且部件的导入可供使用时，组合窗口将对实现接口的任何部件调用此方法。 部件是组合引擎创建，用于满足其他部件的导入。 在设置好部件的导入之前，你无法执行任何依赖于部件构造函数中的导入值或对这些值进行操作的初始化，除非已通过使用 `ImportingConstructor` 特性将这些指定为必备。 此方法通常为首选方法，但在某些情况下，构造函数注入可能不可用。 在这些情况下，可以在 `OnImportsSatisfied`中执行初始化，并且部件应实现 `IPartImportsSatisfiedNotification`。

## <a name="see-also"></a>请参阅

- [第 9 频道视频：使用 Managed Extensibility Framework 打开应用程序](https://channel9.msdn.com/events/TechEd/NorthAmerica/2009/DTL328)
- [第 9 频道视频：Managed Extensibility Framework (MEF) 2.0](https://channel9.msdn.com/posts/NET-45-Oleg-Lvovitch-and-Kevin-Ransom-Managed-Extensibility-Framework-MEF-20)
