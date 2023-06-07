---
title: 传输：UDP 示例上的自定义事务
ms.date: 03/30/2017
ms.assetid: 6cebf975-41bd-443e-9540-fd2463c3eb23
ms.openlocfilehash: 1a5b6afd7dc078b0e6e270888973b34a91bfdb9f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96295660"
---
# <a name="transport-custom-transactions-over-udp-sample"></a>传输：UDP 示例上的自定义事务

此示例基于 Windows Communication Foundation (WCF) [传输扩展性](transport-extensibility.md)中的[传输： UDP](transport-udp.md)示例。 它扩展 UDP 传输示例，以支持自定义事务流并演示 <xref:System.ServiceModel.Channels.TransactionMessageProperty> 属性的用法。  
  
## <a name="code-changes-in-the-udp-transport-sample"></a>UDP 传输示例中的代码更改  

 为了演示事务流，示例对 `ICalculatorContract` 的服务协定进行了更改，用于为 `CalculatorService.Add()` 设定一个事务范围。 本示例还向 `System.Guid` 操作的协定另外添加了一个 `Add` 参数。 此参数用于将客户端事务的标识符传递给服务。  
  
```csharp  
class CalculatorService : IDatagramContract, ICalculatorContract  
{  
    [OperationBehavior(TransactionScopeRequired=true)]  
    public int Add(int x, int y, Guid clientTransactionId)  
    {  
        if(Transaction.Current.TransactionInformation.DistributedIdentifier == clientTransactionId)  
    {  
        Console.WriteLine("The client transaction has flowed to the service");  
    }  
    else  
    {  
     Console.WriteLine("The client transaction has NOT flowed to the service");  
    }  
  
    Console.WriteLine("   adding {0} + {1}", x, y);
    return (x + y);  
    }  
  
    [...]  
}  
```  
  
 [传输： udp](transport-udp.md)示例使用 udp 包在客户端和服务之间传递消息。 [传输：自定义传输示例](transport-custom-transactions-over-udp-sample.md)使用相同的机制传输消息，但当事务流动时，会将其插入 UDP 数据包以及编码的消息中。  
  
```csharp  
byte[] txmsgBuffer = TransactionMessageBuffer.WriteTransactionMessageBuffer(txPropToken, messageBuffer);  
  
int bytesSent = this.socket.SendTo(txmsgBuffer, 0, txmsgBuffer.Length, SocketFlags.None, this.remoteEndPoint);  
```  
  
 `TransactionMessageBuffer.WriteTransactionMessageBuffer` 是一个帮助器方法，包含用于将当前事务的传播程序令牌与消息实体合并，并将合并结果放入缓冲区中的新功能。  
  
 对于自定义事务流传输，客户端实现必须知道哪些服务操作需要事务流，并将此信息传递给 WCF。 还应存在用于将用户事务传递到传输层的机制。 此示例使用 "WCF 消息检查器" 获取此信息。 此处实现的名为 `TransactionFlowInspector` 的客户端消息检查器执行下列任务：  
  
- 确定对于给定的消息操作，事务是否必须进行流处理（这发生在 `IsTxFlowRequiredForThisOperation()` 中）。  
  
- 如果需要对事务进行流处理（这在 `TransactionFlowProperty` 中完成），则使用 `BeforeSendRequest()` 将当前环境事务附加到消息。  
  
```csharp  
public class TransactionFlowInspector : IClientMessageInspector  
{  
   void IClientMessageInspector.AfterReceiveReply(ref           System.ServiceModel.Channels.Message reply, object correlationState)  
  {  
  }  
  
   public object BeforeSendRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel)  
   {  
       // obtain the tx propagation token  
       byte[] propToken = null;
       if (Transaction.Current != null && IsTxFlowRequiredForThisOperation(request.Headers.Action))  
       {  
           try  
           {  
               propToken = TransactionInterop.GetTransmitterPropagationToken(Transaction.Current);  
           }  
           catch (TransactionException e)  
           {  
              throw new CommunicationException("TransactionInterop.GetTransmitterPropagationToken failed.", e);  
           }  
       }  
  
      // set the propToken on the message in a TransactionFlowProperty  
       TransactionFlowProperty.Set(propToken, request);  
  
       return null;
    }  
  }  
  
  static bool IsTxFlowRequiredForThisOperation(String action)  
 {  
       // In general, this should contain logic to identify which operations (actions)      require transaction flow.  
      [...]  
 }  
}  
```  
  
 使用自定义行为 `TransactionFlowInspector` 将 `TransactionFlowBehavior` 本身传递到框架。  
  
```csharp  
public class TransactionFlowBehavior : IEndpointBehavior  
{  
       public void AddBindingParameters(ServiceEndpoint endpoint,            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)  
      {  
      }  
  
       public void ApplyClientBehavior(ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.ClientRuntime clientRuntime)  
      {  
            TransactionFlowInspector inspector = new TransactionFlowInspector();  
            clientRuntime.MessageInspectors.Add(inspector);  
      }  
  
      public void ApplyDispatchBehavior(ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.EndpointDispatcher endpointDispatcher)  
     {  
     }  
  
      public void Validate(ServiceEndpoint endpoint)  
      {  
      }  
}  
```  
  
 根据前面的机制，用户代码在调用服务操作之前创建 `TransactionScope`。 消息检查器确保将事务传递到传输，以防需要将事务流动到服务操作。  
  
```csharp  
CalculatorContractClient calculatorClient = new CalculatorContractClient("SampleProfileUdpBinding_ICalculatorContract");  
calculatorClient.Endpoint.Behaviors.Add(new TransactionFlowBehavior());
  
try  
{  
       for (int i = 0; i < 5; ++i)  
      {  
              // call the 'Add' service operation under a transaction scope  
             using (TransactionScope ts = new TransactionScope())  
             {  
        [...]  
        Console.WriteLine(calculatorClient.Add(i, i * 2));  
         }  
      }
       calculatorClient.Close();  
}  
catch (TimeoutException)  
{  
    calculatorClient.Abort();  
}  
catch (CommunicationException)  
{  
    calculatorClient.Abort();  
}  
catch (Exception)  
{  
    calculatorClient.Abort();  
    throw;  
}  
```  
  
 从客户端接收 UDP 数据包时，服务可对数据包进行反序列化，从而提取消息和可能存在的事务。  
  
```csharp  
count = listenSocket.EndReceiveFrom(result, ref dummy);  
  
// read the transaction and message                       TransactionMessageBuffer.ReadTransactionMessageBuffer(buffer, count, out transaction, out msg);  
```  
  
 `TransactionMessageBuffer.ReadTransactionMessageBuffer()` 是用于反转 `TransactionMessageBuffer.WriteTransactionMessageBuffer()` 执行的序列化过程的帮助器方法。  
  
 如果事务已流入，它会通过 `TransactionMessageProperty` 追加到消息。  
  
```csharp  
message = MessageEncoderFactory.Encoder.ReadMessage(msg, bufferManager);  
  
if (transaction != null)  
{  
       TransactionMessageProperty.Set(transaction, message);  
}  
```  
  
 这可以确保调度程序在调度时选择事务，并在调用由消息处理的服务操作时使用该事务。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
2. 当前示例的运行方式与 [传输： UDP](transport-udp.md) 示例类似。 若要运行该示例，请使用 UdpTestService.exe 启动服务。 如果运行的是 Windows Vista，则必须以提升的权限启动该服务。 为此，请在文件资源管理器中右键单击 UdpTestService.exe，然后单击 "以 **管理员身份运行**"。  
  
3. 将生成以下输出。  
  
    ```console  
    Testing Udp From Code.  
    Service is started from code...  
    Press <ENTER> to terminate the service and start service from config...  
    ```  
  
4. 此时可以通过运行 UdpTestClient.exe 启动客户端。 客户端生成的输出如下所示。  
  
    ```console
    0  
    3  
    6  
    9  
    12  
    Press <ENTER> to complete test.  
    ```  
  
5. 服务输出如下所示。  
  
    ```console
    Hello, world!  
    Hello, world!  
    Hello, world!  
    Hello, world!  
    Hello, world!  
    The client transaction has flowed to the service  
       adding 0 + 0  
    The client transaction has flowed to the service  
       adding 1 + 2  
    The client transaction has flowed to the service  
       adding 2 + 4  
    The client transaction has flowed to the service  
       adding 3 + 6  
    The client transaction has flowed to the service  
       adding 4 + 8  
    ```  
  
6. 如果服务应用程序可以将 `The client transaction has flowed to the service` 操作的 `clientTransactionId` 参数中的客户端发送的事务标识符与服务事务的标识符进行匹配，则服务应用程序会显示消息 `CalculatorService.Add()`。 仅当客户端事务已经流动到服务时才可以获得匹配。  
  
7. 若要对使用配置发布的终结点运行客户端应用程序，请在服务应用程序窗口中按 Enter，并再次运行测试客户端。 您将在服务上看到如下输出。  
  
    ```console  
    Testing Udp From Config.  
    Service is started from config...  
    Press <ENTER> to terminate the service and exit...  
    ```  
  
8. 现在对服务运行客户端将产生与前面相似的输出。  
  
9. 若要使用 Svcutil.exe 重新生成客户端代码和配置，请启动服务应用程序，然后从示例的根目录中运行下面的 Svcutil.exe 命令。  
  
    ```console  
    svcutil http://localhost:8000/udpsample/ /reference:UdpTransport\bin\UdpTransport.dll /svcutilConfig:svcutil.exe.config  
    ```  
  
10. 注意，Svcutil.exe 不会为 `sampleProfileUdpBinding` 生成绑定扩展配置；必须手动添加它。  
  
    ```xml  
    <configuration>  
        <system.serviceModel>
            …  
            <extensions>  
                <!-- This was added manually because svcutil.exe does not add this extension to the file -->  
                <bindingExtensions>  
                    <add name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
                </bindingExtensions>  
            </extensions>  
        </system.serviceModel>  
    </configuration>  
    ```  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transactions\TransactionMessagePropertyUDPTransport`  
  
## <a name="see-also"></a>另请参阅

- [传输：UDP](transport-udp.md)
