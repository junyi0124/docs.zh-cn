---
title: 实现基于事件的异步模式
description: 了解如何在 .NET 中实现基于事件的异步模式 (EAP)。 EAP 是打包具有异步功能的类的标准方式。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET], asynchronous
- Asynchronous Pattern
- AsyncOperationManager class
- threading [.NET], asynchronous features
- components [.NET], asynchronous
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: 43402d19-8d30-426d-8785-1a4478233bfa
ms.openlocfilehash: 3f48f5d4f03928f8c9a2db2724e542be2b38fc63
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830345"
---
# <a name="implementing-the-event-based-asynchronous-pattern"></a>实现基于事件的异步模式

如果要编写的类有一些可能会带来明显延迟的操作，请考虑按照[基于事件的异步模式](event-based-asynchronous-pattern-overview.md)中的步骤操作，为它实现异步功能。

基于事件的异步模式提供了打包具有异步功能的类的标准化方式。 如果使用帮助程序类（如 <xref:System.ComponentModel.AsyncOperationManager>）进行实现，类可以在任何应用模型（包括 ASP.NET、控制台应用和 Windows 窗体应用）下正常运行。

有关实现基于事件的异步模式的示例，请参阅[如何：实现支持基于事件的异步模式的组件](component-that-supports-the-event-based-asynchronous-pattern.md)。

对于简单的异步操作，可能会发现 <xref:System.ComponentModel.BackgroundWorker> 组件非常适合。 有关 <xref:System.ComponentModel.BackgroundWorker> 的详细信息，请参阅[如何：在后台运行操作](/dotnet/desktop/winforms/controls/how-to-run-an-operation-in-the-background)。

以下列表介绍本主题中讨论的基于事件的异步模式的功能。

- 实现基于事件的异步模式的时机

- 命名异步方法

- 选择性地支持取消

- 选择性地支持 IsBusy 属性

- 选择性地为进度报告提供支持

- 选择性地为返回增量结果提供支持

- 处理方法中的 Out 和 Ref 参数

## <a name="opportunities-for-implementing-the-event-based-asynchronous-pattern"></a>实现基于事件的异步模式的时机

请在以下情况下，考虑实现基于事件的异步模式：

- 类的客户端不需要适用于异步操作的 <xref:System.Threading.WaitHandle> 和 <xref:System.IAsyncResult> 对象。也就是说，客户端需要生成轮询和 <xref:System.Threading.WaitHandle.WaitAll%2A> 或 <xref:System.Threading.WaitHandle.WaitAny%2A>。

- 你希望异步操作由客户端使用常见的事件/委托模型进行托管。

对于异步实现，任何操作都是候选项，但应优先考虑预计会产生较长延迟的操作。 最适合的操作是客户端在其中调用方法，并在完成时收到通知，无需进一步的干预。 其次是连续运行、定期向客户端通知进度、增量结果和状态更改的操作。

若要深入了解决定何时支持基于事件的异步模式，请参阅[决定何时实现基于事件的异步模式](deciding-when-to-implement-the-event-based-asynchronous-pattern.md)。

## <a name="naming-asynchronous-methods"></a>命名异步方法

对于要向其提供异步等效方法的每个同步方法 *MethodName*：

定义满足以下条件的 MethodNameAsync 方法：

- 返回 `void`。

- 采用与 *MethodName* 方法相同的参数。

- 接受多个调用。

（可选）定义与 MethodNameAsync 完全相同的 MethodNameAsync 重载，但要额外添加对象赋值参数（即 `userState`）。 如果已准备好管理方法的多个并发调用（在这种情况下，`userState` 值将传递回所有事件处理程序以区分方法的调用），可使用此方法。 也可以选择将其简单地作为存储用户状态以供以后检索的位置。

对于各个 MethodNameAsync 方法签名：

1. 在与方法相同的类中定义以下事件：

    ```vb
    Public Event MethodNameCompleted As MethodNameCompletedEventHandler
    ```

    ```csharp
    public event MethodNameCompletedEventHandler MethodNameCompleted;
    ```

2. 定义以下委托和 <xref:System.ComponentModel.AsyncCompletedEventArgs>。 这些可能会在类本身之外、但在相同命名空间中定义。

    ```vb
    Public Delegate Sub MethodNameCompletedEventHandler( _
        ByVal sender As Object, _
        ByVal e As MethodNameCompletedEventArgs)

    Public Class MethodNameCompletedEventArgs
        Inherits System.ComponentModel.AsyncCompletedEventArgs
    Public ReadOnly Property Result() As MyReturnType
    End Property
    ```

    ```csharp
    public delegate void MethodNameCompletedEventHandler(object sender,
        MethodNameCompletedEventArgs e);

    public class MethodNameCompletedEventArgs : System.ComponentModel.AsyncCompletedEventArgs
    {
        public MyReturnType Result { get; }
    }
    ```

    - 请确保 MethodNameCompletedEventArgs 类将它的成员公开为只读属性（而不是字段），因为字段会阻止数据绑定。

    - 请勿为不产生结果的方法定义任何派生自 <xref:System.ComponentModel.AsyncCompletedEventArgs> 的类。 直接使用 <xref:System.ComponentModel.AsyncCompletedEventArgs> 本身的实例即可。

      > [!NOTE]
      > 在可行且适当的情况下，重用委托和 <xref:System.ComponentModel.AsyncCompletedEventArgs> 类型是完全可以接受的。 在这种情况下，命名会与方法名称不一致，因为给定委托和 <xref:System.ComponentModel.AsyncCompletedEventArgs> 不限于单个方法。

## <a name="optionally-support-cancellation"></a>选择性地支持取消

如果你的类将支持取消异步操作，则应向客户端公开取消（如下所述）。 请注意，定义取消支持之前，需要确定两点：

- 你的类（包括将来预计要添加的内容），是否只具有一个支持取消操作的异步操作？

- 支持取消的异步操作是否能支持多个挂起操作？ 也就是说，MethodNameAsync 方法是否需要使用 `userState` 参数？它是否允许在等待任何操作完成前执行多个调用？

使用下表中的两个问题的答案来确定取消方法的签名。

### <a name="visual-basic"></a>Visual Basic

||支持多个并行操作|每次一个操作|
|------|------------------------------------------------|----------------------------------|
|整个类中具有一个异步操作|`Sub MethodNameAsyncCancel(ByVal userState As Object)`|`Sub MethodNameAsyncCancel()`|
|类中具有多个异步操作|`Sub CancelAsync(ByVal userState As Object)`|`Sub CancelAsync()`|

### <a name="c"></a>C\#

||支持多个并行操作|每次一个操作|
|------|------------------------------------------------|----------------------------------|
|整个类中具有一个异步操作|`void MethodNameAsyncCancel(object userState);`|`void MethodNameAsyncCancel();`|
|类中具有多个异步操作|`void CancelAsync(object userState);`|`void CancelAsync();`|

如果定义了 `CancelAsync(object userState)` 方法，客户端在选择状态值时必须小心，以使其能够区分对象上调用的所有异步方法，而不仅仅是在单个异步方法的所有调用之间进行区分。

决定命名单一异步操作版本 MethodNameAsyncCancel 的依据是，能否在设计环境（如 Visual Studio 的 IntelliSense）中更轻松地发现方法。 这会对相关的成员进行分组，将与异步功能无关的其他成员区分开来。 如果预计可能在后续版本中添加其他异步操作，最好定义 `CancelAsync`。

请勿在同一类中定义上表中的多个方法。 这将毫无意义，或者会由于方法的泛滥而使类接口变得混乱。

通常，这些方法会立即返回，并且操作实际上可能会/无法取消。 在 MethodNameCompleted 事件的事件处理程序中，MethodNameCompletedEventArgs 对象包含 `Cancelled` 字段，客户端可使用此字段来确定是否取消了操作。

请遵守[实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)中所述的取消语义。

## <a name="optionally-support-the-isbusy-property"></a>选择性地支持 IsBusy 属性

如果你的类不支持多个并发调用，请考虑公开 `IsBusy` 属性。 这样一来，开发人员可以确定能否运行 MethodNameAsync 方法，同时又不会捕获到 MethodNameAsync 方法抛出的异常。

请遵守[实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)中所述的 `IsBusy` 语义。

## <a name="optionally-provide-support-for-progress-reporting"></a>选择性地为进度报告提供支持

通常期望异步操作在其操作期间报告进度。 基于事件的异步模式提供了执行此操作的准则。

- 还可以选择性地定义由异步操作引发并在相应线程上调用的事件。 <xref:System.ComponentModel.ProgressChangedEventArgs> 对象随附整数值进度指示器，量程预计在 0 到 100 之间。

- 按照以下规则命名此事件：

  - 如果类具有多个异步操作（或预期将来版本中会包括多个异步操作），则命名为 `ProgressChanged`；

  - MethodNameProgressChanged：如果类包含单一异步操作。

  该命名方法与命名取消方法（如“选择性地支持取消”部分所述）相同。

此事件应使用 <xref:System.ComponentModel.ProgressChangedEventHandler> 委托签名和 <xref:System.ComponentModel.ProgressChangedEventArgs> 类。 或者，如果可以提供更多域专用进度指示器（例如，下载操作的读取字节数和总字节数），应定义 <xref:System.ComponentModel.ProgressChangedEventArgs> 的派生类。

请注意，无论类支持多少个异步方法，都只有一个 `ProgressChanged` 或 MethodNameProgressChanged 事件。 客户端应使用传递给 MethodNameAsync 方法的 `userState` 对象，以区分多个并发操作的进度更新。

可能出现多个操作支持进度，并且每个操作返回不同的进度指示器的情况。 在这种情况下，不合适支持单个 `ProgressChanged` 事件，你可能需要考虑支持多个 `ProgressChanged` 事件。 在这种情况下，对每个 MethodNameAsync 方法使用 MethodNameProgressChanged 命名模式。

请遵守[实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)中所述的进度报告语义。

## <a name="optionally-provide-support-for-returning-incremental-results"></a>选择性地为返回增量结果提供支持

有时异步操作可以在完成之前返回增量结果。 可以通过多种方式支持此方案。 下面是一些示例。

### <a name="single-operation-class"></a>单一操作类

如果你的类仅支持单一异步操作，并且能够返回增量结果，则可以：

- 将 <xref:System.ComponentModel.ProgressChangedEventArgs> 类型扩展为包含增量结果数据，并定义包含此扩展数据的 MethodNameProgressChanged 事件。

- 若有要报告的增量结果，抛出此 MethodNameProgressChanged 事件。

此解决方案特别适用于单一异步操作类，因为发生的同一事件可以对“所有操作”返回增量结果，与 MethodNameProgressChanged 事件一样。

### <a name="multiple-operation-class-with-homogeneous-incremental-results"></a>使用同类增量结果的多操作类

在这种情况下，你的类支持多个异步方法，每个方法都能够返回增量结果，并且这些增量结果具有相同的数据类型。

请遵循上述适用于单一操作类的模型，因为同一 <xref:System.EventArgs> 结构适用于所有增量结果。 定义 `ProgressChanged` 事件，而不是 MethodNameProgressChanged 事件，因为它适用于多个异步方法。

### <a name="multiple-operation-class-with-heterogeneous-incremental-results"></a>使用不同类增量结果的多操作类

如果你的类支持多个异步方法，每个方法返回不同类型的数据，则应该：

- 将增量结果报告与进度报告分开。

- 单独定义针对每个异步方法有适当 <xref:System.EventArgs> 的 MethodNameProgressChanged 事件，以处理此方法的增量结果数据。

按照[实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)所述，在适当线程上调用事件处理程序。

## <a name="handling-out-and-ref-parameters-in-methods"></a>处理方法中的 Out 和 Ref 参数

虽然一般情况下，建议不要在 .NET 中使用 `out` 和 `ref`，但以下是使用它们时要遵循的规则：

给定同步方法 *MethodName*：

- MethodName 的 `out` 参数不应为 MethodNameAsync 的一部分。 它们应是 MethodNameCompletedEventArgs 的一部分，与 MethodName 中的相当参数同名（除非有更合适的名称）。

- MethodName 的 `ref` 参数应显示为 MethodNameAsync 的一部分，并显示为 MethodNameCompletedEventArgs 的一部分，与 MethodName 中的相当参数同名（除非有更合适的名称）。

例如，给定：

```vb
Public Function MethodName(ByVal arg1 As String, ByRef arg2 As String, ByRef arg3 As String) As Integer
```

```csharp
public int MethodName(string arg1, ref string arg2, out string arg3);
```

异步方法及其 <xref:System.ComponentModel.AsyncCompletedEventArgs> 类如下所示：

```vb
Public Sub MethodNameAsync(ByVal arg1 As String, ByVal arg2 As String)

Public Class MethodNameCompletedEventArgs
    Inherits System.ComponentModel.AsyncCompletedEventArgs
    Public ReadOnly Property Result() As Integer
    End Property
    Public ReadOnly Property Arg2() As String
    End Property
    Public ReadOnly Property Arg3() As String
    End Property
End Class
```

```csharp
public void MethodNameAsync(string arg1, string arg2);

public class MethodNameCompletedEventArgs : System.ComponentModel.AsyncCompletedEventArgs
{
    public int Result { get; };
    public string Arg2 { get; };
    public string Arg3 { get; };
}
```

## <a name="see-also"></a>请参阅

- <xref:System.ComponentModel.ProgressChangedEventArgs>
- <xref:System.ComponentModel.AsyncCompletedEventArgs>
- [如何：实现支持基于事件的异步模式的组件](component-that-supports-the-event-based-asynchronous-pattern.md)
- [如何：在后台运行操作](/dotnet/desktop/winforms/controls/how-to-run-an-operation-in-the-background)
- [如何：实现使用后台操作的窗体](/dotnet/desktop/winforms/controls/how-to-implement-a-form-that-uses-a-background-operation)
- [确定何时实现基于事件的异步模式](deciding-when-to-implement-the-event-based-asynchronous-pattern.md)
- [实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)
