---
title: 基于事件的异步模式概述
description: 了解 .NET 中基于事件的异步模式 (EAP)，它具有多线程应用程序的优点，并且可以简化部分设计。
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
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: 792aa8da-918b-458e-b154-9836b97735f3
ms.openlocfilehash: eb7680607c1def7cdc0dd5670b594e2ee1a6bfff
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678111"
---
# <a name="event-based-asynchronous-pattern-overview"></a>基于事件的异步模式概述

那些同时执行多项任务、但仍能响应用户交互的应用程序通常需要实施一种使用多线程的设计方案。 <xref:System.Threading> 命名空间提供了创建高性能多线程应用程序所必需的所有工具，但要想有效地使用这些工具，需要有丰富的使用多线程软件工程的经验。 对于相对简单的多线程应用程序，<xref:System.ComponentModel.BackgroundWorker> 组件提供了一个简单的解决方案。 对于更复杂的异步应用程序，请考虑实现一个符合基于事件的异步模式的类。  
  
 基于事件的异步模式具有多线程应用程序的优点，同时隐藏了多线程设计中固有的许多复杂问题。 使用支持此模式的类，你将能够：  
  
- “在后台”执行耗时任务（例如下载和数据库操作），但不会中断你的应用程序。  
  
- 同时执行多个操作，每个操作完成时都会接到通知。  
  
- 等待资源变得可用，但不会停止（“阻止”）你的应用程序。  
  
- 使用熟悉的事件和委托模型与挂起的异步操作通信。 若要详细了解如何使用事件处理程序和委托，请参阅[事件](../events/index.md)。  
  
 支持基于事件的异步模式的类将具有一个或多个命名为 _MethodName_**Async** 的方法。 这些方法可能会创建同步版本的镜像，这些同步版本会在当前线程上执行相同的操作。 该类还可能具有 _MethodName_**Completed** 事件，并且可能会具有 _MethodName_**AsyncCancel**（或只是 **CancelAsync**）方法。  
  
 <xref:System.Windows.Forms.PictureBox> 是一个支持基于事件的异步模式的典型组件。 你可以通过调用其 <xref:System.Windows.Forms.PictureBox.Load%2A> 方法来同步下载图像，但是如果图像很大，或者网络连接很慢，应用程序将停止响应，直到下载操作完成并且对 <xref:System.Windows.Forms.PictureBox.Load%2A> 的调用返回后才会继续执行。  
  
 如果你想要应用程序在加载图像时保持运行，你可以调用 <xref:System.Windows.Forms.PictureBox.LoadAsync%2A> 方法，处理 <xref:System.Windows.Forms.PictureBox.LoadCompleted> 事件，这与你处理任何其他事件一样。 调用 <xref:System.Windows.Forms.PictureBox.LoadAsync%2A> 方法时，你的应用程序将继续运行，而下载操作将在另一个线程上（“在后台”）继续。 图像加载操作完成时，将调用你的事件处理程序，并且你的事件处理程序可以检查 <xref:System.ComponentModel.AsyncCompletedEventArgs> 参数以确定该下载是否已成功完成。  
  
 基于事件的异步模式要求异步操作可以取消，并且 <xref:System.Windows.Forms.PictureBox> 控件使用其 <xref:System.Windows.Forms.PictureBox.CancelAsync%2A> 方法支持此要求。 调用 <xref:System.Windows.Forms.PictureBox.CancelAsync%2A> 会提交一个停止挂起的下载的请求，任务取消时会引发 <xref:System.Windows.Forms.PictureBox.LoadCompleted> 事件。  
  
> [!CAUTION]
> 下载有可能刚好在发出 <xref:System.Windows.Forms.PictureBox.CancelAsync%2A> 请求时完成，因此 <xref:System.ComponentModel.AsyncCompletedEventArgs.Cancelled%2A> 可能没有反映取消请求。 这称为“争用条件”，也是多线程编程中的常见问题。 若要详细了解多线程编程中的问题，请参阅[托管线程最佳做法](../threading/managed-threading-best-practices.md)。  
  
## <a name="characteristics-of-the-event-based-asynchronous-pattern"></a>基于事件的异步模式的特征  

 基于事件的异步模式可以采用多种形式，具体取决于某个特定类支持的操作的复杂程度。 最简单的类可能包含一个 _MethodName_**Async** 方法和对应的 M _MethodName_**Completed** 事件。 更复杂的类可能包含多个 _MethodName_**Async** 方法（每个方法具有一个对应的 _MethodName_**Completed** 事件），以及这些方法的同步版本。 这些类有选择性地分别支持各种异步方法的取消、进度报告和增量结果。  
  
 异步方法可能还支持多个挂起的调用（多个并发调用），允许你的代码在此方法完成其他挂起的操作之前调用此方法任意多次。 若要正确处理此种情况，需要让你的应用程序能够跟踪各个操作的完成。  
  
### <a name="examples-of-the-event-based-asynchronous-pattern"></a>基于事件的异步模式示例  

 <xref:System.Media.SoundPlayer> 和 <xref:System.Windows.Forms.PictureBox> 组件表示基于事件的异步模式的简单实现。 <xref:System.Net.WebClient> 和 <xref:System.ComponentModel.BackgroundWorker> 组件表示基于事件的异步模式的更复杂实现。  
  
 下面是一个符合此模式的类声明示例：  
  
```vb  
Public Class AsyncExample  
    ' Synchronous methods.  
    Public Function Method1(ByVal param As String) As Integer
    Public Sub Method2(ByVal param As Double)
  
    ' Asynchronous methods.  
    Overloads Public Sub Method1Async(ByVal param As String)
    Overloads Public Sub Method1Async(ByVal param As String, ByVal userState As Object)
    Public Event Method1Completed As Method1CompletedEventHandler  
  
    Overloads Public Sub Method2Async(ByVal param As Double)
    Overloads Public Sub Method2Async(ByVal param As Double, ByVal userState As Object)
    Public Event Method2Completed As Method2CompletedEventHandler  
  
    Public Sub CancelAsync(ByVal userState As Object)
  
    Public ReadOnly Property IsBusy () As Boolean  
  
    ' Class implementation not shown.  
End Class  
```  
  
```csharp  
public class AsyncExample  
{  
    // Synchronous methods.  
    public int Method1(string param);  
    public void Method2(double param);  
  
    // Asynchronous methods.  
    public void Method1Async(string param);  
    public void Method1Async(string param, object userState);  
    public event Method1CompletedEventHandler Method1Completed;  
  
    public void Method2Async(double param);  
    public void Method2Async(double param, object userState);  
    public event Method2CompletedEventHandler Method2Completed;  
  
    public void CancelAsync(object userState);  
  
    public bool IsBusy { get; }  
  
    // Class implementation not shown.  
}  
```  
  
 这里虚构的 `AsyncExample` 类有两个方法，都支持同步和异步调用。 同步重载的行为类似于方法调用，它们对调用线程执行操作；如果操作很耗时，则调用的返回可能会有明显的延迟。 异步重载将在另一个线程上启动操作，然后立即返回，允许在调用线程继续执行的同时让操作“在后台”执行。  
  
### <a name="asynchronous-method-overloads"></a>异步方法重载  

 异步操作可以有两个重载：单调用和多调用。 你可以通过方法签名来区分这两种形式：多调用形式有一个额外的参数，即 `userState`。 使用这种形式，你的代码可以多次调用 `Method1Async(string param, object userState)`，而不必等待任何挂起的异步操作的完成。 另一方面，如果你尝试在前一个调用尚未完成时调用 `Method1Async(string param)`，该方法将引发 <xref:System.InvalidOperationException>。  
  
 多调用重载的 `userState` 参数可帮助你区分各个异步操作。 应分别为各个 `Method1Async(string param, object userState)` 调用提供唯一值（例如，GUID 或哈希代码）；这样，当各个操作完成时，事件处理程序便可以确定是哪个操作实例抛出了完成事件。  
  
### <a name="tracking-pending-operations"></a>跟踪挂起的操作  

 如果你使用多调用重载，你的代码将需要跟踪挂起任务的 `userState` 对象（任务 ID）。 对于各个 `Method1Async(string param, object userState)` 调用，通常会生成新的唯一 `userState` 对象，并将它添加到集合中。 当对应于此 `userState` 对象的任务引发完成事件时，你的完成方法实现将检查 <xref:System.ComponentModel.AsyncCompletedEventArgs.UserState%2A?displayProperty=nameWithType> 并将此对象从集合中删除。 在以这种方式使用时，`userState` 参数充当任务 ID 的角色。  
  
> [!NOTE]
> 在为你对多调用重载的调用中的 `userState` 提供唯一值时，一定要小心。 如果任务 ID 不唯一，将导致异步类引发 <xref:System.ArgumentException>。  
  
### <a name="canceling-pending-operations"></a>取消挂起的操作  

 我们必须能够在异步操作完成之前随时取消它们，这一点很重要。 实现基于事件的异步模式的类包含 `CancelAsync` 方法（如果只有一个异步方法），或 _MethodName_**AsyncCancel** 方法（如果有多个异步方法）。  
  
 允许多个调用采用 `userState` 参数的方法，此类方法可用于跟踪每个任务的生存期。 `CancelAsync` 采用 `userState` 参数，该参数可用于取消特定挂起任务。  
  
 一次只支持一个挂起操作的方法（如 `Method1Async(string param)`）是不可取消的。  
  
### <a name="receiving-progress-updates-and-incremental-results"></a>接收进度更新和增量结果  

 符合基于事件的异步模式的类可以为跟踪进度和增量结果提供事件。 此事件通常命名为 `ProgressChanged` 或 _MethodName_**ProgressChanged**，其对应的事件处理程序将使用 <xref:System.ComponentModel.ProgressChangedEventArgs> 参数。  
  
 `ProgressChanged` 事件的事件处理程序可以检查 <xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A?displayProperty=nameWithType> 属性，以确定异步任务完成百分比。 此属性的范围是 0 到 100，可用来更新 <xref:System.Windows.Forms.ProgressBar.Value%2A> 的 <xref:System.Windows.Forms.ProgressBar> 属性。 如果有多个异步操作挂起，你可以使用 <xref:System.ComponentModel.ProgressChangedEventArgs.UserState%2A?displayProperty=nameWithType> 属性来分辨出哪个操作在报告进度。  
  
 一些类可能会在异步操作继续时报告增量结果。 这些结果将存储在从 <xref:System.ComponentModel.ProgressChangedEventArgs> 派生的类中并显示为此派生类中的属性。 你可以在 `ProgressChanged` 事件的事件处理程序中访问这些结果，就像访问 <xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A> 属性一样。 如果有多个异步操作挂起，你可以使用 <xref:System.ComponentModel.ProgressChangedEventArgs.UserState%2A> 属性来分辨出哪个操作在报告增量结果。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ComponentModel.ProgressChangedEventArgs>
- <xref:System.ComponentModel.BackgroundWorker>
- <xref:System.ComponentModel.AsyncCompletedEventArgs>
- [如何：使用支持基于事件的异步模式的组件](how-to-use-components-that-support-the-event-based-asynchronous-pattern.md)
- [如何：在后台运行操作](/dotnet/desktop/winforms/controls/how-to-run-an-operation-in-the-background)
- [如何：实现使用后台操作的窗体](/dotnet/desktop/winforms/controls/how-to-implement-a-form-that-uses-a-background-operation)
- [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)
- [实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [确定何时实现基于事件的异步模式](deciding-when-to-implement-the-event-based-asynchronous-pattern.md)
