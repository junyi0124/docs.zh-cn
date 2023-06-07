---
title: Net.TCP 端口共享示例
ms.date: 03/30/2017
ms.assetid: 03da5959-0574-4e91-8a53-05854b6c55dc
ms.openlocfilehash: fa62734ed6a4a016011c9f29b3665dae05a000c6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235371"
---
# <a name="nettcp-port-sharing-sample"></a>Net.TCP 端口共享示例

TCP/IP 协议使用一个称为端口的 16 位数字来区分与在同一台计算机上运行的多个网络应用程序的连接。 如果某个应用程序正在侦听一个端口，则此端口的所有 TCP 通信将转至该应用程序。 其他应用程序无法同时侦听此端口。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\TCP\PortSharing`  
  
 许多协议具有所使用的标准或默认端口号。 例如，HTTP 协议通常使用 TCP 端口 80。 Internet 信息服务 (IIS) 具有一个侦听器，可在多个 HTTP 应用程序之间共享一个端口。 IIS 直接侦听此端口，并基于消息流内的信息将消息转发到适当的应用程序。 这就允许多个 HTTP 应用程序使用相同的端口号，而不必竞相保留此端口来接收消息。  
  
 Wcf-nettcp 端口共享是 Windows Communication Foundation (WCF) 功能，类似地允许多个网络应用程序共享一个端口。 NetTcp Port Sharing Service 接受使用 net.tcp 协议的连接，并基于消息的目标地址来转发消息。  
  
 默认情况下不会启用 NetTcp Port Sharing Service。 运行该示例之前，必须手动启用此服务。 有关详细信息，请参阅 [如何：启用 Net.tcp 端口共享服务](../feature-details/how-to-enable-the-net-tcp-port-sharing-service.md)。 如果禁用了此服务，则在启动服务器应用程序时将引发异常。  
  
```console
Unhandled Exception: System.ServiceModel.CommunicationException: The TransportManager failed to listen on the supplied URI using the NetTcpPortSharing service: failed to start the service because it is disabled. An administrator can enable it by running 'sc.exe config NetTcpPortSharing start= demand'.. ---> System.InvalidOperationException: Cannot start service NetTcpPortSharing on computer '.'. ---> System.ComponentModel.Win32Exception: The service cannot be started, either because it is disabled or because it has no enabled devices associated with it  
```  
  
 在服务器上启用端口共享的方法是设置 <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A> 绑定或 <xref:System.ServiceModel.NetTcpBinding> 绑定元素的 <xref:System.ServiceModel.Channels.TcpTransportBindingElement> 属性。 客户端不必知道是如何配置端口共享以便在服务器上使用的。  
  
## <a name="enabling-port-sharing"></a>启用端口共享  

 下面的代码演示如何在服务器上启用端口共享。 它在具有随机 URI 路径的固定端口上启动 `ICalculator` 服务的一个实例。 即使两个服务可以共享同一端口，但它们的整体终结点地址仍必须是唯一的，以便 NetTcp Port Sharing Service 可以将消息路由到正确的应用程序。  

```csharp
// Configure a binding with TCP port sharing enabled  
NetTcpBinding binding = new NetTcpBinding();  
binding.PortSharingEnabled = true;  
  
// Start a service on a fixed TCP port  
ServiceHost host = new ServiceHost(typeof(CalculatorService));  
ushort salt = (ushort)new Random().Next();  
string address = $"net.tcp://localhost:9000/calculator/{salt}";
host.AddServiceEndpoint(typeof(ICalculator), binding, address);  
host.Open();  
```

 启用端口共享后，可以多次运行此服务，而不会引起端口号冲突。 如果更改代码以禁用端口共享，则启动服务的两个副本将导致第二个副本失败，并出现 <xref:System.ServiceModel.AddressAlreadyInUseException>。  
  
```console  
Unhandled Exception: System.ServiceModel.AddressAlreadyInUseException: There is already a listener on IP endpoint 0.0.0.0:9000.  Make sure that you are not trying to use this endpoint multiple times in your application and that there are no other applications listening on this endpoint. ---> System.Net.Sockets.SocketException: Only one usage of each socket address (protocol/network address/port) is normally permitted  
```  
  
## <a name="running-the-sample"></a>运行示例  

 可以使用测试客户端来检查消息是否正确路由到共享端口的服务。  

```csharp
class client  
{  
   static void Main(string[] args)  
   {  
      Console.Write("Enter the service number to test: ");  
      ushort salt = ushort.Parse(Console.ReadLine());  
      string address = $"net.tcp://localhost:9000/calculator/{salt}";
      ChannelFactory<ICalculator> factory = new ChannelFactory<ICalculator>(new NetTcpBinding());  
      ICalculator proxy = factory.CreateChannel(new EndpointAddress(address));  
  
      // Call the Add service operation.  
      double value1 = 100.00D;  
      double value2 = 15.99D;  
      double result = proxy.Add(value1, value2);  
      Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Subtract service operation.  
      value1 = 145.00D;  
      value2 = 76.54D;  
      result = proxy.Subtract(value1, value2);  
      Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Multiply service operation.  
      value1 = 9.00D;  
      value2 = 81.25D;  
      result = proxy.Multiply(value1, value2);  
      Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Divide service operation.  
      value1 = 22.00D;  
      value2 = 7.00D;  
      result = proxy.Divide(value1, value2);  
      Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
  
      Console.WriteLine();  
      Console.WriteLine("Press <ENTER> to terminate client.");  
      Console.ReadLine();  
  
      factory.Close();  
   }  
}  
```

 服务的每个实例将输出其唯一的编号和地址。 例如，运行 service.exe 时可能会看到以下文本。  
  
```console  
Service #4381 listening on net.tcp://localhost:9000/calculator/4381.  
Press <ENTER> to terminate service.  
```  
  
 在此输入运行 client.exe 时看到的服务编号。  
  
```console  
Enter the service number to test: 4381  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
 可以通过更改客户端使用的所生成的地址，使用跨计算机配置来运行此示例。 在 Client.cs 中，更改终结点的地址格式字符串以与服务的新地址相匹配。 将对“localhost”的所有引用替换为服务器计算机的 IP 地址。 进行此更改之后，必须重新编译该示例。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 使用以下命令安装 ASP.NET 4.0。  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
3. 按照前面的简介部分中的说明启用 NetTcp Port Sharing Service。  
  
4. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
5. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。 前面的“运行示例”部分中包含有关运行该示例的特定详细信息。  
