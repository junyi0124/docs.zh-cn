---
title: 过程形参和实参
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], argument lists
- values [Visual Basic], passing to procedures
- arguments [Visual Basic], passing
- procedures [Visual Basic], parameters
- Visual Basic code, argument lists
- Visual Basic code, procedures
- parameters [Visual Basic], Visual Basic procedures
- parameters [Visual Basic], lists
- arguments [Visual Basic], Visual Basic procedures
- arguments [Visual Basic], procedures
- parameter lists [Visual Basic]
- Visual Basic code, parameter lists
- argument lists [Visual Basic]
- procedures [Visual Basic], parameter lists
ms.assetid: ff275aff-aa13-40df-bd4c-63486db8c1e9
ms.openlocfilehash: c7f8eb1fa4e1fa3d87474d048d5a60994b0b7fc5
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071269"
---
# <a name="procedure-parameters-and-arguments-visual-basic"></a>过程参数和自变量 (Visual Basic)

在大多数情况下，过程需要一些有关调用环境的信息。 执行重复或共享任务的过程对每个调用使用不同的信息。 此信息包含变量、常量和在您调用过程时传递给该过程的表达式。  
  
 *参数*表示一个值，该过程期望在调用时提供。 过程声明定义了其参数。  
  
 您可以定义没有参数、一个参数或多个参数的过程。 过程定义中指定参数的部分称为 *参数列表*。  
  
 *参数*表示在调用过程时提供给过程参数的值。 调用代码在调用过程时提供自变量。 过程调用中指定参数的部分称为 *参数列表*。  
  
 下图显示了 `safeSquareRoot` 从两个不同的位置调用过程的代码。 第一次调用将变量的值 `x` (4.0) 传递给参数 `number` ， (2.0) 中的返回值 `root` 将分配给该变量 `y` 。 第二个调用将文本值9.0 传递到 `number` ，并将 (3.0) 的返回值分配给变量 `z` 。  
  
 ![显示将参数传递给参数的关系图](./media/procedure-parameters-and-arguments/pass-argument-parameter.gif)  
  
 有关详细信息，请参阅 [参数和参数之间的差异](./differences-between-parameters-and-arguments.md)。  
  
## <a name="parameter-data-type"></a>参数数据类型  

 可以通过 `As` 在其声明中使用子句来定义参数的数据类型。 例如，以下函数接受字符串和整数。  
  
 [!code-vb[VbVbcnProcedures#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#32)]  
  
 如果类型检查开关 ([Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)) 为 `Off,` `As` 可选，但如果有任何一个参数使用它，则所有参数都必须使用它。 如果类型检查为 `On` ，则 `As` 所有过程参数都需要子句。  
  
 如果调用代码希望提供的参数的数据类型与其对应参数的数据类型不同（如 `Byte` `String` 参数），则必须执行以下操作之一：  
  
- 仅提供数据类型扩大到参数数据类型的自变量;  
  
- 设置 `Option Strict Off` 为允许隐式收缩转换; 或  
  
- 使用转换关键字显式转换数据类型。  
  
### <a name="type-parameters"></a>类型参数  

 *泛型过程*除了定义其正常参数外，还定义了一个或多个*类型参数*。 泛型过程允许调用代码在每次调用该过程时传递不同的数据类型，因此它可以根据每个调用的要求来定制数据类型。 请参阅 [Generic Procedures in Visual Basic](../data-types/generic-procedures.md)。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [Sub 过程](./sub-procedures.md)
- [Function 过程](./function-procedures.md)
- [Property 过程](./property-procedures.md)
- [运算符过程](./operator-procedures.md)
- [如何：为过程定义参数](./how-to-define-a-parameter-for-a-procedure.md)
- [如何：将参数传递给过程](./how-to-pass-arguments-to-a-procedure.md)
- [按值和按引用传递参数](./passing-arguments-by-value-and-by-reference.md)
- [过程重载](./procedure-overloading.md)
- [Visual Basic 中的类型转换](../data-types/type-conversions.md)
