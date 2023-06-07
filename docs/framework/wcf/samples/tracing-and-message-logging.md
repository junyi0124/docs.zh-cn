---
title: 跟踪和消息日志记录
description: '了解如何使用服务跟踪查看器工具 ( # A0) 查看使用此 WFC 示例的跟踪和消息日志。'
ms.date: 03/30/2017
helpviewer_keywords:
- Tracing and logging
ms.assetid: a4f39bfc-3c5e-4d51-a312-71c5c3ce0afd
ms.openlocfilehash: ec29ac03e8930bd30ccd7e90dce3993ca9e7443a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255411"
---
# <a name="tracing-and-message-logging"></a>跟踪和消息日志记录

本示例演示如何启用跟踪和消息日志记录。 使用 [服务跟踪查看器工具 ](../service-trace-viewer-tool-svctraceviewer-exe.md)查看生成的跟踪和消息日志 ( # A0) 。 此示例基于 [入门](getting-started-sample.md)。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
## <a name="tracing"></a>跟踪  

 Windows Communication Foundation (WCF) 使用在命名空间中定义的跟踪机制 <xref:System.Diagnostics> 。 在此跟踪模型中，由应用程序实现的跟踪源生成跟踪数据。 每个源均由名称进行标识。 跟踪使用程序会创建针对要为其检索信息的跟踪源的侦听器。 若要接收跟踪数据，您必须创建针对该跟踪源的侦听器。 在 WCF 中，可以通过设置服务模型跟踪源，通过将以下代码添加到服务或客户端的配置文件来完成此 `switchValue` 操作：  
  
```xml  
<system.diagnostics>  
    <sources>  
      <source name="System.ServiceModel" switchValue="Information,ActivityTracing"  
        propagateActivity="true">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
      <source name="System.ServiceModel.MessageLogging">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add initializeData="C:\logs\TracingAndLogging-service.svclog" type="System.Diagnostics.XmlWriterTraceListener"  
        name="xml" />  
    </sharedListeners>  
    <trace autoflush="true" />  
</system.diagnostics>  
```  
  
 有关跟踪源的详细信息，请参阅 [配置跟踪](../diagnostics/tracing/configuring-tracing.md) 主题中的跟踪源部分。  
  
## <a name="activity-tracing-and-propagation"></a>活动跟踪和传播  

 如果在 `ActivityTracing` `propagateActivity` `true` 客户端和服务的跟踪源中启用并设置为，则会在 `system.ServiceModel` 处理 () 活动的逻辑单元中，通过活动传输) ，并跨跨多个终结点的活动 (活动 ID 传播 (来关联跟踪。  
  
 这三种机制（活动、传输和传播）可帮助您使用服务跟踪查看器工具更快地找到错误的根本原因。 有关详细信息，请参阅 [使用服务跟踪查看器查看相关跟踪和故障排除](../diagnostics/tracing/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md)。  
  
 通过创建用户定义的活动跟踪可以扩展 ServiceModel 提供的跟踪。 用户定义的活动跟踪允许用户创建跟踪活动，以便执行下列操作：  
  
- 将跟踪分组到逻辑工作单元。  
  
- 通过传输和传播关联活动。  
  
- 降低 WCF 跟踪的性能成本 (例如，日志文件的磁盘空间成本) 。  
  
 有关用户定义的活动跟踪的详细信息，请参阅 [扩展跟踪](extending-tracing.md) 示例。  
  
## <a name="message-logging"></a>消息日志记录  

 可以在任何 WCF 应用程序的客户端和服务上启用消息日志记录。 若要启动消息日志记录，必须将下面的代码添加到客户端或服务：  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <diagnostics>  
      <!-- Enable Message Logging here. -->  
      <!-- log all messages received or sent at the transport or service model levels -->  
      <messageLogging logEntireMessage="true"  
                      maxMessagesToLog="300"  
                      logMessagesAtServiceLevel="true"  
                      logMalformedMessages="true"  
                      logMessagesAtTransportLevel="true" />  
    </diagnostics>  
  </system.serviceModel>  
</configuration>  
```  
  
 记录消息时，跟踪类型取决于是在客户端还是在服务器上进行跟踪。 例如，发送到客户端的“Add”消息将在客户端的“TransportWrite”类别下跟踪，而同一消息将在服务的“TransportRead”类别下跟踪。  
  
 通过将下面的代码添加到客户端的 App.config 文件或服务的 Web.config 文件的 <xref:System.Diagnostics> 节，可以配置跟踪侦听器：  
  
```xml  
<system.diagnostics>  
    <sources>  
      <source name="System.ServiceModel" switchValue="Information,ActivityTracing"  
        propagateActivity="true">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
      <source name="System.ServiceModel.MessageLogging">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add initializeData="C:\logs\TracingAndLogging-client.svclog" type="System.Diagnostics.XmlWriterTraceListener"  
        name="xml" />  
    </sharedListeners>  
    <trace autoflush="true" />  
  </system.diagnostics>  
```  
  
 消息将以 XML 格式记录在配置文件中指定的目标目录中。  
  
> [!NOTE]
> 如果开始时未创建日志目录，则不会创建跟踪文件。 请确保 C:\logs\ 目录存在，或在侦听器配置中指定一个替换日志记录目录。 有关更多信息，请参见本文档最后的初始安装说明。  
  
 有关消息日志记录的详细信息，请参阅 [配置消息日志记录](../diagnostics/configuring-message-logging.md) 主题。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 运行“跟踪和消息日志记录”示例之前，创建 C:\logs\ 目录以便服务向其中写入 .svclog 文件。 此目录的名称在配置文件中定义为要记录的跟踪和消息的路径，并可以进行更改。 向用户授予对日志目录的网络服务写权限。  
  
3. 若要生成 c #、c + + 或 Visual Basic .NET 版本的解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
4. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\TracingAndLogging`  
  
## <a name="see-also"></a>另请参阅

- [跟踪](../diagnostics/tracing/index.md)
- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
