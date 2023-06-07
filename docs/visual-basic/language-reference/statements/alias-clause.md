---
title: Alias 子句
ms.date: 07/20/2015
f1_keywords:
- vb.Alias
helpviewer_keywords:
- Alias keyword [Visual Basic]
ms.assetid: 58c06b11-465d-4d87-906a-73200a3d7f19
ms.openlocfilehash: 77d4685f242864842e5a84b3a3de3ba1793e9aa4
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866678"
---
# <a name="alias-clause-visual-basic"></a>Alias 子句 (Visual Basic)

指示外部过程在其 DLL 中有另一个名称。  
  
## <a name="remarks"></a>备注  

 `Alias`关键字可以在此上下文中使用：  
  
 [Declare Statement](declare-statement.md)  
  
 在下面的示例中， `Alias` 关键字用于提供 advapi32.dll 的中的函数名称，该名称在 `GetUserNameA` `getUserName` 此示例中用于代替。 `getUserName`在 sub 中调用函数 `getUser` ，该函数显示当前用户的名称。  
  
 [!code-vb[VbVbalrStatements#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#15)]  
  
## <a name="see-also"></a>另请参阅

- [关键字](../keywords/index.md)
