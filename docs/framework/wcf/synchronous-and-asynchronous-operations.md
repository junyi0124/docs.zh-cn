---
title: 同步和异步操作
description: 了解如何实现和调用异步服务操作。 WCF 服务和客户端可以在应用程序的两个级别使用异步操作。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF], synchronous operations
- service contracts [WCF], asynchronous operations
ms.assetid: db8a51cb-67e6-411b-9035-e5821ed350c9
ms.openlocfilehash: f14e206bb99215a7a9b2535f99feb9971274532b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254189"
---
# <a name="synchronous-and-asynchronous-operations"></a>同步和异步操作

本主题讨论实现和调用异步服务操作。  
  
 许多应用程序都以异步方式调用方法，因为这样允许应用程序在运行方法调用时继续执行有用的工作。 Windows Communication Foundation (WCF) 服务和客户端可以在两个不同的应用程序级别参与异步操作调用，这会为 WCF 应用程序在最大程度提高针对交互性而平衡的吞吐量方面提供更好的灵活性。  
  
## <a name="types-of-asynchronous-operations"></a>异步操作的类型  

 无论参数类型和返回值如何，WCF 中的所有服务协定都使用 WCF 特性来指定客户端和服务之间的特殊消息交换模式。 WCF 自动将入站和出站消息路由到相应的服务操作或正在运行的客户端代码。  
  
 客户端仅拥有为特殊操作指定消息交换模式的服务协定。 客户端可以为开发人员提供他们所选择的任何编程模型，但前提是找到了基础消息交换模式。 因此，只要找到了指定的消息模式，服务也可以按任何方式来实现操作。  
  
 如果服务协定独立于服务或客户端实现，则可以在 WCF 应用程序中实现下列形式的异步执行：  
  
- 客户端可以使用同步消息交换以异步方式调用请求/响应操作。  
  
- 服务可以使用同步消息交换以异步方式实现请求/响应操作。  
  
- 消息交换可以是单向的，而与客户端或服务的实现无关。  
  
### <a name="suggested-asynchronous-scenarios"></a>建议的异步方案  

 如果操作服务实现执行阻止调用（如执行 I/O 工作），则在该服务操作实现中使用异步方法。 在异步操作实现中，可尝试调用异步操作和方法来尽可能远地扩展异步调用路径。 例如，从 `BeginOperationTwo()` 中调用 `BeginOperationOne()`。  
  
- 在下列情况下，可在客户端应用程序或调用应用程序中使用异步方法：  
  
- 如果从中间层应用程序调用操作。 （有关此类方案的详细信息，请参阅[中间层客户端应用程序](./feature-details/middle-tier-client-applications.md)。）  
  
- 如果在 ASP.NET 页中调用操作，可使用异步页。  
  
- 如果从任何单线程的应用程序（如 Windows 窗体或 Windows Presentation Foundation (WPF)）调用操作。 使用基于事件的异步调用模型时，将在 UI 线程上引发结果事件，从而向应用程序添加响应性而无需您自己处理多个线程。  
  
- 通常，如果可在同步调用和异步调用之间进行选择，应选择异步调用。  
  
### <a name="implementing-an-asynchronous-service-operation"></a>实现异步服务操作  

 可以通过使用下列三种方法之一实现异步操作：  
  
1. 基于任务的异步模式  
  
2. 基于事件的异步模式  
  
3. IAsyncResult 异步模式  
  
#### <a name="task-based-asynchronous-pattern"></a>基于任务的异步模式  

 基于任务的异步模式是实现异步操作的首选方法，因为它最简单且最直接。 若要使用此方法，只需实现服务操作并指定任务的返回类型 \<T> ，其中 T 是逻辑运算返回的类型。 例如：  
  
```csharp  
public class SampleService:ISampleService
{
   // ...  
   public async Task<string> SampleMethodTaskAsync(string msg)
   {
      return Task<string>.Factory.StartNew(() =>
      {
         return msg;
      });
   }  
   // ...  
}  
```  
  
 SampleMethodTaskAsync 操作返回任务， \<string> 因为逻辑操作返回一个字符串。 有关基于任务的异步模式的更多信息，请参见[基于任务的异步模式](https://go.microsoft.com/fwlink/?LinkId=232504)。  
  
> [!WARNING]
> 在使用基于任务的异步模式时，如果在等待操作完成时发生异常，可能会引发 T:System.AggregateException。 该异常可在客户端或服务上发生  
  
#### <a name="event-based-asynchronous-pattern"></a>基于事件的异步模式  

 支持基于事件的异步模式的服务将有一个或多个名为 MethodNameAsync 的操作。 这些方法可能会创建同步版本的镜像，这些同步版本会在当前线程上执行相同的操作。 该类还可能具有 MethodNameCompleted 事件，并且可能会具有 MethodNameAsyncCancel（或只是 CancelAsync）方法。 希望调用操作的客户端将定义操作完成时要调用的事件处理程序，  
  
 下面的代码段演示如何使用基于事件的异步模式声明异步操作。  
  
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
  
 有关基于事件的异步模式的更多信息，请参见[基于事件的异步模式](../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)。  
  
#### <a name="iasyncresult-asynchronous-pattern"></a>IAsyncResult 异步模式  

 服务操作可以使用 .NET Framework 异步编程模式，并标记 `<Begin>` 属性设置为的方法，以异步方式实现 <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A> `true` 。 在这种情况下，异步操作将以与同步操作相同的方式在元数据中公开，即作为单个操作随请求消息和相关的响应消息来公开。 随后，客户端编程模型可以进行选择。 客户端编程模型可以将这种模式表示为同步操作，也可以表示为异步操作，但前提是调用该服务时发生了请求-响应消息交换。  
  
 通常，考虑到系统的异步特性，您不应依赖于线程。  将数据传递到操作调度处理的各个阶段的最可靠方式是使用扩展。  
  
 有关示例，请参见[如何：实现异步服务操作](how-to-implement-an-asynchronous-service-operation.md)。  
  
 定义一个异步执行（而不考虑它在客户端应用程序中的调用方式）的协定操作 `X`：  
  
- 使用 `BeginOperation` 和 `EndOperation` 模式定义两个方法。  
  
- `BeginOperation` 方法包括该操作的 `in` 和 `ref` 参数，并返回一个 <xref:System.IAsyncResult> 类型。  
  
- `EndOperation` 方法包括一个 <xref:System.IAsyncResult> 参数以及 `out` 和 `ref` 参数，并返回操作的返回类型。  
  
 有关示例，请参见下面的方法。  
  
```csharp  
int DoWork(string data, ref string inout, out string outonly)  
```  
  
```vb  
Function DoWork(ByVal data As String, ByRef inout As String, _out outonly As out) As Integer  
```  
  
 若要创建异步操作，这两种方法应为：  
  
```csharp  
[OperationContract(AsyncPattern=true)]
IAsyncResult BeginDoWork(string data,
                         ref string inout,
                         AsyncCallback callback,
                         object state);
int EndDoWork(ref string inout, out string outonly, IAsyncResult result);  
```  
  
```vb  
<OperationContract(AsyncPattern := True)>
Function BeginDoWork(ByVal data As String, _
                     ByRef inout As String, _
                     ByVal callback As AsyncCallback, _
                     ByVal state As Object) As IAsyncResult
Function EndDoWork(ByRef inout As String, ByRef outonly As String, ByVal result As IAsyncResult) As Integer  
```  
  
> [!NOTE]
> <xref:System.ServiceModel.OperationContractAttribute> 属性仅适用于 `BeginDoWork` 方法。 所得到的协定具有一个名为 `DoWork` 的 WSDL 操作。  
  
### <a name="client-side-asynchronous-invocations"></a>客户端异步调用  

 WCF 客户端应用程序可使用之前介绍的三个异步调用模型中的任意一个  
  
 在使用基于任务的模型时，只需使用 await 关键字调用操作，如下面的代码段所示。  
  
```csharp  
await simpleServiceClient.SampleMethodTaskAsync("hello, world");  
```  
  
 使用基于事件的异步模式只需添加事件处理程序，即可接收响应的通知 -- 将在用户界面线程上自动引发生成的事件。 若要使用此方法，请使用 [ServiceModel 元数据实用工具 (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) 并同时指定 /async 和 /tcv:Version35 命令选项，如下面的示例所示。  
  
```console  
svcutil http://localhost:8000/servicemodelsamples/service/mex /async /tcv:Version35  
```  
  
 完成此操作后，Svcutil.exe 将生成带有事件基础结构的 WCF 客户端类，该事件基础结构使调用应用程序可以实现和分配事件处理程序，以便接收响应和采取相应的操作。 有关完整示例，请参见[如何：以异步方式调用服务操作](./feature-details/how-to-call-wcf-service-operations-asynchronously.md)。  
  
 不过，基于事件的异步模型仅在 .NET Framework 3.5 中提供。 此外，如果使用创建 WCF 客户端通道，则不支持此方法，即使在 .NET Framework 3.5 中也是如此 <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> 。 使用 WCF 客户端通道对象时，必须使用 <xref:System.IAsyncResult?displayProperty=nameWithType> 对象异步调用操作。 若要使用此方法，请使用 [ServiceModel 元数据实用工具 (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) 并指定 /async 命令选项，如下面的示例所示。  
  
```console  
svcutil http://localhost:8000/servicemodelsamples/service/mex /async
```  
  
 这会生成一个服务协定，在该服务协定中，每个操作都作为 `<Begin>` 方法（其 <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A> 属性设置为 `true`）和相应的 `<End>` 方法来建模。 有关使用 <xref:System.ServiceModel.ChannelFactory%601> 的完整示例，请参见[如何：使用通道工厂以异步方式调用操作](./feature-details/how-to-call-operations-asynchronously-using-a-channel-factory.md)。  
  
 在任何一种情况下，即使服务是以同步方式实现的，应用程序也可以异步方式调用操作，就如同应用程序可以使用同一个模式以异步方式调用本地同步方法一样。 操作的实现方式对于客户端而言并不重要;当响应消息到达时，会将其内容分派给客户端的异步 <`End`> 方法，客户端将检索此信息。  
  
### <a name="one-way-message-exchange-patterns"></a>单向消息交换模式  

 还可以创建一个异步消息交换模式，在该模式下，单向操作（其 <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A?displayProperty=nameWithType> 为 `true` 的操作，该操作没有相关的响应）可以由客户端或服务独立于另一端来单向发送。  (此方法使用双工消息交换模式和单向消息。 ) 在这种情况下，服务协定指定单向消息交换，无论哪一种情况都可以根据需要将其作为异步调用或实现来实现。 通常，当服务协定是单向消息交换时，这些实现极有可能是异步的，因为在发送消息之后，应用程序不会等待答复，而是继续执行其他工作。  
  
### <a name="event-based-asynchronous-clients-and-message-contracts"></a>基于事件的异步模式客户端和消息协定  

 基于事件的异步模型设计准则规定，如果返回了多个值，则一个值会作为 `Result` 属性返回，其他值会作为 <xref:System.EventArgs> 对象上的属性返回。 因此产生的结果之一是，如果客户端使用基于事件的异步命令选项导入元数据，且该操作返回多个值，则默认的 <xref:System.EventArgs> 对象将返回一个值作为 `Result` 属性，返回的其余值是 <xref:System.EventArgs> 对象的属性。  
  
 如果要将消息对象作为 `Result` 属性来接收并要使返回的值作为该对象上的属性，请使用 /messageContract 命令选项。 这会生成一个签名，该签名会将响应消息作为 `Result` 对象上的 <xref:System.EventArgs> 属性返回。 然后，所有内部返回值就都是响应消息对象的属性了。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A>
- <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A>
