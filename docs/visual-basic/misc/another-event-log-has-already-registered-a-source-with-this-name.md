---
title: 另一个事件日志已使用该名称注册了一个源
ms.date: 07/20/2015
ms.assetid: e6f5cd95-bb3f-4845-84fb-ae623a9bd44e
ms.openlocfilehash: 333c28036e2d1b7514c7370b98ff70a6d195a9dd
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058386"
---
# <a name="another-event-log-has-already-registered-a-source-with-this-name"></a>另一个事件日志已使用该名称注册了一个源

已尝试向事件日志写入条目，其中指定的源使用其他事件日志进行注册。  
  
 在组件将条目写入日志前，必须设置 <xref:System.Diagnostics.EventLog.Source%2A> 组件实例的 <xref:System.Diagnostics.EventLog> 属性。 发生此情况时，系统将检查所指定的源是否使用写入该组件的事件日志进行注册，并在需要时调用 <xref:System.Diagnostics.EventLog.CreateEventSource%2A> 。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 使用 <xref:System.Diagnostics.EventLog.DeleteEventSource%2A> 或 <xref:System.Diagnostics.EventLog.DeleteEventSource%2A> 方法删除源与第一个日志之间的关联。  
  
2. 使用新的日志注册源。  
  
## <a name="see-also"></a>请参阅

- [My. .Log](xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log)
