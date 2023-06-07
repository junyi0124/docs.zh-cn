---
title: 已经过事务处理的 MSMQ 绑定
ms.date: 03/30/2017
ms.assetid: 71f5cb8d-f1df-4e1e-b8a2-98e734a75c37
ms.openlocfilehash: fa53099caba144f321698f180fe18f7614a1fa64
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596560"
---
# <a name="transacted-msmq-binding"></a>已经过事务处理的 MSMQ 绑定

本示例演示如何使用消息队列 (MSMQ) 执行已经过事务处理的排队通信。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

在排队通信中，客户端使用队列与服务进行通信。 更确切地说，客户端向队列发送消息。 服务从队列接收消息。 因此，不必同时运行服务和客户端便可使用队列进行通信。

当事务用于发送和接收消息时，实际上有两个单独的事务。 当客户端在事务范围内发送消息时，事务相对于客户端和客户端队列管理器来说是本地事务。 当服务在事务范围内接收消息时，事务相对于服务和接收队列管理器来说是本地事务。 请务必牢记，客户端和服务不会参与同一个事务；实际上，它们在对队列执行操作（如发送和接收）时使用的是不同的事务。

在本示例中，客户端在事务范围内向服务发送一批消息。 然后，服务在服务定义的事务范围内接收发送到队列的消息。

服务协定为 `IOrderProcessor`，如下面的示例代码所示。 此接口定义了适合与队列一起使用的单向服务。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po);
}
```

服务行为通过将 `TransactionScopeRequired` 设置为 `true` 来定义操作行为。 这可确保此方法所访问的任何资源管理器都使用相同的事务范围从队列中检索消息。 它还确保在方法引发异常的情况下，将消息返回到队列。 如果未设置操作行为，队列通道会创建一个从队列中读取消息的事务，并在调度前自动提交该事务，这样当操作失败时，消息将丢失。 对于服务操作而言，最常见的方案是在用于从队列中读取消息的事务中进行登记，如下面的代码中所示。

```csharp
 // This service class that implements the service contract.
 // This added code writes output to the console window.
 public class OrderProcessorService : IOrderProcessor
 {
     [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
     public void SubmitPurchaseOrder(PurchaseOrder po)
     {
         Orders.Add(po);
         Console.WriteLine("Processing {0} ", po);
     }
  …
}
```

服务是自承载服务。 使用 MSMQ 传输时，必须提前创建所使用的队列。 可以手动或通过代码完成此操作。 在此示例中，该服务包含代码，以检查队列是否存在并在不存在时创建队列。 从配置文件中读取队列名称。 基址[元数据实用工具（svcutil.exe）](../servicemodel-metadata-utility-tool-svcutil-exe.md)使用该基址来生成服务的代理。

```csharp
// Host the service within this EXE console application.
public static void Main()
{
    // Get the MSMQ queue name from appSettings in configuration.
    string queueName = ConfigurationManager.AppSettings["queueName"];

    // Create the transacted MSMQ queue if necessary.
    if (!MessageQueue.Exists(queueName))
        MessageQueue.Create(queueName, true);

    // Create a ServiceHost for the OrderProcessorService type.
    using (ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService)))
    {
        // Open the ServiceHost to create listeners and start listening for messages.
        serviceHost.Open();

        // The service can now be accessed.
        Console.WriteLine("The service is ready.");
        Console.WriteLine("Press <ENTER> to terminate service.");
        Console.WriteLine();
        Console.ReadLine();

        // Close the ServiceHost to shut down the service.
        serviceHost.Close();
    }
}
```

MSMQ 队列名称在配置文件的 appSettings 节中指定，如以下示例配置所示。

```xml
<appSettings>
    <add key="queueName" value=".\private$\ServiceModelSamplesTransacted" />
</appSettings>
```

> [!NOTE]
> 队列名称为本地计算机使用圆点 (.)，并在使用 <xref:System.Messaging> 创建队列时在其路径中使用反斜杠分隔符。 Windows Communication Foundation （WCF）终结点使用带有 net.pipe 方案的队列地址，使用 "localhost" 来表示本地计算机，并在其路径中使用正斜杠。

客户端创建事务范围。 与队列的通信发生在事务范围之内，从而可将该事务范围视为原子单元，其中所有消息均将发送到队列或者没有任何消息发送到队列。 通过在事务范围上调用 <xref:System.Transactions.TransactionScope.Complete%2A> 可以提交事务。

```csharp
// Create a client.
OrderProcessorClient client = new OrderProcessorClient();

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

// Create a transaction scope.
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
{
    // Make a queued call to submit the purchase order.
    client.SubmitPurchaseOrder(po);
    // Complete the transaction.
    scope.Complete();
}

// Closing the client gracefully closes the connection and cleans up resources.
client.Close();

Console.WriteLine();
Console.WriteLine("Press <ENTER> to terminate client.");
Console.ReadLine();
```

若要验证事务正在运行，请按照下面的示例代码注释事务范围来修改客户端，重新生成解决方案并运行客户端。

```csharp
//scope.Complete();
```

因为事务没有完成，所以消息未发送到队列。

运行示例时，客户端和服务活动将显示在服务和客户端控制台窗口中。 您可以看到服务从客户端接收消息。 在每个控制台窗口中按 Enter 可以关闭服务和客户端。 请注意：由于正在使用队列，因此不必同时启动和运行客户端和服务。 可以先运行客户端，再将其关闭，然后启动服务，这样服务仍然会收到客户端的消息。

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 7b31ce51-ae7c-4def-9b8b-617e4288eafd
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 如果先运行服务，则它将检查以确保队列存在。 如果队列不存在，则服务将创建一个队列。 可以先运行服务以创建队列或通过 MSMQ 队列管理器创建一个队列。 执行下面的步骤来在 Windows 2008 中创建队列。

    1. 在 Visual Studio 2012 中打开服务器管理器。

    2. 展开 "**功能**" 选项卡。

    3. 右键单击 "**专用消息队列**"，然后选择 "**新建****专用队列**"。

    4. 选中 "**事务性**" 框。

    5. 输入 `ServiceModelSamplesTransacted` 作为新队列的名称。

3. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

4. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

默认情况下使用 <xref:System.ServiceModel.NetMsmqBinding> 启用传输安全。 对于 MSMQ 传输安全存在两个相关属性：<xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> 和 <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A>。 默认情况下，身份验证模式设置为 `Windows`，保护级别设置为 `Sign`。 为了使 MSMQ 提供身份验证和签名功能，MSMQ 必须是域的一部分，并且必须安装 MSMQ 的 Active Directory 集成选项。 如果在未满足这些条件的计算机上运行此示例，将会收到错误。

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup-or-without-active-directory-integration"></a>在加入到工作组或在没有 Active Directory 集成的计算机上运行示例

1. 如果计算机不是域成员或尚未安装 Active Directory 集成，请将身份验证模式和保护级别设置为 `None` 以关闭传输安全性，如下面的示例配置代码所示。

    ```xml
    <system.serviceModel>
      <services>
        <service name="Microsoft.ServiceModel.Samples.OrderProcessorService"
                 behaviorConfiguration="OrderProcessorServiceBehavior">
          <host>
            <baseAddresses>
              <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>
            </baseAddresses>
          </host>
          <!-- Define NetMsmqEndpoint. -->
          <endpoint
              address="net.msmq://localhost/private/ServiceModelSamplesTransacted"
              binding="netMsmqBinding"
              bindingConfiguration="Binding1"
           contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
          <!-- The mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex. -->
          <endpoint address="mex"
                    binding="mexHttpBinding"
                    contract="IMetadataExchange" />
        </service>
      </services>

      <bindings>
        <netMsmqBinding>
          <binding name="Binding1">
            <security mode="None" />
          </binding>
        </netMsmqBinding>
      </bindings>

        <behaviors>
          <serviceBehaviors>
            <behavior name="OrderProcessorServiceBehavior">
              <serviceMetadata httpGetEnabled="True"/>
            </behavior>
          </serviceBehaviors>
        </behaviors>

      </system.serviceModel>
    ```

2. 确保在运行示例前更改服务器和客户端上的配置。

    > [!NOTE]
    > 将 `security mode` 设置为 `None` 等效于将 <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A>、<xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> 和 `Message` 安全设置为 `None`。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\Transacted`
