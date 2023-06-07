---
title: 从 .NET 远程处理迁移到 WCF
description: 了解如何迁移使用 .NET 远程处理的应用程序以使用 WCF。 可以在 WCF 中完成若干常见的远程处理方案。
ms.date: 03/30/2017
ms.assetid: 16902a42-ef80-40e9-8c4c-90e61ddfdfe5
ms.openlocfilehash: 385c6075dab064d5812355bebd1620081cf2bbff
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248872"
---
# <a name="migrating-from-net-remoting-to-wcf"></a>从 .NET 远程处理迁移到 WCF

本文介绍如何迁移借助 .NET 远程处理来使用 Windows Communication Foundation (WCF) 的应用程序。 本文对这些产品之间的相似概念进行比较，并介绍如何在 WCF 中完成若干常见的远程处理方案。  
  
 .NET 远程处理是一项传统技术，仅支持向后兼容性。 由于它无法保持客户端和服务器之间的单独信任级别，因此它在混合信任环境中并不安全。 例如，你绝不应将 .NET 远程处理终结点公开到 Internet 或不受信任的客户端中。 我们建议将现有的远程处理应用程序迁移到更新和更安全的技术中。 如果应用程序的设计仅使用 HTTP 并且为 RESTful，那么我们建议迁移到 ASP.NET Web API。 有关详细信息，请参阅 ASP.NET Web API。 如果应用程序基于 SOAP，或者需要非 Http 协议（如 TCP），那么我们建议迁移到 WCF。  

## <a name="comparing-net-remoting-to-wcf"></a>比较 .NET 远程处理与 WCF  

 本节将对 .NET 远程处理的基本构建基块与其 WCF 等效物进行比较。 稍后我们将使用这些构建基块在 WCF 中创建一些常见的客户端服务器方案。 下表总结了 .NET 远程处理与 WCF 之间的主要相似性和差异。  
  
||.NET 远程处理|WCF|  
|-|-------------------|---------|  
|服务器类型|子类 MarshalByRefObject|标记为具有 [ServiceContract] 属性|  
|服务操作|服务器类型上的公共方法|标记为具有 [OperationContract] 属性|  
|序列化|ISerializable 或 [Serializable]|DataContractSerializer 或 XmlSerializer|  
|传递的对象|按值或按引用|仅按值|  
|错误/异常|任何可序列化的异常|FaultContract\<TDetail>|  
|客户端代理对象|从 MarshalByRefObjects 自动创建强类型透明代理|使用 ChannelFactory 按需生成强类型代理\<TChannel>|  
|所需平台|客户端和服务器必须使用 Microsoft 操作系统和 .NET|跨平台|  
|消息格式|专用|行业标准（SOAP、WS-* 等。）|  
  
### <a name="server-implementation-comparison"></a>服务器实现比较  
  
#### <a name="creating-a-server-in-net-remoting"></a>在 .NET 远程处理中创建服务器  

 .NET 远程处理服务器类型必须从 MarshalByRefObject 中派生，并定义客户端可以调用的方法，如下所示：  
  
```csharp
public class RemotingServer : MarshalByRefObject  
{  
    public Customer GetCustomer(int customerId) { … }  
}  
```  
  
 此服务器类型的公共方法是客户端可使用的公开协定。  一种类型会处理服务器公共接口和其实现，而无需分离。  
  
 一旦定义了服务器类型，其便可用于客户端，如下面示例所示：  
  
```csharp
TcpChannel channel = new TcpChannel(8080);  
ChannelServices.RegisterChannel(channel, ensureSecurity : true);  
RemotingConfiguration.RegisterWellKnownServiceType(  
    typeof(RemotingServer),
    "RemotingServer",
    WellKnownObjectMode.Singleton);  
Console.WriteLine("RemotingServer is running.  Press ENTER to terminate...");  
Console.ReadLine();  
```  
  
 使远程处理类型作为服务器可用有多种方法，其中一种方法为使用配置文件。 这只是一个例子。  
  
#### <a name="creating-a-server-in-wcf"></a>在 WCF 中创建服务器  

 WCF 中的等效步骤涉及创建两种类型 -- 公共“服务协定”类和具体实现。 第一种类型为接口类，标记为 [ServiceContract]。 客户端可用的方法都标有 [OperationContract]：  
  
```csharp
[ServiceContract]  
public interface IWCFServer  
{  
    [OperationContract]  
    Customer GetCustomer(int customerId);  
}  
```  
  
 服务器的实现则定义在单独的具体的类中，如下面示例所示：  
  
```csharp
public class WCFServer : IWCFServer  
{  
    public Customer GetCustomer(int customerId) { … }  
}  
```  
  
 完成定义这些类型后，WCF 服务器便可提供给客户端，如下面示例所示：  
  
```csharp
NetTcpBinding binding = new NetTcpBinding();  
Uri baseAddress = new Uri("net.tcp://localhost:8000/wcfserver");  
  
using (ServiceHost serviceHost = new ServiceHost(typeof(WCFServer), baseAddress))  
{  
    serviceHost.AddServiceEndpoint(typeof(IWCFServer), binding, baseAddress);  
    serviceHost.Open();  
  
    Console.WriteLine($"The WCF server is ready at {baseAddress}.");
    Console.WriteLine("Press <ENTER> to terminate service...");  
    Console.WriteLine();  
    Console.ReadLine();  
}  
```  
  
> [!NOTE]
> 这两个示例中的 TCP 用于使它们尽可能保持类似。 请参阅本主题稍后的使用 HTTP 的示例方案演练。  
  
 有多种方法来配置和承载 WCF 服务。 这仅仅是举一个例子，例如称为“自承载”的方法。 有关详细信息，请参阅下列主题：  
  
- [如何：定义服务协定](how-to-define-a-wcf-service-contract.md)  
  
- [使用配置文件配置服务](configuring-services-using-configuration-files.md)  
  
- [承载服务](hosting-services.md)  
  
### <a name="client-implementation-comparison"></a>客户端实现比较  
  
#### <a name="creating-a-client-in-net-remoting"></a>在 .NET 远程处理中创建客户端  

 一旦 .NET 远程处理服务器对象已经可用，其便可用于客户端，如下面示例所示：  
  
```csharp
TcpChannel channel = new TcpChannel();  
ChannelServices.RegisterChannel(channel, ensureSecurity : true);  
RemotingServer server = (RemotingServer)Activator.GetObject(  
                            typeof(RemotingServer),
                            "tcp://localhost:8080/RemotingServer");  
  
RemotingCustomer customer = server.GetCustomer(42);  
Console.WriteLine($"Customer {customer.FirstName} {customer.LastName} received.");
```  
  
 从 Activator.GetObject() 返回的 RemotingServer 实例称为“透明代理”。 它会实现客户端中的 RemotingServer 类型的公共 API，但所有方法都会调用运行在不同进程或计算机中的服务器对象。  
  
#### <a name="creating-a-client-in-wcf"></a>在 WCF 中创建客户端  

 WCF 中的等效步骤涉及使用通道工厂显式创建代理。 与远程处理相同，代理对象可用于调用服务器中的操作，如下面示例所示：  
  
```csharp
NetTcpBinding binding = new NetTcpBinding();  
String url = "net.tcp://localhost:8000/wcfserver";  
EndpointAddress address = new EndpointAddress(url);  
ChannelFactory<IWCFServer> channelFactory =
    new ChannelFactory<IWCFServer>(binding, address);  
IWCFServer server = channelFactory.CreateChannel();  
  
Customer customer = server.GetCustomer(42);  
Console.WriteLine($"  Customer {customer.FirstName} {customer.LastName} received.");
```  
  
 此示例演示通道级编程，因为它非常类似于远程处理示例。 此外，还提供了 Visual Studio 中的 **添加服务引用** 方法，该方法生成代码以简化客户端编程。 有关详细信息，请参阅下列主题：  
  
- [客户端通道级编程](./extending/client-channel-level-programming.md)  
  
- [如何：添加、更新或移除服务引用](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference)  
  
### <a name="serialization-usage"></a>序列化用法  

 .NET 远程处理和 WCF 均使用序列化发送客户端和服务器之间的对象，但它们在这些重要的方面有所区别：  
  
1. 它们使用不同的序列化程序和约定来指示如何序列化。  
  
2. .NET 远程处理支持“按引用”序列化，即允许一个层上的方法或属性访问执行另一个层上的代码；此为跨安全边界。 这种功能会公开安全漏洞，并且也是为什么远程处理终结点应永远不向不受信任的客户端公开的主要原因之一。  
  
3. 远程处理使用的序列化会选择退出（显式排除不进行序列化的成员）和 WCF 序列化会选择性加入（显式标记要序列化的成员）。  
  
#### <a name="serialization-in-net-remoting"></a>.NET 远程处理中的序列化  

 .NET 远程处理支持两种序列化和反序列化客户端和服务器之间的对象的方法：  
  
- *按值* –对象的值将跨层边界序列化，并在另一层上创建该对象的新实例。 对该新实例的方法或属性的任何调用只是在本地执行，并不影响原始对象或层。  
  
- *按引用* –一种特殊的 "对象引用" 跨层边界序列化。 当一个层与该对象的方法或属性进行交互时，此层会传输回至原始层上的原始对象。 按引用对象可以在任一方向 – 服务器到客户端或客户端到服务器中流动。  
  
 远程处理中按值类型会标记为 [Serializable] 属性或实现 ISerializable，如下面示例所示：  
  
```csharp
[Serializable]  
public class RemotingCustomer  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
    public int CustomerId { get; set; }  
}  
```  
  
 按引用类型派生于 MarshalByRefObject 类，如下面示例所示：  
  
```csharp
public class RemotingCustomerReference : MarshalByRefObject  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
    public int CustomerId { get; set; }  
}  
```  
  
 了解远程处理中按引用对象的影响尤其重要。 如果任一层（客户端或服务器）将按引用对象发送至其他层，那么所有方法调用会在拥有此对象的层中进行执行。 例如，调用服务器返回的按引用对象中方法的客户端将在服务器中执行代码。 同样，调用客户端提供的按引用对象中方法的服务器将在客户端中执行代码。 因此，建议只在完全信任环境中使用 .NET 远程处理。 向不受信任的客户端公开公共 .NET 远程处理终结点将使远程处理服务器容易受到攻击。  
  
#### <a name="serialization-in-wcf"></a>WCF 中的序列化  

 WCF 仅支持按值序列化。 定义客户端和服务器之间交换的类型的最常见方法，如下面示例所示：  
  
```csharp
[DataContract]  
public class WCFCustomer  
{  
    [DataMember]  
    public string FirstName { get; set; }  
  
    [DataMember]  
    public string LastName { get; set; }  
  
    [DataMember]  
    public int CustomerId { get; set; }  
}  
```  
  
 [DataContract] 特性标识此类型为可在客户端和服务器之间进行序列化和反序列化。 [DataMember] 特性标识要序列化的单个属性或字段。  
  
 当 WCF 跨多个层发送某个对象时，它仅序列化值并在另一个层上创建此对象的新实例。 与此对象的值进行的任何交互仅发生在本地 – 它们不能如 .NET 远程处理中的按引用对象一样与其他层进行通信。 有关详细信息，请参阅 [序列化和反序列](./feature-details/serialization-and-deserialization.md)化。  
  
### <a name="exception-handling-capabilities"></a>异常处理功能  
  
#### <a name="exceptions-in-net-remoting"></a>.NET 远程处理中的异常  

 由远程处理服务器引发的异常会进行序列化、发送到客户端并如同其他异常一样引发在本地的客户端中。 可通过子类分类异常类型，并将其标记为 [Serializable] 来创建自定义异常。   多数 Framework 异常已以这种方法进行了标记，从而使多数异常由服务器引发、进行序列化并在客户端重新引发。 尽管这种设计在开发过程中的确方便，但服务器端信息却可以意外地泄露至客户端中。 这是应仅在完全信任环境中使用远程处理的许多原因之一。  
  
#### <a name="exceptions-and-faults-in-wcf"></a>WCF 中的异常和错误  

 WCF 不允许任意异常类型从服务器返回到客户端，因为可能导致意外的信息泄露。 如果服务操作引发了意外的异常，则会导致在客户端引发一般用途的 FaultException。 此异常不会带有关于问题产生的原因或地点的任何信息，并且对于一些应用程序来讲，这些信息也就足够了。 通过定义错误协定，需要将更丰富的错误信息传递给客户端的应用程序会进行此操作。  
  
 若要执行此操作，请首先创建 [DataContract] 类型，以执行故障信息。  
  
```csharp
[DataContract]  
public class CustomerServiceFault  
{  
    [DataMember]  
    public string ErrorMessage { get; set; }  
  
    [DataMember]  
    public int CustomerId {get;set;}  
}  
```  
  
 指定要用于每个服务操作的错误协定。  
  
```csharp
[ServiceContract]  
public interface IWCFServer  
{  
    [OperationContract]  
    [FaultContract(typeof(CustomerServiceFault))]  
    Customer GetCustomer(int customerId);  
}  
```  
  
 服务器会通过引发 FaultException 从而报告错误条件。  
  
```csharp
throw new FaultException<CustomerServiceFault>(  
    new CustomerServiceFault() {
        CustomerId = customerId,
        ErrorMessage = "Illegal customer Id"
    });  
```  
  
 只要客户端向服务器发出请求，都可以作为正常异常进行捕获。  
  
```csharp
try  
{  
    Customer customer = server.GetCustomer(-1);  
}  
catch (FaultException<CustomerServiceFault> fault)  
{  
    Console.WriteLine($"Fault received: {fault.Detail.ErrorMessage}");
}  
```  
  
 有关故障协定的详细信息，请参阅 <xref:System.ServiceModel.FaultException>。  
  
### <a name="security-considerations"></a>安全注意事项  
  
#### <a name="security-in-net-remoting"></a>.NET 远程处理的安全性  

 某些 .NET 远程处理信道支持安全功能，比如通道层（IPC 和 TCP）中的身份验证和加密。 HTTP 通道依赖于身份验证和加密的 Internet Information Services (IIS)。 即使有这种支持，你仍应该将 .NET 远程处理认定为不安全的通信协议并只在完全信任的环境中使用。 永远不要将公共远程处理终结点公开到 Internet 或不受信任的客户端中。  
  
#### <a name="security-in-wcf"></a>WCF 中的安全性  

 WCF 中对于安全性的设计是为了解决出现在 .NET 远程处理中各种的漏洞。 WCF 提供了传输和消息级别的安全性，并提供了多个用于身份验证、授权、加密等的选项。 有关详细信息，请参阅下列主题：  
  
- [安全性](./feature-details/security.md)  
  
- [WCF 安全指南](./feature-details/security-guidance-and-best-practices.md)  
  
## <a name="migrating-to-wcf"></a>迁移到 WCF  
  
### <a name="why-migrate-from-remoting-to-wcf"></a>为什么从远程处理迁移到 WCF？  
  
- **.NET 远程处理是一项传统技术。** 如 [.Net 远程处理](/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100))中所述，它被视为旧产品，不建议用于新的开发。 对于新的和现有的应用程序建议使用 WCF 或 ASP.NET Web API。  
  
- **WCF 使用跨平台标准。** WCF 设计的跨平台互操作性支持许多行业标准（SOAP、Ws-security、Ws-trust 等。）。 除了运行 Windows 操作系统的客户端之外，WCF 服务还可以与运行其他操作系统的客户端进行互操作。 远程处理主要针对使用 Windows 操作系统上的 .NET Framework 运行服务器和客户端应用程序的环境。
  
- **WCF 具有内置安全性。** WCF 的设计考虑到了安全性，并提供了许多用于身份验证、传输级别安全性、消息级别安全性等的选项。远程处理旨在使应用程序更易于互操作，但并不是在不受信任的环境中保证安全。 WCF 设计为可以在受信任和不受信任的两种环境中工作。  
  
### <a name="migration-recommendations"></a>迁移建议  

 若要从 .NET 远程处理迁移到 WCF，建议步骤如下所示：  
  
- **创建服务协定。** 定义服务接口类型，并将其标记为 [ServiceContract] 属性。标记所有允许客户端利用 [OperationContract] 调用的方法。  
  
- **创建数据协定。** 定义将在服务器和客户端之间进行交换的数据类型，并将其标记为 [DataContract] 属性。 标记允许客户端利用 [DataMember] 使用的所有字段和属性。  
  
- **创建错误协定（可选）。** 遇到错误时，请创建将在服务器和客户端之间进行交换的类型。 将这些类型标记为 [DataContract] 和 [DataMember] 以使其可序列化。 对于标记为 [OperationContract] 的所有服务操作，还可将其标记为 [FaultContract]，以指示它们可能会返回哪些错误。  
  
- **配置并托管该服务。** 完成创建服务协定后，下一步则是配置一个绑定以公开终结点中的服务。 有关详细信息，请参阅 [终结点：地址、绑定和协定](./feature-details/endpoints-addresses-bindings-and-contracts.md)。  
  
 将远程处理应用程序迁移到 WCF 后，删除 .NET 远程处理中的依赖仍然很重要。 这可确保删除应用程序中的任何远程处理漏洞。 这些步骤包括：  
  
- **停止使用 MarshalByRefObject。** MarshalByRefObject 类型仅因远程处理而存在并且 WCF 并不使用此类型。 应删除或更改子类 MarshalByRefObject 的任何应用程序类型。  
  
- **停止使用 [Serializable] 和 ISerializable。** [Serializable] 属性和 ISerializable 接口最初用于在受信任的环境中序列化类型，并为远程处理所用。 WCF 序列化依赖于标记为 [DataContract] 和 [DataMember] 的类型。 若要使用 [DataContract]，而不是使用 ISerializable 或 [Serializable]，则应修改应用程序使用的数据类型。  
  
### <a name="migration-scenarios"></a>迁移方案  

 现在让我们了解如何完成以下 WCF 中的常见远程处理方案：  
  
1. 服务器将对象按值返回到客户端  
  
2. 服务器将对象按引用返回到客户端  
  
3. 客户端按值向服务器发送对象  
  
> [!NOTE]
> WCF 中不允许从客户端将对象按引用发送到服务器。  
  
 阅读这些方案后，假定 .NET 远程处理的基线接口将如下面示例所示。 由于在这里仅想说明如何使用 WCF 实现等效功能，因此 .NET 远程处理实现在此并不重要。  
  
```csharp
public class RemotingServer : MarshalByRefObject  
{  
    // Demonstrates server returning object by-value  
    public Customer GetCustomer(int customerId) {…}  
  
    // Demonstrates server returning object by-reference  
    public CustomerReference GetCustomerReference(int customerId) {…}  
  
    // Demonstrates client passing object to server by-value  
    public bool UpdateCustomer(Customer customer) {…}  
}  
```  
  
#### <a name="scenario-1-service-returns-an-object-by-value"></a>方案 1：服务按值返回对象  

 此方案演示服务器按值将对象返回至客户端。 WCF 始终按值从服务器中返回对象，因此以下步骤仅描述了如何创建普通 WCF 服务。  
  
1. 首先定义 WCF 服务的公共接口并标记 [ServiceContract] 属性。 使用 [OperationContract] 标识客户端将调用的服务器端方法。  
  
   ```csharp
   [ServiceContract]  
   public interface ICustomerService  
   {  
       [OperationContract]  
       Customer GetCustomer(int customerId);  
  
       [OperationContract]  
       bool UpdateCustomer(Customer customer);  
   }  
   ```  
  
2. 下一步则是创建此服务的数据约定。 通过创建具有 [DataContract] 属性标记的类（非接口）来执行此操作。 希望对客户端和服务器可见的单独属性或字段标记为 [DataMember]。 如果希望使用允许派生的类型，则必须使用 [KnownType] 属性来进行标记。 WCF 中允许为此服务进行序列化或反序列化的唯一类型是服务接口类型和“已知类型”。 拒绝尝试交换不在此列表中的任何其他类型。  
  
   ```csharp
   [DataContract]  
   [KnownType(typeof(PremiumCustomer))]  
   public class Customer  
   {  
       [DataMember]  
       public string FirstName { get; set; }  
  
       [DataMember]  
       public string LastName { get; set; }  
  
       [DataMember]  
       public int CustomerId { get; set; }  
   }  
  
   [DataContract]  
   public class PremiumCustomer : Customer
   {  
       [DataMember]  
       public int AccountId { get; set; }  
   }  
   ```  
  
3. 接下来，将提供服务接口的实现。  
  
   ```csharp  
   public class CustomerService : ICustomerService  
   {  
       public Customer GetCustomer(int customerId)  
       {  
           // read from database  
       }  
  
       public bool UpdateCustomer(Customer customer)  
       {  
           // write to database  
       }  
   }  
   ```  
  
4. 若要运行 WCF 服务，则需要声明终结点将公开使用特定 WCF 绑定的特定 URL 中的该服务接口。 这通常通过向服务器项目的 web.config 文件中添加以下各节而实现的。  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name="Server.CustomerService">  
            <endpoint address="http://localhost:8083/CustomerService"  
                      binding="basicHttpBinding"  
                      contract="Shared.ICustomerService" />  
          </service>  
        </services>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
5. 然后，可用下面的代码中启动 WCF 服务：  
  
   ```csharp
   ServiceHost customerServiceHost = new ServiceHost(typeof(CustomerService));  
       customerServiceHost.Open();  
   ```  
  
     当启动此 ServiceHost 时，它会使用 web.config 文件以建立适当的协定、绑定和终结点。 有关配置文件的详细信息，请参阅 [使用配置文件配置服务](./configuring-services-using-configuration-files.md)。 这种启动服务器的样式称为自我托管。 若要了解有关承载 WCF 服务的其他选项的详细信息，请参阅 [托管服务](./hosting-services.md)。  
  
6. 客户端项目的 app.config 必须声明匹配服务终结点的绑定信息。 在 Visual Studio 中执行此操作的最简单方法是使用 **添加服务引用**，这将自动更新 app.config 文件。 或者，可以手动添加这些相同的更改。  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <client>  
          <endpoint name="customerservice"  
                    address="http://localhost:8083/CustomerService"  
                    binding="basicHttpBinding"  
                    contract="Shared.ICustomerService"/>  
        </client>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
     有关使用 **添加服务引用** 的详细信息，请参阅 [如何：添加、更新或删除服务引用](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference)。  
  
7. 现在可以从客户端中调用 WCF 服务。 可通过创建该服务的通道工厂、要求提供通道以及直接调用想要用在该通道中的方法实现此操作。 可进行此操作的原因是通道可实现服务接口，并为我们处理基础的请求/答复逻辑。 此方法调用的返回值是服务器响应的反序列化副本。  
  
   ```csharp
   ChannelFactory<ICustomerService> factory =  
       new ChannelFactory<ICustomerService>("customerservice");  
   ICustomerService service = factory.CreateChannel();  
   Customer customer = service.GetCustomer(42);  
   Console.WriteLine($"  Customer {customer.FirstName} {customer.LastName} received.");
   ```  
  
 由 WCF 从服务器返回到客户端的对象始终为按值返回。 对象是由服务器发送的数据反序列化副本。 客户端可以对这些本地副本调用方法，而无需担心通过回调调用服务器。  
  
#### <a name="scenario-2-server-returns-an-object-by-reference"></a>方案 2：服务器按按引用返回对象  

 此方案演示服务器按引用向客户端提供对象。 在 .NET 远程处理中，这会自动处理任何派生自 MarshalByRefObject 的类型，即序列化的引用。 此方案的一个示例是允许多个客户端拥有独立会话的服务端对象。 正如前面所述，WCF 服务返回的对象始终按值返回，因此并没有直接对等的按引用对象，但是有可能实现类似于使用 <xref:System.ServiceModel.EndpointAddress10> 对象的按引用语义。 这是客户端用于获取服务器上会话按引用对象的可序列化的值对象。 这将使多个客户端拥有独立会话的服务器端对象的方案得以实现。  
  
1. 首先，需要定义与会话对象本身对应的 WCF 服务协定。  
  
   ```csharp
   [ServiceContract(SessionMode = SessionMode.Allowed)]  
       public interface ISessionBoundObject  
       {  
           [OperationContract]  
           string GetCurrentValue();  
  
           [OperationContract]  
           void SetCurrentValue(string value);  
       }  
   ```  
  
    > [!TIP]
    > 请注意该会话对象用 [ServiceContract] 进行了标记，使其成为普通的 WCF 服务接口。 设置 SessionMode 属性指示其将为会话服务。 在 WCF 中，会话是使两个终结点之间发送的多个消息关联的一种方法。 这意味着一旦客户端获取与此服务的连接，将会建立客户端和服务器之间的会话。 对于此单个会话中的所有交互，客户端都将使用服务器端对象的单个唯一实例。  
  
2. 接下来，需要提供此服务接口的实现。 通过使用 [ServiceBehavior] 进行表示，并设置 InstanceContextMode，将告知 WCF 希望使用每个会话的此类型的唯一实例。  
  
   ```csharp
   [ServiceBehavior(InstanceContextMode = InstanceContextMode.PerSession)]  
       public class MySessionBoundObject : ISessionBoundObject  
       {  
           private string _value;  
  
           public string GetCurrentValue()  
           {  
               return _value;  
           }  
  
           public void SetCurrentValue(string val)  
           {  
               _value = val;  
           }  
  
       }  
   ```  
  
3. 现在，需要一种方法以获取此会话对象的实例。 通过创建返回 EndpointAddress10 对象的另一个 WCF 服务接口来执行此操作。 这是一种客户端可用来创建会话对象的终结点的可序列化形式。  
  
   ```csharp
   [ServiceContract]  
       public interface ISessionBoundFactory  
       {  
           [OperationContract]  
           EndpointAddress10 GetInstanceAddress();  
       }  
   ```  
  
     并且，我们实现此 WCF 服务：  
  
   ```csharp
   public class SessionBoundFactory : ISessionBoundFactory  
       {  
           public static ChannelFactory<ISessionBoundObject> _factory =
               new ChannelFactory<ISessionBoundObject>("sessionbound");  

           public SessionBoundFactory()  
           {  
           }  
  
           public EndpointAddress10 GetInstanceAddress()  
           {  
               IClientChannel channel = (IClientChannel)_factory.CreateChannel();  
               return EndpointAddress10.FromEndpointAddress(channel.RemoteAddress);  
           }  
       }  
   ```  
  
     此实现维护单一实例通道工厂以创建会话对象。 当调用 GetInstanceAddress() 时，它会创建一个通道，并创建一个有效地指向与此通道关联的远程地址的 EndpointAddress10 对象。 EndpointAddress10 只是一种可按值返回至客户端的数据类型。  
  
4. 需要通过执行以下两个事件，修改服务器的配置文件，如下面的示例所示：  
  
    1. 声明 \<client> 描述会话对象的终结点的部分。 此声明是必需的，原因是在此情况下此服务器还可作为客户端。  
  
    2. 声明工厂和会话对象的终结点。 此声明是必需，原因是这可允许客户端与服务终结点通信以获取 EndpointAddress10 并创建会话通道。  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
         <client>  
          <endpoint name="sessionbound"  
                    address="net.tcp://localhost:8081/SessionBoundObject"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundObject"/>  
        </client>  
        <services>  
          <service name="Server.CustomerService">  
            <endpoint address="http://localhost:8083/CustomerService"  
                      binding="basicHttpBinding"  
                      contract="Shared.ICustomerService" />  
          </service>  
          <service name="Server.MySessionBoundObject">  
            <endpoint address="net.tcp://localhost:8081/SessionBoundObject"  
                      binding="netTcpBinding"  
                      contract="Shared.ISessionBoundObject" />  
          </service>  
          <service name="Server.SessionBoundFactory">  
            <endpoint address="net.tcp://localhost:8081/SessionBoundFactory"  
                      binding="netTcpBinding"  
                      contract="Shared.ISessionBoundFactory" />  
          </service>  
        </services>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
     然后便可启动这些服务：  
  
   ```csharp
   ServiceHost factoryHost = new ServiceHost(typeof(SessionBoundFactory));  
   factoryHost.Open();  
  
   ServiceHost sessionHost = new ServiceHost(typeof(MySessionBoundObject));  
   sessionHost.Open();  
   ```  
  
5. 可通过声明其项目 app.config 文件中这些相同的终结点配置客户端。  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <client>  
          <endpoint name="customerservice"  
                    address="http://localhost:8083/CustomerService"  
                    binding="basicHttpBinding"  
                    contract="Shared.ICustomerService"/>  
          <endpoint name="sessionbound"  
                    address="net.tcp://localhost:8081/SessionBoundObject"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundObject"/>  
          <endpoint name="factory"  
                    address="net.tcp://localhost:8081/SessionBoundFactory"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundFactory"/>  
        </client>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
6. 为了创建和使用此会话对象，客户端必须执行以下步骤：  
  
    1. 创建 ISessionBoundFactory 服务的通道。  
  
    2. 使用该通道调用该服务，以获取 EndpointAddress10。  
  
    3. 使用 EndpointAddress10 创建一个通道，以获取会话对象。  
  
    4. 与会话对象进行交互，以演示多次调用间保持的同一个实例。  
  
   ```csharp
   ChannelFactory<ISessionBoundFactory> channelFactory =
       new ChannelFactory<ISessionBoundFactory>("factory");  
   ISessionBoundFactory sessionFactory = channelFactory.CreateChannel();  
  
   EndpointAddress10 address1 = sessionFactory.GetInstanceAddress();  
   EndpointAddress10 address2 = sessionFactory.GetInstanceAddress();  
  
   ChannelFactory<ISessionBoundObject> sessionObjectFactory1 =
       new ChannelFactory<ISessionBoundObject>(new NetTcpBinding(),
                                               address1.ToEndpointAddress());  
   ChannelFactory<ISessionBoundObject> sessionObjectFactory2 =
       new ChannelFactory<ISessionBoundObject>(new NetTcpBinding(),
                                               address2.ToEndpointAddress());  
  
   ISessionBoundObject sessionInstance1 = sessionObjectFactory1.CreateChannel();  
   ISessionBoundObject sessionInstance2 = sessionObjectFactory2.CreateChannel();  
  
   sessionInstance1.SetCurrentValue("Hello");  
   sessionInstance2.SetCurrentValue("World");  
  
   if (sessionInstance1.GetCurrentValue() == "Hello" &&  
       sessionInstance2.GetCurrentValue() == "World")  
   {  
       Console.WriteLine("sessionful server object works as expected");  
   }  
   ```  
  
 WCF 将始终返回按值的对象，但是通过使用 EndpointAddress10 有可能支持按引用语义的等效项。 这将允许客户端请求会话的 WCF 服务实例，此后客户端可以与此实例进行交互，与其他 WCF 服务并无差异。  
  
#### <a name="scenario-3-client-sends-server-a-by-value-instance"></a>方案 3：客户端向服务器发送按值实例  

 此方案演示客户端按值向服务器发送一个非基元对象实例。 由于 WCF 仅发送按值对象，因此此方案演示正常使用 WCF 的情况。  
  
1. 使用与方案 1 中相同的 WCF 服务。  
  
2. 使用客户端创建一个新的按值对象（客户）、创建与 ICustomerService 服务进行通信的通道，并向其发送该对象。  
  
   ```csharp
   ChannelFactory<ICustomerService> factory =  
       new ChannelFactory<ICustomerService>("customerservice");  
   ICustomerService service = factory.CreateChannel();  
   PremiumCustomer customer = new PremiumCustomer {
   FirstName = "Bob",
   LastName = "Jones",
   CustomerId = 43,
   AccountId = 99};  
   bool success = service.UpdateCustomer(customer);  
   Console.WriteLine($"  Server returned {success}.");
   ```  
  
     将序列化客户对象，并将其发送到服务器，在其中将其反序列化为该对象的新副本。  
  
    > [!NOTE]
    > 此代码还说明了发送派生的类型 (PremiumCustomer)。 服务接口需要 Customer 对象，但 Customer 类上的 [KnownType] 属性指示也允许使用 PremiumCustomer。 WCF 通过此服务接口进行序列化或反序列化任何其他类型的尝试将失败。  
  
 正常的 WCF 数据交换是按值进行的。 这可确保其中某个数据对象上的调用方法仅在本地执行 – 它将不会调用其他层上的代码。 尽管可以实现 *从* 服务器返回的类似于引用对象的内容，但客户端无法将按引用对象传递 *到* 服务器。 在 WCF 中使用双工服务可实现需要在客户端和服务器间来回会话的方案。 有关详细信息，请参阅 [双工服务](./feature-details/duplex-services.md)。  
  
## <a name="summary"></a>总结  

 .NET 远程处理是一种通信框架，仅用于完全信任的环境中。 它是一项传统技术，仅支持向后兼容性。 它不应用于生成新的应用程序。 相反，WCF 融入了安全性，并建议将其用于生成新的和现有的应用程序。 Microsoft 建议将现有的远程处理应用程序迁移到使用 WCF 或 ASP.NET Web API。
