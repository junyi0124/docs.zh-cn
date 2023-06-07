---
title: 循环跟踪
ms.date: 03/30/2017
ms.assetid: 5ff139f9-8806-47bc-8f33-47fe6c436b92
ms.openlocfilehash: d9af1f18a507a79c9c287393652e65dcb3372444
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90552513"
---
# <a name="circular-tracing"></a>循环跟踪

此示例演示如何实现循环缓冲区跟踪侦听器。 生产服务的一个常见方案是，拥有可以长时间使用的服务，并在较低级别启用跟踪日志记录。 这些服务占用大量磁盘空间。 在对某个服务进行疑难解答时，跟踪日志中的最新数据与解决问题相关。 此示例演示如何实现一个循环缓冲区跟踪侦听器，在该侦听器中，磁盘上仅保留最新的跟踪数据，最多可以保存可配置的数据量。 此示例基于 [入门](getting-started-sample.md) ，并包含自定义跟踪侦听器。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

本示例假定您熟悉 [跟踪和消息日志记录](tracing-and-message-logging.md) 示例，并阅读为 [跟踪和消息日志记录](tracing-and-message-logging.md) 示例提供的文档。

## <a name="circular-buffer-trace-listener"></a>循环缓冲区跟踪侦听器

实现循环缓冲区跟踪侦听器所涉及的概念是：拥有两个文件，每个文件最多可以存储所需跟踪日志总数据量的一半。 侦听器创建一个文件并写入该文件，直到它达到其限制（即数据大小的一半），之后侦听器将切换到第二个文件。 当侦听器达到第二个文件的限制时，它会用新跟踪数据覆盖第一个文件。

此侦听器派生自 `XmlWriteTraceListener` ，并允许通过 [服务跟踪查看器工具 ( # A0) ](../service-trace-viewer-tool-svctraceviewer-exe.md)来查看日志。 在尝试查看日志时，可以通过在服务跟踪查看器工具中同时打开这两个日志文件来方便地进行重新合并。 服务跟踪查看器工具会自动负责对跟踪数据进行排序，以便它们按照正确的顺序显示。

## <a name="configuration"></a>Configuration

通过为侦听器和源元素添加下面的代码可以将服务配置为使用循环缓冲区跟踪侦听器。 最大文件大小是通过在循环跟踪侦听器的配置中设置 `maxFileSizeKB` 属性来指定的。 下面的代码对此进行了演示。

```xml
<system.diagnostics>
  <sources>
    <source name="System.ServiceModel" switchValue="Information,ActivityTracing" propagateActivity="true">
      <listeners>
        <add name="CircularTraceListener" />
      </listeners>
    </source>
  </sources>
  <sharedListeners>
    <add name="CircularTraceListener" type="Microsoft. Samples.ServiceModel.CircularTraceListener,CircularTraceListener"
         initializeData="c:\logs\CircularTracing-service.svclog" maxFileSizeKB="100" />
  </sharedListeners>
  <trace autoflush="true" />
</system.diagnostics>
```

#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 请确保已 [为 Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\CircularTracing`

## <a name="see-also"></a>请参阅

- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
