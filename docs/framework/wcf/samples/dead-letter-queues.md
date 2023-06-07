---
title: 死信队列
ms.date: 03/30/2017
ms.assetid: ff664f33-ad02-422c-9041-bab6d993f9cc
ms.openlocfilehash: cf281ff08d56669d0257d693af93d8a9b5b2e81a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96292819"
---
# <a name="dead-letter-queues"></a>死信队列

本示例演示如何处理传递失败的消息。 它基于 [事务性 MSMQ 绑定](transacted-msmq-binding.md) 示例。 本示例使用 `netMsmqBinding` 绑定。 此服务是自承载控制台应用程序，通过它可以观察服务接收排队消息。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

> [!NOTE]
> 此示例演示仅适用于 Windows Vista 的每个应用程序死信队列。 可以修改此示例以在 Windows Server 2003 和 Windows XP 上使用 MSMQ 3.0 的默认系统范围队列。

 在排队通信中，客户端使用队列与服务进行通信。 更确切地说，客户端向队列发送消息。 服务从队列接收消息。 因此不必同时运行服务和客户端便可使用队列进行通信。

 由于排队通信可能需要一定的休眠时间，因此可能需要在消息上关联生存期时间值，以确保消息超过该时间后不会发送到应用程序。 在某些情况下，还必须通知应用程序消息传递是否失败。 在所有这些情况下（如消息的生存期过期或消息传递失败），会将消息放入死信队列中。 然后，执行发送任务的应用程序可以读取死信队列中的消息并采取纠正措施（从不执行任何操作到纠正传递失败的原因），并重新发送消息。

 `NetMsmqBinding` 绑定中的死信队列用下面的属性表示：

- <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> 属性表示客户端需要的死信队列。 此枚举具有下列值：

- `None`：客户端不需要死信队列。

- `System`：系统死信队列用于存储死消息。 系统死信队列由计算机上运行的所有应用程序共享。

- `Custom`：使用 <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> 属性指定的自定义死信队列用于存储死消息。 此功能仅适用于 Windows Vista。 当应用程序必须使用自己的死信队列而不与同一计算机上运行的其他应用程序共享该死信队列时使用此功能。

- <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> 属性表示要用作死信队列的特定队列。 这仅适用于 Windows Vista。

 在本示例中，客户端在事务范围内将一批消息发送到服务并为这些消息的“生存期”指定任意低的值（约 2 秒钟）。 客户端还指定一个自定义死信队列用于排队已过期的消息。

 客户端应用程序可以读取死信队列中的消息，并重试发送消息或纠正导致原始消息放入死信队列的错误，然后发送消息。 在本示例中，客户端显示一条错误消息。

 服务协定为 `IOrderProcessor`，如下面的示例代码所示。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po);
}
```

 该示例中的服务代码是 [事务性 MSMQ 绑定](transacted-msmq-binding.md)的。

 与服务的通信发生在事务范围内。 服务读取队列中的消息，执行操作，然后显示操作的结果。 应用程序还为死信消息创建死信队列。

```csharp
//The service contract is defined in generatedClient.cs, generated from the service by the svcutil tool.

//Client implementation code.
class Client
{
    static void Main()
    {
        // Get MSMQ queue name from app settings in configuration
        string deadLetterQueueName = ConfigurationManager.AppSettings["deadLetterQueueName"];

        // Create the transacted MSMQ queue for storing dead message if necessary.
        if (!MessageQueue.Exists(deadLetterQueueName))
            MessageQueue.Create(deadLetterQueueName, true);

        // Create a proxy with given client endpoint configuration
        OrderProcessorClient client = new OrderProcessorClient("OrderProcessorEndpoint");

        // Create the purchase order
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

        //Create a transaction scope.
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
        {
            // Make a queued call to submit the purchase order
            client.SubmitPurchaseOrder(po);
            // Complete the transaction.
            scope.Complete();
        }

        client.Close();

        Console.WriteLine();
        Console.WriteLine("Press <ENTER> to terminate client.");
        Console.ReadLine();
    }
}
```

 客户端的配置为消息到达服务指定一个短的持续时间。 如果消息无法在指定的持续时间内传输，则该消息将会过期并将移动到死信队列中。

> [!NOTE]
> 客户端可以在指定的时间内将消息传送到服务队列。 为确可以在操作中看到死信服务，应在启动服务之前运行客户端。 消息将超时并被传送到死信服务。

 应用程序必须定义使用哪个队列作为其死信队列。 如果未指定队列，则使用默认系统范围事务性死信队列对死消息排队。 在本示例中，客户端应用程序指定其自己的应用程序死信队列。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <!-- use appSetting to configure MSMQ Dead Letter queue name -->
    <add key="deadLetterQueueName" value=".\private$\ServiceModelSamplesOrdersAppDLQ"/>
  </appSettings>

  <system.serviceModel>
    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                address="net.msmq://localhost/private/ServiceModelSamplesDeadLetter"
                binding="netMsmqBinding"
                bindingConfiguration="PerAppDLQBinding"
                contract="IOrderProcessor" />
    </client>

    <bindings>
      <netMsmqBinding>
        <binding name="PerAppDLQBinding"
                 deadLetterQueue="Custom"
                 customDeadLetterQueue="net.msmq://localhost/private/ServiceModelSamplesOrdersAppDLQ"
                 timeToLive="00:00:02"/>
      </netMsmqBinding>
    </bindings>
  </system.serviceModel>

</configuration>
```

 死信消息服务从死信队列中读取消息。 死信消息服务实现 `IOrderProcessor` 协定。 但其实现不处理订单。 死信消息服务是客户端服务，没有处理订单的功能。

> [!NOTE]
> 死信队列是客户端队列，对于客户端队列管理器来说是本地队列。

 死信消息服务实现可检查消息传递失败的原因并采取纠正措施。 消息失败的原因在两个枚举 <xref:System.ServiceModel.Channels.MsmqMessageProperty.DeliveryFailure%2A> 和 <xref:System.ServiceModel.Channels.MsmqMessageProperty.DeliveryStatus%2A> 中捕获。 您可以从 <xref:System.ServiceModel.Channels.MsmqMessageProperty> 中检索 <xref:System.ServiceModel.OperationContext>，如下面的示例代码所示：

```csharp
public void SubmitPurchaseOrder(PurchaseOrder po)
{
    Console.WriteLine("Submitting purchase order did not succeed ", po);
    MsmqMessageProperty mqProp =
                  OperationContext.Current.IncomingMessageProperties[
                  MsmqMessageProperty.Name] as MsmqMessageProperty;
    Console.WriteLine("Message Delivery Status: {0} ",
                                                mqProp.DeliveryStatus);
    Console.WriteLine("Message Delivery Failure: {0}",
                                               mqProp.DeliveryFailure);
    Console.WriteLine();
    …
}
```

 死信队列中的消息是被发送到处理消息的服务的消息。 因此，当死信消息服务从队列中读取消息时，Windows Communication Foundation (WCF) 通道层查找终结点中不匹配项，并且不会调度消息。 在这种情况下，会将消息发送到订单处理服务，但由死信消息服务接收。 若要接收发送到不同终结点的消息，请在 `ServiceBehavior` 中指定一个可匹配任何地址的地址筛选器。 这是成功处理从死信队列中读取的消息所必需的。

 在此示例中，如果失败的原因是消息超时，则死信消息服务将重新发送消息。出于所有其他原因，它会显示传递失败，如下面的示例代码所示：

```csharp
// Service class that implements the service contract.
// Added code to write output to the console window.
[ServiceBehavior(InstanceContextMode=InstanceContextMode.Single, ConcurrencyMode=ConcurrencyMode.Single, AddressFilterMode=AddressFilterMode.Any)]
public class PurchaseOrderDLQService : IOrderProcessor
{
    OrderProcessorClient orderProcessorService;
    public PurchaseOrderDLQService()
    {
        orderProcessorService = new OrderProcessorClient("OrderProcessorEndpoint");
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po)
    {
        Console.WriteLine("Submitting purchase order did not succeed ", po);
        MsmqMessageProperty mqProp = OperationContext.Current.IncomingMessageProperties[MsmqMessageProperty.Name] as MsmqMessageProperty;

        Console.WriteLine("Message Delivery Status: {0} ", mqProp.DeliveryStatus);
        Console.WriteLine("Message Delivery Failure: {0}", mqProp.DeliveryFailure);
        Console.WriteLine();

        // resend the message if timed out
        if (mqProp.DeliveryFailure == DeliveryFailure.ReachQueueTimeout ||
            mqProp.DeliveryFailure == DeliveryFailure.ReceiveTimeout)
        {
            // re-send
            Console.WriteLine("Purchase order Time To Live expired");
            Console.WriteLine("Trying to resend the message");

            // reuse the same transaction used to read the message from dlq to enqueue the message to app. queue
            orderProcessorService.SubmitPurchaseOrder(po);
            Console.WriteLine("Purchase order resent");
        }
    }

    // Host the service within this EXE console application.
    public static void Main()
    {
        // Create a ServiceHost for the PurchaseOrderDLQService type.
        using (ServiceHost serviceHost = new ServiceHost(typeof(PurchaseOrderDLQService)))
        {
            // Open the ServiceHostBase to create listeners and start listening for messages.
            serviceHost.Open();

            // The service can now be accessed.
            Console.WriteLine("The dead letter service is ready.");
            Console.WriteLine("Press <ENTER> to terminate service.");
            Console.WriteLine();
            Console.ReadLine();
        }
    }
}
```

 下面的示例演示死信消息的配置：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.PurchaseOrderDLQService">
        <!-- Define NetMsmqEndpoint in this case, DLQ end point to read messages-->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesOrdersAppDLQ"
                  binding="netMsmqBinding"
                  bindingConfiguration="DefaultBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
      </service>
    </services>

    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                 address="net.msmq://localhost/private/ServiceModelSamplesDeadLetter"
                 binding="netMsmqBinding"
                 bindingConfiguration="SystemDLQBinding"
                 contract="IOrderProcessor" />
    </client>

    <bindings>
      <netMsmqBinding>
        <binding name="DefaultBinding" />
        <binding name="SystemDLQBinding"
                 deadLetterQueue="System"/>
      </netMsmqBinding>
    </bindings>
  </system.serviceModel>
</configuration>
```

 在运行示例的过程中，需要运行 3 个可执行文件以查看死信队列如何适用于每个应用程序：客户端、服务和死信服务（从每个应用程序的死信队列中读取消息并将消息重新发送到服务）。 所有这些应用程序都是控制台应用程序，均在控制台窗口中输出结果。

> [!NOTE]
> 由于队列正在使用中，因此不必同时启动和运行客户端和服务。 可以先运行客户端，再将其关闭，然后启动服务，这样服务仍然会收到客户端的消息。 您应该启动服务，然后关闭服务，以便创建队列。

 在运行客户端时，客户端将显示消息：

```console
Press <ENTER> to terminate client.
```

 客户端尝试发送消息，但由于短的超时，消息过期并在每个应用程序的死信队列中排队。

 然后运行死信服务，读取消息并显示错误代码，然后将消息重新发回服务。

```console
The dead letter service is ready.
Press <ENTER> to terminate service.

Submitting purchase order did not succeed
Message Delivery Status: InDoubt
Message Delivery Failure: ReachQueueTimeout

Purchase order Time To Live expired
Trying to resend the message
Purchase order resent
```

 服务启动，然后读取重新发送的消息并处理该消息。

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 97897eff-f926-4057-a32b-af8fb11b9bf9
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 如果先运行服务，则它将检查以确保队列存在。 如果队列不存在，则服务将创建一个队列。 可以先运行服务以创建队列或通过 MSMQ 队列管理器创建一个队列。 执行下面的步骤来在 Windows 2008 中创建队列。

    1. 在 Visual Studio 2012 中打开服务器管理器。

    2. 展开 " **功能** " 选项卡。

    3. 右键单击 "**专用消息队列**"，然后选择 "**新建****专用队列**"。

    4. 选中 " **事务性** " 框。

    5. 输入 `ServiceModelSamplesTransacted` 作为新队列的名称。

3. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

4. 若要在单一计算机配置或跨计算机配置中运行示例，请相应地更改队列名称，用计算机的全名替换 localhost，并按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>在加入到工作组的计算机上运行此示例

1. 如果计算机不是域成员，请将身份验证模式和保护级别设置为 `None` 以禁用传输安全性，如下面的示例配置所示：

    ```xml
    <bindings>
        <netMsmqBinding>
            <binding name="TransactedBinding">
                <security mode="None"/>
            </binding>
        </netMsmqBinding>
    </bindings>
    ```

     确保通过设置终结点的 `bindingConfiguration` 属性将终结点与绑定关联。

2. 确保在运行示例前更改 DeadLetterService、服务器和客户端上的配置。

    > [!NOTE]
    > 将 `security mode` 设置为 `None` 等效于将 `MsmqAuthenticationMode`、`MsmqProtectionLevel` 和 `Message` 安全设置为 `None`。

## <a name="comments"></a>注释

 默认情况下对 `netMsmqBinding` 绑定传输启用了安全性。 `MsmqAuthenticationMode` 和 `MsmqProtectionLevel` 这两个属性共同确定了传输安全性的类型。 默认情况下，身份验证模式设置为 `Windows`，保护级别设置为 `Sign`。 MSMQ 必须是域的成员才可以提供身份验证和签名功能。 如果在不是域成员的计算机上运行此示例，则会接收以下错误：“用户的内部消息队列证书不存在”。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\DeadLetter`  
