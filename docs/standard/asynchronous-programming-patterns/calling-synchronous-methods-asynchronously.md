---
title: 使用异步方式调用同步方法
description: 了解如何使用 BeginInvoke 和 EndInvoke 方法在 .NET 中以异步方式调用同步方法。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- asynchronous programming, delegates
- asynchronous delegates
- AsyncWaitHandle property
- callback methods
- calling synchronous methods in asynchronous manner
- WaitHandle class, code examples
- asynchronous programming, status polling
- polling asynchronous operation status
- delegates [.NET], asynchronous
- synchronous calling in asynchronous manner
- waiting for asynchronous calls
- status information [.NET], asynchronous operations
ms.assetid: 41972034-92ed-450a-9664-ab93fcc6f1fb
ms.openlocfilehash: 8d12ab2904b336f38e56387c8aaf2a851a46007e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722721"
---
# <a name="calling-synchronous-methods-asynchronously"></a>使用异步方式调用同步方法

使用 .NET 可以以异步方式调用任何方法。 为此，请定义一个委托，该委托具有与你要调用的方法相同的签名。 公共语言运行时将自动用适当的签名为此委托定义 `BeginInvoke` 和 `EndInvoke` 方法。

> [!NOTE]
> .NET Compact Framework 不支持异步委托调用，也就是 `BeginInvoke` 和 `EndInvoke` 方法。

`BeginInvoke` 方法启动异步调用。 该方法具有与你要异步执行的方法相同的参数，另加两个可选参数。 第一个参数是一个 <xref:System.AsyncCallback> 委托，此委托引用在异步调用完成时要调用的方法。 第二个参数是一个用户定义的对象，该对象将信息传递到回调方法。 `BeginInvoke` 将立即返回，而不会等待异步调用完成。 `BeginInvoke` 返回可用于监视异步调用的进度的 <xref:System.IAsyncResult>。

`EndInvoke` 方法用于检索异步调用的结果。 它可以在调用 `BeginInvoke`之后的任意时间调用。 如果异步调用尚未完成，那么 `EndInvoke` 将阻止调用线程，直到完成异步调用。 `EndInvoke` 的参数包括要异步执行的方法的 `out` 和 `ref` 参数（Visual Basic 中的 `<Out>` `ByRef` 和 `ByRef`）以及 `BeginInvoke` 返回的 <xref:System.IAsyncResult>。

> [!NOTE]
> Visual Studio 中的 IntelliSense 功能可显示 `BeginInvoke` 和 `EndInvoke` 的参数。 如果未使用 Visual Studio 或类似工具，或者如果使用的是包含 Visual Studio 的 C#，请参阅[异步编程模型 (APM)](asynchronous-programming-model-apm.md)，获取关于为这些方法定义的参数的说明。

本主题的代码示例演示了使用 `BeginInvoke` 和 `EndInvoke` 进行异步调用的四种常用方法。 调用 `BeginInvoke` 之后可以执行以下操作：

- 执行一些操作，然后调用 `EndInvoke` 进行阻止，直到调用完成。

- 使用 <xref:System.Threading.WaitHandle> 属性获取 <xref:System.IAsyncResult.AsyncWaitHandle%2A?displayProperty=nameWithType> ，使用它的 <xref:System.Threading.WaitHandle.WaitOne%2A> 方法阻止执行，直到 <xref:System.Threading.WaitHandle> 收到信号，然后调用 `EndInvoke`。

- 对由 <xref:System.IAsyncResult> 返回的 `BeginInvoke` 进行轮询，以确定异步调用完成的时间，然后调用 `EndInvoke`。

- 将回调方法的委托传递到 `BeginInvoke`。 异步调用完成后在 <xref:System.Threading.ThreadPool> 线程上执行此方法。 回调方法将调用 `EndInvoke`。

> [!IMPORTANT]
> 无论使用何种方法，都要调用 `EndInvoke` 来完成异步调用。

## <a name="defining-the-test-method-and-asynchronous-delegate"></a>定义测试方法和异步委托

 下面的代码示例演示了异步调用同一个长时间运行的方法 `TestMethod`的各种方式。 `TestMethod` 方法会显示一条控制台消息，说明该方法已开始处理，休眠了几秒钟，然后结束。 `TestMethod` 有一个 `out` 参数，该参数用于演示将此类参数添加到 `BeginInvoke` 和 `EndInvoke`的签名中的方式。 您可以按同样的方式处理 `ref` 参数。

 下面的代码示例显示了 `TestMethod` 的定义和可用于异步调用 `AsyncMethodCaller` 的名称为 `TestMethod` 的委托。 要编译此代码示例，必须包括 `TestMethod` 和 `AsyncMethodCaller` 委托的定义。

 [!code-cpp[AsyncDelegateExamples#1](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/TestMethod.cpp#1)]
 [!code-csharp[AsyncDelegateExamples#1](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/TestMethod.cs#1)]
 [!code-vb[AsyncDelegateExamples#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/TestMethod.vb#1)]

## <a name="waiting-for-an-asynchronous-call-with-endinvoke"></a>使用 EndInvoke 等待异步调用

 异步执行方法的最简单方式是通过调用委托的 `BeginInvoke` 方法开始执行此方法，在主线程上执行一些操作，然后调用委托的 `EndInvoke` 方法。 `EndInvoke` 可能会阻止调用线程，因为该方法直到异步调用完成后才返回。 这种方式非常适合执行文件或网络操作。

> [!IMPORTANT]
> 因为 `EndInvoke` 可能会阻塞，所以不应从服务于用户界面的线程调用该方法。

 [!code-cpp[AsyncDelegateExamples#2](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/EndInvoke.cpp#2)]
 [!code-csharp[AsyncDelegateExamples#2](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/EndInvoke.cs#2)]
 [!code-vb[AsyncDelegateExamples#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/EndInvoke.vb#2)]

## <a name="waiting-for-an-asynchronous-call-with-waithandle"></a>使用 WaitHandle 等待异步调用

 可以使用由 <xref:System.Threading.WaitHandle> 返回的 <xref:System.IAsyncResult.AsyncWaitHandle%2A> 的 <xref:System.IAsyncResult> 属性来获取 `BeginInvoke`。 当异步调用完成时 <xref:System.Threading.WaitHandle> 会收到信号，而你可以通过调用 <xref:System.Threading.WaitHandle.WaitOne%2A> 方法来等待它。

 如果你使用 <xref:System.Threading.WaitHandle>，则在异步调用完成前后你可以执行其他处理，但必须在调用 `EndInvoke` 检索结果之前。

> [!NOTE]
> 调用 `EndInvoke`时不会自动关闭等待句柄。 如果释放对等待句柄的所有引用，则当垃圾回收功能回收此等待句柄时将释放系统资源。 要在使用完等待句柄后立即释放系统资源，请通过调用 <xref:System.Threading.WaitHandle.Close%2A?displayProperty=nameWithType> 方法来释放等待句柄。 显式释放可释放对象时，垃圾回收的工作效率更高。

 [!code-cpp[AsyncDelegateExamples#3](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/waithandle.cpp#3)]
 [!code-csharp[AsyncDelegateExamples#3](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/waithandle.cs#3)]
 [!code-vb[AsyncDelegateExamples#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/WaitHandle.vb#3)]

## <a name="polling-for-asynchronous-call-completion"></a>对异步调用的完成情况进行轮询

 可以使用由 <xref:System.IAsyncResult.IsCompleted%2A> 返回的 <xref:System.IAsyncResult> 的 `BeginInvoke` 属性来发现异步调用何时完成。 从服务于用户界面的线程执行异步调用时需要执行此操作。 对完成情况进行轮询允许在 <xref:System.Threading.ThreadPool> 线程中执行异步调用时继续执行调用线程。

 [!code-cpp[AsyncDelegateExamples#4](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/polling.cpp#4)]
 [!code-csharp[AsyncDelegateExamples#4](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/polling.cs#4)]
 [!code-vb[AsyncDelegateExamples#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/polling.vb#4)]

## <a name="executing-a-callback-method-when-an-asynchronous-call-completes"></a>异步调用完成时执行回调方法

 如果启动异步调用的线程可以不是处理结果的线程，那么在调用完成时可以执行回调方法。 将在 <xref:System.Threading.ThreadPool> 线程上执行回调方法。

 要使用回调方法，必须向 `BeginInvoke` 传递代表此回调方法的 <xref:System.AsyncCallback> 委托。 你还可以传递包含此回调方法要使用的信息的对象。 在回调方法中，可以将此回调方法的唯一参数 <xref:System.IAsyncResult>转换为 <xref:System.Runtime.Remoting.Messaging.AsyncResult> 对象。 然后使用 <xref:System.Runtime.Remoting.Messaging.AsyncResult.AsyncDelegate%2A?displayProperty=nameWithType> 属性获取用于启动调用的委托，以便可以调用 `EndInvoke`。

 有关示例的注释：

- `TestMethod` 的 `threadId` 参数为 `out` 参数（Visual Basic 中的 [`<Out>` `ByRef`），因此 `TestMethod` 从不使用它的输入值。 会将一个虚拟变量传递给 `BeginInvoke` 调用。 如果 `threadId` 参数是 `ref` 参数（Visual Basic 中的`ByRef` ），那么此变量应为一个类级字段，以便可以将它传递给 `BeginInvoke` 和 `EndInvoke`。

- 传递给 `BeginInvoke` 的状态信息是一个格式字符串，回调方法使用它来设置输出消息的格式。 因为该字符串是作为 <xref:System.Object>类型传递的，所以必须将此状态信息转换为适合的类型才可以使用。

- 回调是在 <xref:System.Threading.ThreadPool> 线程上进行的。 <xref:System.Threading.ThreadPool> 线程是后台线程，当主线程结束时该线程不会使应用程序继续运行，因此该示例的主线程必须休眠足够长的时间以等待回调完成。

 [!code-cpp[AsyncDelegateExamples#5](../../../samples/snippets/cpp/VS_Snippets_CLR/AsyncDelegateExamples/cpp/callback.cpp#5)]
 [!code-csharp[AsyncDelegateExamples#5](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDelegateExamples/CS/callback.cs#5)]
 [!code-vb[AsyncDelegateExamples#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDelegateExamples/VB/callback.vb#5)]

## <a name="see-also"></a>请参阅

- <xref:System.Delegate>
- [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)
