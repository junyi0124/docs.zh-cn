---
title: 无法写入日志文件，因为写入日志文件会使可用磁盘空间低于 ReservedSpace 值
ms.date: 07/20/2015
f1_keywords:
- vbrApplicationLog_ReservedSpaceEncroached
ms.assetid: 95832e70-4ecc-47aa-90c1-f35c4d468151
ms.openlocfilehash: e5247876c683812edf0eb73ab5f1a5e607b48102
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075598"
---
# <a name="unable-to-write-to-log-file-because-writing-to-it-would-reduce-free-disk-space-below-reservedspace-value"></a>无法写入日志文件，因为写入日志文件会使可用磁盘空间低于 ReservedSpace 值

<xref:Microsoft.VisualBasic.Logging.FileLogTraceListener> 类无法写入日志文件，原因如下：  
  
- 可用磁盘空间量（以字节为单位）小于 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.ReserveDiskSpace%2A> 属性的值  
  
     —并且—  
  
- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.DiskSpaceExhaustedBehavior%2A> 属性的值为 <xref:Microsoft.VisualBasic.Logging.DiskSpaceExhaustedOption.ThrowException>。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 将现有日志存档后将其从计算机中删除，以允许 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener> 对象创建新日志。  
  
2. 将 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.ReserveDiskSpace%2A> 属性的值更改为一个较小的数字，以保留更少的磁盘空间。  
  
3. 如果没有足够的可用磁盘空间，则将 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.DiskSpaceExhaustedBehavior%2A> 属性设置为 <xref:Microsoft.VisualBasic.Logging.DiskSpaceExhaustedOption.DiscardMessages> 以放弃消息而不发出警告消息。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.ReserveDiskSpace%2A>
- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.DiskSpaceExhaustedBehavior%2A>
- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener>
- [My. .Log](xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log)
- [DirectoryPath。](xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log)
