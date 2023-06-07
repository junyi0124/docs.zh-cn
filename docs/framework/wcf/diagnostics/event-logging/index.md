---
title: WCF 中的事件日志记录
ms.date: 03/30/2017
helpviewer_keywords:
- event logging [WCF]
ms.assetid: aac0530d-f44c-45a1-bada-e30e0677b41f
ms.openlocfilehash: 535b3570f41cbfb277eeb14fb07242b528acea46
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236151"
---
# <a name="event-logging-in-wcf"></a>WCF 中的事件日志记录

Windows Communication Foundation (WCF) 跟踪 Windows 事件日志中的内部事件。  
  
## <a name="viewing-event-logs"></a>查看事件日志  

 默认情况下自动启用事件日志记录，并且没有禁用它的机制。 可以使用事件查看器查看 WCF 记录的事件。 若要启动此工具，请单击 " **开始**"，单击 " **控制面板**"，双击 " **管理工具**"，然后双击 " **事件查看器**"。  
  
### <a name="application-event-log"></a>应用程序事件日志  

 **应用程序事件日志** 包含 WCF 生成的大多数事件。 大多数项都指示未能为应用程序启用某个功能。 示例包括：  
  
- 消息日志记录/跟踪：在跟踪和消息日志记录失败时，WCF 会将事件写入事件日志。 但是，并非每个跟踪故障都会触发事件。 为了防止事件日志被完全填充跟踪失败，WCF 为此类事件实现了10分钟的掉电期。 这意味着，如果 WCF 将跟踪失败写入事件日志，则至少需要10分钟。  
  
- 共享侦听器：WCF TCP 端口共享服务在未能启动时将记录一个事件。  
  
- CardSpace：在服务启动失败时记录事件。  
  
- 严重和错误事件，如启动故障或崩溃  
  
- 消息日志记录打开：当消息日志记录打开时记录事件。 这将通知管理员可能在消息头和正文中记录应用程序特定的敏感信息。  
  
- 当设置 `enableLoggingKnownPII` 文件的 `machineSettings` 元素中的 `machine.config` 属性时，将记录一个事件。 此属性指定是否允许计算机上运行的任何应用程序来记录已知的个人身份信息 (PII)。  
  
- 如果为特定应用程序将 `logKnownPii` 或 `app.config` 文件中的 `web.config` 属性设置为 `true` 以开启 PII 日志记录，但 `enableLoggingKnownPII` 文件的 `machineSettings` 元素中的 `machine.config` 属性设置为 `false`，则会记录一个事件。 此外，如果将 `logKnownPii` 和 `enableLoggingKnownPII` 都设置为 `true`，也会记录一个事件。 有关这些配置设置的详细信息，请参阅 [配置消息日志记录](../configuring-message-logging.md) 主题的 "安全性" 部分。  
  
### <a name="security-event-log"></a>安全事件日志  

 **安全事件日志** 包含由 WCF 记录的安全审核事件。  
  
### <a name="system-event-log"></a>系统事件日志  

 WCF 不会记录 **系统事件日志** 中的任何内容。  
  
### <a name="event-log-entries"></a>事件日志项  

 事件的 **来源** 是生成日志条目的程序集的名称。  
  
 事件日志项的类型用于指示事件的严重性。 每个事件必须属于一种类型，应用程序在报告事件时将指示该类型。 事件查看器使用该类型来确定在日志的列表视图中显示哪一个图标。 有关事件日志项的不同事件类型，请参见 <xref:System.Diagnostics.EventLogEntryType>。  
  
 在事件查看器中查看事件时单击 "详细信息" 时，事件查看器可能会通过 Internet 发送信息。 有关更多信息，请参见事件查看器帮助。  
  
## <a name="see-also"></a>另请参阅

- [管理和诊断](../index.md)
- [事件常规参考](events-general-reference.md)
