---
title: 如何：将托管代码 DCOM 迁移到 WCF
description: 将服务器和客户端之间的分布式组件对象模型 (DCOM) 托管代码调用迁移到 Windows Communication Foundation (WCF)。
ms.date: 03/30/2017
ms.assetid: 52961ffc-d1c7-4f83-832c-786444b951ba
ms.openlocfilehash: eb84b8071d8dec5d3e70a2f1903f84ee64c31b08
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96274837"
---
# <a name="how-to-migrate-managed-code-dcom-to-wcf"></a>如何：将托管代码 DCOM 迁移到 WCF

Windows Communication Foundation (WCF) 是针对分布式组件对象模型 (DCOM) 建议的安全选择，可用于处理分布式环境中服务器和客户端间的托管代码调用。 本文介绍在以下情景中，如何将代码从 DCOM 迁移到 WCF。  
  
- 远程服务将对象按值返回到客户端  
  
- 客户端将对象按值返回到远程服务  
  
- 远程服务将对象按引用返回到客户端  
  
 出于安全原因，WCF 中不允许将对象按引用从客户端发送到服务。 在 WCF 中使用双工服务可实现需要在客户端和服务器间来回会话的方案。  有关双工服务的详细信息，请参阅[双工服务](../wcf/feature-details/duplex-services.md)  
  
 有关创建 WCF 服务和这些服务的客户端的详细信息，请参阅[基本 WCF 编程](../wcf/basic-wcf-programming.md)、[设计和实现服务](../wcf/designing-and-implementing-services.md)以及[生成客户端](../wcf/building-clients.md)。  
  
## <a name="dcom-example-code"></a>DCOM 代码示例  

 对于这些方案，使用 WCF 所示的 DCOM 接口具有以下结构：  
  
```csharp  
[ComVisible(true)]  
[Guid("AA9C4CDB-55EA-4413-90D2-843F1A49E6E6")]  
public interface IRemoteService  
{  
   Customer GetObjectByValue();  
   IRemoteObject GetObjectByReference();  
   void SendObjectByValue(Customer customer);  
}  
  
[ComVisible(true)]  
[Guid("A12C98DE-B6A1-463D-8C24-81E4BBC4351B")]  
public interface IRemoteObject  
{  
}  
  
public class Customer  
{  
}  
```  
  
## <a name="the-service-returns-an-object-by-value"></a>此服务返回对象按值  

 对于此方案，可调用服务并且此服务的方法将返回对象，此对象按值从服务器传递到客户端对象。 这种方案表示以下 COM 调用：  
  
```csharp  
public interface IRemoteService  
{  
    Customer GetObjectByValue();  
}  
```  
  
 在此方案中，客户端从远程服务接收对象的反序列化副本。 客户端与此本地副本可进行交互，而无需调用返回到服务。  换言之，客户端会得到保证，在调用本地副本的方法时，无论如何都不会涉及到该服务。 WCF 始终按值从服务返回对象，因此以下步骤描述了创建常规 WCF 服务。  
  
### <a name="step-1-define-the-wcf-service-interface"></a>步骤 1：定义 WCF 服务接口  

 定义 WCF 服务的公共接口并标记 [<xref:System.ServiceModel.ServiceContractAttribute>] 属性。  为需要对客户端公开的方法标记 [<xref:System.ServiceModel.OperationContractAttribute>] 属性。 以下示例演示了如何使用这些属性标识客户端可以调用的服务器端接口和接口方法。 此方案中使用的方法用粗体显示。  
  
```csharp  
using System.Runtime.Serialization;  
using System.ServiceModel;  
using System.ServiceModel.Web;
. . .  
[ServiceContract]  
public interface ICustomerManager  
{  
    [OperationContract]  
    void StoreCustomer(Customer customer);  
  
    [OperationContract]     Customer GetCustomer(string firstName, string lastName);
  
}  
```  
  
### <a name="step-2-define-the-data-contract"></a>步骤 2：定义数据协定  

 下一步，你应该创建服务的数据协定，此协定将描述服务及其客户端间将如何交换数据。  应为数据协定中描述的类标记 [<xref:System.Runtime.Serialization.DataContractAttribute>] 属性。 希望对客户端和服务器可见的单独属性或字段标记为 [<xref:System.Runtime.Serialization.DataMemberAttribute>] 属性。 要允许从数据协定的类派生的类型，必须使用 [<xref:System.Runtime.Serialization.KnownTypeAttribute>] 属性标识它们。 WCF 将仅序列化或反序列化服务接口中的类型和标识为已知类型的类型。 如果尝试使用不是已知类型的类型，则将发生异常。  
  
 有关数据协定的详细信息，请参阅[数据协定](../wcf/samples/data-contracts.md)  
  
```csharp  
[DataContract]  
[KnownType(typeof(PremiumCustomer))]  
public class Customer  
{  
    [DataMember]  
    public string Firstname;  
    [DataMember]  
    public string Lastname;  
    [DataMember]  
    public Address DefaultDeliveryAddress;  
    [DataMember]  
    public Address DefaultBillingAddress;  
}  
 [DataContract]  
public class PremiumCustomer : Customer  
{  
    [DataMember]  
    public int AccountID;  
}  
  
 [DataContract]  
public class Address  
{  
    [DataMember]  
    public string Street;  
    [DataMember]  
    public string Zipcode;  
    [DataMember]  
    public string City;  
    [DataMember]  
    public string State;  
    [DataMember]  
    public string Country;  
}  
```  
  
### <a name="step-3-implement-the-wcf-service"></a>步骤 3：实现 WCF 服务  

 下一步，你应该实现 WCF 服务类，此服务类将实现你在上一步中所定义的接口。  
  
```csharp  
public class CustomerService: ICustomerManager
{  
    public void StoreCustomer(Customer customer)  
    {  
        // write to a database  
    }  
    public Customer GetCustomer(string firstName, string lastName)  
    {  
        // read from a database  
    }  
}  
```  
  
### <a name="step-4-configure-the-service-and-the-client"></a>步骤 4：配置服务和客户端  

 若要运行 WCF 服务，则需要声明公开特定 URL 中使用特定 WCF 绑定的服务接口的终结点。 绑定指定传输、编码和协议的详细信息，以便客户端和服务器进行通信。 通常将绑定添加到服务项目的配置文件 (web.config) 中。 下面内容显示了服务示例的绑定项：  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Server.CustomerService">  
        <endpoint address="http://localhost:8083/CustomerManager"
                  binding="basicHttpBinding"  
                  contract="Shared.ICustomerManager" />  
      </service>  
    </services>  
  </system.serviceModel>  
</configuration>  
```  
  
 下一步，需要配置客户端，以匹配由服务所指定的绑定信息。 为此，请将以下代码添加到客户端的应用程序配置 (app.config) 文件中。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <endpoint name="customermanager"
                address="http://localhost:8083/CustomerManager"
                binding="basicHttpBinding"
                contract="Shared.ICustomerManager"/>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="step-5-run-the-service"></a>步骤 5：运行服务  

 最后，通过向服务应用程序添加以下行并启动此应用程序，你可以在控制台应用程序中自承载服务。 有关托管 WCF 服务应用程序的其他方式的详细信息，请参阅[托管服务](../wcf/hosting-services.md)。  
  
```csharp  
ServiceHost customerServiceHost = new ServiceHost(typeof(CustomerService));  
customerServiceHost.Open();  
```  
  
### <a name="step-6-call-the-service-from-the-client"></a>步骤 6：从客户端调用服务  

 若要从客户端调用服务，则需要创建服务的通道工厂，并请求一个通道，这将使你能够直接从客户端调用 `GetCustomer` 方法。 通道可实现服务接口，并为你处理基础的请求/答复逻辑。  此方法调用的返回值是服务响应的反序列化的副本。  
  
```csharp  
ChannelFactory<ICustomerManager> factory =
     new ChannelFactory<ICustomerManager>("customermanager");  
ICustomerManager service = factory.CreateChannel();  
Customer customer = service.GetCustomer("Mary", "Smith");  
```  
  
## <a name="the-client-sends-a-by-value-object-to-the-server"></a>客户端按值向服务器发送对象  

 在此方案中，客户端将对象按值发送到服务器。 这意味着服务器将接收对象的反序列化副本。  服务器可对此副本调用方法，并确保客户端代码中没有回调。 如前所述，正常的 WCF 数据交换是按值进行的。  这可确保对其中一个对象的调用方法仅在本地执行 – 它将不会调用客户端上的代码。  
  
 这种方案表示以下 COM 方法调用：  
  
```csharp  
public interface IRemoteService  
{  
    void SendObjectByValue(Customer customer);  
}  
```  
  
 此方案使用与第一个示例中所示相同的服务接口和数据协定。 此外，客户端和服务将按相同的方法进行配置。 在此示例中，建立了一个信道，以发送对象并按相同的方式运行。 但是，对于此示例，将创建一个客户端，此客户端可调用服务，并按值传递对象。 客户端将在服务协定中调用的服务方法用粗体显示：  
  
```csharp  
[ServiceContract]  
public interface ICustomerManager  
{  
    [OperationContract]     void StoreCustomer(Customer customer);  
  
    [OperationContract]  
    Customer GetCustomer(string firstName, string lastName);  
}  
```  
  
### <a name="add-code-to-the-client-that-sends-a-by-value-object"></a>将代码添加到发送按值对象的客户端  

 以下代码演示了客户端如何创建一个新的按值客户对象、如何创建与 `ICustomerManager` 服务进行通信的通道以及如何其发送客户对象。  
  
 将序列化客户对象，并将其发送到服务，由服务将其反序列化为对象的新副本。  服务对此对象调用的任何方法将仅对服务器本地执行。 请注意，此代码说明发送派生的类型 (`PremiumCustomer`)。  服务协定期望 `Customer` 对象，但服务数据协定使用 [<xref:System.Runtime.Serialization.KnownTypeAttribute>] 属性以表示同样允许 `PremiumCustomer`。  WCF 通过此服务接口序列化或反序列化任何其他类型的尝试将失败。  
  
```csharp  
PremiumCustomer customer = new PremiumCustomer();  
customer.Firstname = "John";  
customer.Lastname = "Doe";  
customer.DefaultBillingAddress = new Address();  
customer.DefaultBillingAddress.Street = "One Microsoft Way";  
customer.DefaultDeliveryAddress = customer.DefaultBillingAddress;  
customer.AccountID = 42;  
  
ChannelFactory<ICustomerManager> factory =  
   new ChannelFactory<ICustomerManager>("customermanager");  
ICustomerManager customerManager = factory.CreateChannel();  
customerManager.StoreCustomer(customer);  
```  
  
## <a name="the-service-returns-an-object-by-reference"></a>服务按引用返回对象  

 对于此方案，客户端应用调用远程服务，此方法将返回一个对象，此对象由引用从服务到传递到客户端。  
  
 如前所述，WCF 服务将始终按值返回对象。  但是，可以通过使用 <xref:System.ServiceModel.EndpointAddress10> 类，实现类似的效果。  <xref:System.ServiceModel.EndpointAddress10> 是客户端可用于获取服务器上会话按引用对象的可序列化的按值对象。  
  
 此方案所示的 WCF 中按引用对象的行为不同于 DCOM。  在 DCOM 中，服务器可直接返回按引用对象到客户端，而客户端可调用此对象的方法，并在服务器上执行。  但是在 WCF 中，始终按值返回对象。  客户端必须取得由 <xref:System.ServiceModel.EndpointAddress10> 表示的按值对象，并用将其用于创建自己的会话按引用对象。  客户端方法对服务器上的会话对象执行进行调用。换言之，WCF 中的按引用对象是配置为会话的普通 WCF 服务。  
  
 在 WCF 中，会话是使两个终结点之间发送的多个消息关联的一种方法。  这意味着一旦客户端获取与此服务的连接，将会建立客户端和服务器之间的会话。  对于此单个会话中的所有交互，客户端都将使用服务器端对象的单个唯一实例。 会话 WCF 协定与面向连接的网络请求/响应模式类似。  
  
 此方案由以下的 DCOM 方法表示。  
  
```csharp  
public interface IRemoteService  
{  
    IRemoteObject GetObjectByReference();  
}  
```  
  
### <a name="step-1-define-the-sessionful-wcf-service-interface-and-implementation"></a>步骤 1：定义会话 WCF 服务接口与实现  

 首先，定义包含会话对象的 WCF 服务接口。  
  
 在此代码中，为会话对象标记 `ServiceContract` 属性，从而将其识别为常规的 WCF 服务接口。  此外，设置 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A> 属性，以指示它将成为会话服务。  
  
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
  
 以下代码演示服务实现。  
  
 为此服务标记 [ServiceBehavior] 属性，并且将其 InstanceContextMode 属性设置为 InstanceContextMode.PerSessions，以指示应为每个会话创建这种类型的唯一实例。  
  
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
  
### <a name="step-2-define-the-wcf-factory-service-for-the-sessionful-object"></a>步骤 2：定义会话对象的 WCF 工厂服务  

 必须定义并实现创建会话对象的服务。 下面的代码演示如何执行此操作。 此代码将创建另一种返回 <xref:System.ServiceModel.EndpointAddress10> 对象的 WCF 服务。  这是一种可用于创建会话对象的终结点的可序列化形式。  
  
```csharp  
[ServiceContract]  
    public interface ISessionBoundFactory  
    {  
        [OperationContract]  
        EndpointAddress10 GetInstanceAddress();  
    }  
```  
  
 以下是此服务的实现。 此实现维护单一实例通道工厂以创建会话对象。  当调用 `GetInstanceAddress` 时，它会创建一个通道，并创建一个指向与此通道关联的远程地址的 <xref:System.ServiceModel.EndpointAddress10> 对象。   <xref:System.ServiceModel.EndpointAddress10> 是一种可按值返回到客户端的数据类型。
  
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
  
### <a name="step-3-configure-and-start-the-wcf-services"></a>步骤 3：配置并启动 WCF 服务  

 若要承载这些服务，将需要添加以下内容到服务器的配置文件 (web.config) 中。  
  
1. 添加描述会话对象终结点的 `<client>` 节。  在此方案中，服务器还充当客户端，并必须配置为启用此功能。  
  
2. 在 `<services>` 节中，声明工厂和会话对象的服务终结点。  这使客户端可与服务终结点通信，可获取 <xref:System.ServiceModel.EndpointAddress10> 并可创建会话通道。  
  
 以下是使用这些设置的配置文件示例：  
  
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
  
 将以下行添加到控制台应用程序中，从而自承载服务并启动应用程序。  
  
```csharp  
ServiceHost factoryHost = new ServiceHost(typeof(SessionBoundFactory));  
factoryHost.Open();  
  
ServiceHost sessionBoundServiceHost = new ServiceHost(  
typeof(MySessionBoundObject));  
sessionBoundServiceHost.Open();  
```  
  
### <a name="step-4-configure-the-client-and-call-the-service"></a>步骤 4：配置客户端并调用服务  

 通过在项目的应用程序配置文件 (app.config) 中进行以下项，配置客户端与 WCF 服务进行通信。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
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
  
 若要调用此服务，请将代码添加到客户端，以执行以下操作：  
  
1. 创建 `ISessionBoundFactory` 服务通道。  
  
2. 使用此通道调用 `ISessionBoundFactory` 服务并获取 <xref:System.ServiceModel.EndpointAddress10> 对象。  
  
3. 使用 <xref:System.ServiceModel.EndpointAddress10> 创建通道，以获取会话对象。  
  
4. 调用 `SetCurrentValue` 和 `GetCurrentValue` 方法，演示它与跨多个调用的对象实例相同。  
  
```csharp  
ChannelFactory<ISessionBoundFactory> factory =  
        new ChannelFactory<ISessionBoundFactory>("factory");  
  
ISessionBoundFactory sessionBoundFactory = factory.CreateChannel();  
  
EndpointAddress10 address = sessionBoundFactory.GetInstanceAddress();  
  
ChannelFactory<ISessionBoundObject> sessionBoundObjectFactory =  
    new ChannelFactory<ISessionBoundObject>(  
        new NetTcpBinding(),  
        address.ToEndpointAddress());  
  
ISessionBoundObject sessionBoundObject =  
        sessionBoundObjectFactory.CreateChannel();  
  
sessionBoundObject.SetCurrentValue("Hello");  
if (sessionBoundObject.GetCurrentValue() == "Hello")  
{  
    Console.WriteLine("Session-full instance management works as expected");  
}  
```  
  
## <a name="see-also"></a>请参阅

- [基本 WCF 编程](../wcf/basic-wcf-programming.md)
- [设计和实现服务](../wcf/designing-and-implementing-services.md)
- [生成客户端](../wcf/building-clients.md)
- [双工服务](../wcf/feature-details/duplex-services.md)
