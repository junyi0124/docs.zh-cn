---
title: 实现基于事件的异步模式的最佳做法
ms.date: 03/30/2017
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET], asynchronous
- AsyncOperationManager class
- threading [.NET], asynchronous features
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: 4acd2094-4f46-4eff-9190-92d0d9ff47db
ms.openlocfilehash: 6c2df4c2877f9191bd2b8190869c359a74de8e8f
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830488"
---
# <a name="best-practices-for-implementing-the-event-based-asynchronous-pattern"></a>实现基于事件的异步模式的最佳做法

基于事件的异步模式提供了一种在类中使用熟悉的事件和委托语义公开异步行为的有效方法。 若要实现基于事件的异步模式，你需要遵循某些特定的行为要求。 以下部分描述了在你实现遵循基于事件的异步模式的类时应该考虑的要求和准则。  
  
 有关概述，请参阅[实现基于事件的异步模式](implementing-the-event-based-asynchronous-pattern.md)。  
  
## <a name="required-behavioral-guarantees"></a>必需的行为保证

 若要实现基于事件的异步模式，你必须提供一些保证来确保类的行为正确且类的客户端能够依赖这种行为。  
  
### <a name="completion"></a>完成

 操作成功完成、出错或取消时，始终应调用 <em>MethodName</em>Completed 事件处理程序  。 任何情况下，应用程序都不应遇到这样的情况：应用程序保持空闲状态，而操作却一直不能完成。 此规则的例外情况是：异步操作本身设计为永不完成。  
  
### <a name="completed-event-and-eventargs"></a>已完成的事件和 EventArgs

针对每个单独的 <em>MethodName</em>Async  方法，请应用以下设计要求：  
  
- 在与该方法相同的类上定义 <em>MethodName</em>Completed 事件  。  
  
- 为派生自 <xref:System.ComponentModel.AsyncCompletedEventArgs> 类的 <em>MethodName</em>Completed 事件定义一个 <xref:System.EventArgs> 类和随附委托。 默认类名应采用 <em>MethodName</em>CompletedEventArgs  形式。  
  
- 确保 <xref:System.EventArgs> 类特定于 <em>MethodName</em> 方法的返回值。 在使用 <xref:System.EventArgs> 类时，切勿要求开发人员强制转换结果。  
  
     下面的代码示例分别演示了此项设计要求的合理实现和错误实现。  
  
```csharp  
// Good design  
private void Form1_MethodNameCompleted(object sender, xxxCompletedEventArgs e)
{
    DemoType result = e.Result;  
}  
  
// Bad design  
private void Form1_MethodNameCompleted(object sender, MethodNameCompletedEventArgs e)
{
    DemoType result = (DemoType)(e.Result);  
}  
```  
  
- 不要为返回 <xref:System.EventArgs> 的返回方法定义 `void` 类。 而应使用 <xref:System.ComponentModel.AsyncCompletedEventArgs> 类的实例。  
  
- 应务必始终抛出 <em>MethodName</em>Completed  事件。 成功完成、出错或者取消时应引发此事件。 任何情况下，应用程序都不应遇到这样的情况：应用程序保持空闲状态，而操作却一直不能完成。  
  
- 确保可以捕获异步操作中发生的任何异常并将捕获的异常分配给 <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> 属性。  
  
- 如果完成任务时出现错误，其结果应当不可访问。 当 <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> 属性不为 `null` 时，确保访问 <xref:System.EventArgs> 结构中的任何属性都会引发异常。 使用 <xref:System.ComponentModel.AsyncCompletedEventArgs.RaiseExceptionIfNecessary%2A> 方法来执行此验证。  
  
- 将超时建模为错误。 如果发生超时，应抛出 <em>MethodName</em>Completed  事件，并将 <xref:System.TimeoutException> 分配给 <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> 属性。  
  
- 如果类支持多个并发调用，应确保 <em>MethodName</em>Completed  事件包含相应的 `userSuppliedState` 对象。  
  
- 应确保在应用生命周期中适时对相应线程抛出 <em>MethodName</em>Completed  事件。 有关更多信息，请参见“线程处理和上下文”部分。  
  
### <a name="simultaneously-executing-operations"></a>同时执行操作  
  
- 如果类支持多个并发调用，应让开发人员可以单独跟踪各个调用，具体操作是定义 <em>MethodName</em>Async  重载，此重载需要使用对象赋值状态参数或任务 ID（名为 `userSuppliedState`）。 此参数应始终是 <em>MethodName</em>Async  方法签名中的最后一个参数。  
  
- 如果类定义了需要使用对象赋值状态参数或任务 ID 的 <em>MethodName</em>Async  重载，应务必使用相应任务 ID 跟踪操作的生存期，并将它 返回给完成事件处理器。 有一些用来提供帮助的帮助器类。 有关并发管理的详细信息，请参阅[如何：实现支持基于事件的异步模式的组件](component-that-supports-the-event-based-asynchronous-pattern.md)。  
  
- 如果类定义了不使用状态参数的 <em>MethodName</em>Async 方法，且它不支持多个并发调用，则应确保在先前 <em>MethodName</em>Async 调用完成前，只要尝试调用 <em>MethodName</em>Async 都会导致 <xref:System.InvalidOperationException> 抛出。  
  
- 一般来说，如果多次调用不使用 `userSuppliedState` 参数的 <em>MethodName</em>Async 方法，导致多个未结操作出现，不得抛出异常。 如果类无法显式处理这种情况，将引发异常，但可假定开发人员能够处理多个不可区分回调。  
  
### <a name="accessing-results"></a>访问结果  
  
- 如果在执行异步操作期间出现错误，其结果应当不可访问。 确保在 <xref:System.ComponentModel.AsyncCompletedEventArgs> 不为 <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> 时访问 `null` 中的任何属性都会引发由 <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> 引用的异常。 <xref:System.ComponentModel.AsyncCompletedEventArgs> 类为达到此目的提供了 <xref:System.ComponentModel.AsyncCompletedEventArgs.RaiseExceptionIfNecessary%2A> 方法。  
  
- 确保访问结果的任何尝试都将引发 <xref:System.InvalidOperationException>，指出该操作已被取消。 使用 <xref:System.ComponentModel.AsyncCompletedEventArgs.RaiseExceptionIfNecessary%2A?displayProperty=nameWithType> 方法来执行此验证。  
  
### <a name="progress-reporting"></a>进度报告  
  
- 如有可能，支持进度报告。 在开发人员使用你的类时，这使他们能够提供更好的应用程序用户体验。  
  
- 如果实现 ProgressChanged  或 <em>MethodName</em>ProgressChanged  事件，应确保在抛出特定异步操作的 <em>MethodName</em>Completed  事件后，不会对此操作抛出所实现的事件。  
  
- 如果正在填充标准的 <xref:System.ComponentModel.ProgressChangedEventArgs>，则请确保始终能够将 <xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A> 解释为一个百分比。 该百分比不必是一个精确值，但它应表示为百分数的形式。 如果你的进度报告指标不能是一个百分比，请从 <xref:System.ComponentModel.ProgressChangedEventArgs> 类中派生一个类并将 <xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A> 保留为 0。 避免使用非百分比的报告指标。  
  
- 请确保在应用程序生命周期中的适当时间在适当的线程上引发了 `ProgressChanged` 事件。 有关更多信息，请参见“线程处理和上下文”部分。  
  
### <a name="isbusy-implementation"></a>IsBusy 实现  
  
- 如果你的类支持多个并发调用，则不要公开 `IsBusy` 属性。 例如，XML Web services 代理不会公开 `IsBusy` 属性，因为它们支持异步方法的多个并发调用。  
  
- 在调用 <em>MethodName</em>Async 方法后，且在抛出 <em>MethodName</em>Completed 事件前，`IsBusy` 属性应返回 `true`。 否则，它应返回 `false`。 <xref:System.ComponentModel.BackgroundWorker> 和 <xref:System.Net.WebClient> 组件是公开 `IsBusy` 属性的类的示例。  
  
### <a name="cancellation"></a>取消  
  
- 如有可能，支持取消。 在开发人员使用你的类时，这使他们能够提供更好的应用程序用户体验。  
  
- 在发生取消时，设置 <xref:System.ComponentModel.AsyncCompletedEventArgs.Cancelled%2A> 对象中的 <xref:System.ComponentModel.AsyncCompletedEventArgs> 标志。  
  
- 确保访问结果的任何尝试都将引发 <xref:System.InvalidOperationException>，指出该操作已被取消。 使用 <xref:System.ComponentModel.AsyncCompletedEventArgs.RaiseExceptionIfNecessary%2A?displayProperty=nameWithType> 方法来执行此验证。  
  
- 请确保对取消方法发出的调用始终能够成功返回，而且从不引发异常。 一般来说，客户端不会得到关于在任何给定时间是否真正可取消某个操作的通知，也不会得到关于以前发出的取消是否已经成功的通知。 不过，应用程序在取消成功时总能得到通知，因为应用程序参与了完成状态。  
  
- 取消操作时，应抛出 <em>MethodName</em>Completed  事件。  
  
### <a name="errors-and-exceptions"></a>错误和异常  
  
- 捕获所有发生在异步操作中的异常并将 <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A?displayProperty=nameWithType> 属性的值设置为该异常。  
  
### <a name="threading-and-contexts"></a>线程处理和上下文

 为了使类正确运行，应当使用给定应用程序模型（包括 ASP.NET 和 Windows 窗体应用程序）的适当线程或上下文调用客户端事件处理程序，这一点很重要。 我们提供了两个重要的帮助器类，以确保你的异步类在任何应用程序模型中都能正确运行，这两个帮助器类是 <xref:System.ComponentModel.AsyncOperation> 和 <xref:System.ComponentModel.AsyncOperationManager>。  
  
 <xref:System.ComponentModel.AsyncOperationManager> 提供了 <xref:System.ComponentModel.AsyncOperationManager.CreateOperation%2A> 方法，该方法会返回一个 <xref:System.ComponentModel.AsyncOperation>。 <em>MethodName</em>Async  方法调用 <xref:System.ComponentModel.AsyncOperationManager.CreateOperation%2A>，且类使用返回的 <xref:System.ComponentModel.AsyncOperation> 跟踪异步任务的生存期。  
  
 若要将进程、增量结果和完成情况报告给客户端，请调用 <xref:System.ComponentModel.AsyncOperation.Post%2A> 上的 <xref:System.ComponentModel.AsyncOperation.OperationCompleted%2A> 和 <xref:System.ComponentModel.AsyncOperation> 方法。 <xref:System.ComponentModel.AsyncOperation> 负责将对客户端事件处理程序的调用封送到合适的线程或上下文。  
  
> [!NOTE]
> 如果你明确想违反应用程序模型的策略，但仍想获得使用基于事件的异步模式的其他好处，则你可以避开这些规则。 例如，你可能希望在 Windows 窗体中进行操作的某个类是自由线程类。 只要开发人员了解隐含的限制，你就可以创建自由线程类。 控制台应用程序不会同步 <xref:System.ComponentModel.AsyncOperation.Post%2A> 调用的执行。 这会导致按错误的顺序引发 `ProgressChanged` 事件。 如果希望序列化 <xref:System.ComponentModel.AsyncOperation.Post%2A> 调用的执行，请实现并安装 <xref:System.Threading.SynchronizationContext?displayProperty=nameWithType> 类。  
  
 若要详细了解如何使用 <xref:System.ComponentModel.AsyncOperation> 和 <xref:System.ComponentModel.AsyncOperationManager> 启用异步操作，请参阅[如何：实现支持基于事件的异步模式的组件](component-that-supports-the-event-based-asynchronous-pattern.md)。  
  
## <a name="guidelines"></a>准则  
  
- 理论上，方法调用与方法调用之间应是相互独立的。 你应当避免在使用调用时使用共享资源。 如果要在不同调用之间共享资源，则你需要在你的实现中提供一个适当的同步机制。  
  
- 建议不要进行需要客户端实现同步的设计。 例如，你可以使用一个异步方法将全局静态对象作为参数来接收；这类方法的多个并发调用可能会导致数据损坏或死锁。  
  
- 如果你使用多个调用重载（签名中的 `userState`）实现方法，你的类将需要管理由一系列用户状态、任务 ID 及其相应的挂起操作构成的一个集合。 应当使用 `lock` 区域保护此集合，因为各种调用都会在此集合中添加和移除 `userState` 对象。  
  
- 请考虑在可行且适当的情况下，重新使用 `CompletedEventArgs` 类。 在这种情况下，命名和方法名不一致，因为给定的委托和 <xref:System.EventArgs> 类型并不会与单独某个方法联系在一起。 不过，强制开发人员对从 <xref:System.EventArgs> 上的属性检索的值进行强制转换，是绝对不可取的。  
  
- 如果你正在创建自 <xref:System.ComponentModel.Component> 派生的类，请不要实现和安装你自己的 <xref:System.Threading.SynchronizationContext> 类。 所使用的 <xref:System.Threading.SynchronizationContext> 由应用程序模型而不是组件控制。  
  
- 当你使用任何形式的多线程时，都有可能会遇到非常严重且复杂的 Bug。 在实现任何使用多线程处理的解决方案之前，请参阅[托管线程处理最佳做法](../threading/managed-threading-best-practices.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ComponentModel.AsyncOperation>
- <xref:System.ComponentModel.AsyncOperationManager>
- <xref:System.ComponentModel.AsyncCompletedEventArgs>
- <xref:System.ComponentModel.ProgressChangedEventArgs>
- <xref:System.ComponentModel.BackgroundWorker>
- [实现基于事件的异步模式](implementing-the-event-based-asynchronous-pattern.md)
- [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)
- [确定何时实现基于事件的异步模式](deciding-when-to-implement-the-event-based-asynchronous-pattern.md)
- [实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [如何：使用支持基于事件的异步模式的组件](how-to-use-components-that-support-the-event-based-asynchronous-pattern.md)
- [如何：实现支持基于事件的异步模式的组件](component-that-supports-the-event-based-asynchronous-pattern.md)
