---
title: 消息检查器
description: 了解如何实现和配置 WCF 客户端和服务消息检查器，这些检查器提供消息验证机制。
ms.date: 03/30/2017
ms.assetid: 9bd1f305-ad03-4dd7-971f-fa1014b97c9b
ms.openlocfilehash: 4b2f7b97d0895e3cb7550217f64a2b0b14545abf
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240701"
---
# <a name="message-inspectors"></a>消息检查器

本示例演示如何实现和配置客户端和服务消息检查器。  
  
 消息检查器是一个扩展性对象，可以编程方式或通过配置用于服务模型的客户端运行时和调度运行时，并可以在接收消息之后或发送消息之前检查和更改消息。  
  
 本示例实现基本客户端和服务消息验证机制，该机制基于一组可配置的 XML 架构文档验证传入消息。 请注意，本示例并不验证每个操作的消息。 这是一种有意的简化。  
  
## <a name="message-inspector"></a>消息检查器  

 客户端消息检查器实现 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector> 接口，而服务消息检查器实现 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector> 接口。 可以将这两种实现合并到单个类中以形成适用于两端的消息检查器。 本示例实现这样的合并消息检查器。 该检查器通过传入一组架构来创建，基于这组架构验证传入和传出消息，并允许开发人员指定是否验证传入或传出消息以及检查器是处于调度模式还是客户端模式，所处的模式将影响错误的处理，如本主题后面的论述。  
  
```csharp
public class SchemaValidationMessageInspector : IClientMessageInspector, IDispatchMessageInspector  
{  
    XmlSchemaSet schemaSet;  
    bool validateRequest;  
    bool validateReply;  
    bool isClientSide;  
    [ThreadStatic]  
    bool isRequest;  
  
    public SchemaValidationMessageInspector(XmlSchemaSet schemaSet,  
         bool validateRequest, bool validateReply, bool isClientSide)  
    {  
        this.schemaSet = schemaSet;  
        this.validateReply = validateReply;  
        this.validateRequest = validateRequest;  
        this.isClientSide = isClientSide;  
    }  
```  
  
 任何服务（调度程序）消息检查器都必须实现两个 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector> 方法：<xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> 和 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%28System.ServiceModel.Channels.Message%40%2CSystem.Object%29>。  
  
 当通道堆栈接收到消息、处理消息并将消息分配给服务后且在将消息反序列化并调度到操作之前，调度程序将调用 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A>。 如果传入消息是加密的消息，则在消息到达消息检查器之时，已经对消息解密。 该方法获取作为引用参数进行传递的 `request` 消息，这允许根据需要检查、操作或替换消息。 返回值可以是任何对象，并可用作服务对当前消息返回答复时传递给 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%2A> 的相关状态对象。 在本示例中，<xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> 将消息的检查（验证）委托给本地私有方法 `ValidateMessageBody` 而不返回相关状态对象。 此方法可确保不会将无效的消息传入服务。  
  
```csharp  
object IDispatchMessageInspector.AfterReceiveRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel, System.ServiceModel.InstanceContext instanceContext)  
{  
    if (validateRequest)  
    {  
        // inspect the message. If a validation error occurs,  
        // the thrown fault exception bubbles up.  
        ValidateMessageBody(ref request, true);  
    }  
    return null;  
}  
```  
  
 每当准备好要发回客户端的答复时，或在单向消息的情况下已处理完传入消息时，均会调用 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%28System.ServiceModel.Channels.Message%40%2CSystem.Object%29>。 这允许对称地调用所需的扩展，而不管 MEP 如何。 与 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> 一样，消息作为引用参数进行传递，并可以进行检查、修改或替换。 在本示例中执行的消息验证也委托给 `ValidMessageBody` 方法，但本例中处理验证错误的方式略有不同。  
  
 如果在服务上发生验证错误，则 `ValidateMessageBody` 方法将引发从 <xref:System.ServiceModel.FaultException> 派生的异常。 在 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> 中，可以将这些异常放入服务模型基础结构中，异常在基础结构中将自动转换为 SOAP 错误并中继到客户端。 在 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%2A> 中，不能将 <xref:System.ServiceModel.FaultException> 异常放入基础结构中，因为由服务引发的错误异常的转换将在调用消息检查器之前发生。 因此，下面的实现捕捉已知的 `ReplyValidationFault` 异常并用显式错误消息替换答复消息。 此方法可确保服务实现不会返回无效的消息。  
  
```csharp  
void IDispatchMessageInspector.BeforeSendReply(ref System.ServiceModel.Channels.Message reply, object correlationState)  
{  
    if (validateReply)  
    {  
        // Inspect the reply, catch a possible validation error
        try  
        {  
            ValidateMessageBody(ref reply, false);  
        }  
        catch (ReplyValidationFault fault)  
        {  
            // if a validation error occurred, the message is replaced  
            // with the validation fault.  
            reply = Message.CreateMessage(reply.Version,
                    fault.CreateMessageFault(), reply.Headers.Action);  
        }  
    }  
```  
  
 客户端消息检查器非常相似。 必须从 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector> 实现的两个方法是 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.AfterReceiveReply%2A> 和 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A>。  
  
 客户端应用程序或操作格式化程序在撰写消息后将调用 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A>。 与调度程序消息检查器一样，可以只检查消息，也可以完全替换消息。 在本示例中，检查器向同一本地 `ValidateMessageBody` 帮助器方法进行委托，该方法也可用于调度消息检查器。  
  
 客户端验证和服务验证（在构造函数中指定）之间的行为差异是客户端验证引发放在用户代码中的本地异常，因为这些异常发生在本地而不是由于服务失败造成的。 一般规则是服务调度程序检查器引发错误，而客户端检查器引发异常。  
  
 此 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A> 实现可确保不会向服务发送无效的消息。  
  
```csharp  
object IClientMessageInspector.BeforeSendRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel)  
{  
    if (validateRequest)  
    {  
        ValidateMessageBody(ref request, true);  
    }  
    return null;  
}  
```  
  
 `AfterReceiveReply` 实现可确保不会将从服务接收的无效消息中继到客户端用户代码中。  
  
```csharp  
void IClientMessageInspector.AfterReceiveReply(ref System.ServiceModel.Channels.Message reply, object correlationState)  
{  
    if (validateReply)  
    {  
        ValidateMessageBody(ref reply, false);  
    }  
}  
```  
  
 此特定消息检查器的核心是 `ValidateMessageBody` 方法。 为了执行工作，检查器会在所传递消息的正文内容子树周围包装一个验证 <xref:System.Xml.XmlReader>。 将用消息检查器包含的一组架构填充该读取器，并会将验证回调设置为引用与此方法并排定义的 `InspectionValidationHandler` 的委托。 为了执行验证，将随后读取消息并后台处理到受内存流支持的 <xref:System.Xml.XmlDictionaryWriter> 中。 如果在进程中发生验证错误或警告，则调用回调方法。  
  
 如果不发生错误，则构造新消息，该消息复制原始消息中的属性和标头并使用内存流中经过验证的信息集，该信息集由一个 <xref:System.Xml.XmlDictionaryReader> 包装并添加到替换消息中。  
  
```csharp  
void ValidateMessageBody(ref System.ServiceModel.Channels.Message message, bool isRequest)  
{  
    if (!message.IsFault)  
    {  
        XmlDictionaryReaderQuotas quotas =
                new XmlDictionaryReaderQuotas();  
        XmlReader bodyReader =
            message.GetReaderAtBodyContents().ReadSubtree();  
        XmlReaderSettings wrapperSettings =
                              new XmlReaderSettings();  
        wrapperSettings.CloseInput = true;  
        wrapperSettings.Schemas = schemaSet;  
        wrapperSettings.ValidationFlags =
                                XmlSchemaValidationFlags.None;  
        wrapperSettings.ValidationType = ValidationType.Schema;  
        wrapperSettings.ValidationEventHandler += new
           ValidationEventHandler(InspectionValidationHandler);  
        XmlReader wrappedReader = XmlReader.Create(bodyReader,
                                            wrapperSettings);  
  
        // pull body into a memory backed writer to validate  
        this.isRequest = isRequest;  
        MemoryStream memStream = new MemoryStream();  
        XmlDictionaryWriter xdw =  
              XmlDictionaryWriter.CreateBinaryWriter(memStream);  
        xdw.WriteNode(wrappedReader, false);  
        xdw.Flush(); memStream.Position = 0;  
        XmlDictionaryReader xdr =
        XmlDictionaryReader.CreateBinaryReader(memStream, quotas);  
  
        // reconstruct the message with the validated body  
        Message replacedMessage =
            Message.CreateMessage(message.Version, null, xdr);  
        replacedMessage.Headers.CopyHeadersFrom(message.Headers);  
        replacedMessage.Properties.CopyProperties(message.Properties);  
        message = replacedMessage;  
    }  
}  
```  
  
 每当发生架构验证错误或警告时，验证 `InspectionValidationHandler` 就会调用 <xref:System.Xml.XmlReader> 方法。 下面的实现仅处理错误，而忽略所有警告。  
  
 首先需要考虑的是，有可能使用消息检查器将验证 <xref:System.Xml.XmlReader> 插入到消息中并在处理消息时执行验证且不对消息进行缓冲处理。 但这意味着在检测到无效 XML 节点时，此回调将在服务模型基础结构或用户代码中的某处引发验证异常，从而导致无法预料的行为。 缓存方法可彻底防止用户代码中出现无效消息。  
  
 如前面所述，由处理程序引发的异常在客户端和服务之间有所不同。 在服务上，异常派生自 <xref:System.ServiceModel.FaultException>，而在客户端，异常是常规自定义异常。  
  
```csharp  
        void InspectionValidationHandler(object sender, ValidationEventArgs e)  
{  
    if (e.Severity == XmlSeverityType.Error)  
    {  
        // We are treating client and service side validation errors  
        // differently here. Client side errors cause exceptions  
        // and are thrown straight up to the user code. Service side  
        // validations cause faults.  
        if (isClientSide)  
        {  
            if (isRequest)  
            {  
                throw new RequestClientValidationException(e.Message);  
            }  
            else  
            {  
                throw new ReplyClientValidationException(e.Message);  
            }  
        }  
        else  
        {  
            if (isRequest)  
            {  
                // this fault is caught by the ServiceModel
                // infrastructure and turned into a fault reply.  
                throw new RequestValidationFault(e.Message);  
             }  
             else  
             {  
                // this fault is caught and turned into a fault message  
                // in BeforeSendReply in this class  
                throw new ReplyValidationFault(e.Message);  
              }  
          }  
      }  
    }  
```  
  
## <a name="behavior"></a>行为  

 消息检查器是客户端运行时或调度运行时的扩展。 此类扩展使用 *行为* 进行配置。 行为是通过更改默认配置或添加扩展（如消息检查器）来更改服务模型运行时行为的类。  
  
 下面的 `SchemaValidationBehavior` 类是用于将本示例的消息检查器添加到客户端运行时或调度运行时的行为。 该实现在这两种情况下都是相当基础的行为。 在 <xref:System.ServiceModel.Description.IEndpointBehavior.ApplyClientBehavior%2A> 和 <xref:System.ServiceModel.Description.IEndpointBehavior.ApplyDispatchBehavior%2A> 中，将创建消息检查器并将其添加到各自运行时的 <xref:System.ServiceModel.Dispatcher.ClientRuntime.MessageInspectors%2A> 集合中。  
  
```csharp
public class SchemaValidationBehavior : IEndpointBehavior  
{  
    XmlSchemaSet schemaSet;
    bool validateRequest;
    bool validateReply;  
  
    public SchemaValidationBehavior(XmlSchemaSet schemaSet, bool
                           inspectRequest, bool inspectReply)  
    {  
        this.schemaSet = schemaSet;  
        this.validateReply = inspectReply;  
        this.validateRequest = inspectRequest;  
    }  
    #region IEndpointBehavior Members  
  
    public void AddBindingParameters(ServiceEndpoint endpoint,
       System.ServiceModel.Channels.BindingParameterCollection
                                            bindingParameters)  
    {  
    }  
  
    public void ApplyClientBehavior(ServiceEndpoint endpoint,
            System.ServiceModel.Dispatcher.ClientRuntime clientRuntime)  
    {  
        SchemaValidationMessageInspector inspector =
           new SchemaValidationMessageInspector(schemaSet,
                      validateRequest, validateReply, true);  
            clientRuntime.MessageInspectors.Add(inspector);  
    }  
  
    public void ApplyDispatchBehavior(ServiceEndpoint endpoint,
         System.ServiceModel.Dispatcher.EndpointDispatcher
                                          endpointDispatcher)  
    {  
        SchemaValidationMessageInspector inspector =
           new SchemaValidationMessageInspector(schemaSet,
                        validateRequest, validateReply, false);  
   endpointDispatcher.DispatchRuntime.MessageInspectors.Add(inspector);  
    }  
  
   public void Validate(ServiceEndpoint endpoint)  
   {  
   }  
  
    #endregion  
}  
```  
  
> [!NOTE]
> 此特定行为不能复制为属性，因此不能以声明性方式添加到服务类型的协定类型上。 这是设计使然，由于架构集合不能加载到属性声明中，因此引用此属性中的其他配置位置（例如引用应用程序设置）意味着会创建与服务模型配置的其余元素不一致的配置元素。 因此，只能通过代码和通过服务模型配置扩展强制添加此行为。  
  
## <a name="adding-the-message-inspector-through-configuration"></a>通过配置添加消息检查器  

 若要在应用程序配置文件中的终结点上配置自定义行为，服务模型要求实现者创建由派生自的类表示的配置 *扩展元素* <xref:System.ServiceModel.Configuration.BehaviorExtensionElement> 。 然后，必须将此扩展添加到服务模型的配置节作为扩展，如本节讨论的以下扩展所示。  
  
```xml  
<system.serviceModel>  
…  
   <extensions>  
      <behaviorExtensions>  
        <add name="schemaValidator" type="Microsoft.ServiceModel.Samples.SchemaValidationBehaviorExtensionElement, MessageInspectors, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>  
      </behaviorExtensions>  
    </extensions>  
…  
</system.serviceModel>  
```  
  
 可以将扩展添加到应用程序或 ASP.NET 配置文件中，这是最常见的选择，也可以添加到计算机配置文件中。  
  
 在将扩展添加到配置范围时，可以将行为添加到行为配置中，如下面的代码所示。 行为配置是可以重复使用的元素，可以根据需要应用于多个终结点。 由于此处要配置的特定行为可实现 <xref:System.ServiceModel.Description.IEndpointBehavior>，因此该行为仅在配置文件的相应配置节中有效。  
  
```xml  
<system.serviceModel>  
   <behaviors>  
      …  
     <endpointBehaviors>  
        <behavior name="HelloServiceEndpointBehavior">  
          <schemaValidator validateRequest="True" validateReply="True">  
            <schemas>  
              <add location="messages.xsd" />
            </schemas>  
          </schemaValidator>  
        </behavior>  
      </endpointBehaviors>  
      …  
    </behaviors>  
</system.serviceModel>  
```  
  
 用于配置消息检查器的 `<schemaValidator>` 元素由 `SchemaValidationBehaviorExtensionElement` 类提供支持。 此类公开两个名为 `ValidateRequest` 和 `ValidateReply` 的布尔型公共属性。 这两个属性均使用 <xref:System.Configuration.ConfigurationPropertyAttribute> 进行标记。 此属性 (attribute) 构成代码属性 (property) 和 XML 属性 (attribute) 之间的链接，这可以在前面的 XML 配置元素上看到。 此类还有一个额外使用 `Schemas` 进行标记的 <xref:System.Configuration.ConfigurationCollectionAttribute> 属性，其类型为 `SchemaCollection`，此属性也是本示例的组成部分，但为了简便起见，本文档中省略了此属性。 此属性以及集合和集合元素类 `SchemaConfigElement` 支持前面配置代码段中的 `<schemas>` 元素，并允许将架构集合添加到验证集中。  
  
 重写的 `CreateBehavior` 方法会在运行时为生成客户端或终结点而计算配置数据时将配置数据转换为行为对象。  
  
```csharp
public class SchemaValidationBehaviorExtensionElement : BehaviorExtensionElement  
{  
    public SchemaValidationBehaviorExtensionElement()  
    {  
    }  
  
    public override Type BehaviorType
    {
        get  
        {  
            return typeof(SchemaValidationBehavior);  
        }
    }  
  
    protected override object CreateBehavior()  
    {  
        XmlSchemaSet schemaSet = new XmlSchemaSet();  
        foreach (SchemaConfigElement schemaCfg in this.Schemas)  
        {  
            Uri baseSchema = new
                Uri(AppDomain.CurrentDomain.BaseDirectory);  
            string location = new
                Uri(baseSchema,schemaCfg.Location).ToString();  
            XmlSchema schema =
                XmlSchema.Read(new XmlTextReader(location), null);  
            schemaSet.Add(schema);  
        }  
     return new
     SchemaValidationBehavior(schemaSet,ValidateRequest,ValidateReply);  
    }  
  
[ConfigurationProperty("validateRequest",DefaultValue=false,IsRequired=false)]  
public bool ValidateRequest  
{  
    get { return (bool)base["validateRequest"]; }  
    set { base["validateRequest"] = value; }  
}  
  
[ConfigurationProperty("validateReply", DefaultValue = false, IsRequired = false)]  
        public bool ValidateReply  
        {  
            get { return (bool)base["validateReply"]; }  
            set { base["validateReply"] = value; }  
        }  
  
     //Declare the Schema collection property.  
     //Note: the "IsDefaultCollection = false" instructs
     //.NET Framework to build a nested section of
     //the kind <Schema> ...</Schema>.  
    [ConfigurationProperty("schemas", IsDefaultCollection = true)]  
    [ConfigurationCollection(typeof(SchemasCollection),  
        AddItemName = "add",  
        ClearItemsName = "clear",  
        RemoveItemName = "remove")]  
    public SchemasCollection Schemas  
    {  
        get  
        {  
            SchemasCollection SchemasCollection =  
            (SchemasCollection)base["schemas"];  
            return SchemasCollection;  
        }  
    }  
}  
```  
  
## <a name="adding-message-inspectors-imperatively"></a>强制添加消息检查器  

 除了通过属性（由于前述原因本示例中不支持属性）和配置添加行为外，使用命令性代码也可以很容易地将行为添加到客户端和服务运行时。 在本示例中，在客户端应用程序中完成此操作以测试客户端消息检查器。 `GenericClient` 类派生自 <xref:System.ServiceModel.ClientBase%601>，可向用户代码公开终结点配置。 在隐式打开客户端之前，可以更改终结点配置（例如通过添加行为），如下面的代码所示。 在服务上添加行为在很大程度上等效于此处所示的客户端技术，并且必须在打开服务主机之前执行。  
  
```csharp  
try  
{  
    Console.WriteLine("*** Call 'Hello' with generic client, with client behavior");  
    GenericClient client = new GenericClient();  
  
    // Configure client programmatically, adding behavior  
    XmlSchema schema = XmlSchema.Read(new StreamReader("messages.xsd"),
                                                          null);  
    XmlSchemaSet schemaSet = new XmlSchemaSet();  
    schemaSet.Add(schema);  
    client.Endpoint.Behaviors.Add(new
                SchemaValidationBehavior(schemaSet, true, true));  
  
    Console.WriteLine("--- Sending valid client request:");  
    GenericCallValid(client, helloAction);  
    Console.WriteLine("--- Sending invalid client request:");  
    GenericCallInvalid(client, helloAction);  
  
    client.Close();  
}  
catch (Exception e)  
{  
    DumpException(e);  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageInspectors`  
