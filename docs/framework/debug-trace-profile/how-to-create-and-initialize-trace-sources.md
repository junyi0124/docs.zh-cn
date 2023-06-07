---
title: 如何：创建和初始化跟踪源
description: 使用 .NET 中的 TraceSource 类创建和初始化跟踪源。 此类提供用于跟踪事件和数据和发出信息性跟踪的方法。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- trace sources
- initializing trace sources
- configuration files [.NET Framework], trace sources
ms.assetid: f88dda6f-5fda-45be-9b3c-745a9b708c4d
ms.openlocfilehash: 3c3624dce9e860a46a9c8c9e9075a03a7c47cb8d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244127"
---
# <a name="how-to-create-and-initialize-trace-sources"></a>如何：创建和初始化跟踪源

<xref:System.Diagnostics.TraceSource> 类由应用程序用来生成可与应用程序相关联的跟踪。 <xref:System.Diagnostics.TraceSource> 提供了一些跟踪方法，利用这些跟踪方法，你可以方便地跟踪事件，跟踪数据和发出信息跟踪。 你可以使用或不使用配置文件来创建和初始化 <xref:System.Diagnostics.TraceSource> 的跟踪输出。 本主题提供这两种选项的说明。 但是，建议你使用配置文件，以便于重新配置在运行时由跟踪源生成的跟踪。  
  
### <a name="to-create-and-initialize-a-trace-source-using-a-configuration-file"></a>使用配置文件创建和初始化跟踪源  
  
1. 创建 Visual Studio 控制台应用程序项目 ( .NET Framework) 并将所提供的代码替换为以下代码。 此代码将记录错误和警告，并将其中一些输出到控制台，将其中一些输出到由配置文件中的各个项创建的 myListener 文件。  
  
     [!code-csharp[TraceSourceExample1#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tracesourceexample1/cs/program.cs#1)]
     [!code-vb[TraceSourceExample1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tracesourceexample1/vb/program.vb#1)]  
  
2. 向项目添加一个应用程序配置文件（如果没有），以初始化步骤 1 代码示例中名为 `TraceSourceApp` 的跟踪源。  
  
3. 将默认配置文件内容替换为以下设置，以便为步骤 1 中创建的跟踪源初始化控制台跟踪侦听器和文本编写器跟踪侦听器。  
  
    ```xml  
    <configuration>  
      <system.diagnostics>  
        <sources>  
          <source name="TraceSourceApp"
            switchName="sourceSwitch"
            switchType="System.Diagnostics.SourceSwitch">  
            <listeners>  
              <add name="console"
                type="System.Diagnostics.ConsoleTraceListener">  
                <filter type="System.Diagnostics.EventTypeFilter"
                  initializeData="Error"/>  
              </add>  
              <add name="myListener"/>  
              <remove name="Default"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="sourceSwitch" value="Error"/>  
        </switches>  
        <sharedListeners>  
          <add name="myListener"
            type="System.Diagnostics.TextWriterTraceListener"
            initializeData="myListener.log">  
            <filter type="System.Diagnostics.EventTypeFilter"
              initializeData="Error"/>  
          </add>  
        </sharedListeners>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
     除配置跟踪侦听器外，配置文件还同时为这两个侦听器创建筛选器，并为跟踪源创建一个源开关。 其中演示了两种添加跟踪侦听器的技术：将侦听器直接添加到跟踪源，以及将侦听器添加到共享的侦听器集合，然后按名称将它添加到跟踪源。 为这两个侦听器标识的筛选器用不同的源级别进行初始化。 这样导致某些消息仅由其中一个侦听器编写。  
  
     配置文件在初始化应用程序时初始化跟踪源的设置。 应用程序可以动态更改配置文件设置的属性，以重写用户指定的任何设置。 例如，你可能想要确保始终将关键消息发送到文本文件，而不管当前配置设置如何。 示例代码演示如何重写配置文件设置以确保将关键消息输出到跟踪侦听器。  
  
     在应用程序执行时更改配置文件设置不会更改初始设置。 若要更改设置，必须重新启动应用程序或使用 <xref:System.Diagnostics.Trace.Refresh%2A?displayProperty=nameWithType> 方法以编程方式刷新应用程序。  
  
### <a name="to-initialize-trace-sources-listeners-and-filters-without-a-configuration-file"></a>不使用配置文件初始化跟踪源、侦听器和筛选器  
  
- 使用以下示例代码在不使用配置文件的情况下通过跟踪源启用跟踪。 这不是建议的做法，但在某此情况下，你可能不想依赖配置文件来确保跟踪。  
  
     [!code-csharp[TraceSourceExample2#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tracesourceexample2/cs/program.cs#1)]
     [!code-vb[TraceSourceExample2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tracesourceexample2/vb/program.vb#1)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics.EventTypeFilter>
- [跟踪应用程序和在应用程序中插入检测点](tracing-and-instrumenting-applications.md)
