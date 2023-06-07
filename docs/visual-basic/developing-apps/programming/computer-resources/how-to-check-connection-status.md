---
title: 如何：检查连接状态
ms.date: 07/20/2015
helpviewer_keywords:
- Web connections [Visual Basic]
- IsAvailable property [Visual Basic], about IsAvailable
- connections [Visual Basic], checking status
- connection status [Visual Basic]
ms.assetid: 4d9ee8ab-9a6f-4279-ace4-b75afc976a74
ms.openlocfilehash: 89ef431759dac25bd213fd954db0712ad95434b0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345885"
---
# <a name="how-to-check-connection-status-in-visual-basic"></a>如何：在 Visual Basic 中检查连接状态

<xref:Microsoft.VisualBasic.Devices.Network.IsAvailable> 属性可用于确定计算机是否具有有效的网络连接或 Internet 连接。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-check-whether-a-computer-has-a-working-connection"></a>检查计算机是否具有有效的网络连接  
  
- 确定 `IsAvailable` 属性是 `True` 还是 `False`。 以下代码检查属性的状态并进行报告：  
  
     [!code-vb[VbResourceTasks#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#3)]  
  
     此代码示例也可作为 IntelliSense 代码片段。 它位于代码片段选取器的“连接和网络”中。  有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Network?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Devices.Network.IsAvailable>
