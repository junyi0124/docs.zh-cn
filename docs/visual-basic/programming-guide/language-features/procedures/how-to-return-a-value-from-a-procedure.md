---
title: 如何：从过程返回值
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], returning from
- procedures [Visual Basic], returning a value
ms.assetid: 4bcc4724-2b4e-4df8-9b4b-16054607f87d
ms.openlocfilehash: cbc785a07aa8a7b299508a093e08d5d0510b838a
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071373"
---
# <a name="how-to-return-a-value-from-a-procedure-visual-basic"></a>如何：从过程返回值 (Visual Basic)

`Function`过程通过执行 `Return` 语句或遇到或语句，将值返回到调用代码 `Exit Function` `End Function` 。  
  
### <a name="to-return-a-value-using-the-return-statement"></a>使用 Return 语句返回值  
  
1. 将 `Return` 语句放在过程任务完成的点。  
  
2. 在关键字后面跟 `Return` 一个表达式，该表达式生成要返回到调用代码的值。  
  
3. 在同一过程中可拥有多个 `Return` 语句。  
  
     下面的 `Function` 过程计算直角三角形的最长边，或斜边，并将其返回给调用代码。  
  
     [!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]  
  
     下面的示例演示对的典型调用 `hypotenuse` ，它存储返回的值。  
  
     [!code-vb[VbVbcnProcedures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#6)]  
  
### <a name="to-return-a-value-using-exit-function-or-end-function"></a>使用 Exit 函数或 End 函数返回值  
  
1. 在过程中的至少一个位置 `Function` ，为过程名称赋值。  
  
2. 执行 `Exit Function` 或 `End Function` 语句时，Visual Basic 返回最近分配给该过程名称的值。  
  
3. 在同一过程中可拥有多个 `Exit Function` 语句，也可混合 `Return` 和 `Exit Function` 语句。  
  
4. 一个过程中只能有一个 `End Function` 语句 `Function` 。  
  
     有关详细信息和示例，请参阅 [Function 语句](../../../language-reference/statements/function-statement.md)中的 "返回值"。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [Sub 过程](./sub-procedures.md)
- [Property 过程](./property-procedures.md)
- [运算符过程](./operator-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Function 语句](../../../language-reference/statements/function-statement.md)
- [Return 语句](../../../language-reference/statements/return-statement.md)
- [如何：创建返回值的过程](./how-to-create-a-procedure-that-returns-a-value.md)
- [如何：调用返回值的过程](./how-to-call-a-procedure-that-returns-a-value.md)
