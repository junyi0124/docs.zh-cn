---
title: 无法写入日志文件，原因是写入将导致日志文件超过 MaximumSize 值。
ms.date: 07/20/2015
f1_keywords:
- vbrApplicationLog_FileExceedsMaximumSize
ms.assetid: 61747a9c-e460-424b-a365-73cdba9dd428
ms.openlocfilehash: 95a7b9036e7c1494cd44c250b0580bab5144417b
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91059452"
---
# <a name="unable-to-write-to-log-file-because-writing-to-it-would-cause-it-to-exceed-maximumsize-value"></a>无法写入日志文件，原因是写入将导致日志文件超过 MaximumSize 值。

<xref:Microsoft.VisualBasic.Logging.FileLogTraceListener> 类无法写入日志文件，原因如下：  
  
- 日志文件的大小（以字节为单位）大于 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.MaxFileSize%2A> 属性的值  
  
     —并且—  
  
- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.DiskSpaceExhaustedBehavior%2A> 属性的值为 <xref:Microsoft.VisualBasic.Logging.DiskSpaceExhaustedOption.ThrowException>。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 将现有日志存档后将其从计算机中删除，以允许 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener> 对象创建新日志。  
  
2. 更改 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.MaxFileSize%2A> 属性的值，以允许写入更大的日志。  
  
3. 如果日志过大，则将 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.DiskSpaceExhaustedBehavior%2A> 属性设置为 <xref:Microsoft.VisualBasic.Logging.DiskSpaceExhaustedOption.DiscardMessages> 以放弃消息而不发出警告。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.MaxFileSize%2A>
- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.DiskSpaceExhaustedBehavior%2A>
- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener>
- [My. .Log](xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log)
- [DirectoryPath。](xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log)
