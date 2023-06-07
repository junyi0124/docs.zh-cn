---
title: 服务框架
ms.date: 03/30/2017
ms.assetid: 75f60b87-f80e-4377-ba7c-8e6becaa2b28
ms.openlocfilehash: 1fb39f2106e027cc5d4125cfb0bc89f3e5983cec
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96285715"
---
# <a name="service-framework"></a>服务框架

本主题列出由服务框架数据生成的所有异常。  
  
## <a name="exception-list"></a>异常列表  
  
|资源代码|资源字符串|  
|-------------------|---------------------|  
|ABindingInstanceHasAlreadyBeenAssociatedTo1|已关联绑定实例以侦听指定的统一资源标识符。 如果两个终结点要共享相同的侦听统一资源标识符，则它们还必须共享相同的绑定对象实例。 在 AddServiceEndpoint() 调用和/或配置文件中指定了两个冲突的终结点。|  
|AChannelServiceEndpointIsNull0|通道或服务终结点为 Null。|  
|AChannelServiceEndpointSContractSNameIsNull0|通道/服务终结点的协定名称为 Null 或是空的。|  
|AChannelServiceEndpointSContractSNamespace0|通道/服务终结点的协定命名空间为 Null。|  
|BaseAddressCannotHaveFragment|基址中不能包含统一资源标识符片段。|  
|BaseAddressCannotHaveQuery|基址中不能包含统一资源标识符查询字符串。|  
|BaseAddressCannotHaveUserInfo|基址中不能包含统一资源标识符用户信息节。|  
|BaseAddressDuplicateScheme|此集合已经包含指定方案的地址。 此集合中的每个方案只能有一个地址。|  
|BaseAddressMustBeAbsolute|只有绝对统一资源标识符才能用作基址。|  
|BindingDoesnTSupportAnyChannelTypes1|指定的绑定不支持创建任何通道类型。 自定义绑定中的绑定元素的堆栈不正确或者顺序错误。 堆栈底部需要一个传输协议元素。 建议绑定元素采用以下顺序：TransactionFlow、ReliableSession、Security、CompositeDuplex、OneWay、StreamSecurity、MessageEncoding、Transport。|  
|BindingDoesnTSupportDuplexButContractRequires1|协定需要双工， 但是指定的绑定不支持它或者因配置不正确而无法支持它。|  
|BindingDoesnTSupportOneWayButContractRequires1|协定需要单向， 但是指定的绑定不支持它或者因配置不正确而无法支持它。|  
|BindingDoesnTSupportRequestReplyButContract1|协定需要请求/答复， 但是指定的绑定不支持它或者因配置不正确而无法支持它。|  
|BindingDoesnTSupportSessionButContractRequires1|协定需要会话，  但是指定的绑定不支持它或者因配置不正确而无法支持它。|  
|BindingDoesnTSupportTwoWayButContractRequires1|协定需要双向（请求-答复或双工）， 但是指定的绑定不支持它或者因配置不正确而无法支持它。|  
|BindingRequirementsAttributeDisallowsQueuedDelivery1|DeliveryRequirementsAttribute 不允许 QueuedDelivery， 但是具有指定协定的终结点的绑定支持它。|  
|BindingRequirementsAttributeRequiresQueuedDelivery1|DeliveryRequirementsAttribute 需要 QueuedDelivery， 但是具有指定协定的终结点的绑定不支持它或者因为配置不正确而不支持它。|  
|channelDoesNotHaveADuplexSession0|当前的通道不支持关闭输出会话， 此通道不实现 ISessionChannel \<IDuplexSession> 。|  
|ClientRuntimeRequiresFormatter0|指定的 ClientOperation 需要格式化程序，因为 SerializeRequest 和 DeserializeReply 并不都是 False。|  
|CommunicationObjectAborted1|无法将指定的通信对象用于通信，因为它已经被停止。|  
|CommunicationObjectAbortedStack2|无法将指定的通信对象用于通信，因为它已停止： {1}|  
|CommunicationObjectBaseClassMethodNotCalled|指定的通信对象已重写虚函数， {1} 但它未调用基类中定义的版本。|  
|ContractIsNotSelfConsistentItHasOneOrMore2|指定的协定包含一个或多个 IsTerminating 或者非 IsInitiating 操作， 但是它未将 SessionMode 属性 (Property) 设置为 SessionMode.Required。 IsInitiating 和 IsTerminating 属性 (Attribute) 只能在会话的上下文中使用。|  
|CouldnTCreateChannelForChannelType2|请求指定的通道类型，但是指定的绑定不支持它或者因配置不正确而无法支持它。|  
|DispatchRuntimeRequiresFormatter0|指定的 DispatchOperation 需要格式化程序，因为 DeserializeRequest 和 SerializeReply 并非均为 False。|  
|EndMethodsCannotBeDecoratedWithOperationContractAttribute|使用 IAsyncResult 设计样式时，End 方法不能使用 OperationContractAttribute 进行修饰。 只有相对应的 Begin 方法才能使用 OperationContractAttribute 进行修饰； 该属性将应用到 Begin-End 方法对中。|  
|EndpointListenerRequirementsCannotBeMetBy3|指定绑定的 IChannelListener 无法满足 ChannelDispatcher 要求，因为协定需要支持这些指定通道类型中的一种， 但是绑定只支持这些指定的通道类型。|  
|EndpointsMustHaveAValidBinding0|终结点必须具有有效的绑定。|  
|InvalidOrUnrecognizedAction|无法处理消息，因为指定的操作无效或无法识别。|  
|MultipleMebesInParameters|在 BindingContext 的 BindingParameters 中找到多个 MessageEncodingBindingElement。 CustomBinding 不能具有多个 MessageEncodingBindingElement。 在这些元素中，移除多余元素，保留一个即可。|  
|MultipleStreamUpgradeProvidersInParameters|在 BindingContext 的 BindingParameters 中找到多个 IStreamUpgradeProviderElement。 CustomBinding 不能具有多个 IStreamUpgradeProviderElement。 在这些元素中，移除多余元素，保留一个即可。|  
|NoChannelBuilderAvailable|不能使用该绑定创建通道工厂或通道侦听器，因为它不具有 TransportBindingElement。 每个绑定都必须至少具有一个从 TransportBindingElement 派生的绑定元素。|  
|NotAllBindingElementsBuilt|生成通道工厂/通道侦听器时，没有使用此绑定中的某些绑定元素。 这可能是因为绑定元素顺序混乱所导致的。 建议绑定元素采用以下顺序：TransactionFlow、ReliableSession、Security、CompositeDuplex、OneWay、StreamSecurity、MessageEncoding、Transport。  请注意，TransportBindingElement 必须是最后一个。 没有生成指定的绑定元素。|  
|RuntimeRequiresInvoker0|调度操作需要调用程序。|  
|ServiceHasZeroAppEndpoints|指定的服务不具有应用程序（非基础结构）终结点。 这可能是因为未找到应用程序的配置文件，或者在配置文件中未找到与服务名称匹配的服务元素，或者服务元素中未定义终结点。|  
|SFxActionMismatch|由于操作不匹配，无法创建类型化消息 期望指定的操作，但是遇到其他操作。|  
|SFxAnonymousTypeNotSupported|指定消息的指定部分无法使用 RPC 导出或编码，因为其类型为匿名的。|  
|SFxBadMetadataLocationNoAppropriateBaseAddress|通过配置节中 serviceMetadata 部分中的 ExternalMetadataLocation 属性 (Property) 或 externalMetadataLocation 属性 (Attribute) 向 ServiceMetadataBehavior 提供的 URL 是相对 URL，而且没有用于对其进行解析的基址。|  
|SFxBadMetadataMustBePolicy|必须提供一个具有指定命名空间和名称的策略 XmlElement。 此 XmlElement 具有指定的命名空间和名称。|  
|SFxBodyObjectTypeCannotBeInherited|除了 RPC 样式中用作正文对象的对象以外，指定的类型无法从任何类中继承。|  
|SFxBodyObjectTypeCannotBeInterface|指定类型实现 RPC 样式中对正文对象不支持的指定接口。|  
|SFxCallbackBehaviorAttributeOnlyOnDuplex|CallbackBehaviorAttribute 只可以在具有双工协定的终结点上作为一种行为运行。 指定的协定不是双工的，并且不包含回调操作。|  
|SFxCallbackRequestReplyInOrder1|在当前消息完成处理以前，无法从此操作收到答复。 如果要允许无序的消息处理，请在指定的对象上将 ConcurrencyMode 指定为 Reentrant 或 Multiple。|  
|SfxCallbackTypeCannotBeNull|为了将指定的协定与 DuplexChannelFactory 一起使用，该协定必须指定一个有效的回调协定。 如果协定没有回调协定，则考虑使用 ChannelFactory 而不是 DuplexChannelFactory。|  
|SFxCannotGetMetadataFromLocation|MetadataExchangeClient 只能从 HTTP 和 HTTPS MetadataLocation 中获取元数据。 它无法从指定地址获取元数据。|  
|SFxCannotHttpGetMetadataFromAddress|使用 MetadataExchangeClientMode HttpGet 时，MetadataExchangeClient 只能从 HTTP 或 HTTPS 地址获取元数据。 它无法从指定地址获取元数据。|  
|SFxCannotImportAsParameters_Bare|正在生成消息协定，因为指定的操作不是 RPC，也不是文档包装的。|  
|SFxCannotImportAsParameters_DifferentWrapperName|正在生成消息协定，因为指定消息的包装名称与默认值不匹配。|  
|SFxCannotImportAsParameters_DifferentWrapperNs|正在生成消息协定，因为指定消息的包装命名空间与默认值不匹配。|  
|SFxCannotImportAsParameters_ElementIsNotNillable|正在生成消息协定，因为指定命名空间中的指定元素名称未标记为 nillable。|  
|SFxCannotImportAsParameters_HeadersAreUnsupported|正在生成消息协定，因为指定的消息具有标头。|  
|SFxCannotImportAsParameters_Message|正在生成消息协定，因为指定的操作将非类型化消息作为自变量或返回类型。|  
|SFxCannotImportAsParameters_MessageHasProtectionLevel|正在生成消息协定，因为指定的消息需要保护。|  
|SFxCannotImportAsParameters_NamespaceMismatch|正在生成消息协定，因为指定的消息部分命名空间与默认值不匹配。|  
|SFxCannotRequireBothSessionAndDatagram3|指定的一个协定指定了 SessionMode.NotAllowed，指定的另一个协定指定了 SessionMode.Required。 应更改 SessionMode 值之一，或者为每个终结点指定其他地址（或 ListenUri）。|  
|SFxCannotSetExtensionsByIndex|此集合不支持按索引设置扩展。 请考虑使用 InsertItem 方法或 RemoveItem 方法。|  
|SFxChannelDispatcherDifferentHost0|ChannelDispatcher 当前未附加到提供的 ServiceHost。|  
|SFxChannelDispatcherMultipleHost0|无法将 ChannelDispatcher 添加到多个 ServiceHost。|  
|SFxChannelDispatcherNoHost0|无法打开 ChannelDispatcher，因为它未附加到 ServiceHost。|  
|SfxChannelFactoryDisposed|无法打开此 ChannelFactory，因为 ChannelFactory 已被释放。 应在使用 ChannelFactory 之前重新创建它。|  
|SFxChannelFactoryNoBinding|无法打开此 ChannelFactory，因为尚未将任何绑定与它的终结点相关联。 请使用构造函数或终结点属性指定绑定。|  
|SFxChannelTerminated0|已经在此通道上调用了标记为 IsTerminating 的操作，并且导致通道连接终止。 不能在此通道上调用更多操作。 请重新创建通道以继续通信。|  
|SFxCloseTimedOut1|ServiceHost 关闭操作在指定时间后停止。 这可能是由于客户端无法在必需的时间内关闭会话通道。 分配给此操作的时间可能是更长超时的一部分。|  
|SfxCloseTimedOutWaitingForDispatchToComplete|关闭进程在等待服务调度完成时超时。|  
|SFxCodeGenIsNotAssignableFrom|不能分配指定的对象。|  
|SFxConfigChannelConfigurationNotFound|找不到具有在 ServiceModel 客户端配置节中指定的名称和协定的终结点元素。|  
|SFxConflictingGlobalElement|具有指定命名空间中指定名称的顶级 XML 元素无法引用指定的类型， 因为它已经引用了其他类型。 请使用不同的操作名称或 MessageBodyMemberAttribute 为消息或消息部分指定不同的名称。|  
|SFxContractHasZeroInitiatingOperations|协定必须至少有一个 IsInitiating=true 操作。|  
|SFxContractHasZeroOperations|协定必须至少有一个操作。|  
|SFxContractInheritanceRequiresInterfaces|指定类型的服务类定义 ServiceContract 并同时从指定类型中继承 ServiceContract。 协定继承只能在接口类型之间使用。 如果使用 ServiceContractAttribute 标记某个类，则该类必须是层次结构中使用 ServiceContractAttribute 的唯一类型。  请考虑将指定类型上的 ServiceContractAttribute 移到指定类型实现的单独接口。|  
|SFxCreateDuplexChannel1|指定协定的回调协定不存在，或者未定义任何操作。 如果这不是双工协定，请考虑使用 ChannelFactory 而不是 DuplexChannelFactory。|  
|SFxCreateDuplexChannelNoCallback|无法在此 DuplexChannelFactory 实例上调用此 CreateChannel 重载， 因为未使用 InstanceContext 初始化 DuplexChannelFactory。 请调用使用 InstanceContext 的 CreateChannel 重载。|  
|SFxCreateDuplexChannelNoCallback1|无法在此 DuplexChannelFactory 实例上调用此 CreateChannel 重载， 因为未使用类型初始化 DuplexChannelFactory，而且未提供有效的 InstanceContext。 请调用使用 InstanceContext 的 CreateChannel 重载。|  
|SFxCreateDuplexChannelNoCallbackUserObject|无法在此 DuplexChannelFactory 实例上调用此 CreateChannel 重载， 因为向 DuplexChannelFactory 提供的 InstanceContext 不包含有效的 UserObject。|  
|SFxCreateNonDuplexChannel1|ChannelFactory 不支持指定的协定， 因为它使用一个或多个操作定义回调协定。 请考虑使用 DuplexChannelFactory 而不是 ChannelFactory。|  
|SFxCustomBindingNeedsTransport1|具有指定协定的 ServiceEndpoint 上的 CustomBinding 缺少 TransportBindingElement。 每个绑定都必须至少具有一个从 TransportBindingElement 派生的绑定元素。|  
|SFxCustomBindingWithoutTransport|无法计算此自定义绑定的方案，因为它缺少 TransportBindingElement。 每个绑定都必须至少具有一个从 TransportBindingElement 派生的绑定元素。|  
|SFxDataContractSerializerDoesNotSupportBareArray|DataContractSerializer 不支持在指定元素上指定的集合。|  
|SFxDictionaryIsEmpty|由于字典为空，因此无法执行此操作。|  
|SFxDocEncodedNotSupported|反射指定的对象时出错。 不支持编码文档。 请将“使用”更改为文本，或将“样式”更改为 RPC。|  
|SFxDuplicateInitiatingActionAtSameVia|此服务具有多个在指定地址侦听的终结点， 它们共享相同的指定初始化操作。 因此，将丢弃具有此操作的消息，因为调度程序将无法确定用于处理消息的正确终结点。|  
|SFXEndpointBehaviorUsedOnWrongSide|指定的 IEndpointBehavior 无法用于服务器； 此行为只能应用于客户端。|  
|SFxEndpointNoMatchingScheme|找不到与具有指定绑定的终结点的指定方案匹配的基址。 注册的基址方案是指定方案。|  
|SFxErrorCreatingMtomReader|为消息传输优化机制消息创建读取器时出错。|  
|SFxErrorDeserializingFault|服务器返回无效的 SOAP 错误。 有关详细信息，请参阅 InnerException。|  
|SFxErrorDeserializingHeader|反序列化指定消息中的标头之一时出错。 有关详细信息，请参见 InnerException。|  
|SFxErrorReflectingOnMethod3|加载指定类型中指定方法上的指定属性时出错。  有关详细信息，请参阅 InnerException。|  
|SFxErrorReflectingOnParameter4|加载指定类型中指定方法的指定参数上的指定属性时出错。 有关详细信息，请参阅 InnerException。|  
|SFxErrorReflectingOnType2|加载指定类型上的指定属性时出错。  有关详细信息，请参阅 InnerException。|  
|SFxErrorSerializingBody|序列化指定消息的正文时出错。 有关详细信息，请参阅 InnerException。|  
|SFxErrorSerializingHeader|序列化指定消息中的标头之一时出错。 有关详细信息，请参阅 InnerException。|  
|SFxExpectedIMethodCallMessage|内部错误。 该消息必须是有效的 IMethodCallMessage。|  
|SFxExportMustHaveType|指定操作中的指定部分无法导出，因为它不具有有效的 CLR 类型。|  
|SFxHeaderNotUnderstood|未处理该消息。 此消息的接收方不能理解来自指定命名空间的指定标头。 该错误通常表明消息发送方启用了接收方无法处理的通信协议。 请确保客户端的绑定配置与服务的绑定一致。|  
|SFxHeadersAreNotSupportedInEncoded|指定的消息不能有用于远程过程调用编码样式的标头。|  
|SFxInconsistentWsdlOperationStyleInMessageParts|指定操作中消息的所有部分都必须包含类型或元素。|  
|SFxInconsistentWsdlOperationStyleInOperationMessages|从指定操作中的消息推断出的指定样式与使用绑定指定的预期样式不匹配。|  
|SFxInvalidCallbackIAsyncResult|IAsyncResult 未提供或类型错误。|  
|SFxInvalidMessageBody|OperationFormatter 遇到了无效的消息正文。 预期找到具有指定名称和命名空间的“Element”节点类型。 找到了具有指定名称和命名空间的指定节点类型。|  
|SFxInvalidMessageBodyEmptyMessage|OperationFormatter 无法对来自消息的任何信息进行反序列化，因为消息为空。|  
|SFxInvalidMessageBodyErrorDeserializingParameter|尝试反序列化指定的参数时出错。 有关更多信息，请参见 InnerException。|  
|SFxInvalidMessageBodyErrorSerializingParameter|尝试序列化指定的参数时出错。 指定了 InnerException 消息。  有关详细信息，请参阅 InnerException。|  
|SFxInvalidMessageBodyUnexpectedNode|反序列化参数时遇到来自指定命名空间的指定意外节点。|  
|SFxInvalidMessageContractSignature|指定的操作具有使用 MessageContractAttribute 标记的参数或返回类型。 在使用消息协定表示请求消息时，该操作必须有一个使用 MessageContractAttribute 标记的参数。 在使用消息协定表示响应消息时，该操作的返回值必须是使用 MessageContractAttribute 标记的类型。 该操作不能有“out”或“ref”参数。|  
|SFxInvalidReplyAction|该操作的传出答复消息具有指定的 Action，但该操作的协定指定了另外的 ReplyAction。 在消息中指定的 Action 必须与协定中的 ReplyAction 匹配，否则操作协定必须指定 ReplyAction='*'。|  
|SFxInvalidRequestAction|该操作的传出请求消息具有指定的 Action，但该操作的协定指定了另外的 RequestAction。 在消息中指定的 Action 必须与协定中的 RequestAction 匹配，否则操作协定必须指定 RequestAction='*'。|  
|SFxInvalidStaticOverloadCalledForDuplexChannelFactory1|静态 CreateChannel 方法不能与指定的协定一起使用，因为该协定定义了回调协定。 在 DuplexChannelFactory 上使用静态 CreateChannel 重载之一 \<TChannel> 。|  
|SFxInvalidStreamInRequest|要使指定操作中的请求成为流，该操作必须具有类型为 Stream 的单个参数。|  
|SFxInvalidStreamInResponse|要使指定操作中的响应成为流，该操作必须具有类型为 Stream 的单个 out 参数或返回值。|  
|SFxInvalidStreamInTypedMessage|要将流与 MessageContract 编程模型一起使用，指定的类型必须具有单个类型为 Stream 的 MessageBody 成员。|  
|SFxInvalidUseOfPrimitiveOperationFormatter|授予了 PrimitiveOperationFormatter 不支持的参数或返回类型。|  
|SFxMessageContractBaseTypeNotValid|指定的类型定义了 MessageContract，并且是从未定义 MessageContract 的指定类型派生而来的。 指定的继承层次结构中的所有对象都必须定义 MessageContract。|  
|SFxMethodNotSupported1|此对象上不支持指定的方法。 如果未使用 OperationContractAttribute 标记该方法或未使用 ServiceContractAttribute 标记接口类型，则会出现此情况。|  
|SFxMethodNotSupportedByType2|指定的 ServiceHost 实现类型未实现指定的服务协定。|  
|SFxMethodNotSupportedOnCallback1|指定的回调方法不受支持。 如果未使用 OperationContractAttribute 标记该方法或其接口类型不是 ServiceContractAttribute 的 CallbackContract 的目标，则会出现此情况。|  
|SFxMismatchedOperationParent|DispatchOperation 或 ClientOperation 只能分别添加到其父 DispatchRuntime 或 ClientRuntime。|  
|SFxNameCannotBeEmpty|Name 属性不能为空字符串。|  
|SfxNoTypeSpecifiedForParameter|没有为参数指定 CLR 类型，无法生成操作。|  
|SFxOperationBehaviorAttributeOnlyOnServiceClass|OperationBehaviorAttribute 只能位于服务类上， 不能将其放在 ServiceContract 接口上。 指定类型上的指定方法违反了这一规则。|  
|SFxOperationContractOnNonServiceContract|指定的方法标记了 OperationContractAttribute，但指定的封闭类型没有用 ServiceContractAttribute 标记。 OperationContractAttribute 只能用于 ServiceContractAttribute 类型中的方法上或其 CallbackContract 类型上。|  
|SFxParameterCountMismatch|所提供参数的数目和预期参数的数目不匹配。 具体说就是指定的自变量有指定数目的元素，而预期自变量有指定数目的元素。|  
|SFxPartNameMustBeUniqueInRpc|指定的消息部分名称在远程过程调用消息中不唯一。|  
|SFxReplyActionMismatch3|收到对带有指定动作的指定操作的答复消息。 但是，客户端代码需要指定的动作。|  
|SFxRequestReplyNone|收到目标为“None”地址、标头为 WS-Addressing ReplyTo 或 FaultTo 的消息。 这些值对于请求-答复操作无效。 如果需要支持值为“None”的 ReplyTo 或 FaultTo，请使用单向操作或启用 ManualAddressing。|  
|SFxRequestTimedOut1|该请求操作在指定的配置时间内未收到答复。 分配给该操作的时间可能是更长超时的一部分。 这可能是由于服务仍在处理该操作或服务无法发送答复消息。|  
|SFxRequestTimedOut2|发送到指定位置的请求操作在指定的配置时间内未收到答复。 分配给该操作的时间可能是更长超时的一部分。 这可能是由于服务仍在处理该操作或服务无法发送答复消息。|  
|SFxSchemaDoesNotContainType|具有指定目标命名空间的架构不包含具有指定名称的类型。|  
|SfxServiceContractAttributeNotFound|指定的协定类型不具有 ServiceContractAttribute 属性。 若要定义有效的协定，指定的类型必须具有 ServiceContractAttribute 属性。 该类型可以是协定接口或服务类。|  
|SFxServiceContractGeneratorConfigRequired|若要使用 GenerateServiceEndpoint 方法生成配置信息，必须使用有效的 Configuration 对象初始化 ServiceContractGenerator 实例。|  
|SFxServiceHostBaseCannotAddEndpointAfterOpen|在 ServiceHost 处于下列状态之一后，无法添加终结点：<br /><br /> -已打开<br />-出错<br />-已终止<br />-已关闭|  
|SFxServiceHostBaseCannotAddEndpointWithoutDescription|初始化 Description 属性以前，无法添加终结点。|  
|SFxServiceMetadataBehaviorNoHttpBaseAddress|ServiceMetadataBehavior 的 HttpGetEnabled 属性设置为 True，而 HttpGetUrl 属性是相对地址，但没有 HTTP 基址。 请提供 HTTP 基址或将 HttpGetUrl 设置为绝对地址。|  
|SFxServiceMetadataBehaviorNoHttpsBaseAddress|ServiceMetadataBehavior 的 HttpsGetEnabled 属性设置为 True，而 HttpsGetUrl 属性是相对地址，但没有 HTTP 基址。 请提供 HTTPS 基址或将 HttpsGetUrl 设置为绝对地址。|  
|SFxServiceMetadataBehaviorUrlMustBeHttpOrRelative|行为 Url 必须是相对统一资源标识符或具有指定方案的绝对统一资源标识符。 指定的 Url 是一个具有指定方案的绝对统一资源标识符。|  
|SFxStreamRequestMessageClosed|包含此流的消息已经关闭。 服务操作返回后，不能访问请求流。|  
|SFxStreamResponseMessageClosed|包含此流的消息已经关闭。|  
|SFxTerminateRequestProcessingException|操作管道中的扩展必须停止处理此消息。|  
|SFxTerminatingOperationAlreadyCalled1|该通道无法发送更多消息，因为已调用 IsTerminating 操作。|  
|SFxThrottleLimitMustBeGreaterThanZero0|控制器限制必须大于零。 若要禁用控制器限制，请将该值设置为 Int32.MaxValue。|  
|SFxTypedOrUntypedMessageCannotBeMixedWithVoidInRpc|当使用 RPC 编码样式时，如果操作没有参数或有一个 void 返回值，则不能使用消息协定类型或 System.ServiceModel.Channels.Message 类型。 将一个空消息协定类型作为参数或返回类型添加到指定的操作。|  
|SFxUserCodeThrewException|指定的用户操作引发了用户代码中未处理的异常。 如果重复出现该问题，则可能表明指定方法的实现中有错误。|  
|SfxUseTypedMessageForCustomAttributes|指定的参数无法映射到操作参数，因为它需要附加属性。|  
|SFxVersionMismatchInOperationContextAndMessage2|无法将传出标头添加到消息，因为 OperationContext.Current 中的 MessageVersion 与被处理的消息的标头版本不匹配。|  
|SFxWellKnownNonSingleton0|要使用采用服务实例作为参数的 ServiceHost 构造函数之一，服务的 InstanceContextMode 必须设置为 InstanceContextMode.Single。 可以使用 ServiceBehaviorAttribute 进行此配置。 否则，请使用采用 Type 参数的 ServiceHost 构造函数。|  
|SFxWrapperTypeHasMultipleNamespaces|指定消息的包装类型不能计划为数据协定类型，因为它有多个命名空间。 请使用 XmlSerializer。|  
|UriMustBeAbsolute|URI 必须是绝对值。|
