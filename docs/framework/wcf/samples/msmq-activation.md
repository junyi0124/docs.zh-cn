---
title: MSMQ 激活
ms.date: 03/30/2017
ms.assetid: e3834149-7b8c-4a54-806b-b4296720f31d
ms.openlocfilehash: 349eadb8f517993c343e81656204ad25e62ed931
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555232"
---
# <a name="msmq-activation"></a>MSMQ 激活

本示例演示如何在 Windows 进程激活服务 (WAS) 中承载从消息队列读取的应用程序。 此示例使用 `netMsmqBinding` 和基于 [双向通信](two-way-communication.md) 示例。 本示例中的服务是一个 Web 承载的应用程序，而客户端是自承载的，并输出到控制台以观察提交的采购订单的状态。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

> [!NOTE]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> \<InstallDrive>： \ WF_WCF_Samples
>
> 如果此目录不存在，请跳到 [Windows Communication Foundation (WCF) ，并 Windows Workflow Foundation (的 WF) 示例](https://www.microsoft.com/download/details.aspx?id=21459) ，.NET Framework 4 下载所有 WCF 和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> \<InstallDrive>:\Samples\WCFWFCardSpace\WCF\Basic\Services\Hosting\WASHost\MsmqActivation.

Windows 进程激活服务 (已) ，用于 Windows Server 2008 的新进程激活机制提供了类似 IIS 的功能，这些功能以前仅对使用非 HTTP 协议的应用程序可用的基于 HTTP 的应用程序可用。 Windows Communication Foundation (WCF) 使用侦听器适配器接口传递通过 WCF 支持的非 HTTP 协议（如 TCP、命名管道和 MSMQ）接收的激活请求。 用于通过非 HTTP 协议接收请求的功能由 SMSvcHost.exe 中运行的托管 Windows 服务承载。

Net.Msmq Listener Adapter 服务 (NetMsmqActivator) 根据队列中的消息激活排队的应用程序。

客户端从事务范围内向服务发送采购订单。 服务在事务中接收订单并处理订单。 然后服务使用订单状态回调客户端。 为了便于双向通信，客户端和服务都使用队列以便将采购订单和订单状态排入队列。

服务协定 `IOrderProcessor` 定义使用队列的单向服务操作。 服务操作使用答复终结点将订单状态发送到客户端。 答复终结点的地址是用于将订单状态发回客户端的队列的 URI。 订单处理应用程序实现下面的协定。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po,
                                           string reportOrderStatusTo);
}
```

向其发送订单状态的答复协定由客户端指定。 客户端实现订单状态协定。 服务使用下面协定的生成的客户端将订单状态发回客户端。

```csharp
[ServiceContract]
public interface IOrderStatus
{
    [OperationContract(IsOneWay = true)]
    void OrderStatus(string poNumber, string status);
}
```

服务操作处理提交的采购订单。 对服务操作应用 <xref:System.ServiceModel.OperationBehaviorAttribute> 以在用于从队列中接收消息的事务中指定自动登记，并指定在服务操作完成时事务自动完成。 `Orders` 类封装了订单处理功能。 在本例中，它将采购订单添加到字典。 `Orders` 类中的操作可以使用服务操作登记的事务。

服务操作除了处理提交的采购订单之外，还向客户端答复有关订单状态的信息。

```csharp
public class OrderProcessorService : IOrderProcessor
{
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po, string reportOrderStatusTo)
    {
        Orders.Add(po);
        Console.WriteLine("Processing {0} ", po);
        Console.WriteLine("Sending back order status information");
        NetMsmqBinding msmqCallbackBinding = new NetMsmqBinding();
        msmqCallbackBinding.Security.Mode = NetMsmqSecurityMode.None;
        OrderStatusClient client = new OrderStatusClient(msmqCallbackBinding, new EndpointAddress(reportOrderStatusTo));
        // please note that the same transaction that is used to dequeue purchase order is used
        // to send back order status
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
        {
            client.OrderStatus(po.PONumber, po.Status);
            scope.Complete();
        }
    }
}
```

要使用的客户端绑定是使用配置文件指定的。

MSMQ 队列名称是在配置文件的 appSettings 节中指定的。 服务的终结点是在配置文件的 System.serviceModel 节中定义的。

> [!NOTE]
> MSMQ 队列名称和终结点地址使用略有不同的寻址约定。 MSMQ 队列名称为本地计算机使用圆点 (.)，并在其路径中使用反斜杠分隔符。 WCF 终结点地址指定一个 net.pipe：方案，使用本地计算机的 "localhost"，并在其路径中使用正斜杠。 若要从在远程计算机上承载的队列读取数据，请将“.”和“localhost”替换为远程计算机名称。

使用一个以类名作为名称的 .svc 文件来承载 WAS 中的服务代码。

Service.svc 文件本身包含用于创建 `OrderProcessorService` 的指令。

`<%@ServiceHost language="c#" Debug="true" Service="Microsoft.ServiceModel.Samples.OrderProcessorService"%>`

Service.svc 文件还包含一个程序集指令以确保加载 System.Transactions.dll。

`<%@Assembly name="System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"%>`

客户端创建事务范围。 与服务的通信在事务范围内进行，从而可以将事务范围视为所有消息在其中成功或失败的原子单元。 通过在事务范围上调用 `Complete` 可以提交事务。

```csharp
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderStatusService)))
{
    // Open the ServiceHostBase to create listeners and start listening
    // for order status messages.
    serviceHost.Open();

    // Create a proxy with given client endpoint configuration
    OrderProcessorClient client = new OrderProcessorClient();

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
    using (TransactionScope scope = new
        TransactionScope(TransactionScopeOption.Required))
    {
        // Make a queued call to submit the purchase order
        client.SubmitPurchaseOrder(po,
       "net.msmq://localhost/private/ServiceModelSamplesOrder/OrderStatus");
        // Complete the transaction.
        scope.Complete();
    }

    //Closing the client gracefully closes the connection and cleans up
    //resources
    client.Close();

    Console.WriteLine();
    Console.WriteLine("Press <ENTER> to terminate client.");
    Console.ReadLine();

    // Close the ServiceHostBase to shutdown the service.
    serviceHost.Close();
    }
```

客户端代码实现 `IOrderStatus` 协定以便从服务接收订单状态。 在本例中，它输出订单状态。

```csharp
[ServiceBehavior]
public class OrderStatusService : IOrderStatus
{
    [OperationBehavior(TransactionAutoComplete = true,
                        TransactionScopeRequired = true)]
    public void OrderStatus(string poNumber, string status)
    {
        Console.WriteLine("Status of order {0}:{1} ",
                                         poNumber , status);
    }
}
```

订单状态队列在 `Main` 方法中创建。 客户端配置包括订单状态服务配置，以便承载订单状态服务，如下面的示例配置所示。

```xml
<appSettings>
    <!-- use appSetting to configure MSMQ queue name -->
    <add key="targetQueueName" value=".\private$\ServiceModelSamples/service.svc" />
    <add key="responseQueueName" value=".\private$\ServiceModelSamples/OrderStatus" />
  </appSettings>

<system.serviceModel>

    <services>
      <service
         name="Microsoft.ServiceModel.Samples.OrderStatusService">
        <!-- Define NetMsmqEndpoint -->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamples/OrderStatus"
                  binding="netMsmqBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderStatus" />
      </service>
    </services>

    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                address="net.msmq://localhost/private/ServiceModelSamples/service.svc"
                binding="netMsmqBinding"
                contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
    </client>

  </system.serviceModel>
```

运行示例时，客户端和服务活动将显示在服务器和客户端控制台窗口中。 您可以看到服务器从客户端接收消息。 在每个控制台窗口中按 Enter 可以关闭服务器和客户端。

客户端显示由服务器发送的订单状态信息：

```console
Press <ENTER> to terminate client.
Status of order 70cf9d63-3dfa-4e69-81c2-23aa4478ebed :Pending
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保安装了 IIS 7.0，因为 WAS 激活所必需的。

2. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。 此外，还必须安装 WCF 非 HTTP 激活组件：

    1. 从“开始”菜单中，选择“控制面板” 。

    2. 选择 " **程序和功能**"。

    3. 单击 **"打开或关闭 Windows 功能"**。

    4. 在 " **功能摘要**" 下，单击 " **添加功能**"。

    5. 展开 **Microsoft .NET Framework 3.0** 节点并检查 **Windows Communication Foundation 非 HTTP 激活** 功能。

3. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

4. 通过从命令窗口执行 client.exe 运行客户端。 这将创建队列并向其发送消息。 让客户端保持运行以观察服务读取消息的结果

5. 在默认情况下 MSMQ 激活服务将作为网络服务运行。 因此，用于激活应用程序的队列必须具有对网络服务的接收和查看权限。 可以通过使用消息队列 MMC 来添加这些权限：

    1. 从 " **开始** " 菜单中，单击 " **运行**"，然后键入 `Compmgmt.msc` 并按 enter。

    2. 在 " **服务和应用程序**" 下，展开 " **消息队列**"。

    3. 单击 " **专用队列**"。

    4. 右键单击队列 (servicemodelsamples/node.js) 并选择 " **属性**"。

    5. 在 " **安全** " 选项卡上，单击 " **添加** " 并向网络服务授予 "查看" 和 "接收" 权限。

6. 将 Windows 进程激活服务 (WAS) 配置为支持 MSMQ 激活。

    为方便起见，在位于示例目录中名为 AddMsmqSiteBinding.cmd 的批处理文件中实现以下步骤。

    1. 若要支持 net.msmq 激活，必须首先将默认网站绑定到 net.msmq 协议。 可以通过使用随 IIS 7.0 管理工具集安装的 appcmd.exe 来执行此操作。 在具有提升权限的（管理员）命令提示符处，运行下列命令。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        -+bindings.[protocol='net.msmq',bindingInformation='localhost']
        ```

        > [!NOTE]
        > 此命令是单行文本。

        此命令可将 net.msmq 站点绑定添加到默认网站。

    2. 尽管网站内的所有应用程序共享一个公共 net.msmq 绑定，但是每个应用程序可以单独启用 net.msmq 支持。 若要启用 /servicemodelsamples 应用程序的 net.msmq，请在具有提升权限的命令提示符处运行以下命令。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http,net.msmq
        ```

        > [!NOTE]
        > 此命令是单行文本。

        此命令允许使用和访问/servicemodelsamples 应用程序 `http://localhost/servicemodelsamples` `net.msmq://localhost/servicemodelsamples` 。

7. 如果您以前没有进行此操作，应确保启用 MSMQ 激活服务。 从 " **开始** " 菜单中，单击 " **运行**"，然后键入 `Services.msc` 。 在服务列表中搜索 **Net.tcp 侦听器适配器**。 右键单击并选择 **“属性”**。 将 " **启动类型** " 设置为 " **自动**"，单击 " **应用** "，然后单击 " **开始** " 按钮。 此步骤只能在第一次使用 Net.Msmq Listener Adapter 服务之前操作一次。

8. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。 此外，在客户端上更改用于提交采购订单的代码，使其在提交采购订单时在队列的 URI 中反映计算机名。 使用以下代码：

    ```csharp
    client.SubmitPurchaseOrder(po, "net.msmq://localhost/private/ServiceModelSamples/OrderStatus");
    ```

9. 移除为此示例添加的 net.msmq 站点绑定。

    为方便起见，在位于示例目录中名为 RemoveMsmqSiteBinding.cmd 的批处理文件中实现以下步骤：

    1. 通过在具有提升权限的命令提示符处运行以下命令，从启用的协议列表中移除 net.msmq。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http
        ```

        > [!NOTE]
        > 此命令是单行文本。

    2. 通过在具有提升权限的命令提示符处运行以下命令移除 net.msmq 站点绑定。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" --bindings.[protocol='net.msmq',bindingInformation='localhost']
        ```

        > [!NOTE]
        > 此命令是单行文本。

    > [!WARNING]
    > 运行该批处理文件会将 DefaultAppPool 重置为使用 .NET Framework 2.0 版运行。

默认情况下对 `netMsmqBinding` 绑定传输启用了安全性。 `MsmqAuthenticationMode` 和 `MsmqProtectionLevel` 这两个属性共同确定了传输安全性的类型。 默认情况下，身份验证模式设置为 `Windows`，保护级别设置为 `Sign`。 MSMQ 必须是域的成员才可以提供身份验证和签名功能。 如果在不是域成员的计算机上运行此示例，则会接收以下错误：“用户的内部消息队列证书不存在”。

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>在加入到工作组的计算机上运行此示例

1. 如果计算机不是域成员，请将身份验证模式和保护级别设置为 None 以禁用传输安全性，如下面的示例配置所示。

    ```xml
    <bindings>
        <netMsmqBinding>
            <binding configurationName="TransactedBinding">
                <security mode="None"/>
            </binding>
        </netMsmqBinding>
    </bindings>
    ```

2. 在运行示例前更改服务和客户端上的配置。

    > [!NOTE]
    > 将 `security mode` 设置为 `None` 等效于将 `MsmqAuthenticationMode`、`MsmqProtectionLevel` 和 `Message` 安全设置为 `None`。

3. 若要在加入到工作组的计算机上启用激活，必须使用特定用户帐户运行激活服务和辅助进程（两者使用的账户必须相同），并且队列必须具有该特定用户帐户的 ACL。

     更改运行辅助进程所用的标识：

    1. 运行 Inetmgr.exe。

    2. 在 " **应用程序池**" 下，右键 **单击 ") ** **DefaultAppPool** "，然后选择 " **设置应用程序池默认值 ...**" (。

    3. 更改标识属性以使用特定用户帐户。

     更改运行激活服务所使用的标识：

    1. 运行 Services.msc。

    2. 右键单击 **Listener 适配器**，然后选择 " **属性**"。

4. 更改 " **登录** " 选项卡中的帐户。

5. 在工作组中，服务还必须使用不受限制的令牌来运行。 为此，请在命令窗口中运行下面的命令：

    ```console
    sc sidtype netmsmqactivator unrestricted
    ```

## <a name="see-also"></a>请参阅

- [AppFabric 承载和持久性示例](/previous-versions/appfabric/ff383418(v=azure.10))
