---
title: 跟踪侦听器
description: 探索跟踪侦听器，这些侦听器是用于收集和记录在 .NET 中发送的跟踪消息的机制。 侦听器将收集、存储和路由消息。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Listener object types
- listeners
- Trace class, listeners
- trace listeners, about trace listeners
- Listeners collection
- trace listeners
- tracing [.NET Framework], trace listeners
- logs, trace listeners
ms.assetid: 444b0d33-67ea-4c36-9e94-79c50f839025
ms.openlocfilehash: 8cd79d21d66d23f834b7ef0012d8360884028ac6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238114"
---
# <a name="trace-listeners"></a>跟踪侦听器

使用 Trace、Debug 和 <xref:System.Diagnostics.TraceSource> 时，必须具有用于收集和记录发送的消息的机制。 跟踪消息由 *侦听器* 接收。 侦听器的用途是收集、存储和路由跟踪消息。 侦听器会将跟踪输出定向到适当的目标，如日志、窗口或文本文件。  
  
 侦听器可用于 Debug、Trace 和 <xref:System.Diagnostics.TraceSource> 类，其中每个类都可以将输出发送到多种侦听器对象。 以下是常用的预定义侦听器：  
  
- <xref:System.Diagnostics.TextWriterTraceListener> 将输出重定向到 <xref:System.IO.TextWriter> 类的实例或为 <xref:System.IO.Stream> 类的任何项。 它也可以写入到控制台或文件，因为它们是 <xref:System.IO.Stream> 类。  
  
- <xref:System.Diagnostics.EventLogTraceListener> 将输出重定向到事件日志。  
  
- <xref:System.Diagnostics.DefaultTraceListener> 向 OutputDebugString 和 Debugger.Log 方法发出 Write 和 WriteLine 消息。 在 Visual Studio 中，这会导致“输出”窗口中显示调试消息。 Fail 和失败的 Assert 消息也发到 OutputDebugString Windows API 和 Debugger.Log 方法，同样将显示消息框。 此行为是 Debug 和 Trace 消息的默认行为，因为 DefaultTraceListener 自动包含在每个 `Listeners` 集合中，且是自动包含的唯一侦听器。  
  
- <xref:System.Diagnostics.ConsoleTraceListener> 将跟踪或调试输出定向到标准输出或标准错误流。  
  
- <xref:System.Diagnostics.DelimitedListTraceListener> 将跟踪或调试输出定向到文本编写器（如流编写器）或流（如文件流）。 跟踪输出采用带分隔符的文本格式，该格式使用属性指定的分隔符 <xref:System.Diagnostics.DelimitedListTraceListener.Delimiter%2A> 。  
  
- <xref:System.Diagnostics.XmlWriterTraceListener> 将跟踪或调试输出以 XML 编码数据的形式定向到 <xref:System.IO.TextWriter> 或 <xref:System.IO.Stream>，例如 <xref:System.IO.FileStream>。  
  
 如果你希望 <xref:System.Diagnostics.DefaultTraceListener> 以外的任何侦听器接收 Debug、Trace 和 <xref:System.Diagnostics.TraceSource> 输出，则必须将其添加到 `Listeners` 集合。 有关详细信息，请参阅[如何：创建和初始化跟踪侦听器](how-to-create-and-initialize-trace-listeners.md)和[如何：将 TraceSource 和筛选器与跟踪侦听器一起使用](how-to-use-tracesource-and-filters-with-trace-listeners.md)。 侦听器集合中的任何侦听器均从跟踪输出方法获取相同消息。 例如，假设你设置了两个侦听器：TextWriterTraceListener 和 EventLogTraceListener。 每个侦听器接收相同消息。 TextWriterTraceListener 将输出定向到流，而 EventLogTraceListener 将输出定向到事件日志。  
  
 以下示例演示如何将输出发送到 Listeners 集合。  
  
```vb  
' Use this example when debugging.  
Debug.WriteLine("Error in Widget 42")  
' Use this example when tracing.  
Trace.WriteLine("Error in Widget 42")  
```  
  
```csharp  
// Use this example when debugging.  
System.Diagnostics.Debug.WriteLine("Error in Widget 42");  
// Use this example when tracing.  
System.Diagnostics.Trace.WriteLine("Error in Widget 42");  
```  
  
 调试和跟踪共享一个 Listeners 集合，因此，如果你在应用程序中将侦听器对象添加到 Debug.Listeners 集合，则它也将被添加到 Trace.Listeners 集合。  
  
 以下示例演示如何使用侦听器将跟踪信息发送到控制台：  
  
```vb  
Trace.Listeners.Clear()  
Trace.Listeners.Add(New TextWriterTraceListener(Console.Out))  
```  
  
```csharp  
System.Diagnostics.Trace.Listeners.Clear();  
System.Diagnostics.Trace.Listeners.Add(  
   new System.Diagnostics.TextWriterTraceListener(Console.Out));  
```  
  
## <a name="developer-defined-listeners"></a>开发人员定义的侦听器  

 可以通过从 TraceListener 基类继承并用自定义方法重写其方法来定义侦听器。 有关创建开发人员定义的侦听器的详细信息，请参阅 .NET Framework 参考中的 <xref:System.Diagnostics.TraceListener>。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.EventLogTraceListener>
- <xref:System.Diagnostics.DefaultTraceListener>
- <xref:System.Diagnostics.TraceListener>
- [跟踪应用程序和在应用程序中插入检测点](tracing-and-instrumenting-applications.md)
- [跟踪开关](trace-switches.md)
