---
title: 诊断事务应用程序
ms.date: 03/30/2017
ms.assetid: 4a993492-1088-4d10-871b-0c09916af05f
ms.openlocfilehash: 696ebe7249a8388eaaf38a678581e28d472e821a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96290239"
---
# <a name="diagnosing-transactional-applications"></a>诊断事务应用程序

本主题介绍如何使用 Windows Communication Foundation (WCF) 管理和诊断功能对事务应用程序进行故障排除。  
  
## <a name="performance-counters"></a>性能计数器  

 WCF 提供了一组标准的性能计数器，用于测量事务应用程序的性能。 有关详细信息，请参阅 [性能计时器](../diagnostics/performance-counters/index.md)。  
  
 性能计数器分为三个不同级别：服务、终结点和操作，如下表所述。  
  
### <a name="service-performance-counters"></a>服务性能计数器  
  
|性能计数器|说明|  
|-------------------------|-----------------|  
|Transactions Flowed（流动的事务数）|流向此服务中的操作的事务数。 每当发送到服务的消息中存在事务时，此计数器即会递增。|  
|Transactions Flowed Per Second（每秒流动的事务数）|每秒钟流向此服务中的操作的事务数。 每当发送到服务的消息中存在事务时，此计数器即会递增。|  
|Transacted Operations Committed（已提交的事务处理操作次数）|此服务中执行的事务处理操作数，这些操作的事务已完成，结果已提交。 这些操作下完成的工作已完全提交。 资源按照操作中完成的工作进行更新。|  
|Transacted Operations Committed Per Second（每秒提交的事务处理操作次数）|此服务中每秒执行的事务处理操作数，这些操作的事务已完成，结果已提交。 这些操作下完成的工作已完全提交。 资源按照操作中完成的工作进行更新。|  
|Transacted Operations Aborted（已中止的事务处理操作次数）|此服务中执行的事务处理操作数，这些操作的事务已完成，结果已中止。 这些操作下完成的工作已回滚。 资源恢复到它们的先前状态。|  
|Transacted Operations Aborted Per Second（每秒中止的事务处理操作次数）|此服务中每秒执行的事务处理操作数，这些操作的事务已完成，结果已中止。 这些操作下完成的工作已回滚。 资源恢复到它们的先前状态。|  
|Transacted Operations In Doubt（不确定的事务处理操作次数）|此服务中执行的事务处理操作数，这些操作的事务已完成，结果不确定。 完成的工作具有不确定的结果，即处于不确定状态。 资源保留在挂起的结果中。|  
|Transacted Operations In Doubt Per Second（每秒不确定的事务处理操作次数）|此服务中每秒执行的事务处理操作数，这些操作的事务已完成，结果不确定。 完成的工作具有不确定的结果，即处于不确定状态。 资源保留在挂起的结果中。|  
  
### <a name="endpoint-performance-counters"></a>终结点性能计数器  
  
|性能计数器|说明|  
|-------------------------|-----------------|  
|Transactions Flowed（流动的事务数）|流向此终结点上的操作的事务数。 每当发送到终结点的消息中存在事务时，此计数器即会递增。|  
|Transactions Flowed Per Second（每秒流动的事务数）|每秒钟流向此终结点上的操作的事务数。 每当发送到终结点的消息中存在事务时，此计数器即会递增。|  
  
### <a name="operation-performance-counters"></a>操作性能计数器  
  
|性能计数器|说明|  
|-------------------------|-----------------|  
|Transactions Flowed（流动的事务数）|流向此终结点上的操作的事务数。 每当发送到终结点的消息中存在事务时，此计数器即会递增。|  
|Transactions Flowed Per Second（每秒流动的事务数）|每秒钟流向此终结点上的操作的事务数。 每当发送到终结点的消息中存在事务时，此计数器即会递增。|  
  
## <a name="windows-management-instrumentation"></a>Windows Management Instrumentation  

 WCF 通过 WCF Windows Management Instrumentation (WMI) 提供程序，在运行时公开服务的检测数据。 有关访问 WMI 数据的详细信息，请参阅 [使用 Windows Management Instrumentation 进行诊断](../diagnostics/wmi/index.md)。  
  
 很多 WMI 只读属性都指示为服务应用的事务设置。 下表列出了所有这些设置。  
  
 在服务上，`ServiceBehaviorAttribute` 具有以下属性。  
  
|名称|类型|描述|  
|----------|----------|-----------------|  
|ReleaseServiceInstanceOnTransactionComplete|Boolean|指定当前事务完成后，是否回收服务对象。|  
|TransactionAutoCompleteOnSessionClose|Boolean|指定当前会话关闭时，挂起的事务是否已完成。|  
|TransactionIsolationLevel|一个字符串，它包含 <xref:System.Transactions.IsolationLevel> 枚举的一个有效值。|指定此服务支持的事务隔离级别。|  
|TransactionTimeout|<xref:System.DateTime>|指定必须完成事务处理的期限。|  
  
 `ServiceTimeoutsBehavior` 具有以下属性。  
  
|名称|类型|描述|  
|----------|----------|-----------------|  
|TransactionTimeout|<xref:System.DateTime>|指定必须完成事务处理的期限。|  
  
 在绑定上，`TransactionFlowBindingElement` 具有以下属性。  
  
|名称|类型|描述|  
|----------|----------|-----------------|  
|TransactionProtocol|一个字符串，它包含 <xref:System.ServiceModel.TransactionProtocol> 类型的一个有效值。|指定在流动事务时使用的事务处理协议。|  
|TransactionFlow|Boolean|指定是否启用传入事务流。|  
  
 在操作上，`OperationBehaviorAttribute` 具有以下属性。  
  
|名称|类型|描述|  
|----------|----------|-----------------|  
|TransactionAutoComplete|Boolean|指定如果未发生未处理的异常，是否自动提交当前事务。|  
|TransactionScopeRequired|Boolean|指定操作是否需要事务。|  
  
 在操作上，`TransactionFlowAttribute` 具有以下属性。  
  
|名称|类型|描述|  
|----------|----------|-----------------|  
|TransactionFlowOption|一个字符串，它包含 <xref:System.ServiceModel.TransactionFlowOption> 枚举的一个有效值。|指定需要事务流的范围。|  
  
## <a name="tracing"></a>跟踪  

 通过跟踪，可以监视和分析事务应用程序中的错误。 可以使用以下方式启用跟踪：  
  
- 标准 WCF 跟踪  
  
     这种类型的跟踪与跟踪任何 WCF 应用程序相同。 有关更多信息，请参见 [Configuring Tracing](../diagnostics/tracing/configuring-tracing.md)。  
  
- WS-AtomicTransaction 跟踪  
  
     WS-AtomicTransaction 可以通过使用 [Ws-atomictransaction 配置实用工具 ( # A0) ](../ws-atomictransaction-configuration-utility-wsatconfig-exe.md)启用跟踪。 这种跟踪有助于详细了解系统中事务和参与者的状态。 若还要启用内部服务模块跟踪，可以将 `HKLM\SOFTWARE\Microsoft\WSAT\3.0\ServiceModelDiagnosticTracing` 注册表项设置为 <xref:System.Diagnostics.SourceLevels> 枚举的一个有效值。 可以按照与其他 WCF 应用程序相同的方式启用消息日志记录。  
  
- `System.Transactions` 跟踪  
  
     使用 OleTransactions 协议时，无法跟踪协议消息。 通过 <xref:System.Transactions> 基础结构提供的跟踪支持（它使用 OleTransactions），用户可以查看事务上发生的事件。 若要为 <xref:System.Transactions> 应用程序启用跟踪，请在 `App.config` 配置文件中包含以下代码。  
  
    ```xml  
    <configuration>  
      <system.diagnostics>  
         <sources>  
            <source name="System.Transactions" switchValue="Verbose, ActivityTracing">  
               <listeners>  
                  <add name="Text"  
                     type="System.Diagnostics.XmlWriterTraceListener"  
                     initializeData="SysTx.log"  
                     traceOutputOptions="Callstack" />  
               </listeners>  
            </source>  
         </sources>  
         <trace autoflush="true" indentsize="4">  
         </trace>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
     这还启用了 WCF 跟踪功能，因为 WCF 还利用 <xref:System.Transactions> 基础结构。  
  
## <a name="see-also"></a>另请参阅

- [管理和诊断](../diagnostics/index.md)
- [配置跟踪](../diagnostics/tracing/configuring-tracing.md)
- [WS-AtomicTransaction 配置实用工具 (wsatConfig.exe)](../ws-atomictransaction-configuration-utility-wsatconfig-exe.md)
