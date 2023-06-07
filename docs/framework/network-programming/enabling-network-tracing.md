---
title: 启用网络跟踪
description: 了解如何启用网络跟踪，以便提供有关 .NET Framework 中托管应用程序的方法调用和网络流量的信息。
ms.date: 03/30/2017
helpviewer_keywords:
- trace destinations
- sending traces to log file
- trace listeners, network tracing
- network tracing, enabling
- CLR Debugger
- default listeners
- logs, trace
- destination for tracing output
ms.assetid: 5fff458c-51a6-4134-ba47-8a6137ddc41e
ms.openlocfilehash: 246a975ead3cb9c1acb4fe0512dfa91d1b8a00c0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287418"
---
# <a name="enabling-network-tracing"></a>启用网络跟踪

网络跟踪允许访问有关方法调用的信息，以及有关托管应用程序所生成的网络流量的信息。 必须完成以下任务才能在应用程序中启用网络跟踪：  
  
- 在启用跟踪的情况下编译代码。 请参阅[如何：使用跟踪和调试进行条件编译](../debug-trace-profile/how-to-compile-conditionally-with-trace-and-debug.md)，了解有关启用跟踪所需编译器开关的详细信息。  
  
- 为跟踪输出指定目标。  
  
- 配置网络跟踪的行为。 请参阅[如何：配置网络跟踪](how-to-configure-network-tracing.md)，了解详细信息。  
  
 最常见的跟踪目标（也称为跟踪侦听器）是默认侦听器和日志文件。  
  
 如果不指定跟踪侦听器，则跟踪使用默认侦听器。 可通过在启用托管代码的调试器中执行代码，查看发送到默认侦听器的消息，例如 .NET Framework SDK 附带的 CLR 调试器 或 Windows SDK 附带的 DBwin32.exe。 使用 CLR 调试器时，跟踪消息将出现在“输出”窗口中。  
  
 如果希望使用文件接收跟踪，则可使用配置设置指定日志文件，如以下示例所示。 （有关配置文件的一般讨论，请参阅[配置文件](../configure-apps/index.md)。）  
  
 若要将跟踪发送到日志文件，请将以下节点添加到相应配置文件（应用程序或计算机）的 `<system.diagnostics>` 节点。 可按需更改文件的名称 (trace.log)。  
  
```xml  
<system.diagnostics>  
  <trace autoflush="true" indentsize="4">  
    <listeners>  
      <add name="file" type="System.Diagnostics.TextWriterTraceListener" initializeData="trace.log"/>  
    </listeners>
  </trace>  
</system.diagnostics>  
```  
  
## <a name="see-also"></a>请参阅

- [解释网络跟踪](interpreting-network-tracing.md)
- [.NET Framework 中的网络跟踪](network-tracing.md)
- [跟踪应用程序和在应用程序中插入检测点](../debug-trace-profile/tracing-and-instrumenting-applications.md)
