---
title: 如何：为过程定义参数
ms.date: 07/20/2015
helpviewer_keywords:
- procedure parameters [Visual Basic], defining data types for
- procedures [Visual Basic], parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters [Visual Basic], defining
ms.assetid: 7962808d-407e-4e84-984e-43e9857c53c9
ms.openlocfilehash: 73b53dcf7cd732af1a4f1d23cd0d3b9ef5b5529b
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91087435"
---
# <a name="how-to-define-a-parameter-for-a-procedure-visual-basic"></a>如何：为过程定义参数 (Visual Basic)

*参数*允许调用代码在调用时将值传递给过程。 声明过程的每个参数的方式与声明变量的方式相同，即指定其名称和数据类型。 还可指定传递机制，以及参数是否为可选。  
  
 有关详细信息，请参阅 [过程参数和参数](./procedure-parameters-and-arguments.md)。  
  
### <a name="to-define-a-procedure-parameter"></a>定义过程参数  
  
1. 在过程声明中，将参数名称添加到过程的参数列表，并将其与其他参数之间用逗号分隔开。  
  
2. 确定参数的数据类型。  
  
3. 使用子句的参数名称 `As` 来指定数据类型。  
  
4. 确定要用于参数的传递机制。 通常，按值传递参数，除非您希望该过程能够在调用代码中更改其值。  
  
5. 在参数名称前面加上 [ByVal](../../../language-reference/modifiers/byval.md) 或 [ByRef](../../../language-reference/modifiers/byref.md) 来指定传递机制。 有关详细信息，请参阅 [按值传递参数和通过引用传递参数之间的差异](./differences-between-passing-an-argument-by-value-and-by-reference.md)。  
  
6. 如果该参数是可选的，请在传递机制之前加上 [optional](../../../language-reference/modifiers/optional.md) ，并在参数数据类型后跟一个等号 (`=`) 和一个默认值。  
  
     下面的示例 `Sub` 使用三个参数定义过程的轮廓。 前两个参数是必需的，第三个参数是可选的。 参数声明在参数列表中用逗号分隔。  
  
     [!code-vb[VbVbcnProcedures#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#33)]  
  
     第一个参数接受一个 `customer` 对象，并且 `updateCustomer` 可以直接更新传递给的变量， `c` 因为参数是传递[ByRef](../../../language-reference/modifiers/byref.md)的。 此过程无法更改最后两个参数的值，因为它们是通过 [ByVal](../../../language-reference/modifiers/byval.md)传递的。  
  
     如果调用代码未提供参数的值 `level` ，Visual Basic 将其设置为默认值0。  
  
     如果类型检查开关 ([Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)) 为，则在 `Off` `As` 定义参数时，子句是可选的。 但是，如果任何一个参数使用 `As` 子句，则所有参数都必须使用它。 如果类型检查开关为 `On` ，则 `As` 每个参数定义都需要子句。  
  
     为所有编程元素指定数据类型称为 *强*类型。 设置时 `Option Strict On` ，Visual Basic 强制执行强类型化。 出于以下原因，强烈建议执行此操作：  
  
    - 它为你的变量和参数启用 IntelliSense 支持。 这使您可以在代码中键入时查看它们的属性和其他成员。  
  
    - 它允许编译器执行类型检查。 这有助于捕获由于溢出等错误而在运行时可能会失败的语句。 它还会捕获对不支持这些对象的方法的调用。  
  
    - 这会使代码的执行速度更快。 导致这种情况的原因之一是，如果未指定编程元素的数据类型，则 Visual Basic 编译器会为其分配 `Object` 类型。 已编译的代码可能需要在 `Object` 和其他数据类型之间来回转换，这会降低性能。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [Sub 过程](./sub-procedures.md)
- [Function 过程](./function-procedures.md)
- [如何：将参数传递给过程](./how-to-pass-arguments-to-a-procedure.md)
- [按值和按引用传递参数](./passing-arguments-by-value-and-by-reference.md)
- [递归过程](./recursive-procedures.md)
- [过程重载](./procedure-overloading.md)
- [对象和类](../objects-and-classes/index.md)
- [面向对象的编程 (Visual Basic)](../../concepts/object-oriented-programming.md)
