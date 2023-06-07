---
title: 如何：将 TraceSource 和筛选器与跟踪侦听器一起使用
description: 在 .NET 中使用 TraceSource 类并筛选跟踪侦听器。 TraceSource 取代了较旧的 Trace 和 Debug 类的静态方法。
ms.date: 03/30/2017
helpviewer_keywords:
- initializing trace listeners
- configuration files [.NET Framework], trace listeners
- app.config files, trace listeners
- levels of writing trace messages
- trace listeners, TraceSource class
- application configuration files, trace listeners
- filters, trace listeners
- TraceSource class, trace listeners
- trace listeners, creating
- trace listeners, filters
- trace listeners, initializing
ms.assetid: 21dc2169-947d-453a-b0e2-3dac3ba0cc9f
ms.openlocfilehash: 94af872e0417941f17c043710688c7b723cd4004
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272795"
---
# <a name="how-to-use-tracesource-and-filters-with-trace-listeners"></a>如何：将 TraceSource 和筛选器与跟踪侦听器一起使用

.NET Framework 版本 2.0 的新功能之一是增强型的跟踪系统。 基本前提不变：跟踪消息通过交换机发送到侦听器，侦听器将数据报告给相关联的输出介质。 2.0 版的主要区别是，可以通过 <xref:System.Diagnostics.TraceSource> 类的实例启动跟踪。 <xref:System.Diagnostics.TraceSource> 用作增强型跟踪系统，并可用来代替较旧的 <xref:System.Diagnostics.Trace> 和 <xref:System.Diagnostics.Debug> 跟踪类的静态方法。 熟悉的 <xref:System.Diagnostics.Trace> 和 <xref:System.Diagnostics.Debug> 类仍存在，但建议使用 <xref:System.Diagnostics.TraceSource> 类进行跟踪。  
  
 本主题描述如何将 <xref:System.Diagnostics.TraceSource> 与应用程序配置文件结合使用。  可在不使用配置文件的情况下使用 <xref:System.Diagnostics.TraceSource> 进行跟踪，但是不建议这样做。 有关在不使用配置文件的情况下进行跟踪的信息，请参阅[如何：创建和初始化跟踪源](how-to-create-and-initialize-trace-sources.md)。  
  
### <a name="to-create-and-initialize-your-trace-source"></a>创建和初始化跟踪源  
  
1. 使用跟踪检测应用程序的第一步是创建跟踪源。 在含有各种组件的大型项目中，可以为每个组件创建一个单独的跟踪源。 建议将应用程序名称作为跟踪源名称。 这会使不同的跟踪更容易保持独立。 以下代码创建一个新的跟踪源 (`mySource)` 并调用跟踪事件的方法 (`Activity1`)。  跟踪消息是通过默认跟踪侦听器写入的。  
  
    ```csharp
    using System;  
    using System.Diagnostics;  
    using System.Threading;  
  
    namespace TraceSourceApp  
    {  
        class Program  
        {  
            private static TraceSource mySource =
                new TraceSource("TraceSourceApp");  
            static void Main(string[] args)  
            {  
                Activity1();  
                mySource.Close();  
                return;  
            }  
            static void Activity1()  
            {  
                mySource.TraceEvent(TraceEventType.Error, 1,
                    "Error message.");  
                mySource.TraceEvent(TraceEventType.Warning, 2,
                    "Warning message.");  
            }  
        }  
    }  
    ```  
  
### <a name="to-create-and-initialize-trace-listeners-and-filters"></a>创建和初始化跟踪侦听器和筛选器  
  
1. 第一个过程中的代码不会以编程方式标识任何跟踪侦听器或筛选器。 代码本身会产生写入到默认跟踪侦听器的跟踪消息。 若要配置特定的跟踪侦听器及其关联的筛选器，可编辑与应用程序名称对应的配置文件。 在此文件中，可以添加或删除侦听器、为侦听器设置属性和筛选器或删除侦听器。 以下配置文件示例演示如何为在前面过程中创建的跟踪源初始化控制台跟踪侦听器和文本编写器跟踪侦听器。 除配置跟踪侦听器外，配置文件还同时为这两个侦听器创建筛选器，并为跟踪源创建一个源开关。 其中演示了两种添加跟踪侦听器的技术：将侦听器直接添加到跟踪源，以及将侦听器添加到共享的侦听器集合，然后按名称将它添加到跟踪源。 为这两个侦听器标识的筛选器用不同的源级别进行初始化。 这样导致某些消息仅由其中一个侦听器编写。  
  
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
                  initializeData="Warning"/>  
              </add>  
              <add name="myListener"/>  
              <remove name="Default"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="sourceSwitch" value="Warning"/>  
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
  
### <a name="to-change-the-level-at-which-a-listener-writes-a-trace-message"></a>更改侦听器写入跟踪消息的级别  
  
1. 配置文件在初始化应用程序时初始化跟踪源的设置。 若要更改这些设置，必须更改配置文件并重启应用，或使用 <xref:System.Diagnostics.Trace.Refresh%2A?displayProperty=nameWithType> 方法以编程方式刷新应用程序。 应用程序可以动态更改配置文件设置的属性，以重写用户指定的任何设置。  例如，可能想要确保始终将关键消息发送到文本文件，而不管当前配置设置如何。  
  
    ```csharp
    using System;  
    using System.Diagnostics;  
    using System.Threading;  
  
    namespace TraceSourceApp  
    {  
        class Program  
        {  
            private static TraceSource mySource =
                new TraceSource("TraceSourceApp");  
            static void Main(string[] args)  
            {  
                Activity1();  
  
                // Change the event type for which tracing occurs.  
                // The console trace listener must be specified
                // in the configuration file. First, save the original  
                // settings from the configuration file.  
                EventTypeFilter configFilter =
                    (EventTypeFilter)mySource.Listeners["console"].Filter;  
  
                // Then create a new event type filter that ensures
                // critical messages will be written.  
                mySource.Listeners["console"].Filter =  
                    new EventTypeFilter(SourceLevels.Critical);  
                Activity2();  
  
                // Allow the trace source to send messages to listeners
                // for all event types. This statement will override
                // any settings in the configuration file.  
                mySource.Switch.Level = SourceLevels.All;  
  
                // Restore the original filter settings.  
                mySource.Listeners["console"].Filter = configFilter;  
                Activity3();  
                mySource.Close();  
                return;  
            }  
            static void Activity1()  
            {  
                mySource.TraceEvent(TraceEventType.Error, 1,
                    "Error message.");  
                mySource.TraceEvent(TraceEventType.Warning, 2,
                    "Warning message.");  
            }  
            static void Activity2()  
            {  
                mySource.TraceEvent(TraceEventType.Critical, 3,
                    "Critical message.");  
                mySource.TraceInformation("Informational message.");  
            }  
            static void Activity3()  
            {  
                mySource.TraceEvent(TraceEventType.Error, 4,
                    "Error message.");  
                mySource.TraceInformation("Informational message.");  
            }  
        }  
    }  
    ```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics.EventTypeFilter>
- [如何：创建和初始化跟踪源](how-to-create-and-initialize-trace-sources.md)
- [跟踪侦听器](trace-listeners.md)
