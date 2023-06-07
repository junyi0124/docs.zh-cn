---
title: 参数数组
ms.date: 07/20/2015
helpviewer_keywords:
- parameter arrays [Visual Basic], about parameter arrays
- ParamArray keyword [Visual Basic], parameter arrays
- Visual Basic code, procedures
- parameters [Visual Basic], parameter arrays
- arguments [Visual Basic], parameter arrays
- procedures [Visual Basic], indefinite number of argument values
- arrays [Visual Basic], parameter arrays
ms.assetid: c43edfae-9114-4096-9ebc-8c5c957a1067
ms.openlocfilehash: 2c8c60015d834ffa3f8618dd98616350e13f0e5c
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91100654"
---
# <a name="parameter-arrays-visual-basic"></a>参数数组 (Visual Basic)

通常，使用的参数不能比过程声明指定的过程要多。 如果需要无数个参数，可以声明一个 *参数数组*，该数组允许过程接受参数的值数组。 定义过程时，无需知道参数数组中的元素数目。 每次调用该过程时，都会单独确定数组大小。  
  
## <a name="declaring-a-paramarray"></a>声明 ParamArray  

 使用 [ParamArray](../../../language-reference/modifiers/paramarray.md) 关键字在参数列表中表示参数数组。 下列规则适用：  
  
- 一个过程只能定义一个参数数组，并且它必须是过程定义中的最后一个参数。  
  
- 必须通过值传递参数数组。 在过程定义中显式包含 [ByVal](../../../language-reference/modifiers/byval.md) 关键字是一种很好的编程做法。  
  
- 参数数组是自动可选的。 它的默认值是一个空的一维数组，它是参数数组的元素类型。  
  
- 参数数组前面的所有参数都必须是必需的。 参数数组必须是唯一的可选参数。  
  
## <a name="calling-a-paramarray"></a>调用 ParamArray  

 调用定义参数数组的过程时，可以通过以下方式之一提供参数：  
  
- 无-也就是说，可以省略 [ParamArray](../../../language-reference/modifiers/paramarray.md) 参数。 在这种情况下，空数组将传递给该过程。 如果显式传递 [Nothing](../../../language-reference/nothing.md) 关键字，则空数组将传递给该过程，并且如果被调用的过程不检查此条件，则可能导致 NullReferenceException。
  
- 任意数量的参数的列表（用逗号分隔）。 每个参数的数据类型必须可隐式转换为 `ParamArray` 元素类型。  
  
- 元素类型与参数数组的元素类型相同的数组。  
  
 在所有情况下，该过程中的代码将参数数组视为一维数组，其元素的数据类型与 `ParamArray` 数据类型相同。  
  
> [!IMPORTANT]
> 无论何时处理可能会无限大的阵列，都有 overrunning 应用程序的一些内部容量的风险。 如果接受参数数组，则应测试调用代码传递给它的数组大小。 如果对应用程序来说太大，请采取适当的措施。 有关详细信息，请参阅 [array](../arrays/index.md)。  
  
## <a name="example"></a>示例  

 下面的示例定义并调用函数 `calcSum` 。 `ParamArray`参数的修饰符 `args` 使函数接受数量可变的参数。  
  
 [!code-vb[VbVbalrStatements#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#26)]  
  
 下面的示例使用参数数组定义过程，并输出传递给参数数组的所有数组元素的值。  
  
 [!code-vb[VbVbcnProcedures#48](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#48)]  
  
 [!code-vb[VbVbcnProcedures#49](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#49)]  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Information.UBound%2A>
- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [按值和按引用传递参数](./passing-arguments-by-value-and-by-reference.md)
- [按位置和按名称传递自变量](./passing-arguments-by-position-and-by-name.md)
- [可选参数](./optional-parameters.md)
- [过程重载](./procedure-overloading.md)
- [数组](../arrays/index.md)
- [可选](../../../language-reference/modifiers/optional.md)
