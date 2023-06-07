---
title: 使用 Windows Store 应用商店客户端应用访问 WCF 服务
ms.date: 03/30/2017
ms.assetid: e2002ef4-5dee-4a54-9d87-03b33d35fc52
ms.openlocfilehash: ab57adbe0effa2b74541053aa0fcc5b572a6b7fd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293931"
---
# <a name="access-wcf-services-with-a-windows-store-client-app"></a>使用 Windows 应用商店客户端应用访问 WCF 服务

Windows 8 引入了一种新应用程序，称为 Windows 应用商店应用程序。 这些应用程序是围绕触摸屏界面设计的。 通过 .NET Framework 4.5，Windows 商店应用程序可以调用 WCF 服务。  
  
## <a name="wcf-support-in-windows-store-applications"></a>Windows 应用商店应用程序中的 WCF 支持  

 Windows 应用商店应用程序中提供了 WCF 功能的子集，请参见以下各节以了解更多详细信息。  
  
> [!IMPORTANT]
> 请使用 WinRT 联合 API 而不使用由 WCF 公开的 API。 有关更多信息，请参见 [WinRT 联合 API](xref:Windows.Web.Syndication)  
  
> [!WARNING]
> 不支持使用添加服务引用向 Windows 运行时组件添加 web 服务引用。  
  
### <a name="supported-bindings"></a>支持的绑定  

 Windows 应用商店应用程序支持以下 WCF 绑定：  
  
1. <xref:System.ServiceModel.BasicHttpBinding>  
  
2. <xref:System.ServiceModel.NetTcpBinding>  
  
3. <xref:System.ServiceModel.NetHttpBinding>  
  
4. <xref:System.ServiceModel.Channels.CustomBinding>
  
 Windows 应用商店应用程序支持以下绑定元素：  
  
1. <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>  
  
2. <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>  
  
3. <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement>  
  
4. <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement>  
  
5. <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement>  
  
6. <xref:System.ServiceModel.Channels.TcpTransportBindingElement>  
  
7. <xref:System.ServiceModel.Channels.HttpTransportBindingElement>  
  
8. <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>  
  
9. <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>  
  
 支持文本编码和二进制编码。 支持所有 WCF 传输模式。 有关更多信息，请参见 [Streaming Message Transfer](streaming-message-transfer.md)。  
  
### <a name="add-service-reference"></a>中的  

 若要从 Windows 应用商店应用程序调用 WCF 服务，请使用 Visual Studio 2012 的“添加服务引用”功能。 在 Windows 应用商店应用程序中完成操作后，您将注意到添加服务引用功能发生了一些变化。 首先，不会生成配置文件。 Windows 应用商店应用程序不使用配置文件，因此，必须用代码来对这些应用程序进行配置。 此配置代码可在由添加服务引用功能生成的 References.cs 文件中找到。 若要查看此文件，请确保在解决方案资源管理器中选择 "显示所有文件"。 该文件位于项目中“服务引用”的 Reference.svcmap 节点下面。 为 Windows 应用商店应用程序中的 WCF 服务生成的所有操作都将使用基于任务的异步模式实现异步。 有关详细信息，请参阅 [异步任务-通过任务简化异步编程](/archive/msdn-magazine/2010/september/async-tasks-simplify-asynchronous-programming-with-tasks)。  
  
 由于现在将使用代码生成配置，因此，每次更新服务引用时，都会覆盖 Reference.cs 文件中所做的任何更改。 为了纠正这种情况，将在一个部分方法中生成配置代码，您可以在客户端代理类中实现此部分方法。 该部分方法的声明如下：  
  
```csharp  
static partial void Configure(System.ServiceModel.Description.ServiceEndpoint serviceEndpoint,  
            System.ServiceModel.Description.ClientCredentials clientCredentials);  
```  
  
 然后，您可以实现此部分方法并更改您的客户端代理类中的绑定或终结点，如下所示：  
  
```csharp  
public partial class Service1Client : System.ServiceModel.ClientBase<MetroWcfClient.ServiceRefMultiEndpt.IService1>, MetroWcfClient.ServiceRefMultiEndpt.IService1  
    {
        static partial void Configure(System.ServiceModel.Description.ServiceEndpoint serviceEndpoint,
            System.ServiceModel.Description.ClientCredentials clientCredentials)  
        {  
            if (serviceEndpoint.Name ==
                    ServiceRefMultiEndpt.Service1Client.EndpointConfiguration.BasicHttpBinding_IService1.ToString())  
            {  
                serviceEndpoint.Binding.SendTimeout = new System.TimeSpan(0, 1, 0);  
            }  
            else if (serviceEndpoint.Name ==
                    ServiceRefMultiEndpt.Service1Client.EndpointConfiguration.BasicHttpBinding_IService11.ToString())  
            {  
                serviceEndpoint.Binding.SendTimeout = new System.TimeSpan(0, 1, 0);  
                clientCredentials.UserName.UserName = "username1";  
                clientCredentials.UserName.Password = "password";  
            }  
            else if (serviceEndpoint.Name ==
                    ServiceRefMultiEndpt.Service1Client.EndpointConfiguration.NetTcpBinding_IService1.ToString())  
            {  
                serviceEndpoint.Binding.Name = "MyTcpBinding";  
                serviceEndpoint.Address = new System.ServiceModel.EndpointAddress("net.tcp://localhost/tcp");  
            }  
        }  
    }  
```  
  
### <a name="serialization"></a>序列化  

 Windows 应用商店应用程序支持以下序列化程序：  
  
1. DataContractSerializer  
  
2. DataContractJsonSerializer  
  
3. XmlSerializer  
  
> [!WARNING]
> XmlDictionaryWriter.Write(DateTime) 现在会将 DateTime 对象作为字符串写入。  
  
### <a name="security"></a>安全性  

Windows 应用商店应用程序支持以下安全模式：
  
1. <xref:System.ServiceModel.SecurityMode.None>  
  
2. <xref:System.ServiceModel.SecurityMode.Transport>  
  
3. <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential>
  
4. <xref:System.ServiceModel.SecurityMode.Message>
  
Windows 应用商店应用程序支持以下客户端凭据类型：
  
1. 无  
  
2. 基本  
  
3. 摘要  
  
4. Negotiate  
  
5. NTLM  
  
6. Windows  
  
7. 用户名（消息安全）  
  
8. Windows（传输安全）  
  
 为了使 Windows 应用商店应用程序访问并发送默认的 Windows 凭据，您必须在 Package.appmanifest 文件中启用此功能。 打开此文件并选择 "功能" 选项卡，然后选择 "默认 Windows 凭据"。 这样，应用程序就可以连接到需要域凭据的 Intranet 资源。  
  
> [!IMPORTANT]
> 为了使 Windows 应用商店应用程序进行跨计算机调用，你必须启用另一个名为 "家庭/工作网络" 的功能。 此设置也位于 "功能" 选项卡下的 appmanifest 文件中。选择 "家庭/工作网络" 复选框。 这样，应用程序就可以入站和出站访问用户的受信任位置（例如家庭和工作）的网络。 将始终阻止关键入站端口。 为了访问 Internet 上的服务，您还必须启用“Internet (客户端}”功能。  
  
### <a name="misc"></a>杂项  

 Windows 应用商店应用程序支持使用下面的类：  
  
1. <xref:System.ServiceModel.ChannelFactory>  
  
2. <xref:System.ServiceModel.DuplexChannelFactory%601>
  
3. <xref:System.ServiceModel.CallbackBehaviorAttribute>  
  
### <a name="defining-service-contracts"></a>定义服务协定  

 建议使用基于任务的异步模式仅定义异步服务操作。 这可以确保 Windows 应用商店应用程序在调用服务操作时保持响应。  
  
> [!WARNING]
> 虽然在定义了同步操作的情况下不会不引发任何异常，但强烈建议仅定义异步操作。  
  
### <a name="calling-wcf-services-from-windows-store-applications"></a>从 Windows 应用商店应用程序调用 WCF 服务  

 正如前面所述，必须在生成的代理类中的 GetBindingForEndpoint 方法中用代码完成所有配置。 调用服务操作的方式与调用任何基于任务的异步方法的方式相同，如下面的代码段所示：  
  
```csharp  
void async SomeMethod()  
{  
    ServiceClient proxy = new ServiceClient();  
    Task<T> results = await proxy.CallAsync(param1, param2);  
    T result = results.Result;  
    if (result.Success)  
    {  
       // Do something with result  
    }  
}  
```  
  
 请注意进行异步调用的方法上 async 关键字的使用以及调用该异步方法时 await 关键字的使用。  
  
## <a name="see-also"></a>另请参阅

- [WCF 安全编程](programming-wcf-security.md)
- [绑定](../bindings.md)
