---
title: 分析跟踪事件参考
ms.date: 03/30/2017
helpviewer_keywords:
- analytic tracing [WCF]. reference
ms.assetid: e44540cf-44a1-4efc-b965-7fbfd2131d73
ms.openlocfilehash: 28ae252d562b57df0553f0fd4370845e836ef537
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254839"
---
# <a name="analytic-trace-event-reference"></a>分析跟踪事件参考

下表定义了与 WCF 分析跟踪相关联的事件级别、标识符和消息。  
  
## <a name="event-reference"></a>事件参考  
  
|事件 ID|事件级别|事件消息|关键字|  
|--------------|-----------------|-------------------|--------------|  
|[131 - BufferPoolAllocation](131-bufferpoolallocation.md)|“详细”|池正在分配 %1 个字节。|基础结构|  
|[132 - BufferPoolChangeQuota](132-bufferpoolchangequota.md)|“详细”|BufferPool 的大小为 %1，按 %2 更改配额。|基础结构|  
|[133 - ActionItemScheduled](133-actionitemscheduled.md)|“详细”|已调用 IO 线程调度程序回调。|基础结构|  
|[134 - ActionItemCallbackInvoked](134-actionitemcallbackinvoked.md)|“详细”|已调用 IO 线程调度程序回调。|基础结构|  
|[201 - ClientMessageInspectorAfterReceiveInvoked](201-clientmessageinspectorafterreceiveinvoked.md)|信息|调度程序对“%1”类型的 ClientMessageInspector 调用了“AfterReceiveReply”。|疑难解答，ServiceModel|  
|[202 - ClientMessageInspectorBeforeSendInvoked](202-clientmessageinspectorbeforesendinvoked.md)|信息|调度程序对“%1”类型的 ClientMessageInspector 调用了“BeforeSendRequest”。|疑难解答，ServiceModel|  
|[203 - ClientParameterInspectorAfterCallInvoked](203-clientparameterinspectoraftercallinvoked.md)|信息|Dispatcher 对“%1”类型的 ClientParameterInspector 调用 了“AfterCall”。|疑难解答，ServiceModel|  
|[204 - ClientParameterInspectorBeforeCallInvoked](204-clientparameterinspectorbeforecallinvoked.md)|信息|调度程序对“%1”类型的 ClientParameterInspector 调用了“BeforeCall”。|疑难解答，ServiceModel|  
|[205 - OperationInvoked](205-operationinvoked.md)|信息|OperationInvoker 调用了“%1”方法。|EndToEndMonitoring，疑难解答，ServiceModel|  
|[206 - ErrorHandlerInvoked](206-errorhandlerinvoked.md)|信息|调度程序调用了“%1”类型的 ErrorHandler 来处理“%3”类型的异常。  ErrorHandled 为“%2”。|疑难解答，ServiceModel|  
|[207 - FaultProviderInvoked](207-faultproviderinvoked.md)|信息|调度程序调用了“%1”类型的 FaultProvider 来处理“%2”类型的异常。|疑难解答，ServiceModel|  
|[208 - MessageInspectorAfterReceiveInvoked](208-messageinspectorafterreceiveinvoked.md)|信息|调度程序对“%1”类型的 MessageInspector 调用了“AfterReceiveReply”。|疑难解答，ServiceModel|  
|[209 - MessageInspectorBeforeSendInvoked](209-messageinspectorbeforesendinvoked.md)|信息|调度程序对“%1”类型的 MessageInspector 调用了“BeforeSendRequest”。|疑难解答，ServiceModel|  
|[210 - MessageThrottleExceeded](210-messagethrottleexceeded.md)|警告|达到了“%2”的中止值“%1”。|HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[211 - ParameterInspectorAfterCallInvoked](211-parameterinspectoraftercallinvoked.md)|信息|调度程序对“%1”类型的 ParameterInspector 调用了“AfterCall”。|疑难解答，ServiceModel|  
|[212 - ParameterInspectorBeforeCallInvoked](212-parameterinspectorbeforecallinvoked.md)|信息|调度程序对“%1”类型的 ParameterInspector 调用了“BeforeCall”。|疑难解答，ServiceModel|  
|[213 - ServiceHostStarted](213-servicehoststarted.md)|LogAlways|ServiceHost 已启动:“%1”。|HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[214 - OperationCompleted](214-operationcompleted.md)|信息|OperationInvoker 已完成对“%1”方法的调用。  该方法调用持续了“%2”毫秒。|HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[215 - MessageReceivedByTransport](215-messagereceivedbytransport.md)|信息|传输从“%1”收到了一条消息。|疑难解答，ServiceModel|  
|[216 - MessageSentByTransport](216-messagesentbytransport.md)|信息|传输向“%1”发送了一条消息。|疑难解答，ServiceModel|  
|[217 - ClientOperationPrepared](217-clientoperationprepared.md)|信息|客户端正在执行“%2”协定中定义的“%1”操作。 消息将发送到“%3”。|疑难解答，ServiceModel|  
|[218 - ClientOperationCompleted](218-clientoperationcompleted.md)|信息|客户端执行完在“%2”协定中定义的“%1”操作。 消息已发送到“%3”。|疑难解答，ServiceModel|  
|[219 - ServiceException](219-serviceexception.md)|错误|消息处理过程中出现了“%2”类型的未经处理的异常。  完整异常 ToString: %1。|HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[220 - MessageSentToTransport](220-messagesenttotransport.md)|信息|调度程序向传输发送了一条消息。 相关 ID 为“%1”。|EndToEndMonitoring，疑难解答，ServiceModel|  
|[221 - MessageReceivedFromTransport](221-messagereceivedfromtransport.md)|信息|调度程序从传输收到一条消息。 相关 ID 为“%1”。|EndToEndMonitoring，疑难解答，ServiceModel|  
|[222 - OperationFailed](222-operationfailed.md)|警告|“%1”方法在由 OperationInvoker 调用时引发了未经处理的异常。 该方法调用持续了“%2”毫秒。|HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[223 - OperationFaulted](223-operationfaulted.md)|警告|“%1”方法在由 OperationInvoker 调用时引发了一个 FaultException。 该方法调用持续了“%2”毫秒。|HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[224 - MessageThrottleAtSeventyPercent](224-messagethrottleatseventypercent.md)|警告|“%2”的中止值“%1”为 70%。|HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[226 - IdleServicesClosed](226-idleservicesclosed.md)|LogAlways|总计 %2 个已激活服务中的 %1 个空闲服务已关闭。|HealthMonitoring WebHost|  
|[301 - UserDefinedErrorOccurred](301-userdefinederroroccurred.md)|错误|Name“%1”、Reference“%2”、Payload: %3。|UserEvents，HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[302 - 出现用户定义的警告](302-userdefinedwarningoccurred.md)|警告|Name“%1”、Reference“%2”、Payload: %3。|UserEvents，HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[303 - UserDefinedInformationEventOccured](303-userdefinedinformationeventoccured.md)|信息|Name“%1”、Reference“%2”、Payload: %3。|UserEvents，HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|[401- StopSignPostEvent](401-stopsignpostevent.md)|信息|活动边界。|疑难解答|  
|[402 - StartSignpostEvent](402-startsignpostevent.md)|信息|活动边界。|疑难解答|  
|[403 - SuspendSignpostEvent](403-suspendsignpostevent.md)|信息|活动边界。|疑难解答|  
|[404 - ResumeSignpostEvent](404-resumesignpostevent.md)|信息|活动边界。|疑难解答|  
|[451 - MessageLogInfo](451-messageloginfo.md)|信息|%1|疑难解答，WCFMessageLogging|  
|[452 - MessageLogWarning](452-messagelogwarning.md)|警告|%1|疑难解答，WCFMessageLogging|  
|[499 - TransferEmitted](499-transferemitted.md)|LogAlways|发出了传输事件。|疑难解答，UserEvents，EndToEndMonitoring，ServiceModel，WFTracking，ServiceHost，WCFMessageLogging|  
|[501 - CompilationStart](501-compilationstart.md)|信息|开始编译。|WebHost|  
|[502 - CompilationStop](502-compilationstop.md)|信息|结束编译。|WebHost|  
|[503 - ServiceHostFactoryCreationStart](503-servicehostfactorycreationstart.md)|信息|ServiceHostFactory 开始创建。|WebHost|  
|[504 - ServiceHostFactoryCreationStop](504-servicehostfactorycreationstop.md)|信息|ServiceHostFactory 结束创建。|WebHost|  
|[505 - CreateServiceHostStart](505-createservicehoststart.md)|信息|开始 CreateServiceHost。|WebHost|  
|[506 - CreateServiceHostStop](506-createservicehoststop.md)|信息|结束 CreateServiceHost。|WebHost|  
|[507 - HostedTransportConfigurationManagerConfigInitStart](507-hostedtransportconfigurationmanagerconfiginitstart.md)|信息|HostedTransportConfigurationManager 开始配置初始化。|WebHost|  
|[508 - HostedTransportConfigurationManagerConfigInitStop](508-hostedtransportconfigurationmanagerconfiginitstop.md)|信息|HostedTransportConfigurationManager 结束配置初始化。|WebHost|  
|[509 - ServiceHostOpenStart](509-servicehostopenstart.md)|信息|HostedTransportConfigurationManager 结束配置初始化。|服务主机|  
|[510 - ServiceHostOpenStop](510-servicehostopenstop.md)|信息|ServiceHost 已完成打开。|服务主机|  
|[513 - WebHostRequestStart](513-webhostrequeststart.md)|信息|从 AppDomain“%1”收到了虚拟路径为“%2”的请求。|WebHost|  
|[514 - WebHostRequestStop](514-webhostrequeststop.md)|信息|WebHostRequest 停止。|WebHost|  
|[601 - CBAEntryRead](601-cbaentryread.md)|“详细”|已处理的 ServiceActivation 元素的相对地址为“%1”，规范化的相对地址为“%2”。||  
|[602 - CBAMatchFound](602-cbamatchfound.md)|“详细”|传入请求与带有地址“%1”的 ServiceActivation 元素相匹配。||  
|[603 - AspNetRoutingService](603-aspnetroutingservice.md)|“详细”|传入请求与地址为 %1 的 ASP.NET 路由中定义的 WCF 服务匹配。|RoutingServices|  
|[604 - AspNetRoute](604-aspnetroute.md)|“详细”|添加了具有 serviceType "%2" 和 Servicehostfactorytype 为 "%3" 的新 ASP.NET 路由 "%1"。|RoutingServices|  
|[605 - IncrementBusyCount](605-incrementbusycount.md)|“详细”|调用 IncrementBusyCount。 源：%1|WebHost|  
|[606 - DecrementBusyCount](606-decrementbusycount.md)|“详细”|调用 IncrementBusyCount。 源：%1|WebHost|  
|[701 - ServiceChannelOpenStart](701-servicechannelopenstart.md)|“详细”|ServiceChannelOpen 已启动。|WebHost|  
|[702 - ServiceChannelOpenStop](702-servicechannelopenstop.md)|信息|ServiceChannelOpen 已完成。|ServiceModel|  
|[703 - ServiceChannelCallStart](703-servicechannelcallstart.md)|信息|ServiceChannelCall 已启动。|ServiceModel|  
|[704 - ServiceChannelBeginCallStart](704-servicechannelbegincallstart.md)|信息|ServiceChannel 异步调用已启动。|ServiceModel|  
|[706 - HttpSendMessageStart](706-httpsendmessagestart.md)|“详细”|Http 发送请求启动。|HTTP|  
|[707 - HttpSendStop](707-httpsendstop.md)|“详细”|Http 发送请求停止。|HTTP|  
|[708 - HttpMessageReceiveStart](708-httpmessagereceivestart.md)|“详细”|已从 http 传输接收消息。|HTTP|  
|[709 - DispatchMessageStart](709-dispatchmessagestart.md)|信息|消息调度已启动。|ServiceModel|  
|[710 - HttpContextBeforeProcessAuthentication](710-httpcontextbeforeprocessauthentication.md)|“详细”|启动用于消息调度的身份验证。|ServiceModel|  
|[711 - DispatchMessageBeforeAuthorization](711-dispatchmessagebeforeauthorization.md)|“详细”|启动用于消息调度的授权。|ServiceModel|  
|[712 - DispatchMessageStop](712-dispatchmessagestop.md)|信息|消息调度已完成。|ServiceModel|  
|[715 - ClientChannelOpenStart](715-clientchannelopenstart.md)|信息|ServiceChannel 打开启动。|ServiceModel|  
|[716 - ClientChannelOpenStop](716-clientchannelopenstop.md)|信息|ServiceChannel 打开停止。|ServiceModel|  
|[717 - HttpSendStreamedMessageStart](717-httpsendstreamedmessagestart.md)|信息|Http Send 启动流处理消息。|HTTP|  
|[1400 - ChannelInitializationTimeout](1400-channelinitializationtimeout.md)|错误|1%|ServiceModel|  
|[1401 - CloseTimeout](1401-closetimeout.md)|错误|1%|ServiceModel|  
|[1402 - IdleTimeout](1402-idletimeout.md)|错误|%1 连接池键：%2|ServiceModel|  
|[1403 - LeaseTimeout](1403-leasetimeout.md)|信息|%1 连接池键：%2|ServiceModel|  
|[1405 - OpenTimeout](1405-opentimeout.md)|错误|%1|ServiceModel|  
|[1406 - ReceiveTimeout](1406-receivetimeout.md)|错误|%1|ServiceModel|  
|[1407 - SendTimeout](1407-sendtimeout.md)|错误|%1|ServiceModel|  
|[1409 - InactivityTimeout](1409-inactivitytimeout.md)|信息|%1|ServiceModel|  
|[1416 - MaxReceivedMessageSizeExceeded](1416-maxreceivedmessagesizeexceeded.md)|错误|%1|Quota|  
|[1417 - MaxSentMessageSizeExceeded](1417-maxsentmessagesizeexceeded.md)|错误|%1|Quota|  
|[1418 - MaxOutboundConnectionsPerEndpointExceeded](1418-maxoutboundconnectionsperendpointexceeded.md)|信息|%1|Quota|  
|[1419 - MaxPendingConnectionsExceeded](1419-maxpendingconnectionsexceeded.md)|信息|%1|Quota|  
|[1420 - ReaderQuotaExceeded](1420-readerquotaexceeded.md)|错误|%1|Quota|  
|[1422 - NegotiateTokenAuthenticatorStateCacheExceeded](1422-negotiatetokenauthenticatorstatecacheexceeded.md)|错误|%1|Quota|  
|[1423 - NegotiateTokenAuthenticatorStateCacheRatio](1423-negotiatetokenauthenticatorstatecacheratio.md)|“详细”|协商令牌身份验证器状态的比率：%1/%2|Quota|  
|[1424 - SecuritySessionRatio](1424-securitysessionratio.md)|“详细”|安全会话比率：%1/%2|Quota|  
|[1430 - PendingConnectionsRatio](1430-pendingconnectionsratio.md)|“详细”|挂起的连接比率：%1/%2|Quota|  
|[1431 - ConcurrentCallsRatio](1431-concurrentcallsratio.md)|“详细”|并发会话比率：%1/%2|Quota|  
|[1432 - ConcurrentSessionsRatio](1432-concurrentsessionsratio.md)|“详细”|并发会话比率：%1/%2|Quota|  
|[1433 - OutboundConnectionsPerEndpointRatio](1433-outboundconnectionsperendpointratio.md)|“详细”|每个终结点的出站连接比率：%1/%2|Quota|  
|[1436 - PendingMessagesPerChannelRatio](1436-pendingmessagesperchannelratio.md)|“详细”|每个通道的挂起消息比率：%1/%2|Quota|  
|[1438 - ConcurrentInstancesRatio](1438-concurrentinstancesratio.md)|“详细”|并发实例比率： %1/%2|Quota|  
|[1439 - PendingAcceptsAtZero](1439-pendingacceptsatzero.md)|信息|剩余零个等待接受|Quota|  
|[1441 - MaxSessionSizeReached](1441-maxsessionsizereached.md)|警告|1%|Quota|  
|[1442 - ReceiveRetryCountReached](1442-receiveretrycountreached.md)|警告|对于 id 为“%1”的 MSMQ 消息，已达到接收重试计数限制|Quota|  
|[1443 - MaxRetryCyclesExceededMsmq](1443-maxretrycyclesexceededmsmq.md)|错误|对于 id 为“%1”的 MSMQ 消息，已超过最大重试周期数|Quota|  
|[1445 - ReadPoolMiss](1445-readpoolmiss.md)|“详细”|已新建“%1”|Quota|  
|[1446 - WritePoolMiss](1446-writepoolmiss.md)|“详细”|已新建“%1”|Quota|  
|[1451 - MaxRetryCyclesExceeded](1451-maxretrycyclesexceeded.md)|错误|1%|Quota|  
|[3300 - ReceiveContextCompleteFailed](3300-receivecontextcompletefailed.md)|警告|未能完成 %1。|通道|  
|[3301 - ReceiveContextAbandonFailed](3301-receivecontextabandonfailed.md)|警告|未能放弃 %1。|通道|  
|[3303 - ReceiveContextAbandonWithException](3303-receivecontextabandonwithexception.md)|警告|接收上下文出错。|ServiceModel|  
|[3303 - ReceiveContextAbandonWithException](3303-receivecontextabandonwithexception.md)|信息|%1 被放弃并且具有异常 %2。|通道|  
|[3305 - ClientBaseCachedChannelFactoryCount](3305-clientbasecachedchannelfactorycount.md)|信息|缓存的通道工厂数量为:“%1”。  最多可以缓存“%2”个通道工厂。|ServiceModel|  
|[3306 - ClientBaseChannelFactoryAgedOutofCache](3306-clientbasechannelfactoryagedoutofcache.md)|信息|通道工厂在缓存中已到期，因为缓存已达到其限制“%1”。|ServiceModel|  
|[3307 - ClientBaseChannelFactoryCacheHit](3307-clientbasechannelfactorycachehit.md)|信息|使用了在缓存中找到的匹配通道工厂。|ServiceModel|  
|[3308 - ClientBaseUsingLocalChannelFactory](3308-clientbaseusinglocalchannelfactory.md)|信息|不使用缓存中的通道工厂，即为实例禁用了缓存。|ServiceModel|  
|[3309 - QueryCompositionExecuted](3309-querycompositionexecuted.md)|信息|已对请求 Uri“%2”执行了使用“%1”的查询组合。|ServiceModel|  
|[3310 - DispatchFailed](3310-dispatchfailed.md)|错误|已调度“%1”操作，但出现错误。|ServiceModel|  
|[3311 - DispatchSuccessful](3311-dispatchsuccessful.md)|信息|已成功调度“%1”操作。|ServiceModel|  
|[3312 - MessageReadByEncoder](3312-messagereadbyencoder.md)|信息|编码器已读取大小为“%1”字节的消息。|通道|  
|[3312 - MessageReadByEncoder](3312-messagereadbyencoder.md)|信息|编码器已写入大小为“%1”字节的消息。|通道|  
|[3314 - SessionIdleTimeout](3314-sessionidletimeout.md)|错误|会话正在中止 URI“%1”的空闲通道。|ServiceModel|  
|[3319 - SocketAcceptEnqueued](3319-socketacceptenqueued.md)|“详细”|连接接受已启动。|TCP|  
|[3320 - SocketAccepted](3320-socketaccepted.md)|“详细”|ListenerId：%1 接受 SocketId：%2。|TCP|  
|[3321 - ConnectionPoolMiss](3321-connectionpoolmiss.md)|“详细”|%1 的池没有可用的连接，%2 个连接忙。|通道|  
|[3322 - DispatchFormatterDeserializeRequestStart](3322-dispatchformatterdeserializerequeststart.md)|“详细”|调度程序已开始反序列化请求消息。|ServiceModel|  
|[3323 - DispatchFormatterDeserializeRequestStop](3323-dispatchformatterdeserializerequeststop.md)|“详细”|调度程序已完成反序列化请求消息。|ServiceModel|  
|[3324 - DispatchFormatterSerializeReplyStart](3324-dispatchformatterserializereplystart.md)|“详细”|调度程序已开始序列化答复消息。|ServiceModel|  
|[3325 - DispatchFormatterSerializeReplyStop](3325-dispatchformatterserializereplystop.md)|“详细”|调度程序已完成序列化答复消息。|ServiceModel|  
|[3326 - ClientFormatterSerializeRequestStart](3326-clientformatterserializerequeststart.md)|“详细”|已开始客户端请求序列化。|ServiceModel|  
|[3327 - ClientFormatterSerializeRequestStop](3327-clientformatterserializerequeststop.md)|“详细”|客户端已完成序列化请求消息。|ServiceModel|  
|[3328 - ClientFormatterDeserializeReplyStart](3328-clientformatterdeserializereplystart.md)|“详细”|客户端已开始反序列化答复消息。|ServiceModel|  
|[3329 - ClientFormatterDeserializeReplyStop](3329-clientformatterdeserializereplystop.md)|“详细”|客户端已完成反序列化答复消息。|ServiceModel|  
|[3330 - SecurityNegotiationStart](3330-securitynegotiationstart.md)|“详细”|安全协商已启动。|安全性|  
|[3331 - SecurityNegotiationStop](3331-securitynegotiationstop.md)|“详细”|安全协商已完成。|安全性|  
|[3332 - SecurityTokenProviderOpened](3332-securitytokenprovideropened.md)|“详细”|SecurityTokenProvider 打开已完成。|安全性|  
|[3333 - OutgoingMessageSecured](3333-outgoingmessagesecured.md)|“详细”|已保护传出消息的安全。|安全性|  
|[3334 - IncomingMessageVerified](3334-incomingmessageverified.md)|“详细”|已验证传入消息。|安全 ServiceModel|  
|[3335 - GetServiceInstanceStart](3335-getserviceinstancestart.md)|“详细”|服务实例检索已启动。|ServiceModel|  
|[3336 - GetServiceInstanceStop](3336-getserviceinstancestop.md)|“详细”|已检索服务实例。|ServiceModel|  
|[3337 - ChannelReceiveStart](3337-channelreceivestart.md)|“详细”|ChannelHandlerId：%1 - 消息接收循环已启动。|通道|  
|[3338 - ChannelReceiveStop](3338-channelreceivestop.md)|“详细”|ChannelHandlerId：%1 - 消息接收循环已停止。|通道|  
|[3339 - ChannelFactoryCreated](3339-channelfactorycreated.md)|“详细”|已创建 ChannelFactory。|ServiceModel|  
|[3340 - PipeConnectionAcceptStart](3340-pipeconnectionacceptstart.md)|“详细”|管道连接接受已在 %1 开始。|通道|  
|[3341 - PipeConnectionAcceptStop](3341-pipeconnectionacceptstop.md)|“详细”|已接受管道连接。|通道|  
|[3342 - EstablishConnectionStart](3342-establishconnectionstart.md)|“详细”|已为 %1 开始建立连接。|通道|  
|[3343 - EstablishConnectionStop](3343-establishconnectionstop.md)|“详细”|已建立连接。|通道|  
|[3345 - SessionPreambleUnderstood](3345-sessionpreambleunderstood.md)|“详细”|理解“%1”的会话前导码。|通道|  
|[3346 - ConnectionReaderSendFault](3346-connectionreadersendfault.md)|错误|连接读取器发送错误“%1”。|通道|  
|[3347 - SocketAcceptClosed](3347-socketacceptclosed.md)|“详细”|套接字接受已关闭。|TCP|  
|[3348 - ServiceHostFaulted](3348-servicehostfaulted.md)|严重|服务主机出现故障。|TCP|  
|[3349 - ListenerOpenStart](3349-listeneropenstart.md)|“详细”|正在为“%1”打开侦听器。|通道|  
|[3350 - ListenerOpenStop](3350-listeneropenstop.md)|“详细”|侦听器打开已完成。|通道|  
|[3351 - ServerMaxPooledConnectionsQuotaReached](3351-servermaxpooledconnectionsquotareached.md)|“详细”|已达到服务器最大入池连接配额。|Quota|  
|[3352 - TcpConnectionTimedOut](3352-tcpconnectiontimedout.md)|错误|SocketId：%1 到远程地址 %2 超时。|TCP|  
|[3353 - TcpConnectionResetError](3353-tcpconnectionreseterror.md)|警告|SocketId：%1 到远程地址 %2 出现连接重置错误。|TCP|  
|[3354 - ServiceSecurityNegotiationCompleted](3354-servicesecuritynegotiationcompleted.md)|“详细”|服务安全协商已完成。|安全性|  
|[3355 - SecurityNegotiationProcessingFailure](3355-securitynegotiationprocessingfailure.md)|错误|安全协商处理失败。|安全性|  
|[3356 - SecurityIdentityVerificationSuccess](3356-securityidentityverificationsuccess.md)|“详细”|安全验证成功。|安全性|  
|[3357 - SecurityIdentityVerificationFailure](3357-securityidentityverificationfailure.md)|错误|安全验证失败。|安全性|  
|[3358 - PortSharingDuplicatedSocket](3358-portsharingduplicatedsocket.md)|“详细”|%1 的套接字重复。|ActivationServices|  
|[3359 - SecurityImpersonationSuccess](3359-securityimpersonationsuccess.md)|“详细”|安全模拟成功。|安全性|  
|[3360 - SecurityImpersonationFailure](3360-securityimpersonationfailure.md)|警告|安全模拟失败。|安全性|  
|[3361 - HttpChannelRequestAborted](3361-httpchannelrequestaborted.md)|警告|Http 通道请求已中止。|HTTP|  
|[3362 - HttpChannelResponseAborted](3362-httpchannelresponseaborted.md)|警告|Http 通道响应已中止。|HTTP|  
|[3363 - HttpAuthFailed](3363-httpauthfailed.md)|警告|Http 身份验证失败。|HTTP|  
|[3364 - SharedListenerProxyRegisterStart](3364-sharedlistenerproxyregisterstart.md)|“详细”|已对 URI“%1”启动 SharedListenerProxy 注册。|ActivationServices|  
|[3365 - SharedListenerProxyRegisterStop](3365-sharedlistenerproxyregisterstop.md)|“详细”|SharedListenerProxy 注册停止。|ActivationServices|  
|[3366 - SharedListenerProxyRegisterFailed](3366-sharedlistenerproxyregisterfailed.md)|错误|SharedListenerProxy 注册失败，状态为“%1”。|ActivationServices|  
|[3367 - ConnectionPoolPreambleFailed](3367-connectionpoolpreamblefailed.md)|错误|ConnectionPoolPreambleFailed。|通道|  
|[3368 - SslOnInitiateUpgrade](3368-ssloninitiateupgrade.md)|“详细”|SslOnAcceptUpgradeStart|安全性|  
|[3369 - SslOnAcceptUpgrade](3369-sslonacceptupgrade.md)|“详细”|SslOnAcceptUpgradeStop|安全性|  
|[3370 - BinaryMessageEncodingStart](3370-binarymessageencodingstart.md)|“详细”|BinaryMessageEncoder 已开始对消息进行编码。|通道|  
|[3371 - MtomMessageEncodingStart](3371-mtommessageencodingstart.md)|“详细”|MtomMessageEncoder 已开始对消息进行编码。|通道|  
|[3372 - TextMessageEncodingStart](3372-textmessageencodingstart.md)|“详细”|TextMessageEncoder 已开始编码消息。|通道|  
|[3373 - BinaryMessageDecodingStart](3373-binarymessagedecodingstart.md)|“详细”|BinaryMessageEncoder 已开始解码消息。|通道|  
|[3374 - MtomMessageDecodingStart](3374-mtommessagedecodingstart.md)|“详细”|MtomMessageEncoder 已开始解码消息。|通道|  
|[3375 - TextMessageDecodingStart](3375-textmessagedecodingstart.md)|“详细”|TextMessageEncoder 已开始解码消息。|通道|  
|[3376 - HttpResponseReceiveStart](3376-httpresponsereceivestart.md)|信息|Http 传输已开始接收消息。|HTTP|  
|[3377 - SocketReadStop](3377-socketreadstop.md)|“详细”|SocketId：%1 从“%3”读取“%2”个字节。|TCP|  
|[3378 - SocketAsyncReadStop](3378-socketasyncreadstop.md)|“详细”|SocketId：%1 从“%3”读取“%2”个字节。|TCP|  
|[3379 - SocketWriteStart](3379-socketwritestart.md)|“详细”|SocketId：%1 正在将“%2”个字节写入“%3”。|TCP|  
|[3380 - SocketAsyncWriteStart](3380-socketasyncwritestart.md)|“详细”|SocketId：%1 正在将“%2”个字节写入“%3”。|TCP|  
|[3381 - SequenceAcknowledgementSent](3381-sequenceacknowledgementsent.md)|“详细”|SessionId：%1 确认已发送。|通道|  
|[3382 - ClientReliableSessionReconnect](3382-clientreliablesessionreconnect.md)|信息|SessionId：%1 正在重新连接。|通道|  
|[3383 - ReliableSessionChannelFaulted](3383-reliablesessionchannelfaulted.md)|信息|SessionId：%1 出错。|通道|  
|[3384 - WindowsStreamSecurityOnInitiateUpgrade](3384-windowsstreamsecurityoninitiateupgrade.md)|“详细”|WindowsStreamSecurity 正在启动安全升级。|安全性|  
|[3385 - WindowsStreamSecurityOnAcceptUpgrade](3385-windowsstreamsecurityonacceptupgrade.md)|“详细”|Windows 正在对接受升级的安全性进行流式处理。|安全性|  
|[3386 - SocketConnectionAbort](3386-socketconnectionabort.md)|警告|SocketId：%1 正在中止。|TCP|  
|[3388 - HttpGetContextStart](3388-httpgetcontextstart.md)|“详细”|HttpGetContext 启动。|HTTP|  
|[3389 - ClientSendPreambleStart](3389-clientsendpreamblestart.md)|“详细”|客户端开始发送前导码。|通道|  
|[3390 - ClientSendPreambleStop](3390-clientsendpreamblestop.md)|“详细”|客户端停止发送前导码。|通道|  
|[3391 - HttpMessageReceiveFailed](3391-httpmessagereceivefailed.md)|警告|Http 消息接收失败。|HTTP|  
|[3392 - TransactionScopeCreate](3392-transactionscopecreate.md)|信息|正在使用 LocalIdentifier“%1”和 DistributedIdentifier“%2”创建 TransactionScope。|ServiceModel|  
|[3393 - StreamedMessageReadByEncoder](3393-streamedmessagereadbyencoder.md)|信息|编码器已读取流处理消息。|通道|  
|[3394 - StreamedMessageWrittenByEncoder](3394-streamedmessagewrittenbyencoder.md)|信息|编码器已写入流处理消息。|通道|  
|[3395 - MessageWrittenAsynchronouslyByEncoder](3395-messagewrittenasynchronouslybyencoder.md)|信息|编码器已以异步方式写入消息。|通道|  
|[3396 - BufferedAsyncWriteStart](3396-bufferedasyncwritestart.md)|信息|BufferId：%1 已完成将“%2”个字节写入基础流。|通道|  
|[3397 - BufferedAsyncWriteStop](3397-bufferedasyncwritestop.md)|信息|编码器已以异步方式写入消息。|通道|  
|[3398 - PipeSharedMemoryCreated](3398-pipesharedmemorycreated.md)|“详细”|已在“%1”处创建管道共享内存。|通道|  
|[3399 - NamedPipeCreated](3399-namedpipecreated.md)|“详细”|已创建 NamedPipe“%1”。|通道|  
|[3401 - SignatureVerificationStart](3401-signatureverificationstart.md)|“详细”|签名验证已开始。|安全性|  
|[3402 - SignatureVerificationSuccess](3402-signatureverificationsuccess.md)|“详细”|签名验证已成功。|安全性|  
|[3403 - WrappedKeyDecryptionStart](3403-wrappedkeydecryptionstart.md)|“详细”|封装的密钥解密已开始。|安全性|  
|[3404 - WrappedKeyDecryptionSuccess](3404-wrappedkeydecryptionsuccess.md)|“详细”|封装的密钥解密已成功。|安全性|  
|[3405 - EncryptedDataProcessingStart](3405-encrypteddataprocessingstart.md)|“详细”|加密数据处理已开始。|安全性|  
|[3406 - EncryptedDataProcessingSuccess](3406-encrypteddataprocessingsuccess.md)|“详细”|加密数据处理已成功。|安全性|  
|[3407 - HttpPipelineProcessInboundRequestStart](3407-httppipelineprocessinboundrequeststart.md)|“详细”|Http 消息处理程序已开始处理入站请求。|HTTP|  
|[3408 - HttpPipelineBeginProcessInboundRequestStart](3408-httppipelinebeginprocessinboundrequeststart.md)|“详细”|Http 消息处理程序已开始以异步方式处理入站请求。|HTTP|  
|[3409 - HttpPipelineProcessInboundRequestStop](3409-httppipelineprocessinboundrequeststop.md)|“详细”|Http 消息处理程序已完成处理入站请求。|HTTP|  
|[3410 - HttpPipelineFaulted](3410-httppipelinefaulted.md)|警告|Http 消息处理程序出错。|HTTP|  
|[3411 - HttpPipelineTimeoutException](3411-httppipelinetimeoutexception.md)|错误|WebSocket 连接超时。|HTTP|  
|[3412 - HttpPipelineProcessResponseStart](3412-httppipelineprocessresponsestart.md)|“详细”|Http 消息处理程序已开始处理响应。|HTTP|  
|[3413 - HttpPipelineBeginProcessResponseStart](3413-httppipelinebeginprocessresponsestart.md)|“详细”|Http 消息处理程序已开始以异步方式处理响应。|HTTP|  
|[3414 - HttpPipelineProcessResponseStop](3414-httppipelineprocessresponsestop.md)|“详细”|Http 消息处理程序已完成处理响应。|HTTP|  
|[3415 - WebSocketConnectionRequestSendStart](3415-websocketconnectionrequestsendstart.md)|“详细”|针对“%1”的 WebSocket 连接请求发送开始。|HTTP|  
|[3416 - WebSocketConnectionRequestSendStop](3416-websocketconnectionrequestsendstop.md)|“详细”|WebSocketId：%1 连接请求已发送。|HTTP|  
|[3417 - WebSocketConnectionAcceptStart](3417-websocketconnectionacceptstart.md)|“详细”|WebSocket 连接接受开始。|HTTP|  
|[3418 - WebSocketConnectionAccepted](3418-websocketconnectionaccepted.md)|“详细”|WebSocketId：%1 连接已接受。|HTTP|  
|[3419 - WebSocketConnectionDeclined](3419-websocketconnectiondeclined.md)|错误|WebSocket 连接被拒绝，状态代码为“%1”|HTTP|  
|[3420 - WebSocketConnectionFailed](3420-websocketconnectionfailed.md)|错误|WebSocket 连接请求失败：“%1”|HTTP|  
|[3421 - WebSocketConnectionAborted](3421-websocketconnectionaborted.md)|错误|WebSocketId：%1 连接已中止。|HTTP|  
|[3422 - WebSocketAsyncWriteStart](3422-websocketasyncwritestart.md)|“详细”|WebSocketId：%1 正在将“%2”个字节写入“%3”。|HTTP|  
|[3423 - WebSocketAsyncWriteStop](3423-websocketasyncwritestop.md)|“详细”|WebSocketId：%1 异步写入停止。|HTTP|  
|[3424 - WebSocketAsyncReadStart](3424-websocketasyncreadstart.md)|“详细”|WebSocketId：%1 读取开始。|HTTP|  
|[3425 - WebSocketAsyncReadStop](3425-websocketasyncreadstop.md)|“详细”|WebSocketId：%1 从“%3”读取“%2”个字节。|HTTP|  
|[3426 - WebSocketCloseSent](3426-websocketclosesent.md)|“详细”|WebSocketId：%1 正在向“%2”发送关闭消息，关闭状态为“%3”。|HTTP|  
|[3427 - WebSocketCloseOutputSent](3427-websocketcloseoutputsent.md)|“详细”|WebSocketId：%1 正在向“%2”发送关闭输出消息，关闭状态为“%3”。|HTTP|  
|[3428 - WebSocketConnectionClosed](3428-websocketconnectionclosed.md)|“详细”|WebSocketId：%1 连接已关闭。|HTTP|  
|[3429 - WebSocketCloseStatusReceived](3429-websocketclosestatusreceived.md)|“详细”|WebSocketId：%1 收到了连接关闭消息，状态为“%2”。|HTTP|  
|[3430 - WebSocketUseVersionFromClientWebSocketFactory](3430-websocketuseversionfromclientwebsocketfactory.md)|“详细”|正在使用来自类型为“%1”的客户端 WebSocket 工厂的 WebSocketVersion。|HTTP|  
|[3431 - WebSocketCreateClientWebSocketWithFactory](3431-websocketcreateclientwebsocketwithfactory.md)|“详细”|正在创建包含类型为“%1”的工厂的客户端 WebSocket。|HTTP|  
|[3553 - XamlServicesLoadStart](3553-xamlservicesloadstart.md)|信息|XamlServicesLoad 开始|WebHost|  
|[3554 - XamlServicesLoadStop](3554-xamlservicesloadstop.md)|信息|XamlServicesLoad 停止|WebHost|  
|[3555 - CreateWorkflowServiceHostStart](3555-createworkflowservicehoststart.md)|信息|CreateWorkflowServiceHost 开始|WebHost|  
|[3556 - CreateWorkflowServiceHostStop](3556-createworkflowservicehoststop.md)|信息|CreateWorkflowServiceHost 停止|WebHost|  
|[3558 - ServiceActivationStart](3558-serviceactivationstart.md)|信息|服务激活开始|WebHost|  
|[3559 - ServiceActivationStop](3559-serviceactivationstop.md)|信息|服务激活停止|WebHost|  
|[3560 - ServiceActivationAvailableMemory](3560-serviceactivationavailablememory.md)|“详细”|可用内存（字节）：%1|Quota|  
|[3800 - RoutingServiceClosingClient](3800-routingserviceclosingclient.md)|信息|路由服务正在关闭客户端“%1”。|RoutingServices|  
|[3800 - RoutingServiceClosingClient](3800-routingserviceclosingclient.md)|警告|路由服务客户端“%1”出现故障。|RoutingServices|  
|[3802 - RoutingServiceCompletingOneWay](3802-routingservicecompletingoneway.md)|信息|路由服务单向消息正在完成。|RoutingServices|  
|[3803 - RoutingServiceProcessingFailure](3803-routingserviceprocessingfailure.md)|错误|在地址为“%1”的终结点上处理消息时，路由服务失败。|RoutingServices|  
|[3804 - RoutingServiceCreatingClientForEndpoint](3804-routingservicecreatingclientforendpoint.md)|信息|路由服务正在为终结点“%1”创建客户端。|RoutingServices|  
|[3805 - RoutingServiceDisplayConfig](3805-routingservicedisplayconfig.md)|“详细”|使用 RouteOnHeadersOnly %1、SoapProcessingEnabled %2 和 EnsureOrderedDispatch %3 配置路由服务。|RoutingServices|  
|[3807 - RoutingServiceCompletingTwoWay](3807-routingservicecompletingtwoway.md)|信息|路由服务请求答复消息正在完成。|RoutingServices|  
|[3809 - RoutingServiceMessageRoutedToEndpoints](3809-routingservicemessageroutedtoendpoints.md)|“详细”|路由服务已将 ID 为“%1”的消息路由到 %2 个终结点列表。|RoutingServices|  
|[3810 - RoutingServiceConfigurationApplied](3810-routingserviceconfigurationapplied.md)|信息|一个新的 RoutingConfiguration 已应用于路由服务。|RoutingServices|  
|[3815 - RoutingServiceProcessingMessage](3815-routingserviceprocessingmessage.md)|信息|路由服务正在处理事务 %4 中收到的 ID 为“%1”、操作为“%2”、入站 URL 为“%3”的消息。|RoutingServices|  
|[3816 - RoutingServiceTransmittingMessage](3816-routingservicetransmittingmessage.md)|信息|路由服务正在将 ID 为“%1”的消息 [操作 %2] 传输到“%3”。|RoutingServices|  
|[3817 - RoutingServiceCommittingTransaction](3817-routingservicecommittingtransaction.md)|信息|路由服务正在提交 ID 为“%1”的事务。|RoutingServices|  
|[3818 - RoutingServiceDuplexCallbackException](3818-routingserviceduplexcallbackexception.md)|错误|路由服务组件 %1 遇到了双向回调异常。|RoutingServices|  
|[3819 - RoutingServiceMovedToBackup](3819-routingservicemovedtobackup.md)|信息|ID 为“%1”的路由服务消息 [操作 %2] 已移动以便备份终结点“%3”。|RoutingServices|  
|[3820 - RoutingServiceCreatingTransaction](3820-routingservicecreatingtransaction.md)|信息|路由服务创建了一个 ID 为“%1”的新事务来处理消息。|RoutingServices|  
|[3821 - RoutingServiceCloseFailed](3821-routingserviceclosefailed.md)|警告|关闭出站客户端“%1”时，路由服务失败。|RoutingServices|  
|[3822 - RoutingServiceSendingResponse](3822-routingservicesendingresponse.md)|信息|路由服务正在发回含有操作“%1”的响应消息。|RoutingServices|  
|[3823 - RoutingServiceSendingFaultResponse](3823-routingservicesendingfaultresponse.md)|警告|路由服务正在发回含有操作“%1”的错误响应消息。|RoutingServices|  
|[3824 - RoutingServiceCompletingReceiveContext](3824-routingservicecompletingreceivecontext.md)|“详细”|路由服务正在为 ID 是“%1”的消息调用 ReceiveContext.Complete。|RoutingServices|  
|[3825 - RoutingServiceAbandoningReceiveContext](3825-routingserviceabandoningreceivecontext.md)|警告|路由服务正在为 ID 是“%1”的消息调用 ReceiveContext.Abandon。|RoutingServices|  
|[3826 - RoutingServiceUsingExistingTransaction](3826-routingserviceusingexistingtransaction.md)|“详细”|路由服务将使用现有事务“%1”发送消息。|RoutingServices|  
|[3827 - RoutingServiceTransmitFailed](3827-routingservicetransmitfailed.md)|警告|向“%1”发送时，路由服务失败。|RoutingServices|  
|[3828 - RoutingServiceFilterTableMatchStart](3828-routingservicefiltertablematchstart.md)|信息|路由服务 MessageFilterTable 匹配开始。|RoutingServices|  
|[3829 - RoutingServiceFilterTableMatchStop](3829-routingservicefiltertablematchstop.md)|信息|路由服务 MessageFilterTable 匹配停止。|RoutingServices|  
|[3830 - RoutingServiceAbortingChannel](3830-routingserviceabortingchannel.md)|“详细”|路由服务正在通道“%1”上调用中止。|RoutingServices|  
|[3831 - RoutingServiceHandledException](3831-routingservicehandledexception.md)|“详细”|路由服务已处理异常。|RoutingServices|  
|[3832 - RoutingServiceTransmitSucceeded](3832-routingservicetransmitsucceeded.md)|信息|路由服务已成功将 ID 为“%1”的消息 [操作 %2] 传输到“%3”。|RoutingServices|  
|[4001 - TransportListenerSessionsReceived](4001-transportlistenersessionsreceived.md)|“详细”|通过“%1”接收了传输侦听器会话|ActivationServices|  
|[4002 - FailFastException](4002-failfastexception.md)|严重|FailFastException。|ActivationServices|  
|[4003 - ServiceStartPipeError](4003-servicestartpipeerror.md)|错误|服务启动管道出错。|ActivationServices|  
|[4008 - DispatchSessionStart](4008-dispatchsessionstart.md)|“详细”|会话调度已启动。|ActivationServices|  
|[4008 - DispatchSessionStart](4008-dispatchsessionstart.md)|警告|对“%1”的会话调度失败，因为挂起的会话队列已满，其中包含“%2”个挂起项。|ActivationServices|  
|[4011 - MessageQueueRegisterStart](4011-messagequeueregisterstart.md)|“详细”|消息队列注册开始。|ActivationServices|  
|[4012 - MessageQueueRegisterAbort](4012-messagequeueregisterabort.md)|错误|URI“%2”的消息队列注册已中止，状态为“%1”。|ActivationServices|  
|[4013 - MessageQueueUnregisterSucceeded](4013-messagequeueunregistersucceeded.md)|“详细”|URI“%1”的消息队列注销已成功。|ActivationServices|  
|[4014 - MessageQueueRegisterFailed](4014-messagequeueregisterfailed.md)|错误|URI“%1”的消息队列注册已失败，状态为“%2”。|ActivationServices|  
|[4015 - MessageQueueRegisterCompleted](4015-messagequeueregistercompleted.md)|信息|URI“%1”的消息队列注册已完成。|ActivationServices|  
|[4016 - MessageQueueDuplicatedSocketError](4016-messagequeueduplicatedsocketerror.md)|错误|消息队列无法复制套接字。|ActivationServices|  
|[4019 - MessageQueueDuplicatedSocketComplete](4019-messagequeueduplicatedsocketcomplete.md)|“详细”|MessageQueueDuplicatedSocketComplete|ActivationServices|  
|[4020 - TcpTransportListenerListeningStart](4020-tcptransportlistenerlisteningstart.md)|“详细”|Tcp 传输侦听器正在开始侦听 URI“%1”。|ActivationServices|  
|[4021 - TcpTransportListenerListeningStop](4021-tcptransportlistenerlisteningstop.md)|“详细”|Tcp 传输侦听器正在侦听。|ActivationServices|  
|[4022 - WebhostUnregisterProtocolFailed](4022-webhostunregisterprotocolfailed.md)|错误|错误代码：%1|ActivationServices|  
|[4023 - WasCloseAllListenerChannelInstancesCompleted](4023-wasclosealllistenerchannelinstancescompleted.md)|信息|正在关闭所有已完成的侦听器通道实例。|ActivationServices|  
|[4024 - WasCloseAllListenerChannelInstancesFailed](4024-wasclosealllistenerchannelinstancesfailed.md)|错误|错误代码：%1|ActivationServices|  
|[4025 - OpenListenerChannelInstanceFailed](4025-openlistenerchannelinstancefailed.md)|错误|错误代码：%1|ActivationServices|  
|[4026 - WasConnected](4026-wasconnected.md)|“详细”|WAS 已连接。|ActivationServices|  
|[4027 - WasDisconnected](4027-wasdisconnected.md)|“详细”|WAS 已断开连接。|ActivationServices|  
|[4028 - PipeTransportListenerListeningStart](4028-pipetransportlistenerlisteningstart.md)|“详细”|管道传输侦听器开始侦听 URI“%1”。|ActivationServices|  
|[4029 - PipeTransportListenerListeningStop](4029-pipetransportlistenerlisteningstop.md)|“详细”|管道传输侦听器停止侦听。|ActivationServices|  
|[4030 - DispatchSessionSuccess](4030-dispatchsessionsuccess.md)|信息|会话调度成功。|ActivationServices|  
|[4031 - DispatchSessionFailed](4031-dispatchsessionfailed.md)|错误|会话调度失败。|ActivationServices|  
|[4032 - WasConnectionTimedout](4032-wasconnectiontimedout.md)|严重|WAS 连接超时。|ActivationServices|  
|[4033 - RoutingTableLookupStart](4033-routingtablelookupstart.md)|“详细”|已开始路由表查找。|ActivationServices|  
|[4034 - RoutingTableLookupStop](4034-routingtablelookupstop.md)|“详细”|已完成路由表查找。|ActivationServices|  
|[4035 - PendingSessionQueueRatio](4035-pendingsessionqueueratio.md)|“详细”|挂起会话队列比率：%1/%2|Quota|  
|[4600 - MessageLogEventSizeExceeded](4600-messagelogeventsizeexceeded.md)|警告|无法记录消息，因为它超出了 ETW 事件大小|WCFMessageLogging|  
|[4801 - DiscoveryClientInClientChannelFailedToClose](4801-discoveryclientinclientchannelfailedtoclose.md)|警告|在 DiscoveryClientChannel 内部创建的 DiscoveryClient 无法关闭，因此已中止。|发现|  
|[4802 - DiscoveryClientProtocolExceptionSuppressed](4802-discoveryclientprotocolexceptionsuppressed.md)|信息|关闭 DiscoveryClient 时，ProtocolException 被禁止。 这可能是由于 DiscoveryService 仍在尝试向 DiscoveryClient 发送响应。|发现|  
|[4803 - DiscoveryClientReceivedMulticastSuppression](4803-discoveryclientreceivedmulticastsuppression.md)|信息|DiscoveryClient 收到来自 DiscoveryProxy 的多播禁止消息。|发现|  
|[4804 - DiscoveryMessageReceivedAfterOperationCompleted](4804-discoverymessagereceivedafteroperationcompleted.md)|信息|messageId 为“%2”的 %1 消息已被 DiscoveryClient 丢弃，因为相应的 %3 操作已完成。|发现|  
|[4805 - DiscoveryMessageWithInvalidContent](4805-discoverymessagewithinvalidcontent.md)|警告|messageId 为“%2”的 %1 消息由于具有无效内容而被丢弃。|发现|  
|[4806 - DiscoveryMessageWithInvalidRelatesToOrOperationCompleted](4806-discoverymessagewithinvalidrelatestooroperationcompleted.md)|警告|messageId 为“%2”且 relatesTo 为“%3”的 %1 消息已被 DiscoveryClient 丢弃，因为相应的 %4 操作已完成或 relatesTo 值无效。|发现|  
|[4807 - DiscoveryMessageWithInvalidReplyTo](4807-discoverymessagewithinvalidreplyto.md)|警告|messageId 为“%1”的发现请求消息由于具有无效 ReplyTo 地址而被丢弃。|发现|  
|[4808 - DiscoveryMessageWithNoContent](4808-discoverymessagewithnocontent.md)|警告|%1 消息由于没有任何内容而被丢弃。|发现|  
|[4809 - DiscoveryMessageWithNullMessageId](4809-discoverymessagewithnullmessageid.md)|警告|%1 消息由于消息标头不包含所需的 MessageId 属性而被丢弃。|发现|  
|[4810 - DiscoveryMessageWithNullMessageSequence](4810-discoverymessagewithnullmessagesequence.md)|警告|messageId 为“%2”的 %1 消息由于没有 DiscoveryMessageSequence 属性而被 DiscoveryClient 丢弃。|发现|  
|[4811 - DiscoveryMessageWithNullRelatesTo](4811-discoverymessagewithnullrelatesto.md)|警告|messageId 为“%2”的 %1 消息由于消息标头不包含所需的 RelatesTo 属性而被 DiscoveryClient 丢弃。|发现|  
|[4812 - DiscoveryMessageWithNullReplyTo](4812-discoverymessagewithnullreplyto.md)|警告|messageId 为“%1”的发现请求消息由于没有 ReplyTo 地址而被丢弃。|发现|  
|[4813 - DuplicateDiscoveryMessage](4813-duplicatediscoverymessage.md)|警告|messageId 为“%2”的 %1 消息由于重复而被丢弃。|发现|  
|[4814 - EndpointDiscoverabilityDisabled](4814-endpointdiscoverabilitydisabled.md)|信息|已禁用 EndpointAddress 为“%1”和 ListenUri 为“%2”的终结点的可发现性。|发现|  
|[4814 - EndpointDiscoverabilityDisabled](4814-endpointdiscoverabilitydisabled.md)|信息|已启用 EndpointAddress 为“%1”和 ListenUri 为“%2”的终结点的可发现性。|发现|  
|[4816 - FindInitiatedInDiscoveryClientChannel](4816-findinitiatedindiscoveryclientchannel.md)|“详细”|在 DiscoveryClientChannel 中启动了查找操作以发现终结点。|发现|  
|[4817 - InnerChannelCreationFailed](4817-innerchannelcreationfailed.md)|警告|DiscoveryClientChannel 无法使用 EndpointAddress 为“%1”和 Via 为“%2”的已发现终结点创建通道。 DiscoveryClientChannel 现在将尝试使用下一个可用的已发现终结点。|发现|  
|[4818 - InnerChannelOpenFailed](4818-innerchannelopenfailed.md)|警告|DiscoveryClientChannel 无法使用 EndpointAddress 为“%1”和 Via 为“%2”的已发现终结点打开通道。 DiscoveryClientChannel 现在将尝试使用下一个可用的已发现终结点。|发现|  
|[4819 - InnerChannelOpenSucceeded](4819-innerchannelopensucceeded.md)|信息|DiscoveryClientChannel 成功发现了一个终结点并使用该终结点打开了通道。 使用 EndpointAddress=“%1”和 Via=“%2”将客户端连接到服务。|发现|  
|[4820 - SynchronizationContextReset](4820-synchronizationcontextreset.md)|信息|SynchronizationContext 已由 DiscoveryClientChannel 重置为 %1 的原始值。|发现|  
|[4821 - SynchronizationContextSetToNull](4821-synchronizationcontextsettonull.md)|信息|在初始化 Find 操作之前，SynchronizationContext 已由 DiscoveryClientChannel 设置为 null。|发现|  
|[5001 - DCSerializeWithSurrogateStart](5001-dcserializewithsurrogatestart.md)|“详细”|DataContract 开始使用代理项序列化 %1。|序列化|  
|[5002 - DCSerializeWithSurrogateStop](5002-dcserializewithsurrogatestop.md)|“详细”|DataContract 停止使用代理项序列化。|序列化|  
|[5003 - DCDeserializeWithSurrogateStart](5003-dcdeserializewithsurrogatestart.md)|“详细”|DataContract 开始使用代理项反序列化 %1。|序列化|  
|[5004 - DCDeserializeWithSurrogateStop](5004-dcdeserializewithsurrogatestop.md)|“详细”|DataContract 停止使用代理项反序列化。|序列化|  
|[5005 - ImportKnownTypesStart](5005-importknowntypesstart.md)|“详细”|ImportKnownTypes 开始。|序列化|  
|[5006 - ImportKnownTypesStop](5006-importknowntypesstop.md)|“详细”|ImportKnownTypes 停止。|序列化|  
|[5007 - DCResolverResolve](5007-dcresolverresolve.md)|“详细”|DataContract 解析程序开始解析 %1。|序列化|  
|[5008 - DCGenWriterStart](5008-dcgenwriterstart.md)|“详细”|DataContract 开始为 %2 生成 %1 编写器。|序列化|  
|[5009 - DCGenWriterStop](5009-dcgenwriterstop.md)|“详细”|DataContract 生成编写器停止。|序列化|  
|[5010 - DCGenReaderStart](5010-dcgenreaderstart.md)|“详细”|DataContract 为 %2 启动生成 %1 读取器。|序列化|  
|[5011 - DCGenReaderStop](5011-dcgenreaderstop.md)|“详细”|DataContract 生成停止。|序列化|  
|[5012 - DCJsonGenReaderStart](5012-dcjsongenreaderstart.md)|“详细”|Json 为 %2 启动生成 %1 读取器。|序列化|  
|[5013 - DCJsonGenReaderStop](5013-dcjsongenreaderstop.md)|“详细”|Json 读取器生成停止。|序列化|  
|[5014 - DCJsonGenWriterStart](5014-dcjsongenwriterstart.md)|“详细”|Json 为 %2 启动生成 %1 编写器。|序列化|  
|[5015 - DCJsonGenWriterStop](5015-dcjsongenwriterstop.md)|“详细”|Json 生成编写器停止。|序列化|  
|[5016 - GenXmlSerializableStart](5016-genxmlserializablestart.md)|“详细”|开始为“%1”生成 Xml 可序列化内容。|序列化|  
|[5017 - GenXmlSerializableStop](5017-genxmlserializablestop.md)|“详细”|生成 Xml 可序列化内容停止。|序列化|  
|[5203 - JsonMessageDecodingStart](5203-jsonmessagedecodingstart.md)|“详细”|JsonMessageEncoder 已开始解码消息。|通道|  
|[5204 - JsonMessageEncodingStart](5204-jsonmessageencodingstart.md)|“详细”|JsonMessageEncoder 已开始编码消息。|通道|  
|[5402 - TokenValidationStarted](5402-tokenvalidationstarted.md)|“详细”|SecurityToken（类型为“%1”，ID 为“%2”）验证已开始。|安全性|  
|[5403 - TokenValidationSuccess](5403-tokenvalidationsuccess.md)|“详细”|SecurityToken（类型为“%1”，ID 为“%2”）验证已成功。|安全性|  
|[5404 - TokenValidationFailure](5404-tokenvalidationfailure.md)|错误|SecurityToken（类型为“%1”，ID 为“%2”）验证已失败。 %3|安全性|  
|[5405 - GetIssuerNameSuccess](5405-getissuernamesuccess.md)|“详细”|从 tokenId %2 检索颁发者名称 %1 已成功。|安全性|  
|[5406 - GetIssuerNameFailure](5406-getissuernamefailure.md)|错误|从 tokenId %1 检索颁发者名称失败。|安全性|  
|[5600 - FederationMessageProcessingStarted](5600-federationmessageprocessingstarted.md)|“详细”|联合身份验证消息处理已开始。|安全性|  
|[5601 - FederationMessageProcessingSuccess](5601-federationmessageprocessingsuccess.md)|“详细”|联合身份验证消息处理已成功。|安全性|  
|[5602 - FederationMessageCreationStarted](5602-federationmessagecreationstarted.md)|“详细”|从窗体发布创建联合身份验证消息已开始。|安全性|  
|[5603 - FederationMessageCreationSuccess](5603-federationmessagecreationsuccess.md)|“详细”|从窗体发布创建联合身份验证消息已成功。|安全性|  
|[5604 - SessionCookieReadingStarted](5604-sessioncookiereadingstarted.md)|“详细”|从会话 Cookie 读取会话令牌已开始。|安全性|  
|[5605 - SessionCookieReadingSuccess](5605-sessioncookiereadingsuccess.md)|“详细”|从会话 Cookie 读取会话令牌已成功。|安全性|  
|[5606 - PrincipalSettingFromSessionTokenStarted](5606-principalsettingfromsessiontokenstarted.md)|“详细”|基于会话令牌的主体设置已开始。|安全性|  
|[5607 - PrincipalSettingFromSessionTokenSuccess](5607-principalsettingfromsessiontokensuccess.md)|“详细”|基于会话令牌的主体设置已成功。|安全性|  
|[57393 - AppDomainUnload](57393-appdomainunload.md)|信息|AppDomain 正在卸载。 AppDomain.FriendlyName %1，ProcessName %2，ProcessId %3。|基础结构|  
|[57394 - HandledException](57394-handledexception.md)|信息|正在处理异常。|基础结构|  
|[57395 - ShipAssertExceptionMessage](57395-shipassertexceptionmessage.md)|错误|发生了意外失败。 应用程序不应尝试处理此错误。 出于诊断目的，此英语消息与下列失败关联：%1。|基础结构|  
|[57396 - ThrowingException](57396-throwingexception.md)|警告|正在引发异常。 源 %1。|基础结构|  
|[57397 - UnhandledException](57397-unhandledexception.md)|严重|未经处理的异常。|基础结构|  
|[57399 - TraceCodeEventLogCritical](57399-tracecodeeventlogcritical.md)|严重|已写入到 EventLog。|基础结构|  
|[57400 - TraceCodeEventLogError](57400-tracecodeeventlogerror.md)|错误|已写入到 EventLog。|基础结构|  
|[57401 - TraceCodeEventLogInfo](57401-tracecodeeventloginfo.md)|信息|已写入到 EventLog。|基础结构|  
|[57402 - TraceCodeEventLogVerbose](57402-tracecodeeventlogverbose.md)|“详细”|已写入到 EventLog。|基础结构|  
|[57403 - TraceCodeEventLogWarning](57403-tracecodeeventlogwarning.md)|警告|已写入到 EventLog。|基础结构|  
|[57404 - HandledExceptionWarning](57404-handledexceptionwarning.md)|警告|正在处理异常。|基础结构|  
|[62326 - HttpHandlerPickedForUrl](62326-httphandlerpickedforurl.md)|信息|URL“%1”承载含有根元素类型“%2”的 XAML 文档。 选取 HTTP 处理程序类型“%3”为对此 URL 做出的所有请求提供服务。|WebHost|
