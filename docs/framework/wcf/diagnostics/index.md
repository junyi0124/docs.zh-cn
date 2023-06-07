---
title: 管理和诊断
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, diagnostics
- Windows Communication Foundation, administration
- diagnostics [WCF]
- WCF, diagnostics
- administration [WCF]
- WCF, administration
ms.assetid: 34c81c08-0e0f-4fbc-9ae8-91948640ee43
ms.openlocfilehash: 703724c3040f2508f011e002c22fa45dd3197884
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96285559"
---
# <a name="administration-and-diagnostics"></a>管理和诊断

Windows Communication Foundation (WCF) 提供了一组丰富的功能，可帮助你监视应用程序生命周期的不同阶段。 例如，您可以在部署时使用配置来设置服务和客户端。 WCF 包含大量性能计数器，可帮助你衡量应用程序的性能。 WCF 还通过 WCF Windows Management Instrumentation (WMI) 提供程序在运行时公开服务的检测数据。 当应用程序出现错误或者开始错误操作时，可使用事件日志来了解是否有重大事件发生。 此外，还可以使用消息日志记录和跟踪来从头至尾查看应用程序中发生了哪些事件。 这些功能可帮助开发人员和 IT 专业人员对 WCF 应用程序的正常运行进行故障排除。  
  
> [!NOTE]
> 如果收到的错误没有特定的详细信息，应启用 `includeExceptionDetailInFaults` [\<serviceDebug>](../../configure-apps/file-schema/wcf/servicedebug.md) 配置元素的属性。 这会指示 WCF 将异常详细信息发送到客户端，这使你能够在不需要更多高级诊断的情况下检测多个常见问题。 有关详细信息，请参阅 [发送和接收错误](../sending-and-receiving-faults.md)。  
  
## <a name="diagnostics-features-provided-by-wcf"></a>WCF 提供的诊断功能  

 WCF 提供以下诊断功能：  
  
- 端到端跟踪提供的检测数据可用于对应用程序进行故障排除而无需使用调试器。 WCF 输出进程里程碑的跟踪以及错误消息。 这可能包括打开通道工厂或者通过服务主机发送和接收消息。 可以为运行中的应用程序启用跟踪，以监视其进度。 有关详细信息，请参阅 [跟踪](./tracing/index.md) 主题。 若要了解如何使用跟踪来调试应用程序，请参阅 [使用跟踪对应用程序进行故障排除](./tracing/using-tracing-to-troubleshoot-your-application.md) 主题。  
  
- 消息日志记录用于查看消息在传输前后的情况。 有关详细信息，请参阅 [消息日志记录](message-logging.md) 主题。  
  
- 事件跟踪针对所有主要问题将事件写入事件日志。 然后，可以使用事件查看器检查任何异常情况。 有关详细信息，请参阅 [事件日志记录](./event-logging/index.md) 主题。  
  
- 通过性能监视器公开的性能计数器可用于监视应用程序和系统的运行状况。 有关详细信息，请参阅 [性能计数器](./performance-counters/index.md) 主题。  
  
- <xref:System.ServiceModel.Configuration> 命名空间用于加载配置文件和设置服务或客户端终结点。 如果必须将更新部署到很多计算机，可使用对象模型通过脚本对很多应用程序进行更改。 或者，您可以使用 [配置编辑器工具 ( # A0) ](../configuration-editor-tool-svcconfigeditor-exe.md) 使用 GUI 向导编辑配置设置。 有关详细信息，请参阅 [配置应用程序](configuring-your-application.md) 主题。  
  
- WMI 可用于查找哪些服务正在计算机上进行侦听，以及正在使用哪些绑定。 有关详细信息，请参阅 [使用诊断 Windows Management Instrumentation](./wmi/index.md) 主题。  
  
 WCF 还提供多个 GUI 和命令行工具，使您可以更轻松地创建、部署和管理 WCF 应用程序。 有关详细信息，请参阅 [Windows Communication Foundation 工具](../tools.md)。 例如，您可以使用 [配置编辑器工具 ( # A0) ](../configuration-editor-tool-svcconfigeditor-exe.md) 使用向导来创建和编辑 WCF 配置设置，而不是直接编辑 XML。 你还可以使用 [服务跟踪查看器工具 ( # A0) ](../service-trace-viewer-tool-svctraceviewer-exe.md) 查看、分组和筛选跟踪消息，以便能够诊断、修复和验证 WCF 服务的问题。  
  
## <a name="see-also"></a>另请参阅

- [配置应用程序](configuring-your-application.md)
- [部署服务](deploying-services.md)
- [异常引用](./exceptions-reference/index.md)
- [事件日志记录](./event-logging/index.md)
- [消息日志记录](message-logging.md)
- [配置编辑器工具 (SvcConfigEditor.exe)](../configuration-editor-tool-svcconfigeditor-exe.md)
- [服务跟踪查看器工具 (SvcTraceViewer.exe)](../service-trace-viewer-tool-svctraceviewer-exe.md)
- [ServiceModel 注册工具](servicemodel-registration-tool.md)
- [跟踪](./tracing/index.md)
- [使用 Windows Management Instrumentation 诊断](./wmi/index.md)
- [性能计数器](./performance-counters/index.md)
- [Windows Communication Foundation 工具](../tools.md)
