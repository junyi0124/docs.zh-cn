---
title: 如何：防止过程参数的值被更改
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- arguments [Visual Basic], passing by reference
- Visual Basic code, procedures
- arguments [Visual Basic], ByVal
- arguments [Visual Basic], passing by value
- procedure parameters
- procedures [Visual Basic], calling
- arguments [Visual Basic], ByRef
- arguments [Visual Basic], changing value
ms.assetid: d2b7c766-ce16-4d2c-8d79-3fc0e7ba2227
ms.openlocfilehash: 84eadf3d364b69120221d80e464b1175b1602e13
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071477"
---
# <a name="how-to-protect-a-procedure-argument-against-value-changes-visual-basic"></a>如何：防止过程自变量的值被更改 (Visual Basic)

如果过程将参数声明为 [ByRef](../../../language-reference/modifiers/byref.md)，Visual Basic 将为过程代码提供对调用代码中参数的基础编程元素的直接引用。 这允许过程更改调用代码中参数的基础值。 在某些情况下，调用代码可能需要防止这种更改。  
  
 始终可以通过在过程中声明相应的参数 [ByVal](../../../language-reference/modifiers/byval.md) ，来保护参数不受更改。 如果希望能够在某些情况下更改给定的自变量，而不是其他情况，则可以声明该参数， `ByRef` 并让调用代码确定每个调用中的传递机制。 为此，可将相应的自变量括在括号中，以便按值传递它，或者不将其括在括号中，以便通过引用传递它。 有关详细信息，请参阅 [如何：强制通过值传递参数](./how-to-force-an-argument-to-be-passed-by-value.md)。  
  
## <a name="example"></a>示例  

 下面的示例演示了两个过程，这些过程使用数组变量并对其元素进行操作。 此 `increase` 过程只是向每个元素添加一个元素。 此 `replace` 过程将新数组分配给参数 `a()` ，然后将一个数组添加到每个元素。 但是，重新分配不会影响调用代码中的基础数组变量。  
  
 [!code-vb[VbVbcnProcedures#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#35)]  
  
 [!code-vb[VbVbcnProcedures#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#38)]  
  
 [!code-vb[VbVbcnProcedures#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#37)]  
  
 第一次 `MsgBox` 调用显示 "增加 (n) ：11，21，31，41" 之后。 由于数组 `n` 是引用类型，因此 `increase` 即使传递机制为，也可以更改其成员 `ByVal` 。  
  
 第二次 `MsgBox` 调用显示 "replace (n) ：11，21，31，41" 之后。 由于 `n` 已传递 `ByVal` ，因此 `replace` 无法 `n` 通过向其分配新数组来修改调用代码中的变量。 当 `replace` 创建新的数组实例 `k` 并将其分配给本地变量时 `a` ，它将失去 `n` 调用代码传入的引用。 当它更改的成员时 `a` ，只 `k` 会影响本地数组。 因此，不 `replace` 会 `n` 在调用代码中递增数组的值。  
  
## <a name="compile-the-code"></a>编译代码  

 Visual Basic 中的默认值是按值传递参数。 但是，将 [ByVal](../../../language-reference/modifiers/byval.md) 或 [ByRef](../../../language-reference/modifiers/byref.md) 关键字用于每个声明的参数是一种好的编程做法。 这使代码更易于阅读。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [如何：将参数传递给过程](./how-to-pass-arguments-to-a-procedure.md)
- [按值和按引用传递参数](./passing-arguments-by-value-and-by-reference.md)
- [可修改和不可修改参数之间的差异](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [通过值传递参数和通过引用传递参数之间的差异](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [如何：更改过程参数的值](./how-to-change-the-value-of-a-procedure-argument.md)
- [如何：强制通过值传递参数](./how-to-force-an-argument-to-be-passed-by-value.md)
- [按位置和按名称传递自变量](./passing-arguments-by-position-and-by-name.md)
- [Value Types and Reference Types](../data-types/value-types-and-reference-types.md)
