---
title: 如何：调用重载过程
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], overloading
- procedures [Visual Basic], calling
- procedures [Visual Basic], multiple versions
- procedure calls [Visual Basic], overloaded
ms.assetid: 3bb331fb-f6bc-406f-9ca0-9609b497014c
ms.openlocfilehash: 68b8a9898cba846b63ed8ce9d8329516f12e90cb
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075169"
---
# <a name="how-to-call-an-overloaded-procedure-visual-basic"></a>如何：调用重载过程 (Visual Basic)

重载过程的优点是调用的灵活性。 调用代码可以获取它需要传递给过程的信息，然后调用单个过程名称，无论它传递了哪些参数。  
  
### <a name="to-call-a-procedure-that-has-more-than-one-version-defined"></a>调用具有多个定义版本的过程  
  
1. 在调用代码中，确定要传递给过程的数据。  
  
2. 以正常方式编写过程调用，并在参数列表中显示数据。 请确保参数与为该过程定义的版本之一中的参数列表匹配。  
  
3. 您不必确定要调用的过程的版本。 Visual Basic 将控制传递到与参数列表匹配的版本。  
  
     下面的示例调用 `post` [如何：定义过程的多个版本](./how-to-define-multiple-versions-of-a-procedure.md)中声明的过程。 它获取客户标识，确定它是 `String` 还是 `Integer` ，然后在任一情况下都调用相同的过程。  
  
     [!code-vb[VbVbcnProcedures#56](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#56)]  
  
     [!code-vb[VbVbcnProcedures#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#57)]  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [过程重载](./procedure-overloading.md)
- [过程疑难解答](./troubleshooting-procedures.md)
- [如何：定义一个过程的多个版本](./how-to-define-multiple-versions-of-a-procedure.md)
- [如何：重载带有可选参数的过程](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [如何：重载参数数量不确定的过程](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [重载过程注意事项](./considerations-in-overloading-procedures.md)
- [重载决策](./overload-resolution.md)
- [重载](../../../language-reference/modifiers/overloads.md)
