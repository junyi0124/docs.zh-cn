---
title: 重载过程注意事项
ms.date: 07/20/2015
helpviewer_keywords:
- signatures [Visual Basic], ParamArray arguments
- ParamArray keyword [Visual Basic], parameter arrays
- ParamArray keyword [Visual Basic], arguments and signatures
- function overloading [Visual Basic], implicit overloads for ParamArray
- ParamArray keyword [Visual Basic], signatures
- Visual Basic code, procedures
- arguments [Visual Basic], parameter arrays
- procedures [Visual Basic], overloading
- parameters [Visual Basic], lists
- function overloading [Visual Basic], typeless programming
- typeless programming
- function overloading [Visual Basic], restrictions
- arguments [Visual Basic], optional
- optional arguments [Visual Basic], overloading
- signatures [Visual Basic], procedure
- parameter lists [Visual Basic]
- parameter arrays [Visual Basic], overloading arguments
- Visual Basic code, parameter lists
- procedure overloading [Visual Basic], considerations
- Option Explicit statement [Visual Basic]
- restrictions [Visual Basic], overloading procedures
- procedures [Visual Basic], parameter lists
ms.assetid: a2001248-10d0-42c5-b0ce-eeedc987319f
ms.openlocfilehash: 6197fa4041a22944542b2657e35bc293a6e5c244
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075052"
---
# <a name="considerations-in-overloading-procedures-visual-basic"></a>重载过程注意事项 (Visual Basic)

重载过程时，必须为每个重载版本使用不同的 *签名* 。 这通常意味着每个版本都必须指定不同的参数列表。 有关详细信息，请参阅 [过程重载](./procedure-overloading.md)中的 "不同签名"。  
  
 如果过程具有不同的签名，则可以 `Function` 使用过程重载过程 `Sub` ，反之亦然。 两个重载只能不同于有一个返回值，而另一个没有返回值。  
  
 您可以重载属性，方法与重载过程的方法相同，并且具有相同的限制。 但是，不能使用属性重载过程，反之亦然。  
  
## <a name="alternatives-to-overloaded-versions"></a>重载版本的替代项  

 有时会有一些重载版本的替代方法，特别是在有可选参数时，或参数的数目是可变的。  
  
 请记住，可选参数并不一定支持所有语言，参数数组仅限 Visual Basic。 如果要编写的过程可能会从用多种不同语言的任意语言编写的代码中调用，则重载版本提供最大的灵活性。  
  
### <a name="overloads-and-optional-arguments"></a>重载和可选参数  

 如果调用代码可以选择提供或忽略一个或多个参数，则可以定义多个重载版本或使用可选参数。  
  
#### <a name="when-to-use-overloaded-versions"></a>何时使用重载版本  

 可以考虑在以下情况下定义一系列重载版本：  
  
- 过程代码中的逻辑明显不同，具体取决于调用代码是否提供可选参数。  
  
- 过程代码无法可靠地测试调用代码是否提供了可选参数。 例如，如果不能提供调用代码预期无法提供的默认值，则会出现这种情况。  
  
#### <a name="when-to-use-optional-parameters"></a>何时使用可选参数  

 在以下情况下，你可能更倾向于使用一个或多个可选参数：  
  
- 调用代码未提供可选参数时唯一必需的操作是将参数设置为默认值。 在这种情况下，如果使用一个或多个参数定义单个版本，则过程代码可能会比较复杂 `Optional` 。  
  
 有关详细信息，请参阅 [可选参数](./optional-parameters.md)。  
  
### <a name="overloads-and-paramarrays"></a>重载和 ParamArrays  

 当调用代码可以传递可变数量的参数时，可以定义多个重载版本或使用参数数组。  
  
#### <a name="when-to-use-overloaded-versions"></a>何时使用重载版本  

 可以考虑在以下情况下定义一系列重载版本：  
  
- 您知道，调用代码永远不会向参数数组传递超过少量的值。  
  
- 过程代码中的逻辑明显不同，具体取决于调用代码传递的值的数量。  
  
- 调用代码可以传递不同数据类型的值。  
  
#### <a name="when-to-use-a-parameter-array"></a>何时使用参数数组  

 在以下情况下，你可以更好地处理 `ParamArray` 参数：  
  
- 您无法预测调用代码可传递给参数数组的值数量，并且它可能是一个较大的数值。  
  
- 过程逻辑适合于循环访问调用代码所通过的所有值，实际上对每个值执行相同的操作。  
  
 有关详细信息，请参阅 [参数数组](./parameter-arrays.md)。  
  
## <a name="implicit-overloads-for-optional-parameters"></a>可选参数的隐式重载  

 带有 [可选](../../../language-reference/modifiers/optional.md) 参数的过程等效于两个重载过程，其中一个参数具有可选参数，一个参数不包含它。 不能使用与上述任一方法对应的参数列表重载此类过程。 以下声明阐释了这一点。  
  
 [!code-vb[VbVbcnProcedures#58](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#58)]  
  
 [!code-vb[VbVbcnProcedures#60](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#60)]  
  
 [!code-vb[VbVbcnProcedures#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#61)]  
  
 对于具有多个可选参数的过程，有一组隐式重载，它们与前面示例中的逻辑类似。  
  
## <a name="implicit-overloads-for-a-paramarray-parameter"></a>ParamArray 参数的隐式重载  

 编译器将具有 [ParamArray](../../../language-reference/modifiers/paramarray.md) 参数的过程视为具有无限数目的重载，这些重载在调用代码传递给参数数组的内容中彼此不同，如下所示：  
  
- 调用代码未向以下项提供参数时的一个重载 `ParamArray`  
  
- 如果调用代码提供一维元素类型的数组，则为一个重载 `ParamArray`  
  
- 对于每个正整数，调用代码提供该数量的参数的重载，每个 `ParamArray` 元素类型  
  
 以下声明阐释了这些隐式重载。  
  
 [!code-vb[VbVbcnProcedures#68](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#68)]  
  
 [!code-vb[VbVbcnProcedures#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#70)]  
  
 不能使用参数列表重载此类过程，该参数列表采用一维数组作为参数数组。 但是，可以使用其他隐式重载的签名。 以下声明阐释了这一点。  
  
 [!code-vb[VbVbcnProcedures#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#71)]  
  
## <a name="typeless-programming-as-an-alternative-to-overloading"></a>作为重载替代的替代方法  

 如果要允许调用代码将不同数据类型传递给参数，则可以使用一种无类型编程方法。 你可以将类型检查开关设置为， `Off` 同时 [选择 Option Strict 语句](../../../language-reference/statements/option-strict-statement.md) 或 [-optionstrict](../../../reference/command-line-compiler/optionstrict.md) 编译器选项。 然后不必声明参数的数据类型。 不过，与重载相比，此方法具有以下缺点：  
  
- 无程序编程产生的执行代码效率较低。  
  
- 此过程必须测试其预期要传递的每个数据类型。  
  
- 如果调用代码传递了过程不支持的数据类型，则编译器无法发出错误信号。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [过程疑难解答](./troubleshooting-procedures.md)
- [如何：定义一个过程的多个版本](./how-to-define-multiple-versions-of-a-procedure.md)
- [如何：调用重载过程](./how-to-call-an-overloaded-procedure.md)
- [如何：重载带有可选参数的过程](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [如何：重载参数数量不确定的过程](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [重载决策](./overload-resolution.md)
- [重载](../../../language-reference/modifiers/overloads.md)
