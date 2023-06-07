---
title: 按值和按引用传递参数
ms.date: 07/20/2015
helpviewer_keywords:
- ByRef keyword [Visual Basic], passing arguments by reference
- Visual Basic code, procedures
- passing arguments [Visual Basic], by value or by reference
- ByVal keyword [Visual Basic], passing arguments by value
- arguments [Visual Basic], passing by value or by reference
- argument passing [Visual Basic], by value or by reference
ms.assetid: fd8a9de6-7178-44d5-a9bf-458d4ad907c2
ms.openlocfilehash: b7430b209f53a0a924ec587a0097178baf0075e3
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91059208"
---
# <a name="passing-arguments-by-value-and-by-reference-visual-basic"></a>通过值和通过引用传递参数 (Visual Basic)

在 Visual Basic 中，可以 *通过值* 或 *引用*将参数传递给过程。 这称为 *传递机制*，它确定过程是否可以修改调用代码中参数的基础编程元素。 过程声明通过指定 [ByVal](../../../language-reference/modifiers/byval.md) 或 [ByRef](../../../language-reference/modifiers/byref.md) 关键字来确定每个参数的传递机制。  
  
## <a name="distinctions"></a>区别  

 将参数传递给过程时，请注意彼此交互的几个不同的区别：  
  
- 基础编程元素是否可修改或不可更改  
  
- 参数本身是否可修改或不可更改  
  
- 参数是通过值还是通过引用传递  
  
- 参数数据类型是值类型还是引用类型  
  
 有关详细信息，请参阅可 [修改和不可修改参数](./differences-between-modifiable-and-nonmodifiable-arguments.md) 与通过 [值传递参数和通过引用传递参数](./differences-between-passing-an-argument-by-value-and-by-reference.md)之间的差异。  
  
## <a name="choice-of-passing-mechanism"></a>选择传递机制  

 应为每个参数仔细选择传递机制。  
  
- **保护**。 在两个传递机制之间进行选择时，最重要的条件是公开调用变量以进行更改。 传递参数的优点 `ByRef` 是过程可以通过该参数将值返回给调用代码。 传递参数的优点在于 `ByVal` ，它可以防止变量被过程更改。  
  
- **性能**。 尽管传递机制可能会影响你的代码的性能，但差别通常是不重要的。 这种情况的一个例外是传递了一个值类型 `ByVal` 。 在这种情况下，Visual Basic 会复制参数的所有数据内容。 因此，对于大值类型（如结构），传递它会更有效 `ByRef` 。  
  
     对于引用类型，仅将指向数据的指针复制 (32 位平台上的四个字节，64位平台上的8个字节) 。 因此，可以传递类型的参数或通过值传递的参数， `String` `Object` 而不会对性能有损害。  
  
## <a name="determination-of-the-passing-mechanism"></a>确定传递机制  

 过程声明指定每个参数的传递机制。 调用代码无法重写 `ByVal` 机制。  
  
 如果使用声明参数 `ByRef` ，则调用代码可以 `ByVal` 通过在调用中将参数名称括在括号中来强制机制。 有关详细信息，请参阅 [如何：强制通过值传递参数](./how-to-force-an-argument-to-be-passed-by-value.md)。  
  
 Visual Basic 中的默认值是按值传递参数。  
  
## <a name="when-to-pass-an-argument-by-value"></a>何时按值传递参数  
  
- 如果作为参数的基础的调用代码元素是不可更改的元素，则声明相应的参数 [ByVal](../../../language-reference/modifiers/byval.md)。 任何代码都不能更改不可更改元素的值。  
  
- 如果基础元素可修改，但您不希望该过程更改其值，则声明参数 `ByVal` 。 只有调用代码可以更改通过值传递的可修改元素的值。  
  
## <a name="when-to-pass-an-argument-by-reference"></a>何时通过引用传递自变量  
  
- 如果过程确实需要更改调用代码中的基础元素，请声明相应的参数 [ByRef](../../../language-reference/modifiers/byref.md)。  
  
- 如果正确执行代码取决于更改调用代码中基础元素的过程，请声明参数 `ByRef` 。 如果按值传递它，或者调用代码 `ByRef` 通过将参数括在括号中来覆盖传递机制，则过程调用可能会产生意外的结果。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 下面的示例说明何时按值传递参数，以及何时通过引用传递参数。 过程 `Calculate` 具有 `ByVal` 和 `ByRef` 参数。 对于利率、 `rate` 和金钱的总和， `debt` 此过程的任务是为计算一个新值 `debt` ，这是将利率应用于原始值的结果 `debt` 。 由于 `debt` 是一个 `ByRef` 参数，因此新的总计将反映在与对应的调用代码中的参数值中 `debt` 。 参数 `rate` 是一个 `ByVal` 参数，因为 `Calculate` 不应更改其值。  
  
### <a name="code"></a>代码  

 [!code-vb[VbVbcnProcedures#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class2.vb#74)]  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [如何：将参数传递给过程](./how-to-pass-arguments-to-a-procedure.md)
- [如何：更改过程参数的值](./how-to-change-the-value-of-a-procedure-argument.md)
- [如何：防止过程参数的值被更改](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [如何：强制通过值传递参数](./how-to-force-an-argument-to-be-passed-by-value.md)
- [按位置和按名称传递自变量](./passing-arguments-by-position-and-by-name.md)
- [Value Types and Reference Types](../data-types/value-types-and-reference-types.md)
