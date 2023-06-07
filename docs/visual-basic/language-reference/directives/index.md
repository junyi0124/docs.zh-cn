---
title: 指令
ms.date: 07/20/2015
helpviewer_keywords:
- directives, Visual Basic compiler
- Visual Basic code, directives
- directives
ms.assetid: 20d5fe65-490a-4c23-88c2-ee4f490ed762
ms.openlocfilehash: b5fcf3cb8801bc99dd2096c28cc41ebefeb34592
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409993"
---
# <a name="directives-visual-basic"></a>指令 (Visual Basic)

本节中的主题记录 Visual Basic 源代码编译器指令。  
  
## <a name="in-this-section"></a>本节内容  

 [#Const 指令](const-directive.md)--定义编译器常量  
  
 [#ExternalSource 指令](externalsource-directive.md)--指示源行与源外部的文本之间的映射  
  
 [#If .。。Then ... #Else 指令](if-then-else-directives.md)-编译选定的代码块  
  
 [#Region 指令](region-directive.md)--折叠和隐藏 Visual Studio 编辑器中的代码段  
  
 **#Disable，#Enable** --针对代码区域禁用和启用特定警告。  
  
```vb  
#Disable Warning BC42356 ' suppress warning about no awaits in this method  
    Async Function TestAsync() As Task  
        Console.WriteLine("testing")  
    End Function  
#Enable Warning BC42356  
```  
  
 还可以禁用和启用以逗号分隔的警告代码列表。  
  
## <a name="related-sections"></a>相关章节  

 [Visual Basic 语言参考](../index.md)  
  