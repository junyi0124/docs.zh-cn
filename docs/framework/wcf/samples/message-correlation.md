---
title: 消息相关性
ms.date: 03/30/2017
ms.assetid: 3f62babd-c991-421f-bcd8-391655c82a1f
ms.openlocfilehash: c6c68ec36ecee294aa217f77f462dcea31f1e211
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90557965"
---
# <a name="message-correlation"></a>消息相关性

此示例演示消息队列 (MSMQ) 应用程序如何将 MSMQ 消息发送到 Windows Communication Foundation (WCF) 服务，以及如何在请求/响应方案中将消息与发送方和接收方应用程序相关联。 此示例使用 msmqIntegrationBinding 绑定。 这种情况下的服务是自承载控制台应用程序，通过它可以观察接收排队消息的服务。 k

 该服务处理接收的来自发送方的消息，并向发送方回发一个响应消息。 发送方将它收到的响应与其最初发送的请求关联。 可以使用消息的 `MessageID` 和 `CorrelationID` 属性将请求消息与响应消息关联。

 `IOrderProcessor` 服务协定定义了适合与队列一起使用的单向服务操作。 MSMQ 消息没有 Action 标头，因此无法将不同的 MSMQ 消息自动映射到操作协定。 因此，在这种情况下只有一个操作协定。 如果您希望在服务中定义更多的操作协定，则应用程序必须提供相关信息，如 MSMQ 消息中的哪个标头（例如标签或 correlationID）可用于确定要调度的操作协定。

 MSMQ 消息也不包含诸如哪些标头映射到操作协定的不同参数等信息。 因此，在该操作协定中只有一个参数。 参数的类型为 <xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601> ，包含基础 MSMQ 消息。 `MsmqMessage<T>` 类中的“T”类型表示序列化到 MSMQ 消息正文中的数据。 在此示例中，`PurchaseOrder` 类型序列化到 MSMQ 消息正文中。

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]
[ServiceKnownType(typeof(PurchaseOrder))]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true, Action = "*")]
    void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg);
}
```

 服务操作处理采购订单，并在服务控制台窗口中显示采购订单的内容及其状态。 <xref:System.ServiceModel.OperationBehaviorAttribute> 使用队列配置要在事务中登记的操作并在返回操作时标记事务已完成。 `PurchaseOrder` 包含必须由服务处理的订单详细信息。

```csharp
// Service class that implements the service contract.
public class OrderProcessorService : IOrderProcessor
{
   [OperationBehavior(TransactionScopeRequired = true,
          TransactionAutoComplete = true)]
   public void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> ordermsg)
   {
       PurchaseOrder po = (PurchaseOrder)ordermsg.Body;
       Random statusIndexer = new Random();
       po.Status = PurchaseOrder.OrderStates[statusIndexer.Next(3)];
       Console.WriteLine("Processing {0} ", po);
       //Send a response to the client that the order has been received
         and is pending fullfillment.
       SendResponse(ordermsg);
    }

    private void SendResponse(MsmqMessage<PurchaseOrder> ordermsg)
    {
        OrderResponseClient client = new OrderResponseClient("OrderResponseEndpoint");

        //Set the correlation ID such that the client can correlate the response to the order.
        MsmqMessage<PurchaseOrder> orderResponsemsg = new MsmqMessage<PurchaseOrder>(ordermsg.Body);
        orderResponsemsg.CorrelationId = ordermsg.Id;
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
        {
            client.SendOrderResponse(orderResponsemsg);
            scope.Complete();
        }

        client.Close();
    }
}
```

 服务使用自定义客户端 `OrderResponseClient` 将 MSMQ 消息发送到队列。 由于接收和处理消息的应用程序是 MSMQ 应用程序，而不是 WCF 应用程序，因此两个应用程序之间没有隐式服务协定。 所以在此方案中，我们不能使用 Svcutil.exe 工具创建代理。

 对于使用绑定发送消息的所有 WCF 应用程序而言，自定义代理本质上是相同的 `msmqIntegrationBinding` 。 与其他代理不同，它不包含一系列服务操作。 它只是一个提交消息操作。

```csharp
[System.ServiceModel.ServiceContractAttribute(Namespace = "http://Microsoft.ServiceModel.Samples")]
public interface IOrderResponse
{

    [System.ServiceModel.OperationContractAttribute(IsOneWay = true, Action = "*")]
    void SendOrderResponse(MsmqMessage<PurchaseOrder> msg);
}

public partial class OrderResponseClient : System.ServiceModel.ClientBase<IOrderResponse>, IOrderResponse
{

    public OrderResponseClient()
    { }

    public OrderResponseClient(string configurationName)
        : base(configurationName)
    { }

    public OrderResponseClient(System.ServiceModel.Channels.Binding binding, System.ServiceModel.EndpointAddress address)
        : base(binding, address)
    { }

    public void SendOrderResponse(MsmqMessage<PurchaseOrder> msg)
    {
        base.Channel.SendOrderResponse(msg);
    }
}
```

 服务是自承载服务。 使用 MSMQ 集成传输时，必须提前创建所使用的队列。 可以手动或通过代码完成此操作。 在此示例中，服务包含 <xref:System.Messaging> 代码，以检查队列是否存在并在必要时创建队列。 从配置文件中读取队列名称。

```csharp
public static void Main()
{
       // Get the MSMQ queue name from application settings in configuration.
      string queueName =
                ConfigurationManager.AppSettings["orderQueueName"];
      // Create the transacted MSMQ queue if necessary.
      if (!MessageQueue.Exists(queueName))
                MessageQueue.Create(queueName, true);
     // Create a ServiceHost for the OrderProcessorService type.
     using (ServiceHost serviceHost = new
                   ServiceHost(typeof(OrderProcessorService)))
     {
            serviceHost.Open();
            // The service can now be accessed.
            Console.WriteLine("The service is ready.");
            Console.WriteLine("Press <ENTER> to terminate service.");
            Console.ReadLine();
            // Close the ServiceHost to shutdown the service.
            serviceHost.Close();
      }
}
```

 订单请求发送至的 MSMQ 队列是在配置文件的 appSettings 节中指定的。 客户端终结点和服务终结点是在配置文件的 system.serviceModel 节中定义的。 二者均指定了 `msmqIntegrationbinding` 绑定。

```xml
<appSettings>
  <add key="orderQueueName" value=".\private$\Orders" />
</appSettings>

<system.serviceModel>
  <client>
    <endpoint    name="OrderResponseEndpoint"
              address="msmq.formatname:DIRECT=OS:.\private$\OrderResponse"
              binding="msmqIntegrationBinding"
              bindingConfiguration="OrderProcessorBinding"
              contract="Microsoft.ServiceModel.Samples.IOrderResponse">
    </endpoint>
  </client>

  <services>
    <service
      name="Microsoft.ServiceModel.Samples.OrderProcessorService">
      <endpoint address="msmq.formatname:DIRECT=OS:.\private$\Orders"
                            binding="msmqIntegrationBinding"
                bindingConfiguration="OrderProcessorBinding"
                contract="Microsoft.ServiceModel.Samples.IOrderProcessor">
      </endpoint>
    </service>
  </services>

  <bindings>
    <msmqIntegrationBinding>
      <binding name="OrderProcessorBinding" >
        <security mode="None" />
      </binding>
    </msmqIntegrationBinding>
  </bindings>

</system.serviceModel>
```

 客户端应用程序使用 <xref:System.Messaging> 向队列发送持久的事务性消息。 消息的正文包含采购订单。

```csharp
static void PlaceOrder()
{
    //Connect to the queue
    MessageQueue orderQueue =
            new MessageQueue(
                    ConfigurationManager.AppSettings["orderQueueName"])
    // Create the purchase order.
    PurchaseOrder po = new PurchaseOrder();
    po.CustomerId = "somecustomer.com";
    po.PONumber = Guid.NewGuid().ToString();
    PurchaseOrderLineItem lineItem1 = new PurchaseOrderLineItem();
    lineItem1.ProductId = "Blue Widget";
    lineItem1.Quantity = 54;
    lineItem1.UnitCost = 29.99F;

    PurchaseOrderLineItem lineItem2 = new PurchaseOrderLineItem();
    lineItem2.ProductId = "Red Widget";
    lineItem2.Quantity = 890;
    lineItem2.UnitCost = 45.89F;

    po.orderLineItems = new PurchaseOrderLineItem[2];
    po.orderLineItems[0] = lineItem1;
    po.orderLineItems[1] = lineItem2;

    Message msg = new Message();
    msg.UseDeadLetterQueue = true;
    msg.Body = po;

    //Create a transaction scope.
    using (TransactionScope scope = new
     TransactionScope(TransactionScopeOption.Required))
    {
        // Submit the purchase order.
        orderQueue.Send(msg, MessageQueueTransactionType.Automatic);
        // Complete the transaction.
        scope.Complete();
    }
    //Save the messageID for order response correlation.
    orderMessageID = msg.Id;
    Console.WriteLine("Placed the order, waiting for response...");
}
```

 从中接收订单响应的 MSMQ 队列是在配置文件的 appSettings 节中定义的，如下面的示例配置所示。

> [!NOTE]
> 队列名称为本地计算机使用圆点 (.)，并在其路径中使用反斜杠分隔符。 WCF 终结点地址指定 msmq.formatname 方案，并为本地计算机使用 "localhost"。 根据 MSMQ 准则，格式正确的格式名称应遵循 URI 中的 msmq.formatname。

```xml
<appSettings>
    <add key=" orderResponseQueueName" value=".\private$\Orders" />
</appSettings>
```

 客户端应用程序保存发送到服务的订单请求消息的 `messageID`，并等待服务响应。 一旦响应到达队列，客户端使用消息的 `correlationID` 属性将该响应与客户端发送的订单消息关联，此属性包含客户端最初向服务发送的订单消息的 `messageID`。

```csharp
static void DisplayOrderStatus()
{
    MessageQueue orderResponseQueue = new
     MessageQueue(ConfigurationManager.AppSettings
                  ["orderResponseQueueName"]);
    //Create a transaction scope.
    bool responseReceived = false;
    orderResponseQueue.MessageReadPropertyFilter.CorrelationId = true;
    while (!responseReceived)
    {
       Message responseMsg;
       using (TransactionScope scope2 = new
         TransactionScope(TransactionScopeOption.Required))
       {
          //Receive the Order Response message.
          responseMsg =
              orderResponseQueue.Receive
                   (MessageQueueTransactionType.Automatic);
          scope2.Complete();
     }
     responseMsg.Formatter = new
     System.Messaging.XmlMessageFormatter(new Type[] {
         typeof(PurchaseOrder) });
     PurchaseOrder responsepo = (PurchaseOrder)responseMsg.Body;
    //Check if the response is for the order placed.
    if (orderMessageID == responseMsg.CorrelationId)
    {
       responseReceived = true;
       Console.WriteLine("Status of current Order: OrderID-{0},Order
            Status-{1}", responsepo.PONumber, responsepo.Status);
    }
    else
    {
       Console.WriteLine("Status of previous Order: OrderID-{0},Order
            Status-{1}", responsepo.PONumber, responsepo.Status);
    }
  }
}
```

 运行示例时，客户端和服务活动将显示在服务和客户端控制台窗口中。 可以看到服务接收来自客户端的消息，并向客户端回发响应。 客户端会显示来自服务的响应。 在每个控制台窗口中按 Enter 可以关闭服务和客户端。

> [!NOTE]
> 此示例要求安装消息队列 (MSMQ)。 请参阅“请参见”部分中的 MSMQ 安装说明。

## <a name="set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 如果先运行服务，则它将检查以确保队列存在。 如果队列不存在，则服务将创建一个队列。 可以先运行服务以创建队列或通过 MSMQ 队列管理器创建一个队列。 执行下面的步骤来在 Windows 2008 中创建队列。

    1. 在 Visual Studio 2012 中打开服务器管理器。

    2. 展开 " **功能** " 选项卡。

    3. 右键单击 "**专用消息队列**"，然后选择 "**新建****专用队列**"。

    4. 选中 " **事务性** " 框。

    5. 输入 `ServiceModelSamplesTransacted` 作为新队列的名称。

3. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

4. 若要在单计算机配置中运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

## <a name="run-the-sample-across-computers"></a>跨计算机运行示例

1. 将 \service\bin\ 文件夹（在语言特定文件夹内）中的服务程序文件复制到服务计算机上。

2. 将 \client\bin\ 文件夹（在语言特定文件夹内）中的客户端程序文件复制到客户端计算机上。

3. 在 Client.exe.config 文件中，更改 orderQueueName 以指定服务计算机名称，而不是使用“.”。

4. 在 Service.exe.config 文件中，更改客户端终结点地址以指定客户端计算机名称，而不是使用“.”。

5. 在服务计算机上，在命令提示符下启动 Service.exe。

6. 在客户端计算机上，在命令提示符下启动 Client.exe。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\MSMQIntegration\MessageCorrelation`

## <a name="see-also"></a>请参阅

- [在 WCF 中排队](../feature-details/queuing-in-wcf.md)
- [消息队列](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85))
