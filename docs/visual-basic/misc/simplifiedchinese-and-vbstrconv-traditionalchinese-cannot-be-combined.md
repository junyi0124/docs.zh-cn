---
title: 不能组合 SimplifiedChinese 和 VbStrConv.TraditionalChinese
ms.date: 07/20/2015
f1_keywords:
- vbrArgument_StrConvSCandTC
ms.assetid: d8e6a11b-f549-43b5-8337-0594340e1325
ms.openlocfilehash: 512651add89de23b35512e736dbf74f0891c995a
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91060479"
---
# <a name="simplifiedchinese-and-vbstrconvtraditionalchinese-cannot-be-combined"></a>不能组合 SimplifiedChinese 和 VbStrConv.TraditionalChinese

应用程序尝试组合 `VbStrConv` 枚举成员 `SimplifiedChinese` 和 `TraditionalChinese`，而它们是互斥的。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 删除 `VbStrConv.SimplifiedChinese` 或 `VbStrConv.TraditionalChinese`。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Globalization>

- [开发全球化和本地化应用](/visualstudio/ide/globalizing-and-localizing-applications)
