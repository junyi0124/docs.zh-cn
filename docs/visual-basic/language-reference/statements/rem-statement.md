---
title: REM 语句
ms.date: 07/20/2015
f1_keywords:
- vb.'
- vb.Rem
- Rem
- "'"
helpviewer_keywords:
- REM statement [Visual Basic]
- comments, Visual Basic code
- code comments, Visual Basic
- Visual Basic code, comments
- "' comment marker character [Visual Basic]"
ms.assetid: 34126d7f-e0f9-476d-91e6-b31b398615dc
ms.openlocfilehash: 6161a9f7e589988882b5f76477bc069afd2b9b41
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90871952"
---
# <a name="rem-statement-visual-basic"></a>REM 语句 (Visual Basic)

用于在程序的源代码中包括解释性注释。  
  
## <a name="syntax"></a>语法  
  
```vb  
REM comment  
' comment  
```  
  
## <a name="parts"></a>组成部分  

 `comment`  
 可选。 要包括的任何注释的文本。 关键字和之间必须有一个空格 `REM` `comment` 。  
  
## <a name="remarks"></a>备注  

 您可以将 `REM` 语句放在一行上，也可以将其放在另一语句后面的行上。 `REM`语句必须是行中的最后一条语句。 如果它跟在另一语句之后，则 `REM` 必须用空格将其与该语句分隔开。  
  
 您可以 (`'`) 而不是使用单引号 `REM` 。 无论您的注释是在同一行后面还是单独的行上，都是如此。  
  
> [!NOTE]
> 不能 `REM` 使用行继续符序列 () 继续执行语句 `_` 。 注释开始后，编译器不会检查字符的特殊含义。 对于多行注释，请 `REM` 在每行上使用另一个语句或注释符号 (`'`) 。  
  
## <a name="example"></a>示例  

 下面的示例说明 `REM` 语句，该语句用于在程序中包含解释性注释。 它还显示了使用单引号字符 (`'`) 而不是的另一种方法 `REM` 。  
  
 [!code-vb[VbVbalrStatements#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>另请参阅

- [代码中的注释](../../programming-guide/program-structure/comments-in-code.md)
- [如何：在代码中拆分和合并语句](../../programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
