---
title: 由于内部错误，无法获取完整的操作系统名
ms.date: 07/20/2015
f1_keywords:
- vbrDiagnosticInfo_FullOSName
ms.assetid: f69da02b-eb9a-4284-bb9e-3025517ae6c1
ms.openlocfilehash: 744d549b9313727af2feb82e45c24b729cae7262
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91079082"
---
# <a name="could-not-obtain-full-operation-system-name-due-to-internal-error"></a>由于内部错误，无法获取完整的操作系统名

由于内部错误，无法获取完整的操作系统名。 这可能由当前计算机上不存在 WMI 导致。  
  
 对 `My.Computer.Info.OSFullName` 属性的调用失败。 此失败的可能原因是当前计算机上是否未安装 Windows Management Instrumentation (WMI)。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 将调用周围的 `Try...Catch` 块添加到 `My.Computer.Info.OSFullName` 属性。  
  
2. 有关 WMI 及其安装方法的详细信息，请参阅和搜索 "Windows Management Instrumentation Core"。  
  
## <a name="see-also"></a>请参阅

- [OSFullName。](xref:Microsoft.VisualBasic.Devices.ComputerInfo.OSFullName)
- [在 .NET 中处理和引发异常](../../standard/exceptions/index.md)
- [尝试 .。。Catch .。。Finally 语句](../language-reference/statements/try-catch-finally-statement.md)
