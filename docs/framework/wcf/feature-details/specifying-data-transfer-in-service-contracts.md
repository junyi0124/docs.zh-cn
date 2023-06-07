---
title: 在服务协定中指定数据传输
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF], data transfer
ms.assetid: 7c5a26c8-89c9-4bcb-a4bc-7131e6d01f0c
ms.openlocfilehash: 30cdae7e15b80ca826eeea652566a535271477d9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96246389"
---
# <a name="specifying-data-transfer-in-service-contracts"></a>在服务协定中指定数据传输

可以将 Windows Communication Foundation (WCF) 视为一种消息传递基础结构。 服务操作可以接收消息、处理消息以及发送消息。 消息是使用操作协定描述的。 例如，请考虑以下协定。  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    float GetAirfare(string fromCity, string toCity);  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
  
    <OperationContract()>  
    Function GetAirfare(fromCity As String, toCity As String) As Double  
End Interface  
```  
  
 这里，`GetAirfare` 操作接受了一个包含有关 `fromCity` 和 `toCity` 的信息的消息，然后返回一个包含一个数字的消息。  
  
 本主题说明操作协定可用来描述消息的各种方式。  
  
## <a name="describing-messages-by-using-parameters"></a>使用参数描述消息  

 描述消息的最简单方式是使用参数列表和返回值。 在上面的示例中，`fromCity` 和 `toCity` 字符串参数用于描述请求消息，浮点型返回值用于描述答复消息。 如果返回值本身不足以描述答复消息，则可以使用 out 参数。 例如，下面的操作在其请求消息中包含 `fromCity` 和 `toCity`，在其答复消息中包含一个数字和一种货币。  
  
```csharp  
[OperationContract]  
float GetAirfare(string fromCity, string toCity, out string currency);  
```  
  
```vb  
<OperationContract()>  
    Function GetAirfare(fromCity As String, toCity As String) As Double  
```  
  
 此外，可以使用引用参数让某个参数同时成为请求消息和答复消息的一部分。 这些参数必须是可以序列化（转换为 XML）的类型。 默认情况下，WCF 使用称为类的组件 <xref:System.Runtime.Serialization.DataContractSerializer> 来执行此转换。 支持大多数基元类型（如 `int`、`string`、`float` 和 `DateTime`）。 用户定义的类型通常必须具有数据协定。 有关详细信息，请参阅 [使用数据协定](using-data-contracts.md)。  
  
```csharp
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    float GetAirfare(Itinerary itinerary, DateTime date);  
  
    [DataContract]  
    public class Itinerary  
    {  
        [DataMember]  
        public string fromCity;  
        [DataMember]  
        public string toCity;  
   }  
}  
```  
  
```vb  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    GetAirfare(itinerary as Itinerary, date as DateTime) as Double  
  
    <DataContract()>  
    Class Itinerary  
  
        <DataMember()>  
        Public fromCity As String  
        <DataMember()>  
        Public toCity As String  
    End Class  
End Interface  
```  
  
 偶尔，`DataContractSerializer` 不足以序列化您的类型。 WCF 支持替代序列化引擎，该引擎 <xref:System.Xml.Serialization.XmlSerializer> 还可用于序列化参数。 <xref:System.Xml.Serialization.XmlSerializer> 使您可以使用诸如 `XmlAttributeAttribute` 之类的属性对得到的 XML 进行更多控制。 若要切换到对特定操作或整个服务使用 <xref:System.Xml.Serialization.XmlSerializer>，请将 <xref:System.ServiceModel.XmlSerializerFormatAttribute> 属性应用到相应的操作或服务。 例如：  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    [XmlSerializerFormat]  
    float GetAirfare(Itinerary itinerary, DateTime date);  
}  
public class Itinerary  
{  
    public string fromCity;  
    public string toCity;  
    [XmlAttribute]  
    public bool isFirstClass;  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    <XmlSerializerFormat>  
    GetAirfare(itinerary as Itinerary, date as DateTime) as Double  
  
End Interface  
  
Class Itinerary  
  
    Public fromCity As String  
    Public toCity As String  
    <XmlSerializerFormat()>  
    Public isFirstClass As Boolean  
End Class  
```  
  
 有关详细信息，请参阅 [使用 XmlSerializer 类](using-the-xmlserializer-class.md)。 请记住，最好不要如此处所示手动切换到 <xref:System.Xml.Serialization.XmlSerializer>，除非有该主题详细讨论的特定原因需要执行此操作。  
  
 若要将 .NET 参数名称与协定名称分开，可以使用 <xref:System.ServiceModel.MessageParameterAttribute> 属性 (Attribute)，并且使用 `Name` 属性 (Property) 来设置协定名称。 例如，下面的操作协定等效于本主题中的第一个示例。  
  
```csharp  
[OperationContract]  
public float GetAirfare(  
    [MessageParameter(Name="fromCity")] string originCity,  
    [MessageParameter(Name="toCity")] string destinationCity);  
```  
  
```vb  
<OperationContract()>  
  Function GetAirfare(<MessageParameter(Name := "fromCity")> fromCity As String, <MessageParameter(Name := "toCity")> toCity As String) As Double  
```  
  
## <a name="describing-empty-messages"></a>描述空消息  

 可以通过不使用输入参数和引用参数来描述空请求消息。 例如，在 c # 中：  
  
 `[OperationContract]`  
  
 `public int GetCurrentTemperature();`  
  
 例如，在 Visual Basic 中：  
  
 `<OperationContract()>`  
  
 `Function GetCurrentTemperature() as Integer`  
  
 可以通过使用 `void` 返回类型并且不使用输出参数和引用参数来描述空答复消息。 例如，在：  
  
```csharp  
[OperationContract]  
public void SetTemperature(int temperature);  
```  
  
```vb  
<OperationContract()>  
Sub SetTemperature(temperature As Integer)  
```  
  
 这不同于单向操作，例如：  
  
```csharp  
[OperationContract(IsOneWay=true)]  
public void SetLightbulbStatus(bool isOn);  
```  
  
```vb  
<OperationContract(IsOneWay:=True)>  
Sub SetLightbulbStatus(isOne As Boolean)  
```  
  
 `SetTemperatureStatus` 操作返回一个空消息。 如果在处理输入消息时出现问题，它可能改而返回错误。 `SetLightbulbStatus` 操作不返回任何内容。 无法在此操作中传递错误条件。  
  
## <a name="describing-messages-by-using-message-contracts"></a>使用消息协定描述消息  

 您可能希望使用单个类型来表示整个消息。 虽然可将数据协定用于此目的，但建议使用消息协定来执行此操作，这可以避免在得到的 XML 中采用不必要的包装级别。 此外，使用消息协定可以对得到的消息进行更多的控制。 例如，您可以决定哪些信息段应包含在消息正文中，哪些信息段应包含在消息头中。 下面的示例演示消息协定的用法。  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    GetAirfareResponse GetAirfare(GetAirfareRequest request);  
}  
  
[MessageContract]  
public class GetAirfareRequest  
{  
    [MessageHeader] public DateTime date;  
    [MessageBodyMember] public Itinerary itinerary;  
}  
  
[MessageContract]  
public class GetAirfareResponse  
{  
    [MessageBodyMember] public float airfare;  
    [MessageBodyMember] public string currency;  
}  
  
[DataContract]  
public class Itinerary  
{  
    [DataMember] public string fromCity;  
    [DataMember] public string toCity;  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    Function GetAirfare(request As GetAirfareRequest) As GetAirfareResponse  
End Interface  
  
<MessageContract()>  
Public Class GetAirfareRequest  
    <MessageHeader()>
    Public Property date as DateTime  
    <MessageBodyMember()>  
    Public Property itinerary As Itinerary  
End Class  
  
<MessageContract()>  
Public Class GetAirfareResponse  
    <MessageBodyMember()>  
    Public Property airfare As Double  
    <MessageBodyMember()> Public Property currency As String  
End Class  
  
<DataContract()>  
Public Class Itinerary  
    <DataMember()> Public Property fromCity As String  
    <DataMember()> Public Property toCity As String  
End Class  
```  
  
 有关详细信息，请参阅 [使用消息协定](using-message-contracts.md)。  
  
 在上面的示例中，默认情况下仍然使用 <xref:System.Runtime.Serialization.DataContractSerializer> 类。 <xref:System.Xml.Serialization.XmlSerializer> 类也可用于消息协定。 为此，请将 <xref:System.ServiceModel.XmlSerializerFormatAttribute> 属性应用于操作或协定，并在消息头和正文成员中使用与 <xref:System.Xml.Serialization.XmlSerializer> 类兼容的类型。  
  
## <a name="describing-messages-by-using-streams"></a>使用流描述消息  

 另一种在操作中描述消息的方式是在操作协定中使用 <xref:System.IO.Stream> 类或它的派生类之一，或者将该类用作消息协定正文成员（此时，它必须是唯一的成员）。 对于传入消息，类型必须是 `Stream` — 您不能使用派生类。  
  
 WCF 不会调用序列化程序，而是检索流中的数据，并将数据直接放入传出消息，或从传入消息中检索数据并将数据直接放入流中。 下面的示例演示流的用法。  
  
```csharp  
[OperationContract]  
public Stream DownloadFile(string fileName);  
```  
  
```vb  
<OperationContract()>  
Function DownloadFile(fileName As String) As String  
```  
  
 不能将 `Stream` 和非流数据一起放入单个消息正文中。 使用消息协定可将额外的数据放入消息头中。 下面的示例演示在定义操作协定时不正确的流用法。  
  
```csharp  
//Incorrect:  
// [OperationContract]  
// public void UploadFile (string fileName, Stream fileData);  
```  
  
```vb  
'Incorrect:  
    '<OperationContract()>  
    Public Sub UploadFile(fileName As String, fileData As StreamingContext)  
```  
  
 下面的示例演示在定义操作协定时正确的流用法。  
  
```csharp  
[OperationContract]  
public void UploadFile (UploadFileMessage message);  
//code omitted  
[MessageContract]  
public class UploadFileMessage  
{  
    [MessageHeader] public string fileName;  
    [MessageBodyMember] public Stream fileData;  
}  
```  
  
```vb  
<OperationContract()>  
Public Sub UploadFile(fileName As String, fileData As StreamingContext)  
'Code Omitted  
<MessageContract()>  
Public Class UploadFileMessage  
   <MessageHeader()>  
    Public Property fileName As String  
    <MessageBodyMember()>  
    Public Property fileData As Stream  
End Class  
```  
  
 有关详细信息，请参阅 [大数据和流式处理](large-data-and-streaming.md)。  
  
## <a name="using-the-message-class"></a>使用 Message 类  

 若要对发送或接收的消息具有完全编程控制，可以直接使用 <xref:System.ServiceModel.Channels.Message> 类，如下面的示例代码所示。  
  
```csharp  
[OperationContract]  
public void LogMessage(Message m);  
```  
  
```vb  
<OperationContract()>  
Sub LogMessage(m As Message)  
```  
  
 这是一个高级方案，在 [使用 Message 类](using-the-message-class.md)中进行了详细说明。  
  
## <a name="describing-fault-messages"></a>描述错误消息  

 除了由返回值以及输出参数或引用参数描述的消息以外，任何非单向操作都至少可以返回两种可能的消息：它的正常响应消息和错误消息。 请考虑下面的操作协定。  
  
```csharp  
[OperationContract]  
float GetAirfare(string fromCity, string toCity, DateTime date);  
```  
  
```vb  
<OperationContract()>  
Function GetAirfare(fromCity As String, toCity As String, date as DateTime)  
```  
  
 此操作可能返回包含一个 `float` 数的正常消息，也可能返回包含一个错误代码和说明的错误消息。 这可以通过在服务实现中引发 <xref:System.ServiceModel.FaultException> 来实现。  
  
 使用 <xref:System.ServiceModel.FaultContractAttribute> 属性可以指定其他可能的错误消息。 其他错误必须可以使用 <xref:System.Runtime.Serialization.DataContractSerializer> 进行序列化，如下面的示例代码所示。  
  
```csharp  
[OperationContract]  
[FaultContract(typeof(ItineraryNotAvailableFault))]  
float GetAirfare(string fromCity, string toCity, DateTime date);  
  
//code omitted  
  
[DataContract]  
public class ItineraryNotAvailableFault  
{  
    [DataMember]  
    public bool IsAlternativeDateAvailable;  
  
    [DataMember]  
    public DateTime alternativeSuggestedDate;  
}  
```  
  
```vb  
<OperationContract()>  
<FaultContract(GetType(ItineraryNotAvailableFault))>  
Function GetAirfare(fromCity As String, toCity As String, date as DateTime) As Double  
  
'Code Omitted  
<DataContract()>  
Public Class  
  <DataMember()>  
  Public Property IsAlternativeDateAvailable As Boolean  
  <DataMember()>  
  Public Property alternativeSuggestedDate As DateTime  
End Class  
```  
  
 可以通过引发相应数据协定类型的 <xref:System.ServiceModel.FaultException%601> 来生成这些其他错误。 有关详细信息，请参阅 [处理异常和错误](../extending/handling-exceptions-and-faults.md)。  
  
 不能使用 <xref:System.Xml.Serialization.XmlSerializer> 类描述错误。 <xref:System.ServiceModel.XmlSerializerFormatAttribute> 对错误协定无效。  
  
## <a name="using-derived-types"></a>使用派生类型  

 您可能希望在操作或消息协定中使用基类型，然后在实际调用操作时使用派生类型。 在这种情况下，必须使用 <xref:System.ServiceModel.ServiceKnownTypeAttribute> 属性或某种替代机制，以允许使用派生类型。 请考虑下面的操作。  
  
```csharp  
[OperationContract]  
public bool IsLibraryItemAvailable(LibraryItem item);  
```  
  
```vb
<OperationContract()>  
    Function IsLibraryItemAvailable(item As LibraryItem) As Boolean  
```  
  
 假定 `Book` 和 `Magazine` 这两个类型派生自 `LibraryItem`。 若要在 `IsLibraryItemAvailable` 操作中使用这些类型，可以按如下方式更改操作：  
  
 `[OperationContract]`  
  
 `[ServiceKnownType(typeof(Book))]`  
  
 `[ServiceKnownType(typeof(Magazine))]`  
  
 `public bool IsLibraryItemAvailable(LibraryItem item);`  
  
 或者，当使用默认 <xref:System.Runtime.Serialization.KnownTypeAttribute> 时，也可以使用 <xref:System.Runtime.Serialization.DataContractSerializer> 属性，如下面的示例代码所示。  
  
```csharp  
[OperationContract]  
public bool IsLibraryItemAvailable(LibraryItem item);  
  
// code omitted
  
[DataContract]  
[KnownType(typeof(Book))]  
[KnownType(typeof(Magazine))]  
public class LibraryItem  
{  
    //code omitted  
}  
```  
  
```vb  
<OperationContract()>  
Function IsLibraryItemAvailable(item As LibraryItem) As Boolean  
  
'Code Omitted  
<DataContract()>  
<KnownType(GetType(Book))>  
<KnownType(GetType(Magazine))>  
Public Class LibraryItem  
  'Code Omitted  
End Class  
```  
  
 使用 <xref:System.Xml.Serialization.XmlIncludeAttribute> 时，可以使用 <xref:System.Xml.Serialization.XmlSerializer> 属性。  
  
 可以将 <xref:System.ServiceModel.ServiceKnownTypeAttribute> 属性应用于某个操作或整个服务。 它接受一个类型或要调用的方法的名称，以获取已知类型的列表，就像 <xref:System.Runtime.Serialization.KnownTypeAttribute> 属性一样。 有关详细信息，请参阅 [数据协定已知类型](data-contract-known-types.md)。  
  
## <a name="specifying-the-use-and-style"></a>指定用法和样式  

 在使用 Web Services 描述语言 (WSDL) 描述服务时，两种常用的样式是文档和远程过程调用 (RPC)。 在文档样式中，使用架构来描述整个消息正文，并且 WSDL 通过引用该架构内的元素来描述各种消息正文部分。 在 RPC 样式中，WSDL 引用每个消息部分的架构类型而不是元素来描述消息正文部分。 在某些情况下，您必须手动选择其中的一种样式。 若要执行此操作，可以应用 <xref:System.ServiceModel.DataContractFormatAttribute> 属性并设置 `Style` 属性 (Property)（使用 <xref:System.Runtime.Serialization.DataContractSerializer> 时），或者设置 `Style` 属性 (Attribute) 上的 <xref:System.ServiceModel.XmlSerializerFormatAttribute>（使用 <xref:System.Xml.Serialization.XmlSerializer> 时）。  
  
 此外，<xref:System.Xml.Serialization.XmlSerializer> 还支持两种形式的序列化 XML：`Literal` 和 `Encoded`。 `Literal` 是最广为接受的格式，也是 <xref:System.Runtime.Serialization.DataContractSerializer> 唯一支持的格式。 `Encoded` 是 SOAP 规范第 5 节中描述的旧格式，建议不要用于新服务。 若要切换到 `Encoded` 模式，请将 `Use` 属性 (Attribute) 上的 <xref:System.ServiceModel.XmlSerializerFormatAttribute> 属性 (Property) 设置为 `Encoded`。  
  
 在大多数情况下，不应更改 `Style` 和 `Use` 属性的默认设置。  
  
## <a name="controlling-the-serialization-process"></a>控制序列化过程  

 可以执行许多操作来自定义序列化数据的方式。  
  
### <a name="changing-server-serialization-settings"></a>更改服务器序列化设置  

 当使用默认 <xref:System.Runtime.Serialization.DataContractSerializer> 时，可以通过将 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性应用于服务，控制服务上的序列化过程的某些方面。 具体说来，您可以使用 `MaxItemsInObjectGraph` 属性设置相应的配额，以便对 <xref:System.Runtime.Serialization.DataContractSerializer> 反序列化的最大对象数进行限制。 可以使用 `IgnoreExtensionDataObject` 属性关闭往返过程版本管理功能。 有关配额的详细信息，请参阅[数据的安全注意事项](security-considerations-for-data.md)。 有关往返的详细信息，请参阅 [向前兼容的数据协定](forward-compatible-data-contracts.md)。  
  
```csharp  
[ServiceBehavior(MaxItemsInObjectGraph=100000)]  
public class MyDataService:IDataService  
{  
    public DataPoint[] GetData()  
    {  
       // Implementation omitted  
    }  
}  
```  
  
```vb  
<ServiceBehavior(MaxItemsInObjectGraph:=100000)>  
Public Class MyDataService Implements IDataService  
  
    Function GetData() As DataPoint()  
         ‘ Implementation omitted  
    End Function  
End Interface  
```  
  
### <a name="serialization-behaviors"></a>序列化行为  

 WCF 中提供了两种行为， <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 并根据 <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> 要用于特定操作的序列化程序自动插入。 因为这些行为是自动应用的，您通常不必了解它们。  
  
 但是，`DataContractSerializerOperationBehavior` 具有可用于自定义序列化过程的 `MaxItemsInObjectGraph`、`IgnoreExtensionDataObject` 和 `DataContractSurrogate` 属性。 前两个属性的含义与上一节中讨论的相同。 您可以使用 `DataContractSurrogate` 属性来启用数据协定代理项，这是一种用于自定义和扩展序列化过程的强大机制。 有关详细信息，请参阅 [数据协定代理](../extending/data-contract-surrogates.md)项。  
  
 可以使用 `DataContractSerializerOperationBehavior` 来自定义客户端和服务器序列化。 下面的示例演示如何增加客户端上的 `MaxItemsInObjectGraph` 配额。  
  
```csharp  
ChannelFactory<IDataService> factory = new ChannelFactory<IDataService>(binding, address);  
foreach (OperationDescription op in factory.Endpoint.Contract.Operations)  
{  
    DataContractSerializerOperationBehavior dataContractBehavior =  
                op.Behaviors.Find<DataContractSerializerOperationBehavior>()  
                as DataContractSerializerOperationBehavior;  
    if (dataContractBehavior != null)  
    {  
        dataContractBehavior.MaxItemsInObjectGraph = 100000;  
    }  
}  
IDataService client = factory.CreateChannel();  
```  
  
```vb  
Dim factory As ChannelFactory(Of IDataService) = New ChannelFactory(Of IDataService)(binding, address)  
For Each op As OperationDescription In factory.Endpoint.Contract.Operations  
        Dim dataContractBehavior As DataContractSerializerOperationBehavior = op.Behaviors.Find(Of DataContractSerializerOperationBehavior)()  
        If dataContractBehavior IsNot Nothing Then  
            dataContractBehavior.MaxItemsInObjectGraph = 100000  
        End If  
     Next  
    Dim client As IDataService = factory.CreateChannel  
```  
  
下面是自承载的情况下服务的等效代码：
  
```csharp  
ServiceHost serviceHost = new ServiceHost(typeof(IDataService))  
foreach (ServiceEndpoint ep in serviceHost.Description.Endpoints)  
{  
foreach (OperationDescription op in ep.Contract.Operations)  
{  
        DataContractSerializerOperationBehavior dataContractBehavior =  
           op.Behaviors.Find<DataContractSerializerOperationBehavior>()  
                as DataContractSerializerOperationBehavior;  
        if (dataContractBehavior != null)  
        {  
            dataContractBehavior.MaxItemsInObjectGraph = 100000;  
        }  
}  
}  
serviceHost.Open();  
```  
  
```vb  
Dim serviceHost As ServiceHost = New ServiceHost(GetType(IDataService))  
        For Each ep As ServiceEndpoint In serviceHost.Description.Endpoints  
            For Each op As OperationDescription In ep.Contract.Operations  
                Dim dataContractBehavior As DataContractSerializerOperationBehavior = op.Behaviors.Find(Of DataContractSerializerOperationBehavior)()  
  
                If dataContractBehavior IsNot Nothing Then  
                    dataContractBehavior.MaxItemsInObjectGraph = 100000  
                End If  
            Next  
        Next  
        serviceHost.Open()  
```  
  
 在以 Web 为宿主的情况下，必须创建一个新的 `ServiceHost` 派生类，并且使用服务主机工厂来插入该类。  
  
### <a name="controlling-serialization-settings-in-configuration"></a>在配置中控制序列化设置  

 使用 `MaxItemsInObjectGraph` 终结点或服务行为，可以通过配置来控制 `IgnoreExtensionDataObject` 和 `dataContractSerializer`，如下面的示例所示。  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name="LargeQuotaBehavior">  
                    <dataContractSerializer  
                      maxItemsInObjectGraph="100000" />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
        <client>  
            <endpoint address="http://example.com/myservice"  
                  behaviorConfiguration="LargeQuotaBehavior"  
                binding="basicHttpBinding" bindingConfiguration=""
                            contract="IDataService"  
                name="" />  
        </client>  
    </system.serviceModel>  
</configuration>  
```  
  
### <a name="shared-type-serialization-object-graph-preservation-and-custom-serializers"></a>共享类型序列化、对象图保留和自定义序列化程序  

 <xref:System.Runtime.Serialization.DataContractSerializer> 使用数据协定名称而不是 .NET 类型名称序列化。 这与面向服务的体系结构原则是一致的，并且支持更大程度的灵活性 — .NET 类型可以在不影响网络协定的情况下进行更改。 在极少数情况下，您可能希望序列化实际的 .NET 类型名称，从而在客户端和服务器之间引入紧耦合，就像 .NET Framework 远程处理技术那样。 这不是建议的做法，但在从 .NET Framework 远程处理迁移到 WCF 的罕见情况下除外。 在这种情况下，必须使用 <xref:System.Runtime.Serialization.NetDataContractSerializer> 类而不是 <xref:System.Runtime.Serialization.DataContractSerializer> 类。  
  
 <xref:System.Runtime.Serialization.DataContractSerializer> 通常按对象树形式序列化对象图。 即，如果多次引用同一个对象，则会将该对象序列化多次。 例如，考虑一个 `PurchaseOrder` 实例，它有两个名为 `billTo` 和 `shipTo` 的 Address 类型的字段。 如果这两个字段设置为同一个 Address 实例，那么在序列化和反序列化后将有两个相同的 Address 实例。 这样做的原因是 XML 中没有用于表示对象图的标准且可互操作的方式（<xref:System.Xml.Serialization.XmlSerializer> 上可用的旧 SOAP 编码标准除外，如上一节中关于 `Style` 和 `Use` 的介绍中所述）。 按对象树形式序列化对象图具有某些缺点，例如：无法序列化含有循环引用的图。 有时，需要切换到真正的对象图序列化，即使它不是可互操作的。 这可以通过使用在将 <xref:System.Runtime.Serialization.DataContractSerializer> 参数设置为 `preserveObjectReferences` 的情况下构造的 `true` 来完成。  
  
 有时，内置序列化程序不足以满足方案的需要。 在大多数情况下，仍可使用用来派生 <xref:System.Runtime.Serialization.XmlObjectSerializer> 和 <xref:System.Runtime.Serialization.DataContractSerializer> 的 <xref:System.Runtime.Serialization.NetDataContractSerializer> 抽象类。  
  
 前面的三种情况（.NET 类型保留、对象图保留和完全基于 `XmlObjectSerializer` 的自定义序列化）都需要插入一个自定义序列化程序。 为此，请执行下列步骤：  
  
1. 编写自己的派生自 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 的行为。  
  
2. 重写两个 `CreateSerializer` 方法以返回自己的序列化程序（<xref:System.Runtime.Serialization.NetDataContractSerializer>、将 <xref:System.Runtime.Serialization.DataContractSerializer> 设置为 `preserveObjectReferences` 的 `true` 或自己的自定义 <xref:System.Runtime.Serialization.XmlObjectSerializer>）。  
  
3. 在打开服务主机或创建客户端通道之前，移除现有的 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 行为并插入在前面的步骤中创建的自定义派生类。  
  
 有关高级序列化概念的详细信息，请参阅 [序列化和反序列](serialization-and-deserialization.md)化。  
  
## <a name="see-also"></a>另请参阅

- [使用 XmlSerializer 类](using-the-xmlserializer-class.md)
- [如何：启用流处理](how-to-enable-streaming.md)
- [如何：创建类或结构的基本数据协定](how-to-create-a-basic-data-contract-for-a-class-or-structure.md)
