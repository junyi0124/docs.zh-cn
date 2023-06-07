---
title: EventLogSource 中指定的源名称已注册到不同于 EventLogName 中所指定的日志的另一个日志
ms.date: 07/20/2015
ms.assetid: 7317e100-098b-408d-86e5-7c74cf8558c7
ms.openlocfilehash: 1b577e3b0613001b6241dcfdc59c8c84029197d2
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058925"
---
# <a name="source-name-specified-in-eventlogsource-is-registered-to-a-log-other-than-that-specified-in-eventlogname"></a>EventLogSource 中指定的源名称已注册到不同于 EventLogName 中所指定的日志的另一个日志

`EventLog` 正在尝试引用注册到其他日志的源。 如果要向事件日志写入条目，则必须指定 <xref:System.Diagnostics.EventLog.Source%2A> 属性。 <xref:System.Diagnostics.EventLog.Source%2A> 属性将具有事件日志的组件注册为条目的有效源。 单个源一次仅可关联（从而仅能将条目写入）一个事件日志。  
  
 默认情况下，如果在未事先将组件注册为有效源的情况下尝试写入条目，系统将自动使用 <xref:System.Diagnostics.EventLog.Source%2A> 属性的值作为源字符串将该源注册到事件日志。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 确保已将源注册到正确的日志。 要执行此操作，请使用 <xref:System.Diagnostics.EventLog.CreateEventSource%2A> 方法或其重载之一来指定向事件日志唯一标识你的组件的字符串。  
  
## <a name="see-also"></a>请参阅

- [管理事件日志](/previous-versions/visualstudio/visual-studio-2008/4f69axw4(v=vs.90))
- [事件日志引用](/previous-versions/visualstudio/visual-studio-2008/k43k9z2a(v=vs.90))
- [如何：将应用程序添加为事件日志条目的源](/previous-versions/visualstudio/visual-studio-2008/xz73e171(v=vs.90))
- [如何：移除事件源](/previous-versions/visualstudio/visual-studio-2008/k57466fc(v=vs.90))
