---
title: WMI 类引用
ms.date: 03/30/2017
ms.assetid: b95a51f5-8251-4619-ae05-7de88cb90f9a
ms.openlocfilehash: 9830fbf50e8df625e3d3077a66c66e0370204acb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262250"
---
# <a name="wmi-class-reference"></a>WMI 类引用

本部分列出了 Windows Communication Foundation (WCF) WMI 提供程序公开的所有 WMI 类。  
  
## <a name="accessing-wmi-instances"></a>访问 WMI 实例  

 除 Service、AppDomain、Contract、ServiceAppDomain、ServiceToEndpointAssociation 和 Endpoint 外，不能直接实例化 WMI 对象引用中列出的所有类。 若要访问其他实例，则可以访问上述顶级类的属性。 例如，你可以从终结点实例-> Binding-> Bindingelement 访问 TransportBindingElement 实例。  
  
## <a name="in-this-section"></a>本节内容  

 [ActivityTransfer](activitytransfer.md)  
  
 [AppDomainInfo](appdomaininfo.md)  
  
 [AspNetCompatibilityRequirementsAttribute](aspnetcompatibilityrequirementsattribute.md)  
  
 [AsymmetricSecurityBindingElement](asymmetricsecuritybindingelement.md)  
  
 "行为类"  
  
 [BinaryMessageEncodingBindingElement](binarymessageencodingbindingelement.md)  
  
 [Binding](binding.md)  
  
 [BindingElement](bindingelement.md)  
  
 [CallbackBehavior](callbackbehavior.md)  
  
 [Channel 类](channel-class.md)  
  
 [ChannelPoolSettings](channelpoolsettings.md)  
  
 [ClientCredentials](clientcredentials.md)  
  
 [ClientViaBehavior](clientviabehavior.md)  
  
 [CompositeDuplexBindingElement](compositeduplexbindingelement.md)  
  
 [ConnectionOrientedTransportBindingElement](connectionorientedtransportbindingelement.md)  
  
 [合同期](contract.md)  
  
 [CustomBindingElement](custombindingelement.md)  
  
 [DeliveryRequirementsAttribute](deliveryrequirementsattribute.md)  
  
 [终结点](endpoint.md)  
  
 [HttpsTransportBindingElement](httpstransportbindingelement.md)  
  
 [HttpTransportBindingElement](httptransportbindingelement.md)  
  
 [LocalServiceSecuritySettings](localservicesecuritysettings.md)  
  
 [MatchAllEndpointBehavior](matchallendpointbehavior.md)  
  
 [MessageEncodingBindingElement](messageencodingbindingelement.md)  
  
 [MsmqBindingElementBase](msmqbindingelementbase.md)  
  
 [MsmqIntegrationBindingElement](msmqintegrationbindingelement.md)  
  
 [MsmqTransportBindingElement](msmqtransportbindingelement.md)  
  
 [MtomMessageEncodingBindingElement](mtommessageencodingbindingelement.md)  
  
 [MustUnderstandBehavior](mustunderstandbehavior.md)  
  
 [NamedPipeConnectionPoolSettings](namedpipeconnectionpoolsettings.md)  
  
 [NamedPipeTransportBindingElement](namedpipetransportbindingelement.md)  
  
 [OneWayBindingElement](onewaybindingelement.md)  
  
 "Operation class"  
  
 [OperationBehaviorAttribute](operationbehaviorattribute.md)  
  
 [PeerCustomResolverBindingElement](peercustomresolverbindingelement.md)  
  
 [PeerResolverBindingElement](peerresolverbindingelement.md)  
  
 [PeerSecuritySettings](peersecuritysettings.md)  
  
 [PeerTransportBindingElement](peertransportbindingelement.md)  
  
 [PeerTransportSecuritySettings](peertransportsecuritysettings.md)  
  
 [PnrpPeerResolverBindingElement](pnrppeerresolverbindingelement.md)  
  
 [PrivacyNoticeBindingElement](privacynoticebindingelement.md)  
  
 [ReliableSessionBindingElement](reliablesessionbindingelement.md)  
  
 [SecurityBindingElement](securitybindingelement.md)  
  
 [服务](service.md)  
  
 [ServiceAppDomain](serviceappdomain.md)  
  
 [ServiceAuthorizationBehavior](serviceauthorizationbehavior.md)  
  
 [ServiceBehaviorAttribute](servicebehaviorattribute.md)  
  
 [ServiceCredentials](servicecredentials.md)  
  
 [ServiceDebugBehavior](servicedebugbehavior.md)  
  
 [ServiceMetadataBehavior](servicemetadatabehavior.md)  
  
 [ServiceSecurityAuditBehavior](servicesecurityauditbehavior.md)  
  
 [ServiceThrottlingBehavior](servicethrottlingbehavior.md)  
  
 [ServiceTimeoutsBehavior](servicetimeoutsbehavior.md)  
  
 [ServiceToEndpointAssociation](servicetoendpointassociation.md)  
  
 [SslStreamSecurityBindingElement](sslstreamsecuritybindingelement.md)  
  
 [SymmetricSecurityBindingElement](symmetricsecuritybindingelement.md)  
  
 [SynchronousReceiveBehavior](synchronousreceivebehavior.md)  
  
 [TcpConnectionPoolSettings](tcpconnectionpoolsettings.md)  
  
 [TcpTransportBindingElement](tcptransportbindingelement.md)  
  
 [TextMessageEncodingBindingElement](textmessageencodingbindingelement.md)  
  
 [TraceListener](tracelistener.md)  
  
 [TraceListenerArgument](tracelistenerargument.md)  
  
 [TransactedBatchingBehavior](transactedbatchingbehavior.md)  
  
 [TransactionFlowAttribute](transactionflowattribute.md)  
  
 [TransactionFlowBindingElement](transactionflowbindingelement.md)  
  
 [TransportBindingElement](transportbindingelement.md)  
  
 [TransportSecurityBindingElement](transportsecuritybindingelement.md)  
  
 [UseManagedPresentationBindingElement](usemanagedpresentationbindingelement.md)  
  
 [WindowsStreamSecurityBindingElement](windowsstreamsecuritybindingelement.md)  
  
 [WSAT_TraceEvent](wsat-traceevent.md)  
  
 [WSAT_TraceProvider](wsat-traceprovider.md)  
  
 [WSAT_TraceRecord](wsat-tracerecord.md)  
  
 [XmlDictionaryReaderQuotas](xmldictionaryreaderquotas.md)  
  
 [XmlSerializerOperationBehavior](xmlserializeroperationbehavior.md)
