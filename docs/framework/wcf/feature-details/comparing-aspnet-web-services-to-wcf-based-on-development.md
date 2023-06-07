---
title: 从开发的角度比较 ASP.NET Web 服务与 WCF
ms.date: 03/30/2017
ms.assetid: f362d00e-ce82-484f-9d4f-27e579d5c320
ms.openlocfilehash: c6e83bb234751dc477776f0fa540ffa8688dc667
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597587"
---
# <a name="comparing-aspnet-web-services-to-wcf-based-on-development"></a>从开发的角度比较 ASP.NET Web 服务与 WCF

Windows Communication Foundation （WCF）具有 ASP.NET 兼容模式选项，可对 WCF 应用程序进行编程和配置（如 ASP.NET Web 服务）并模拟它们的行为。 以下各节基于使用这两种技术开发应用程序所需的内容，对 ASP.NET Web 服务和 WCF 进行比较。

## <a name="data-representation"></a>数据表示形式

一般情况下，使用 ASP.NET 开发 Web 服务首先要定义服务要使用的任意复杂数据类型。 ASP.NET 依赖于 <xref:System.Xml.Serialization.XmlSerializer> 将由 .NET Framework 类型表示的数据转换为 XML，以便与服务进行相互传输并将以 XML 形式接收的数据转换为 .NET Framework 对象。 定义 ASP.NET 服务要使用的复杂数据类型需要使用 .NET Framework 类的定义，这些类可由 <xref:System.Xml.Serialization.XmlSerializer> 序列化为 XML 或从 XML 进行反序列化。 这些类可以手动编写，或者使用命令行 XML 架构/数据类型支持实用工具 xsd.exe 从 XML 架构中的类型定义生成。

下面是定义可由 <xref:System.Xml.Serialization.XmlSerializer> 序列化为 XML 或从 XML 序列化的 .NET Framework 类时需要了解的关键问题列表：

- 只有 .NET Framework 对象的公共字段和属性才能转换为 XML。

- 仅当集合类实现 <xref:System.Collections.IEnumerable> 或 <xref:System.Collections.ICollection> 接口时才能将集合类的实例序列化为 XML。

- 实现 <xref:System.Collections.IDictionary> 接口的类，如 <xref:System.Collections.Hashtable>，不能序列化为 XML。

- <xref:System.Xml.Serialization> 命名空间中的许多属性类型均可添加到 .NET Framework 类及其成员中，用于控制类实例的 XML 表示方式。

WCF 应用程序开发通常也以复杂类型的定义开始。 可以使用 WCF 与 ASP.NET Web 服务使用相同的 .NET Framework 类型。

WCF <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 可以添加到 .NET Framework 类型，以指示要将类型的实例序列化为 XML，并对类型的哪些特定字段或属性进行序列化，如下面的示例代码所示。

```csharp
//Example One:
[DataContract]
public class LineItem
{
    [DataMember]
    public string ItemNumber;
    [DataMember]
    public decimal Quantity;
    [DataMember]
    public decimal UnitPrice;
}

//Example Two:
public class LineItem
{
    [DataMember]
    private string itemNumber;
    [DataMember]
    private decimal quantity;
    [DataMember]
    private decimal unitPrice;

    public string ItemNumber
    {
      get
      {
          return this.itemNumber;
      }

      set
      {
          this.itemNumber = value;
      }
    }

    public decimal Quantity
    {
        get
        {
            return this.quantity;
        }

        set
        {
            this.quantity = value;
        }
    }

    public decimal UnitPrice
    {
      get
      {
          return this.unitPrice;
      }

      set
      {
          this.unitPrice = value;
      }
    }
}

//Example Three:
public class LineItem
{
     private string itemNumber;
     private decimal quantity;
     private decimal unitPrice;

     [DataMember]
     public string ItemNumber
     {
       get
       {
          return this.itemNumber;
       }

       set
       {
           this.itemNumber = value;
       }
     }

     [DataMember]
     public decimal Quantity
     {
          get
          {
              return this.quantity;
          }

          set
          {
             this.quantity = value;
          }
     }

     [DataMember]
     public decimal UnitPrice
     {
          get
          {
              return this.unitPrice;
          }

          set
          {
              this.unitPrice = value;
          }
     }
}
```

<xref:System.Runtime.Serialization.DataContractAttribute> 表示要对零个或多个类型字段或属性进行序列化，而 <xref:System.Runtime.Serialization.DataMemberAttribute> 表示要对某个特定字段或属性进行序列化。 <xref:System.Runtime.Serialization.DataContractAttribute> 可应用于类或结构。 <xref:System.Runtime.Serialization.DataMemberAttribute> 可以应用于字段或属性 (Property)，对其应用此属性 (Attribute) 的字段和属性 (Property) 既可以是公共的也可以是私有的。 应用了的类型的实例在 <xref:System.Runtime.Serialization.DataContractAttribute> WCF 中称为数据协定。 它们通过 <xref:System.Runtime.Serialization.DataContractSerializer> 序列化为 XML。

下面是使用 <xref:System.Runtime.Serialization.DataContractSerializer> 和使用 <xref:System.Xml.Serialization.XmlSerializer> 以及 <xref:System.Xml.Serialization> 命名空间的各种属性之间的重大差异列表。

- <xref:System.Xml.Serialization.XmlSerializer> 和 <xref:System.Xml.Serialization> 命名空间的属性旨在让您可以将 .NET Framework 类型映射为在 XML 架构中定义的任意有效类型，因此它们对类型如何以 XML 方式表示提供了十分精确的控制。 <xref:System.Runtime.Serialization.DataContractSerializer>、<xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 则几乎不对类型如何以 XML 方式表示提供控制。 您只能指定用于在 XML 中表示类型及其字段或属性的命名空间和名称，以及字段和属性在 XML 中的显示顺序：

  ```csharp
  [DataContract(
  Namespace="urn:Contoso:2006:January:29",
  Name="LineItem")]
  public class LineItem
  {
        [DataMember(Name="ItemNumber",IsRequired=true,Order=0)]
        public string itemNumber;
        [DataMember(Name="Quantity",IsRequired=false,Order = 1)]
        public decimal quantity;
        [DataMember(Name="Price",IsRequired=false,Order = 2)]
        public decimal unitPrice;
  }
  ```

  任何其他与用于表示 .NET 类型的 XML 结构相关的数据均由 <xref:System.Runtime.Serialization.DataContractSerializer> 确定。

- 通过不允许过多地控制如何以 XML 方式表示类型，<xref:System.Runtime.Serialization.DataContractSerializer> 序列化过程的可预知程度变得很高，因此，更容易实现优化。 <xref:System.Runtime.Serialization.DataContractSerializer> 设计的一个实用优势是它具有更好的性能，大约提高了 10%。

- 用于 <xref:System.Xml.Serialization.XmlSerializer> 的属性 (Attribute) 并不指明此类型的哪些字段或属性 (Property) 将序列化为 XML，而用于 <xref:System.Runtime.Serialization.DataMemberAttribute> 的 <xref:System.Runtime.Serialization.DataContractSerializer> 则显式指明将对哪些字段或属性 (Property) 执行序列化。 因此，数据协定是与应用程序所要发送和接收的数据的结构有关的显式协定。

- <xref:System.Xml.Serialization.XmlSerializer> 只能将 .NET 对象的公共成员转换为 XML；而无论对象成员的访问修饰符如何，<xref:System.Runtime.Serialization.DataContractSerializer> 都可以将这些成员转换为 XML。

- 由于可将各种类型的非公共成员序列化为 XML，因此，<xref:System.Runtime.Serialization.DataContractSerializer> 对于可序列化为 XML 的 .NET 类型的多样性限制较少。 尤其是，它可以转换为 XML 类型，如实现 <xref:System.Collections.Hashtable> 接口的 <xref:System.Collections.IDictionary>。 <xref:System.Runtime.Serialization.DataContractSerializer> 更有可能会将任意预先存在的 .NET 类型序列化为 XML 而无需修改类型的定义或开发类型的包装。

- 由于 <xref:System.Runtime.Serialization.DataContractSerializer> 可以访问类型的非公共成员，因此，它还要求完全信任，而 <xref:System.Xml.Serialization.XmlSerializer> 不要求。 完全信任代码访问权限提供对计算机上的所有资源的完全访问权限，可以使用执行代码的凭据访问该计算机上的所有资源。 当完全受信任的代码访问计算机上的所有资源时，应谨慎使用此选项。

- <xref:System.Runtime.Serialization.DataContractSerializer> 中集成了一些对版本管理的支持：

  - <xref:System.Runtime.Serialization.DataMemberAttribute> 具有一个 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 属性，将它赋予 False 值可指示早期版本中不存在将成员添加到的新版本数据协定，因此使得包含新版协定的应用程序可以处理早期的版本。

  - 通过让某个数据协定实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口，使得 <xref:System.Runtime.Serialization.DataContractSerializer> 可通过包含早期版本协定的应用程序传递新版数据协定中定义的成员。

不管存在何种差异，只要 XML 具有显式定义的命名空间，默认情况下，<xref:System.Xml.Serialization.XmlSerializer> 将类型序列化为的 XML 与 <xref:System.Runtime.Serialization.DataContractSerializer> 将类型序列化为的 XML 语义相同。 下面的类具有与这两个序列化程序一起使用的特性，由和转换为语义上完全相同的 XML <xref:System.Xml.Serialization.XmlSerializer> <xref:System.Runtime.Serialization.DataContractAttribute> ：

```csharp
[Serializable]
[XmlRoot(Namespace="urn:Contoso:2006:January:29")]
[DataContract(Namespace="urn:Contoso:2006:January:29")]
public class LineItem
{
     [DataMember]
     public string ItemNumber;
     [DataMember]
     public decimal Quantity;
     [DataMember]
     public decimal UnitPrice;
}
```

Windows 软件开发工具包（SDK）包含一个名为 "Svcutil.exe" 的[元数据实用工具（）](../servicemodel-metadata-utility-tool-svcutil-exe.md)的命令行工具。 与用于 ASP.NET Web 服务的 xsd.exe 工具一样，Svcutil.exe 可以从 XML 架构生成 .NET 数据类型的定义。 如果 <xref:System.Runtime.Serialization.DataContractSerializer> 可发出由 XML 架构定义的格式的 XML，类型将为数据协定；否则，它们专用于通过 <xref:System.Xml.Serialization.XmlSerializer> 进行序列化。 Svcutil.exe 还可以通过使用数据协定的开关从数据协定生成 XML 架构 `dataContractOnly` 。

> [!NOTE]
> 尽管 ASP.NET Web 服务使用 <xref:System.Xml.Serialization.XmlSerializer> ，并且 wcf ASP.NET 兼容模式使 wcf 服务能够模仿 ASP.NET Web 服务的行为，但 ASP.NET 兼容性选项不会限制一个使用 <xref:System.Xml.Serialization.XmlSerializer> 。 用户仍然可以将 <xref:System.Runtime.Serialization.DataContractSerializer> 用于以 ASP.NET 兼容模式运行的服务。

## <a name="service-development"></a>服务开发

要使用 ASP.NET 开发服务，通常会将 <xref:System.Web.Services.WebService> 属性添加到类中，并将 <xref:System.Web.Services.WebMethodAttribute> 添加到该类中将作为服务操作使用的任意方法上：

```csharp
[WebService]
public class Service : T:System.Web.Services.WebService
{
    [WebMethod]
    public string Echo(string input)
    {
       return input;
    }
}
```

ASP.NET 2.0 中引入了将属性 <xref:System.Web.Services.WebService> 和 <xref:System.Web.Services.WebMethodAttribute> 添加到接口（而不是类）的选项，并编写了一个类实现此接口：

```csharp
[WebService]
public interface IEcho
{
    [WebMethod]
    string Echo(string input);
}

public class Service : IEcho
{

   public string Echo(string input)
   {
        return input;
    }
}
```

使用此选项是首选方法，因为具有 <xref:System.Web.Services.WebService> 属性的接口包含服务所执行操作的协定，该服务可以在通过多种不同方式实现同一协定的各种类中重复使用。

WCF 服务通过定义一个或多个 WCF 端点提供。 终结点由地址、绑定和服务协定定义。 地址用于定义服务的位置。 绑定用于指定与服务的通信方式。 服务协定用于定义服务可执行的操作。

通常最先定义服务协定，通过将 <xref:System.ServiceModel.ServiceContractAttribute> 和 <xref:System.ServiceModel.OperationContractAttribute> 添加到接口中实现：

```csharp
[ServiceContract]
public interface IEcho
{
     [OperationContract]
     string Echo(string input);
}
```

<xref:System.ServiceModel.ServiceContractAttribute>指定接口定义 WCF 服务协定，并 <xref:System.ServiceModel.OperationContractAttribute> 指示接口的哪些方法（如果有）定义服务协定的操作。

定义服务协定后，它将在类中实现，方法是使类实现服务协定定义的接口：

```csharp
public class Service : IEcho
{
    public string Echo(string input)
    {
       return input;
    }
}
```

在 WCF 中，实现服务协定的类称为 "服务类型"。

下一步是将地址和绑定与服务类型关联。 通常通过直接编辑文件或使用 WCF 提供的配置编辑器在配置文件中完成此操作。 下面是配置文件的示例。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
     <system.serviceModel>
      <services>
      <service name="Service ">
       <endpoint
        address="EchoService"
        binding="basicHttpBinding"
        contract="IEchoService "/>
      </service>
      </services>
     </system.serviceModel>
</configuration>
```

绑定指定用于与应用程序通信的协议集。 下表列出由系统提供的表示常用选项的绑定。

|名称|目的|
|----------|-------------|
|BasicHttpBinding|可以与支持 WS-BasicProfile 1.1 和 Basic Security Profile 1.0 的 Web 服务和客户端相互操作。|
|WSHttpBinding|可以与通过 HTTP 支持 WS* 协议的 Web 服务和客户端相互操作。|
|WSDualHttpBinding|双向 HTTP 通信，使用此绑定时初始消息的接收方不直接回复初始发送方，而是在一段时间内使用与 WS* 协议一致的 HTTP 传输任意数量的响应。|
|WSFederationBinding|访问服务资源时所用的 HTTP 通信可根据由显式标识的凭据提供程序发出的凭据进行控制。|
|NetTcpBinding|跨网络在 WCF 软件实体之间进行安全、可靠、高性能的通信。|
|NetNamedPipeBinding|同一台计算机上 WCF 软件实体之间的安全、可靠、高性能的通信。|
|NetMsmqBinding|使用 MSMQ 在 WCF 软件实体之间进行通信。|
|MsmqIntegrationBinding|使用 MSMQ 在 WCF 软件实体与其他软件实体之间进行通信。|
|NetPeerTcpBinding|使用 Windows 对等网络建立 WCF 软件实体之间的通信。|

系统提供的绑定 <xref:System.ServiceModel.BasicHttpBinding> 集成了 ASP.NET Web 服务支持的协议集。

WCF 应用程序的自定义绑定可以轻松定义为 WCF 用于实现各个协议的绑定元素类的集合。 可以将新的绑定元素编写为表示其他协议。

服务类型的内部行为可以使用一系列称为行为的类的属性对其进行调整。 其中，<xref:System.ServiceModel.ServiceBehaviorAttribute> 类用于指定服务类型为多线程。

```csharp
[ServiceBehavior(ConcurrencyMode=ConcurrencyMode.Multiple)]
public class DerivativesCalculatorServiceType: IDerivativesCalculator
```

某些行为是属性，如 <xref:System.ServiceModel.ServiceBehaviorAttribute>。 其他具有管理员希望设置的属性的行为可以在配置应用程序时修改。

在编程服务类型中，通常使用 <xref:System.ServiceModel.OperationContext> 类。 它的静态 <xref:System.ServiceModel.OperationContext.Current%2A> 属性提供对有关运行操作上下文的信息的访问权限。 <xref:System.ServiceModel.OperationContext> 类似于 <xref:System.Web.HttpContext> 和 <xref:System.EnterpriseServices.ContextUtil> 类。

## <a name="hosting"></a>Hosting

ASP.NET Web 服务编译为类库程序集。 将提供一个称为服务文件且扩展名为 .asmx 的文件，该文件包含一条 `@ WebService` 指令，该指令标识的类包含表示服务和服务所在程序集的代码。

`<%@ WebService Language="C#" Class="Service,ServiceAssembly" %>`

在 Internet 信息服务 (IIS) 中，服务文件将复制到 ASP.NET 应用程序根目录下，程序集则复制到此应用程序根目录的 \bin 子目录下。 然后，您可以在应用程序根目录下使用服务文件的统一资源定位符 (URL) 访问此应用程序。

WCF 服务可随时在 IIS 5.1 或6.0 中托管，Windows 进程激活服务（WAS）作为 IIS 7.0 的一部分提供，并且在任何 .NET 应用程序中提供。 若要在 IIS 5.1 或 6.0 中承载某项服务，此服务必须使用 HTTP 作为通信传输协议。

若要在 IIS 5.1、6.0 或 WAS 中承载某项服务，请使用下列步骤：

1. 将此服务类型编译为类库程序集。

2. 使用标识服务类型的 `@ ServiceHost` 指令创建一个扩展名为 .svc 的服务文件：

    `<%@ServiceHost language="c#" Service="MyService" %>`

3. 将服务文件复制到虚拟目录下，并将程序集复制到该虚拟目录的 \bin 子目录下。

4. 将配置文件复制到虚拟目录下，并将其命名为 Web.config。

然后，您可以在应用程序根目录下使用服务文件的 URL 访问此应用程序

若要在 .NET 应用程序中托管 WCF 服务，请将服务类型编译为该应用程序引用的类库程序集，并对该应用程序进行编程以使用类承载服务 <xref:System.ServiceModel.ServiceHost> 。 下面是所需基本编程的示例：

```csharp
string httpBaseAddress = "http://www.contoso.com:8000/";
string tcpBaseAddress = "net.tcp://www.contoso.com:8080/";

Uri httpBaseAddressUri = new Uri(httpBaseAddress);
Uri tcpBaseAddressUri = new Uri(tcpBaseAddress);

Uri[] baseAddresses = new Uri[] {
 httpBaseAddressUri,
 tcpBaseAddressUri};

using(ServiceHost host = new ServiceHost(
typeof(Service), //"Service" is the name of the service type baseAddresses))
{
     host.Open();

     […] //Wait to receive messages
     host.Close();
}
```

此示例演示如何在构造 <xref:System.ServiceModel.ServiceHost> 时指定一个或多个传输协议的地址。 这些地址称为基址。

为 WCF 服务的任何终结点提供的地址是相对于终结点主机的基址的地址。 对于每个通信传输协议，宿主都可以有一个基址。 在上面的配置文件配置示例中，为终结点选定的 <xref:System.ServiceModel.BasicHttpBinding> 将 HTTP 用作传输协议，因此终结点的地址 `EchoService` 与宿主的 HTTP 基址相关。 对于前面的示例中的主机，HTTP 基址为 `http://www.contoso.com:8000/` 。 对于在 IIS 或 WAS 中承载的服务，其基址为服务的服务文件的 URL。

只能使用在 IIS 或 WAS 中承载的服务，以及仅使用 HTTP 作为传输协议进行配置的服务，才能使用 WCF ASP.NET 兼容模式选项。 启用此选项需要执行下列步骤。

1. 程序员必须将 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> 属性添加到服务类型中，并指定是允许还是需要 ASP.NET 兼容模式。

    ```csharp
    [System.ServiceModel.Activation.AspNetCompatibilityRequirements(
          RequirementsMode=AspNetCompatibilityRequirementsMode.Require)]
    public class DerivativesCalculatorServiceType: IDerivativesCalculator
    ```

2. 管理员必须将应用程序配置为使用 ASP.NET 兼容模式。

    ```xml
    <configuration>
         <system.serviceModel>
          <services>
          […]
          </services>
          <serviceHostingEnvironment aspNetCompatibilityEnabled="true"/>
        </system.serviceModel>
    </configuration>
    ```

    WCF 应用程序也可以配置为使用 .asmx 作为其服务文件（而不是 .svc）的扩展。

    ```xml
    <system.web>
         <compilation>
          <compilation debug="true">
          <buildProviders>
           <remove extension=".asmx"/>
           <add extension=".asmx"
            type="System.ServiceModel.ServiceBuildProvider,
            System.ServiceModel,
            Version=3.0.0.0,
            Culture=neutral,
            PublicKeyToken=b77a5c561934e089" />
          </buildProviders>
          </compilation>
         </compilation>
    </system.web>
    ```

    使用此选项，你无需修改在将服务修改为使用 WCF 时配置为使用 .asmx 服务文件的 Url 的客户端。

## <a name="client-development"></a>客户端开发

ASP.NET Web 服务的客户端通过使用命令行工具 WSDL.exe 生成，该工具可将 .asmx 文件的 URL 作为输入提供。 WCF 提供的相应工具为 " [Svcutil.exe 元数据实用工具（）](../servicemodel-metadata-utility-tool-svcutil-exe.md)"。 它使用服务协定的定义和 WCF 客户端类的定义生成代码模块。 它还使用服务的地址和绑定生成一个配置文件。

通常，建议您基于异步模式对远程服务的客户端进行编程。 默认情况下，不论是同步模式还是异步模式，始终都会提供由 WSDL.exe 工具生成的代码。 可为任一模式提供 " [svcutil.exe" 元数据实用工具（）](../servicemodel-metadata-utility-tool-svcutil-exe.md)生成的代码。 默认为对同步模式提供。 如果执行该工具时使用 `/async` 开关，则将为异步模式提供生成的代码。

不保证 ASP.NET 生成的 WCF 客户端类中的名称。默认情况下，NET 的 WSDL.EXE 工具与 Svcutil.exe 工具生成的 WCF 客户端类中的名称相匹配。 尤其是，在默认情况下，必须使用 <xref:System.Xml.Serialization.XmlSerializer> 序列化的类的属性名将在 Svcutil.exe 工具生成的代码中获得后缀 Property，这与 WSDL.exe 工具的情况并不一样。

## <a name="message-representation"></a>消息表示形式

可以对由 ASP.NET Web 服务发送和接收的 SOAP 消息头执行自定义操作。 将从 <xref:System.Web.Services.Protocols.SoapHeader> 派生一个类，用于定义消息头的结构，然后 <xref:System.Web.Services.Protocols.SoapHeaderAttribute> 用于指示存在消息头。

```csharp
public class SomeProtocol : SoapHeader
{
     public long CurrentValue;
     public long Total;
}

[WebService]
public interface IEcho
{
     SomeProtocol ProtocolHeader
     {
      get;
     set;
     }

     [WebMethod]
     [SoapHeader("ProtocolHeader")]
     string PlaceOrders(PurchaseOrderType order);
}

public class Service: WebService, IEcho
{
     private SomeProtocol protocolHeader;

     public SomeProtocol ProtocolHeader
     {
         get
         {
              return this.protocolHeader;
         }

         set
         {
              this.protocolHeader = value;
         }
     }

     string PlaceOrders(PurchaseOrderType order)
     {
         long currentValue = this.protocolHeader.CurrentValue;
     }
}
```

WCF 提供属性、、 <xref:System.ServiceModel.MessageContractAttribute> 和， <xref:System.ServiceModel.MessageHeaderAttribute> <xref:System.ServiceModel.MessageBodyMemberAttribute> 以描述服务发送和接收的 SOAP 消息的结构。

```csharp
[DataContract]
public class SomeProtocol
{
     [DataMember]
     public long CurrentValue;
     [DataMember]
     public long Total;
}

[DataContract]
public class Item
{
     [DataMember]
     public string ItemNumber;
     [DataMember]
     public decimal Quantity;
     [DataMember]
     public decimal UnitPrice;
}

[MessageContract]
public class ItemMessage
{
     [MessageHeader]
     public SomeProtocol ProtocolHeader;
     [MessageBody]
     public Item Content;
}

[ServiceContract]
public interface IItemService
{
     [OperationContract]
     public void DeliverItem(ItemMessage itemMessage);
}
```

此语法将生成消息结构的显式表示形式，同时 ASP.NET Web 服务的代码也将暗示消息结构。 此外，在 ASP.NET 语法中，消息头表示为服务的属性，例如 `ProtocolHeader` 上一示例中的属性，而在 WCF 语法中，它们可以更准确地表示为消息的属性。 同时，WCF 还允许将消息标头添加到终结点的配置中。

```xml
<service name="Service ">
     <endpoint
      address="EchoService"
      binding="basicHttpBinding"
      contract="IEchoService ">
      <headers>
      <dsig:X509Certificate
       xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
       ...
      </dsig:X509Certificate>
      </headers>
     </endpoint>
</service>
```

使用此选项，可以在客户端或服务的代码中避免任何对基础协议标头的引用：标头将根据终结点的配置方式添加到消息中。

## <a name="service-description"></a>服务说明

使用查询 WSDL 对 ASP.NET Web 服务的 .asmx 文件发出 HTTP GET 请求将使 ASP.NET 生成描述服务的 WSDL。 ASP.NET 将返回该 WSDL 作为此请求的响应。

通过 ASP.NET 2.0，用户可以验证服务是否与 Web 服务互操作性组织 (WS-I) 的基本配置文件 1.1 兼容，还可以插入一个声明表明服务与其 WSDL 兼容。 这可以通过使用 `ConformsTo` 属性的 `EmitConformanceClaims` 和 <xref:System.Web.Services.WebServiceBindingAttribute> 参数实现。

```csharp
[WebService(Namespace = "http://tempuri.org/")]
[WebServiceBinding(
     ConformsTo = WsiProfiles.BasicProfile1_1,
     EmitConformanceClaims=true)]
public interface IEcho
```

可以自定义 ASP.NET 为服务生成的 WSDL。 通过创建 <xref:System.Web.Services.Description.ServiceDescriptionFormatExtension> 的派生类以将项目添加到 WSDL 中可实现自定义。

使用在 IIS 51、6.0 或中承载的 HTTP 终结点的 WCF 服务的 .svc 文件的查询 WSDL 发出 HTTP GET 请求会导致 WCF 使用 WSDL 进行响应，以描述该服务。 如果 httpGetEnabled 设置为 true，则使用查询 WSDL 对 .NET 应用程序所承载服务的 HTTP 基址发出 HTTP GET 请求具有相同的效果。

但是，WCF 还会使用其生成的用于描述服务的 WSDL 响应 Ws-metadataexchange 请求。 ASP.NET Web 服务没有对 WS-MetadataExchange 请求的内置支持。

WCF 生成的 WSDL 可以广泛地自定义。 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 类提供了一些用于自定义 WSDL 的功能。 也可以将 WCF 配置为不生成 WSDL，而是使用给定 URL 处的静态 WSDL 文件。

```xml
<behaviors>
     <behavior name="DescriptionBehavior">
     <metadataPublishing
      enableMetadataExchange="true"
      enableGetWsdl="true"
      enableHelpPage="true"
      metadataLocation=
      "http://localhost/DerivativesCalculatorService/Service.WSDL"/>
     </behavior>
</behaviors>
```

## <a name="exception-handling"></a>异常处理

在 ASP.NET Web 服务中，未处理的异常将作为 SOAP 错误返回客户端。 您还可以显式抛出 <xref:System.Web.Services.Protocols.SoapException> 类的实例并对传输到客户端的 SOAP 错误内容加大控制力度。

在 WCF 服务中，未处理的异常不会作为 SOAP 错误返回到客户端，以防止无意中通过异常公开敏感信息。 为了进行调试，系统将提供一个配置设置以将未处理的异常返回客户端。

若要将 SOAP 错误返回客户端，您可以使用数据协定类型作为泛型类型抛出泛型类型的实例 <xref:System.ServiceModel.FaultException%601>。 您还可以将 <xref:System.ServiceModel.FaultContractAttribute> 属性添加到操作中以指定操作可能会生成的错误。

```csharp
[DataContract]
public class MathFault
{
     [DataMember]
     public string operation;
     [DataMember]
     public string problemType;
}

[ServiceContract]
public interface ICalculator
{
     [OperationContract]
     [FaultContract(typeof(MathFault))]
     int Divide(int n1, int n2);
}
```

这样做会导致在服务的 WSDL 中显示对可能错误的建议，从而使客户端程序员可以预计操作可能会导致哪些错误，并编写相应的 Catch 语句。

```csharp
try
{
     result = client.Divide(value1, value2);
}
catch (FaultException<MathFault> e)
{
 Console.WriteLine("FaultException<MathFault>: Math fault while doing "
  + e.Detail.operation
  + ". Problem: "
  + e.Detail.problemType);
}
```

## <a name="state-management"></a>状态管理

用于实现 ASP.NET Web 服务的类可以从 <xref:System.Web.Services.WebService> 派生。

```csharp
public class Service : WebService, IEcho
{

 public string Echo(string input)
 {
  return input;
 }
}
```

在此情况下，可以将此类编程为使用 <xref:System.Web.Services.WebService> 基类的“上下文”属性访问 <xref:System.Web.HttpContext> 对象。 通过使用 <xref:System.Web.HttpContext> 对象的 Application 属性，该对象可用于更新和检索应用程序状态信息，而且通过使用其 Session 属性，该对象可用于更新和检索会话状态信息。

ASP.NET 通过使用实际存储着 <xref:System.Web.HttpContext> 的 Session 属性对会话状态信息的访问位置提供高度控制。 它可以存储在 Cookie、数据库、当前服务器的内存或指定服务器的内存中。 在服务的配置文件中做出此项选择。

WCF 为状态管理提供可扩展对象。 可扩展对象就是实现 <xref:System.ServiceModel.IExtensibleObject%601> 的对象。 最重要的可扩展对象为 <xref:System.ServiceModel.ServiceHostBase> 和 <xref:System.ServiceModel.InstanceContext>。 `ServiceHostBase` 允许你保持同一个主机上所有服务类型的所有实例都可访问的状态，而 `InstanceContext` 允许你保持可以被某个服务类型的同一个实例中运行的任意代码访问的状态。

在此，服务类型 `TradingSystem` 具有一个， <xref:System.ServiceModel.ServiceBehaviorAttribute> 它指定从同一 WCF 客户端实例进行的所有调用都路由到服务类型的同一个实例。

```csharp
[ServiceBehavior(InstanceContextMode = InstanceContextMode.PerSession)]
public class TradingSystem: ITradingService
```

`DealData` 类用于定义可以由某个服务类型的相同实例中运行的任意代码访问的状态。

```csharp
internal class DealData: IExtension<InstanceContext>
{
 public string DealIdentifier = null;
 public Trade[] Trades = null;
}
```

在实现服务协定中任一操作的服务类型的代码中，将 `DealData` 状态对象添加到当前服务类型实例的状态中。

```csharp
string ITradingService.BeginDeal()
{
 string dealIdentifier = Guid.NewGuid().ToString();
 DealData state = new DealData(dealIdentifier);
 OperationContext.Current.InstanceContext.Extensions.Add(state);
 return dealIdentifier;
}
```

随后，可以使用实现另一个服务协定操作的代码检索和修改此状态对象。

```csharp
void ITradingService.AddTrade(Trade trade)
{
 DealData dealData =  OperationContext.Current.InstanceContext.Extensions.Find<DealData>();
 dealData.AddTrade(trade);
}
```

尽管 ASP.NET 提供了对类中状态信息的 <xref:System.Web.HttpContext> 实际存储位置的控制，但 WCF 至少在其初始版本中不提供对存储可扩展对象的位置的控制。 这构成了为 WCF 服务选择 ASP.NET 兼容模式的最佳理由。 如果必须管理可配置状态，则选择 ASP.NET 兼容模式使您可以按照它们在 ASP.NET 中的使用方式使用 <xref:System.Web.HttpContext> 类的功能，而且还可以通过使用存储的 <xref:System.Web.HttpContext> 类配置状态信息的管理位置。

## <a name="security"></a>安全性

用于保证 ASP.NET Web 服务安全的选项就是那些用于保证任意 IIS 应用程序安全的选项。 因为 WCF 应用程序不仅可以在 IIS 中托管，也可以在任何 .NET 可执行文件中托管，所以用于保护 WCF 应用程序的选项必须与 IIS 的功能无关。 但是，为 ASP.NET Web 服务提供的功能也可用于在 ASP.NET 兼容模式下运行的 WCF 服务。

### <a name="security-authentication"></a>安全性：身份验证

IIS 可提供对应用程序的访问进行控制的功能，您可以使用此功能选择匿名访问或各种身份验证模式：Windows 身份验证、摘要式身份验证、基本身份验证和 .NET Passport 身份验证。 Windows 身份验证选项可以用于控制对 ASP.NET Web 服务的访问。 但是，当 WCF 应用程序承载于 IIS 中时，必须将 IIS 配置为允许对应用程序进行匿名访问，以便 WCF 本身可以管理身份验证，这在不同的选项之间支持 Windows 身份验证。 其他内置选项包括用户名令牌、X.509 证书、SAML 令牌和 CardSpace 卡，但是也可以定义自定义身份验证机制。

### <a name="security-impersonation"></a>安全性：模拟

ASP.NET 提供一个标识元素，ASP.NET Web 服务可通过使用该元素模拟当前请求提供了某个特定用户或任何用户的凭据。 该元素可用于在 ASP.NET 兼容模式下运行的 WCF 应用程序中配置模拟。

WCF 配置系统提供自己的标识元素，用于指定要模拟的特定用户。 另外，WCF 客户端和服务可以单独配置为进行模拟。 当客户端发送请求时，可将它们配置为模拟当前用户。

```xml
<behaviors>
     <behavior name="DerivativesCalculatorClientBehavior">
      <clientCredentials>
      <windows allowedImpersonationLevel="Impersonation"/>
      </clientCredentials>
     </behavior>
</behaviors>
```

可将服务操作配置为模拟当前请求提供了任意用户的凭据。

```csharp
[OperationBehavior(Impersonation = ImpersonationOption.Required)]
public void Receive(Message input)
```

### <a name="security-authorization-using-access-control-lists"></a>安全性：使用访问控制列表进行授权

访问控制列表 (ACL) 可用于限制对 .asmx 文件的访问。 但在 ASP.NET 兼容模式下，将忽略 WCF .svc 文件上的 Acl。

### <a name="security-role-based-authorization"></a>安全性：基于角色的授权

IIS Windows 身份验证选项可与由 ASP.NET 配置语言提供的授权元素一起使用，以便根据将用户指定到的 Windows 组简化对 ASP.NET Web 服务的基于角色的授权。 ASP.NET 2.0 引入了一种更常用的基于角色的授权机制：角色提供程序。

角色提供程序就是所有实现基本接口以查询对用户分配哪些角色的类，但每个角色提供程序都知道如何从其他来源检索此信息。 ASP.NET 2.0 提供一个可从 Microsoft SQL Server 数据库检索角色分配的角色提供程序，同时还提供另一个可从 Windows Server 2003 授权管理器检索角色分配的角色提供程序。

在任何 .NET 应用程序（包括 WCF 应用程序）中，实际上可以独立于 ASP.NET 使用角色提供程序机制。 以下 WCF 应用程序示例配置演示了如何使用 ASP.NET 角色提供程序，这是一个通过的方法选择的选项 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> 。

```xml
<system.serviceModel>
     <services>
         <service name="Service.ResourceAccessServiceType"
             behaviorConfiguration="ServiceBehavior">
             <endpoint
              address="ResourceAccessService"
              binding="wsHttpBinding"
              contract="Service.IResourceAccessContract"/>
         </service>
     </services>
     <behaviors>
       <behavior name="ServiceBehavior">
       <serviceAuthorization principalPermissionMode="UseAspNetRoles"/>
      </behavior>
     </behaviors>
</system.serviceModel>
```

### <a name="security-claims-based-authorization"></a>安全性：基于声明的授权

WCF 的最重要的一项创新是，它完全支持基于声明对受保护资源的访问权限。 声明包含类型、权限和值等，例如驾驶执照。 它提出一组有关持有人的声明，其中一个是持有人的出生日期。 此声明的类型为出生日期，而声明的值为司机的出生日期。 声明授予持有人的权限指定持有人可对声明的值执行的操作。 如果是要声明司机的出生日期，权限为拥有：司机拥有该出生日期但是不能更改它。 基于声明的授权包含基于角色的授权，因为角色也是一种声明类型。

基于声明的授权可通过将一组声明与操作的访问要求进行比较实现，根据比较的结果，可获得操作的访问权或遭到拒绝。 在 WCF 中，可以指定一个用于运行基于声明的授权的类，也可以通过为的属性赋值来指定一个值 `ServiceAuthorizationManager` <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> 。

```xml
<behaviors>
     <behavior name='ServiceBehavior'>
     <serviceAuthorization
     serviceAuthorizationManagerType=
                   'Service.AccessChecker, Service' />
     </behavior>
</behaviors>
```

用于运行基于声明的授权的类必须派生自只有一种重写方法（即 <xref:System.ServiceModel.ServiceAuthorizationManager>）的 `AccessCheck()`。 每当调用服务的操作并提供对象时，WCF 都将调用该方法 <xref:System.ServiceModel.OperationContext> ，该对象在其属性中具有用户的声明 `ServiceSecurityContext.AuthorizationContext` 。 WCF 从用户提供的用于身份验证的任何安全令牌收集有关用户的声明的工作，这会使评估这些声明是否足以满足相关操作的任务。

WCF 自动将来自任何类型的安全令牌的声明组合在一起，这是一种高度重要的创新，因为它会根据声明完全独立于身份验证机制来授权代码。 与此相反，使用 ACL 或 ASP.NET 中的角色进行授权则与 Windows 身份验证息息相关。

### <a name="security-confidentiality"></a>安全性：机密性

通过将 IIS 中的应用程序配置为使用安全超文本传输协议 (HTTPS)，可以在传输级别确保与 ASP.NET Web 服务交换的消息的机密性。 对于在 IIS 中承载的 WCF 应用程序也是如此。 但是，托管在 IIS 之外的 WCF 应用程序也可以配置为使用安全传输协议。 更重要的是，还可以使用 WS 安全协议将 WCF 应用程序配置为在传输消息之前对其进行保护。 如果使用 WS-Security 协议保护消息正文的安全，则消息正文到达最终目的地之前在中介中加密传输。

## <a name="globalization"></a>全球化

通过 ASP.NET 配置语言，您可以为这些服务单独指定区域性。 WCF 不支持 ASP.NET 兼容模式下的该配置设置。 若要本地化不使用 ASP.NET 兼容模式的 WCF 服务，请将服务类型编译为区域性特定的程序集，并为每个区域性特定的程序集提供单独的区域性特定的终结点。

## <a name="see-also"></a>另请参阅

- [基于目标和使用的标准比较 ASP.NET Web 服务与 WCF](comparing-aspnet-web-services-to-wcf-based-on-purpose-and-standards-used.md)
