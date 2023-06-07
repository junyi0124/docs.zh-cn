---
title: Nothing 和字符串
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], Nothing
ms.assetid: 261380af-2024-4ecf-823b-43b1034d92cd
ms.openlocfilehash: d4c7ee6d13334617a80abb845af52bf388a12797
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072517"
---
# <a name="nothing-and-strings-in-visual-basic"></a>Nothing 和字符串 (Visual Basic)

当涉及到字符串时，Visual Basic 运行时和 .NET Framework 的评估 `Nothing` 方式有所不同。  
  
## <a name="visual-basic-runtime-and-the-net-framework"></a>Visual Basic 运行时和 .NET Framework  

 请考虑以下示例：  
  
 [!code-vb[VbVbalrStrings#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#47)]  
  
 Visual Basic 运行时通常计算 `Nothing` 为空字符串 ( "" ) 。 但是，无论何时尝试对执行字符串操作，.NET Framework 都不会引发异常 `Nothing` 。  
  
## <a name="see-also"></a>请参阅

- [字符串介绍 (Visual Basic)](introduction-to-strings.md)
