---
title: Windows Communication Foundation 到消息队列
ms.date: 03/30/2017
ms.assetid: 78d0d0c9-648e-4d4a-8f0a-14d9cafeead9
ms.openlocfilehash: a6e322936740f7d88d30b9a205ac937a807bedc1
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90552922"
---
# <a name="windows-communication-foundation-to-message-queuing"></a>Windows Communication Foundation 到消息队列

此示例演示了 Windows Communication Foundation (WCF) 应用程序如何将消息发送到消息队列 (MSMQ) 应用程序。 此服务是自承载控制台应用程序，通过它可以观察服务接收排队消息。 服务和客户端不需要同时运行。

 服务从队列接收消息并处理订单。 服务创建一个事务性队列，并为收到的消息设置消息处理程序，如下面的示例代码所示。

```csharp
static void Main(string[] args)
{
    if (!MessageQueue.Exists(
              ConfigurationManager.AppSettings["queueName"]))
       MessageQueue.Create(
           ConfigurationManager.AppSettings["queueName"], true);
        //Connect to the queue
        MessageQueue Queue = new
    MessageQueue(ConfigurationManager.AppSettings["queueName"]);
    Queue.ReceiveCompleted +=
                 new ReceiveCompletedEventHandler(ProcessOrder);
    Queue.BeginReceive();
    Console.WriteLine("Order Service is running");
    Console.ReadLine();
}
```

 当消息到达队列中时，将调用消息处理程序 `ProcessOrder`。

```csharp
public static void ProcessOrder(Object source,
    ReceiveCompletedEventArgs asyncResult)
{
    try
    {
        // Connect to the queue.
        MessageQueue Queue = (MessageQueue)source;
        // End the asynchronous receive operation.
        System.Messaging.Message msg =
                     Queue.EndReceive(asyncResult.AsyncResult);
        msg.Formatter = new System.Messaging.XmlMessageFormatter(
                                new Type[] { typeof(PurchaseOrder) });
        PurchaseOrder po = (PurchaseOrder) msg.Body;
        Random statusIndexer = new Random();
        po.Status = PurchaseOrder.OrderStates[statusIndexer.Next(3)];
        Console.WriteLine("Processing {0} ", po);
        Queue.BeginReceive();
    }
    catch (System.Exception ex)
    {
        Console.WriteLine(ex.Message);
    }

}
```

 服务从 MSMQ 消息正文中提取 `ProcessOrder` 并处理订单。

 MSMQ 队列名称在配置文件的 appSettings 节中指定，如以下示例配置所示。

```xml
<appSettings>
    <add key="orderQueueName" value=".\private$\Orders" />
</appSettings>
```

> [!NOTE]
> 队列名称为本地计算机使用圆点 (.)，并在其路径中使用反斜杠分隔符。

 客户端创建采购订单并在事务的范围内提交该采购订单，如下面的示例代码所示。

```csharp
// Create the purchase order
PurchaseOrder po = new PurchaseOrder();
// Fill in the details
...

OrderProcessorClient client = new OrderProcessorClient("OrderResponseEndpoint");

MsmqMessage<PurchaseOrder> ordermsg = new MsmqMessage<PurchaseOrder>(po);
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
{
    client.SubmitPurchaseOrder(ordermsg);
    scope.Complete();
}
Console.WriteLine("Order has been submitted:{0}", po);

//Closing the client gracefully closes the connection and cleans up resources
client.Close();
```

 客户端按顺序使用自定义客户端将 MSMQ 消息发送给队列。 由于接收和处理消息的应用程序是 MSMQ 应用程序，而不是 WCF 应用程序，因此两个应用程序之间没有隐式服务协定。 所以在此方案中，我们不能使用 Svcutil.exe 工具创建代理。

 对于使用绑定发送消息的所有 WCF 应用程序而言，自定义客户端本质上都是相同的 `MsmqIntegration` 。 与其他客户端不同，它不包含一系列服务操作。 它只是一个提交消息操作。

```csharp
[System.ServiceModel.ServiceContractAttribute(Namespace = "http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true, Action = "*")]
    void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg);
}

public partial class OrderProcessorClient : System.ServiceModel.ClientBase<IOrderProcessor>, IOrderProcessor
{
    public OrderProcessorClient(){}

    public OrderProcessorClient(string configurationName)
        : base(configurationName)
    { }

    public OrderProcessorClient(System.ServiceModel.Channels.Binding binding, System.ServiceModel.EndpointAddress address)
        : base(binding, address)
    { }

    public void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg)
    {
        base.Channel.SubmitPurchaseOrder(msg);
    }
}
```

 运行示例时，客户端和服务活动将显示在服务和客户端控制台窗口中。 您可以看到服务从客户端接收消息。 在每个控制台窗口中按 Enter 可以关闭服务和客户端。 请注意：由于正在使用队列，因此不必同时启动和运行客户端和服务。 例如，可以先运行客户端，再将其关闭，然后启动服务，这样服务仍然会收到客户端的消息。

> [!NOTE]
> 此示例需要安装消息队列。 请参阅 [消息队列](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85))中的安装说明。

## <a name="set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 如果先运行服务，则它将检查以确保队列存在。 如果队列不存在，则服务将创建一个队列。 可以先运行服务以创建队列或通过 MSMQ 队列管理器创建一个队列。 执行下面的步骤来在 Windows 2008 中创建队列。

    1. 在 Visual Studio 2012 中打开服务器管理器。

    2. 展开 " **功能** " 选项卡。

    3. 右键单击 "**专用消息队列**"，然后选择 "**新建**  >  **专用队列**"。

    4. 选中 " **事务性** " 框。

    5. 输入 `ServiceModelSamplesTransacted` 作为新队列的名称。

3. 若要生成 c # 或 Visual Basic 版解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

4. 若要在单计算机配置中运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

## <a name="run-the-sample-across-computers"></a>跨计算机运行示例

1. 将 \service\bin\ 文件夹（在语言特定文件夹内）中的服务程序文件复制到服务计算机上。

2. 将 \client\bin\ 文件夹（在语言特定文件夹内）中的客户端程序文件复制到客户端计算机上。

3. 在 Client.exe.config 文件中，更改客户端终结点地址以指定服务计算机名称，而不是使用“.”。

4. 在服务计算机上，在命令提示符下启动 Service.exe。

5. 在客户端计算机上，在命令提示符下启动 Client.exe。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\MSMQIntegration\WcfToMsmq`

## <a name="see-also"></a>请参阅

- [如何：与 WCF 终结点和消息队列应用程序交换消息](../feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)
- [消息队列](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85))
