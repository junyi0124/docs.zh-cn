---
title: 无法获取日志的流
ms.date: 07/20/2015
f1_keywords:
- vbrApplicationLog_ExhaustedPossibleStreamNames
ms.assetid: 33994f52-8efb-4790-a459-033e5c1db632
ms.openlocfilehash: 887356fac3abe5c9d28751f7c4d3b1908ed35acb
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91078380"
---
# <a name="unable-to-obtain-a-stream-for-the-log"></a>无法获取日志的流

无法获取日志的流。 可能的基于的文件名 \<name> 已在使用中。  
  
 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener>类无法创建新的日志文件，因为基于的所有潜在日志文件名 \<name> 都已在使用中。  
  
 具有过多的日志文件可能表明应用程序存在结构问题。 有关详细信息，请参阅 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener> 类的文档。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 将 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.LogFileCreationSchedule%2A> 属性设置为 <xref:Microsoft.VisualBasic.Logging.LogFileCreationScheduleOption.Daily> 或 <xref:Microsoft.VisualBasic.Logging.LogFileCreationScheduleOption.Weekly> ，以便在日志文件名中包含日期戳。  
  
2. 将现有日志存档后将其从计算机中删除，以允许 <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener> 对象创建新日志。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener>
- <xref:Microsoft.VisualBasic.Logging.FileLogTraceListener.LogFileCreationSchedule%2A>
- [My. .Log](xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log)
- [DirectoryPath。](xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log)
