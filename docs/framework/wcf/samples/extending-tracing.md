---
title: 扩展跟踪
ms.date: 03/30/2017
ms.assetid: 2b971a99-16ec-4949-ad2e-b0c8731a873f
ms.openlocfilehash: 5e3329238998f11467511960f32b177953036ab1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265331"
---
# <a name="extend-tracing"></a>扩展跟踪

此示例演示如何通过在客户端和服务代码中编写用户定义的活动跟踪来扩展 Windows Communication Foundation (WCF) 跟踪功能。 通过编写用户定义的活动跟踪，用户可以创建跟踪活动，并将跟踪分组为逻辑工作单元。 还可以通过传输（在同一个终结点内）和传播（在终结点之间）来关联活动。 在此示例中，同时为客户端和服务启用了跟踪。 有关如何在客户端和服务配置文件中启用跟踪的详细信息，请参阅 [跟踪和消息日志记录](tracing-and-message-logging.md)。  
  
 此示例基于 [入门](getting-started-sample.md)。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\ExtendingTracing`  
  
## <a name="tracing-and-activity-propagation"></a>跟踪和活动传播  

 用户定义的活动跟踪允许用户创建自己的跟踪活动，以便将跟踪分组为逻辑工作单元，通过传输和传播关联活动，并降低 WCF 跟踪的性能成本 (例如，日志文件的磁盘空间成本) 。  
  
### <a name="add-custom-sources"></a>添加自定义源  

 用户定义的跟踪既可以添加到客户端代码中，又可以添加到服务代码中。 将跟踪源添加到客户端或服务配置文件后，便可以记录这些自定义跟踪并将其显示在 [服务跟踪查看器工具中 ( # A0) ](../service-trace-viewer-tool-svctraceviewer-exe.md)。 下面的代码演示如何在配置文件中添加一个名为 `ServerCalculatorTraceSource` 的用户定义的跟踪源。  
  
```xml  
<system.diagnostics>  
    <sources>  
        <source name="System.ServiceModel" switchValue="Warning" propagateActivity="true">  
            <listeners>  
                <add type="System.Diagnostics.DefaultTraceListener" name="Default">  
                    <filter type="" />  
                </add>  
                <add name="xml">  
                    <filter type="" />  
                </add>  
            </listeners>  
        </source>  
        <source name="ServerCalculatorTraceSource" switchValue="Information,ActivityTracing">  
            <listeners>  
                <add type="System.Diagnostics.DefaultTraceListener" name="Default">  
                    <filter type="" />  
                </add>  
                <add name="xml">  
                    <filter type="" />  
                </add>  
            </listeners>  
        </source>  
    </sources>  
    <sharedListeners>  
       <add initializeData="C:\logs\ServerTraces.svclog" type="System.Diagnostics.XmlWriterTraceListener"  
        name="xml" traceOutputOptions="Callstack">  
            <filter type="" />  
        </add>  
    </sharedListeners>  
    <trace autoflush="true" />  
</system.diagnostics>
....
```  
  
### <a name="correlate-activities"></a>关联活动  

 若要跨终结点直接关联活动，必须将 `propagateActivity` 跟踪源中的 `true` 属性设置为 `System.ServiceModel`。 此外，若要传播跟踪而无需执行 WCF 活动，则必须关闭 "一起活动跟踪"。 这可以在以下代码示例中看到。  
  
> [!NOTE]
> 关闭 ServiceModel 活动跟踪与将 `switchValue` 属性表示的跟踪级别设置为 off 并不一样。  
  
```xml  
<system.diagnostics>  
    <sources>  
      <source name="System.ServiceModel" switchValue="Warning" propagateActivity="true">  
  
    ...  
  
       </source>  
    </sources>  
</system.diagnostics>  
```  
  
### <a name="lessen-performance-cost"></a>降低性能成本  

 将 `ActivityTracing` 跟踪源中的 `System.ServiceModel` 设置为 off 将生成一个只包含用户定义的活动跟踪的跟踪文件，而不包含任何 ServiceModel 活动跟踪。 排除一起活动跟踪会导致更小的日志文件。 但会丢失关联 WCF 处理跟踪的机会。  
  
## <a name="set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
## <a name="see-also"></a>另请参阅

- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
