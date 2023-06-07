---
title: 可选参数
ms.date: 07/20/2015
helpviewer_keywords:
- parameters [Visual Basic], optional
- Visual Basic code, procedures
- procedures [Visual Basic], optional arguments
- optional arguments
- named arguments [Visual Basic], and optional arguments
- optional parameters
- Optional keyword [Visual Basic], optional arguments
- arguments [Visual Basic], optional
- optional arguments [Visual Basic], and named arguments
ms.assetid: 398d2845-1069-4e94-b934-a73b545c8b87
ms.openlocfilehash: 73a51ab65a8ea4c38b6fd6737279fb19fb1cfe73
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071854"
---
# <a name="optional-parameters-visual-basic"></a>可选参数 (Visual Basic)

可以指定过程参数是可选的，并且在调用过程时不必为其提供自变量。 *可选参数* 由 `Optional` 过程定义中的关键字指示。 下列规则适用：  
  
- 过程定义中的每个可选参数都必须指定默认值。  
  
- 可选参数的默认值必须是一个常数表达式。  
  
- 过程定义中跟在可选参数后的每个参数也都必须是可选的。  
  
 下面的语法显示带可选参数的过程声明：  
  
```vb  
Sub name(ByVal parameter1 As datatype1, Optional ByVal parameter2 As datatype2 = defaultvalue)  
```  
  
## <a name="calling-procedures-with-optional-parameters"></a>调用带可选参数的过程  

 调用带可选参数的过程时，可以选择是否提供该自变量。 如果不提供，过程将使用为该参数声明的默认值。  
  
 当省略自变量列表中的一个或多个可选自变量时，使用连续的逗号来标记它们的位置。 下面的调用示例提供了第一个和第四个自变量，省略了第二个和第三个：  
  
```vb  
Sub name(argument 1, , , argument 4)  
```  
  
 下面的示例对 `MsgBox` 函数进行多次调用。 `MsgBox` 有一个必需参数和两个可选参数。  
  
 对 `MsgBox` 的第一个调用将按照 `MsgBox` 定义参数的顺序提供所有三个参数。 第二个调用仅提供必选自变量。 第三个和第四个调用分别提供第一个和第三个自变量。 第三个调用按位置提供参数，第四个调用按名称提供参数。  
  
 [!code-vb[VbVbcnProcedures#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#47)]  
  
## <a name="determining-whether-an-optional-argument-is-present"></a>确定可选自变量是否存在  

 过程在运行时无法检测到给定的自变量是否已被省略，或者调用代码是否已显式提供默认值。 如果需要弄清楚这一点，可以设置一个不可能的值作为默认值。 下面的过程定义可选参数 `office` ，并测试其默认值， `QJZ` 以确定是否在调用中省略了该参数：  
  
 [!code-vb[VbVbcnProcedures#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#46)]  
  
 如果可选参数是像 `String` 这样的引用类型，只要它不是该自变量所预期的值，就可以使用 `Nothing` 作为默认值。  
  
## <a name="optional-parameters-and-overloading"></a>可选参数和重载  

 定义带可选参数的过程的另一种方法是使用重载。 如果有一个可选参数，可以定义过程的两个重载版本，一个接受此参数，另一个则不带参数。 此方法随可选参数数目的增加而变得更复杂。 然而，这样做的优点是可以完全确定调用程序是否提供了每个可选自变量。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [按值和按引用传递参数](./passing-arguments-by-value-and-by-reference.md)
- [按位置和按名称传递自变量](./passing-arguments-by-position-and-by-name.md)
- [参数数组](./parameter-arrays.md)
- [过程重载](./procedure-overloading.md)
- [可选](../../../language-reference/modifiers/optional.md)
- [ParamArray](../../../language-reference/modifiers/paramarray.md)
