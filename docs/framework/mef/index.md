---
title: Managed Extensibility Framework (MEF)
description: 探索 Managed Extensibility Framework (MEF)，它可便于应用程序开发人员发现并使用扩展，而无需在 .NET 4 或更高版本中进行配置。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Managed Extensibility Framework, overview
- MEF, overview
ms.assetid: 6c61b4ec-c6df-4651-80f1-4854f8b14dde
ms.openlocfilehash: b743a26dd401e7015c588be2a197551aa891a687
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555570"
---
# <a name="managed-extensibility-framework-mef"></a>Managed Extensibility Framework (MEF)

该话题为在 .NET Framework 4 中引入的 Managed Extensibility Framework 提供了一个概述。

## <a name="what-is-mef"></a>什么是 MEF？

Managed Extensibility Framework (MEF) 是用于创建可扩展的轻量级应用程序的库。 它让应用程序开发人员得以发现和使用扩展且无需配置。 它还让扩展开发人员得以轻松地封装代码并避免脆弱的紧密依赖性。 MEF 让扩展不仅可在应用程序内重复使用，还可以跨程序重复使用。

## <a name="the-problem-of-extensibility"></a>扩展性问题

想象你是必须为扩展性提供支持的大型应用程序的设计者。 你的应用程序必须包含可能很多的较小组件，且其负责创建和运行较小组件。

解决问题最简单的方法就是将组件作为源代码包括在你的应用程序并直接从代码中调用它们。 这有很多明显缺点。 最重要的是，你无法在未调试源代码的情况下添加新组件，这是一项可能被接受的限制（例如在 Web 应用程序中），但不适用于客户端应用程序。 同样造成问题的是，你无法访问组件的源代码，因为组件可能由第三方进行了发展，同时由于同样的原因你不能允许第三方访问你的源代码。

一个稍许复杂的方法为提供扩展点或接口来允许应用程序和组件间的分离。 在此模块下，你可能提供组件可实现的接口以及使接口能与应用程序交互使用的 API。 这解决了要求源代码访问的问题，但它仍然存在自身问题。

因为应用程序缺少自行发现组件的能力，所以必须明确对其指出可用且应加载的组件。 这通常通过在配置文件中明确记录可用组件来完成。 这意味着确保组件正确变成了维护问题，尤其当要求执行更新的人员是最终用户而非开发人员时。

此外，组件能够与另外的组件进行沟通，除了通过应用程序自身的严格定义的通道。 通常不会出现应用程序设计者未预测到特定通信的需求的情况。

最终，组件开发人员必须接受包含他们实现的接口的程序集所在的硬依赖项。 这使得在一个以上的应用程序中使用组件变得困难，且它还会在你创建组件的测试框架时造成问题。

## <a name="what-mef-provides"></a>MEF 所含内容

MEF 通过组合提供了一种隐式发现它们的方法，而不是明确记录可用组件。 MEF 组件（称为一个部件），以声明方式详细说明了其依赖项（称为导入）及其可提供的功能（称为导出）。 当创建一个部分时，MEF 组合引擎利用从其他部分获得的功能满足其导入需要。

该方法解决了在之前部分中讨论的问题。 因为 MEF 部分以声明方式详细说明了其可在运行时发现自身的功能，这意味着应用程序不使用硬编码引用或脆弱的配置文件也能够使用 MEF 部分。 MEF 让应用程序得以通过元数据发现和检查部分，而无须将部分实例化甚至于加载其程序集。 因此，无须仔细说明应当何时加载扩展以及如何加载。

除了它提供的导出之外，部件能够指出其导入（将由其他部件填写）。 这不仅使部件之间的通信成为可能还使其变得简单，并且实现了良好的代码分解。 例如，对于许多组件都很普通的服务可被分解为单独部分，可轻易地进行调试和替换。

因为 MEF 模块在特定的应用程序集上没有硬依赖项，所以它允许扩展在不同应用程序之间重复使用。 这使得开发用于测试扩展组件的测试工具（独立于应用程序）变得简单。

通过使用 MEF 编写的可扩展应用程序声明了一个可由扩展组件填写的输入，它还可能为了向扩展揭示应用程序服务而声明导出。 每个扩展组件都声明一个导出，还有可能声明导入。 通过此方法，扩展组件自身为自动可扩展。

## <a name="where-mef-is-available"></a>MEF 适用场景

MEF 是 .NET Framework 4 的组成部分，适用于所有使用 .NET Framework 的地方。 可以在客户端应用程序（不论其是否使用 Windows 窗体或其他技术）或使用 ASP.NET 的服务端应用程序里使用 MEF。

## <a name="mef-and-maf"></a>MEF 和 MAF

.NET Framework 的早期版本引入了 Managed Add-in Framework (MAF)，旨在让应用程序隔离和托管扩展。 MAF 与 MEF 相比，MAF 的重点级别较高，它关注扩展隔离和程序集加载及卸载，而 MEF 则关注可发现性、可扩展性和可移植性。 这两个框架可以平稳地相互操作，且一个单独的应用程序可以利用这两者。

## <a name="simplecalculator-an-example-application"></a>简单计算器：一个示例应用程序

查看 MEF 能做什么最简单的方法就是构建一个简单的 MEF 应用程序。 在此示例中，你构建了一个叫作简单计算器的非常简单的计算器。 简单计算器旨在创建一个接受形式为“5+3”或“6-2”的基础运算命令然后返回正确答案的控制台应用程序。 使用 MEF，能够添加新的操作人员而无须更改应用程序代码。

要下载此示例的完整代码，请参阅 [SimpleCalculator 示例 (Visual Basic)](/samples/dotnet/samples/simple-calculator-vb/)。

> [!NOTE]
> 简单计算器旨在演示 MEF 的概念和语法而非必须为其使用提供现实情况。 许多将从 MEF 的功能受益最大的应用程序比简单计算器更加复杂。 有关更多扩展性示例，请参阅 GitHub 上的 [Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef)。

- 首先，在 Visual Studio 中，创建一个新的控制台应用程序项目，并将其命名为 `SimpleCalculator`。

- 添加对 MEF 所驻留的 `System.ComponentModel.Composition` 程序集的引用。

- 打开 Module1.vb 或 Program.cs，再为 `System.ComponentModel.Composition` 和 `System.ComponentModel.Composition.Hosting` 添加 `Imports` 或 `using` 语句 。 这两个名称空间包含你开发可扩展应用程序将需要的 MEF 类型。

- 如果使用的是 Visual Basic，则将 `Public` 关键字添加到声明 `Module1` 模块的行中。

## <a name="composition-container-and-catalogs"></a>撰写容器和目录

MEF 组合模型的核心是包含所有可用部件并执行组合的组合容器。 组合是对导入到导出进行的匹配。 撰写容器最常用的类型是 <xref:System.ComponentModel.Composition.Hosting.CompositionContainer>，它将用于简单计算器。

如果使用的是 Visual Basic，则在 Module1.vb 中添加一个名为 `Program` 的公共类。

将以下行添加到 Module1.vb 或 Program.cs 中的 `Program` 类 ：

```vb
Dim _container As CompositionContainer
```

```csharp
private CompositionContainer _container;
```

为了发现对其可用的部件，组合容器使用目录。 目录是让你从某些来源中发现可用部件的对象。 MEF 提供目录来发现来自提供的类型、程序集或目录的部件。 应用程序开发人员可以轻松地创建新的目录来发现来自其他来源（例如 Web 服务）的部件。

将下面的构造函数添加到 `Program` 类中:

```vb
Public Sub New()
    ' An aggregate catalog that combines multiple catalogs.
     Dim catalog = New AggregateCatalog()

    ' Adds all the parts found in the same assembly as the Program class.
    catalog.Catalogs.Add(New AssemblyCatalog(GetType(Program).Assembly))

    ' Create the CompositionContainer with the parts in the catalog.
    _container = New CompositionContainer(catalog)

    ' Fill the imports of this object.
    Try
        _container.ComposeParts(Me)
    Catch ex As CompositionException
        Console.WriteLine(ex.ToString)
    End Try
End Sub
```

```csharp
private Program()
{
    // An aggregate catalog that combines multiple catalogs.
    var catalog = new AggregateCatalog();
    // Adds all the parts found in the same assembly as the Program class.
    catalog.Catalogs.Add(new AssemblyCatalog(typeof(Program).Assembly));

    // Create the CompositionContainer with the parts in the catalog.
    _container = new CompositionContainer(catalog);

    // Fill the imports of this object.
    try
    {
        this._container.ComposeParts(this);
    }
    catch (CompositionException compositionException)
    {
        Console.WriteLine(compositionException.ToString());
   }
}
```

对 <xref:System.ComponentModel.Composition.AttributedModelServices.ComposeParts%2A> 的调用通知撰写容器撰写一组特定组件，在此情况下即为 `Program` 的当前实例。 但此时不会发生任何情况，因为 `Program` 没有需要填充的导入。

## <a name="imports-and-exports-with-attributes"></a>具有属性的导入和导出

首先，你已向 `Program` 导入一个计算器。 这使得用户界面顾虑（例如将转到 `Program` 的控制台输入和导出）从计算机逻辑中隔离出来。

将下面的代码添加到 `Program` 类中:

```vb
<Import(GetType(ICalculator))>
Public Property calculator As ICalculator
```

```csharp
[Import(typeof(ICalculator))]
public ICalculator calculator;
```

请注意，`calculator` 对象的声明并非异常，但它使用了 <xref:System.ComponentModel.Composition.ImportAttribute> 属性进行修饰。 属性声明了某些操作为导入；在撰写对象时，它将由组合引擎进行填写。

每个导入都有一个决定其与什么导出相匹配的协定。 协定可以是显示指定的字符串，还可由 MEF 在给定类型中自动产生，在此情况下即为界面 `ICalculator`。 任一使用匹配协定进行声明的导出将完成此导入。 请注意，尽管 `calculator` 对象的类型实际上为 `ICalculator`，但对此并无要求。 协定独立于导入对象的类型。 （在此情况下，可以略去 `typeof(ICalculator)`。 除非你明确指定，否则 MEF 会自动假定协定基于导入的类型。）

将此非常简单的界面添加到模块或 `SimpleCalculator` 名称空间中：

```vb
Public Interface ICalculator
    Function Calculate(input As String) As String
End Interface
```

```csharp
public interface ICalculator
{
    String Calculate(String input);
}
```

既然你已经定义了 `ICalculator`，则需要一个执行它的类。 将下面的类添加到模块或 `SimpleCalculator` 命名空间中：

```vb
<Export(GetType(ICalculator))>
Public Class MySimpleCalculator
   Implements ICalculator

End Class
```

```csharp
[Export(typeof(ICalculator))]
class MySimpleCalculator : ICalculator
{

}
```

下面是将与 `Program` 中的导入相匹配的导出。 为了使导出与导入相匹配，导出必须使用相同的协定。 在基于 `typeof(MySimpleCalculator)` 的协定下导出将生成不匹配，也不会填写导入，因此协定需要完全匹配。

鉴于此程序集中的所有可用部件将填充撰写容器，`MySimpleCalculator` 部件将可使用。 当用于 `Program` 的构造函数在 `Program` 上执行组合时，`MySimpleCalculator` 对象（将处于此目的进行创建）将填写其导入。

用户界面层（`Program`）无需了解其他内容。 因此，可以填写 `Main` 方法中的用户界面逻辑的剩余部分。

将以下代码添加到 `Main` 方法中：

```vb
Sub Main()
    ' Composition is performed in the constructor.
    Dim p As New Program()
    Dim s As String
    Console.WriteLine("Enter Command:")
    While (True)
        s = Console.ReadLine()
        Console.WriteLine(p.calculator.Calculate(s))
    End While
End Sub
```

```csharp
static void Main(string[] args)
{
    // Composition is performed in the constructor.
    var p = new Program();
    string s;
    Console.WriteLine("Enter Command:");
    while (true)
    {
        s = Console.ReadLine();
        Console.WriteLine(p.calculator.Calculate(s));
    }
}
```

 该代码仅读出一行导入并在结尾调用 `Calculate` 的 `ICalculator` 函数（由代码写回控制台）。 这是你在 `Program` 中所需的全部代码。 剩余工作将在部件中进行。

## <a name="further-imports-and-importmany"></a>进一步的导入和 ImportMany

为了使简单计算器获得可扩性，它需要导入一组操作。 一般 <xref:System.ComponentModel.Composition.ImportAttribute> 属性由且只能由 <xref:System.ComponentModel.Composition.ExportAttribute> 填写。 如果可用数目超过一，则组合引擎生成错误。 可以使用 <xref:System.ComponentModel.Composition.ImportManyAttribute> 属性来创建可由任意数目的导出填写的导入。

将下列操作属性添加到 `MySimpleCalculator` 类：

```vb
<ImportMany()>
Public Property operations As IEnumerable(Of Lazy(Of IOperation, IOperationData))
```

```csharp
[ImportMany]
IEnumerable<Lazy<IOperation, IOperationData>> operations;
```

<xref:System.Lazy%602> 是由 MEF 提供来保存对导出的间接引用的类型。 除了导出对象本身，还可以获取导出元数据，或描述导出对象的信息。 每个 <xref:System.Lazy%602> 都包含一个代表实际操作的 `IOperation` 对象和一个代表元数据的 `IOperationData` 对象。

将下列简单界面添加到模块或 `SimpleCalculator` 名称空间中：

```vb
Public Interface IOperation
    Function Operate(left As Integer, right As Integer) As Integer
End Interface

Public Interface IOperationData
    ReadOnly Property Symbol As Char
End Interface
```

```csharp
public interface IOperation
{
     int Operate(int left, int right);
}

public interface IOperationData
{
    Char Symbol { get; }
}
```

 在此情况下，每项操作的元数据是表示该操作的符号，例如 +、-、\*。 为了运行加法操作，将下面的类添加到模块或 `SimpleCalculator` 名称空间中：

```vb
<Export(GetType(IOperation))>
<ExportMetadata("Symbol", "+"c)>
Public Class Add
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left + right
    End Function
End Class
```

```csharp
[Export(typeof(IOperation))]
[ExportMetadata("Symbol", '+')]
class Add: IOperation
{
    public int Operate(int left, int right)
    {
        return left + right;
    }
}
```

<xref:System.ComponentModel.Composition.ExportAttribute> 属性函数和之前一致。 <xref:System.ComponentModel.Composition.ExportMetadataAttribute> 属性将采用名称值对形式的元数据附加到导出。 尽管 `Add` 类执行 `IOperation`，但执行 `IOperationData` 的类并无明确定义。 相反，由 MEF 隐式创建的类具有基于提供的元数据名称的属性。 （这是用于访问 MEF 中的元数据的方法之一。）

MEF 中的组合是递归的。 你明确撰写了 `Program` 对象（导入了结果为 `ICalculator` 类型的 `MySimpleCalculator`）。 反过来，`MySimpleCalculator` 导入一组 `IOperation` 对象，且该导入将在创建 `MySimpleCalculator` 时进行填写，同时进行 `Program` 的导入。 如果 `Add` 类声明了一项进一步的导入，则该导入也须进行填写，等等。 任何将空缺结果留在撰写错误中的导入。 （然而，有可能声明导入为可选的或为其分配默认值。）

## <a name="calculator-logic"></a>计算器逻辑

准备好这些部件，剩下的就是计算器逻辑本身了。 将下面的代码添加到 `MySimpleCalculator` 类来执行 `Calculate` 方法中：

```vb
Public Function Calculate(input As String) As String Implements ICalculator.Calculate
    Dim left, right As Integer
    Dim operation As Char
    ' Finds the operator.
    Dim fn = FindFirstNonDigit(input)
    If fn < 0 Then
        Return "Could not parse command."
    End If
    operation = input(fn)
    Try
        ' Separate out the operands.
        left = Integer.Parse(input.Substring(0, fn))
        right = Integer.Parse(input.Substring(fn + 1))
    Catch ex As Exception
        Return "Could not parse command."
    End Try
    For Each i As Lazy(Of IOperation, IOperationData) In operations
        If i.Metadata.symbol = operation Then
            Return i.Value.Operate(left, right).ToString()
        End If
    Next
    Return "Operation not found!"
End Function
```

```csharp
public String Calculate(string input)
{
    int left;
    int right;
    char operation;
    // Finds the operator.
    int fn = FindFirstNonDigit(input);
    if (fn < 0) return "Could not parse command.";

    try
    {
        // Separate out the operands.
        left = int.Parse(input.Substring(0, fn));
        right = int.Parse(input.Substring(fn + 1));
    }
    catch
    {
        return "Could not parse command.";
    }

    operation = input[fn];

    foreach (Lazy<IOperation, IOperationData> i in operations)
    {
        if (i.Metadata.Symbol.Equals(operation)) return i.Value.Operate(left, right).ToString();
    }
    return "Operation Not Found!";
}
```

初始步骤将输入字符串解析到左右操作数和一个运算符字符。 在 `foreach` 循环中，`operations` 集合的每个成员都要检查。 这些对象是 <xref:System.Lazy%602> 类型，其元数据值和导出对象可分别使用 <xref:System.Lazy%602.Metadata%2A> 属性和<xref:System.Lazy%601.Value%2A> 属性进行访问。 在此情况下，如果发现 `Symbol` 对象的 `IOperationData` 属性为匹配项，则计算器调用 `Operate` 对象的 `IOperation` 方法并返回结果。

还需要返回字符串中的首个非数字字符位置的 helper 方法来完成计算器。 将下面的 helper 方法添加到 `MySimpleCalculator` 类中：

```vb
Private Function FindFirstNonDigit(s As String) As Integer
    For i = 0 To s.Length - 1
        If Not Char.IsDigit(s(i)) Then Return i
    Next
    Return -1
End Function
```

```csharp
private int FindFirstNonDigit(string s)
{
    for (int i = 0; i < s.Length; i++)
    {
        if (!Char.IsDigit(s[i])) return i;
    }
    return -1;
}
```

现在，应该可以编译和运行项目了。 确保在 Visual Basic 里将 `Public` 关键字添加到 `Module1` 中。 在控制台窗口中键入“5+3”等加法运算计算器会返回结果。 任何其他运算符会导致“找不到运算!” 消息。

## <a name="extending-simplecalculator-using-a-new-class"></a>采用新类扩展 SimpleCalculator

既然计算器能够运行，则可以简单地添加新操作。 将下面的类添加到模块或 `SimpleCalculator` 命名空间中：

```vb
<Export(GetType(IOperation))>
<ExportMetadata("Symbol", "-"c)>
Public Class Subtract
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left - right
    End Function
End Class
```

```csharp
[Export(typeof(IOperation))]
[ExportMetadata("Symbol", '-')]
class Subtract : IOperation
{
    public int Operate(int left, int right)
    {
        return left - right;
    }
}
```

编译并运行该项目。 键入“5-3”等减法运算。 现在，计算器既支持减法运算也支持加法运算。

## <a name="extending-simplecalculator-using-a-new-assembly"></a>采用新程序集扩展 SimpleCalculator

将类添加到源代码非常简单，但 MEF 提供了观察应用程序部件源之外的内容的功能。 为了演示它，将需要通过添加 <xref:System.ComponentModel.Composition.Hosting.DirectoryCatalog> 来修改简单计算器从而为部件搜索目录以及其自身的程序集。

将叫作 `Extensions` 的新目录添加到 SimpleCalculator 项目中。 确保将其添加到项目级别（而非解决方法级别）中。 然后将新类库项目添加到叫作 `ExtendedOperations` 的解决方法中。 新项目将编译为一个单独的程序集。

打开 ExtendedOperations 项目的项目属性设计器，然后单击“编译”或“生成”选项卡。更改“生成输出路径”或“输出路径”以指向扩展目录，它位于 SimpleCalculator 项目目录中 (..\SimpleCalculator\Extensions\\) 。

 在 Module1.vb 或 Program.cs 中，将以下行添加到 `Program` 构造函数中 ：

```vb
catalog.Catalogs.Add(New DirectoryCatalog("C:\SimpleCalculator\SimpleCalculator\Extensions"))
```

```csharp
catalog.Catalogs.Add(new DirectoryCatalog("C:\\SimpleCalculator\\SimpleCalculator\\Extensions"));
```

将示例路径替换为指向扩展目录的路径。 （此绝对路径仅供调试目的使用。 在生产应用程序中，应该使用相对路径。）现在，<xref:System.ComponentModel.Composition.Hosting.DirectoryCatalog> 将把在扩展目录中的所有程序集中发现的部件添加到撰写容器中。

在扩展操作项目中，将参考添加到简单计算器和 System.ComponentModel.Composition 中。 在扩展操作类文件中，添加一个 System.ComponentModel.Composition 的 `Imports` 或 `using` 语句。 在 Visual Basic 中，也添加一个简单计算器的 `Imports` 语句。 然后将下面的类添加到扩展操作类文件中：

```vb
<Export(GetType(SimpleCalculator.IOperation))>
<ExportMetadata("Symbol", "%"c)>
Public Class Modulo
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left Mod right
    End Function
End Class
```

```csharp
[Export(typeof(SimpleCalculator.IOperation))]
[ExportMetadata("Symbol", '%')]
public class Mod : SimpleCalculator.IOperation
{
    public int Operate(int left, int right)
    {
        return left % right;
    }
}
```

请注意，为了使协定匹配，<xref:System.ComponentModel.Composition.ExportAttribute> 属性必须与 <xref:System.ComponentModel.Composition.ImportAttribute> 的类型相同。

编译并运行该项目。 测试新的 Mod (%) 运算符。

## <a name="conclusion"></a>结束语

本主题包括 MEF 的基础概念。

- 部件，目录和撰写容器

     部件和撰写容器是 MEF 应用程序的基础构造块。 部件是所有导入或导出值（最多包含自己）的对象。 目录提供了一组来自特定源的部件。 撰写容器使用目录提供的部件来执行将导入绑定到导出的组合。

- 导入和导出

     导入和导出是组件进行通信的方式。 组件使用导入指定对特定值或对象的需求，使用导出指定值的可用性。 每个导入都通过其协定匹配了一组导出。

## <a name="next-steps"></a>后续步骤

要下载此示例的完整代码，请参阅 [SimpleCalculator 示例 (Visual Basic)](/samples/dotnet/samples/simple-calculator-vb/)。

 有关详细信息和代码示例，请参阅 [Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef)。 有关 MEF 类型的列表，请参阅 <xref:System.ComponentModel.Composition?displayProperty=nameWithType> 命名空间。
