---
title: 如何：重载参数数量不确定的过程
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], parameters
- procedure overloading [Visual Basic], indefinite number of parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
ms.assetid: c7042de2-2422-4039-94e8-ac298896af69
ms.openlocfilehash: 10cd7d11b0efe9fa5eb3ae24269a4cdbe33bc08a
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071542"
---
# <a name="how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters-visual-basic"></a>如何：重载参数数量不确定的过程 (Visual Basic)

如果过程具有 [ParamArray](../../../language-reference/modifiers/paramarray.md) 参数，则不能为参数数组定义采用一维数组的重载版本。 有关详细信息，请参阅 [重载过程](./considerations-in-overloading-procedures.md)中的注意事项中的 "ParamArray 参数的隐式重载"。  
  
### <a name="to-overload-a-procedure-that-takes-a-variable-number-of-parameters"></a>重载采用可变数量的参数的过程  
  
1. 确定过程和调用代码逻辑从重载版本中受益于比参数更多 `ParamArray` 。 有关 [重载过程的注意事项](./considerations-in-overloading-procedures.md)，请参阅 "Overloads and ParamArrays"。  
  
2. 确定过程应在参数列表的变量部分中接受的所提供值的数量。 这可能包括无值的情况，还可能包括单个一维数组的大小写。  
  
3. 对于提供的每个可接受的值， `Sub` 编写 `Function` 定义相应参数列表的或声明语句。 不要 `Optional` `ParamArray` 在此重载版本中使用或关键字。  
  
4. 在每个声明中，在或关键字之前加上 `Sub` `Function` [Overloads](../../../language-reference/modifiers/overloads.md) 关键字。  
  
5. 遵循每个声明，编写调用代码提供与声明的参数列表对应的值时应执行的过程代码。  
  
6. 根据需要，用或语句终止每个过程 `End Sub` `End Function` 。  
  
## <a name="example"></a>示例  

 下面的示例演示了一个使用 [ParamArray](../../../language-reference/modifiers/paramarray.md) 参数定义的过程，以及一个等效的重载过程集。  
  
 [!code-vb[VbVbcnProcedures#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#69)]  
  
 [!code-vb[VbVbcnProcedures#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#70)]  
  
 不能使用参数列表重载此类过程，该参数列表采用一维数组作为参数数组。 但是，可以使用其他隐式重载的签名。 以下声明阐释了这一点。  
  
 [!code-vb[VbVbcnProcedures#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#71)]  
  
 重载版本中的代码无需测试调用代码是否为参数提供了一个或多个值，如果有，则不需要测试 `ParamArray` 。 Visual Basic 将控制传递到与调用参数列表匹配的版本。  
  
## <a name="compile-the-code"></a>编译代码  

 由于具有参数的过程 `ParamArray` 等效于一组重载版本，因此不能使用与任何这些隐式重载对应的参数列表重载此类过程。 有关详细信息，请参阅 [重载过程中的注意事项](./considerations-in-overloading-procedures.md)。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 无论何时处理可能会无限大的阵列，都有 overrunning 应用程序的一些内部容量的风险。 如果接受参数数组，则应测试调用代码传递给它的数组长度，如果该数组对您的应用程序而言太大，则采取适当的措施。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [可选参数](./optional-parameters.md)
- [参数数组](./parameter-arrays.md)
- [过程重载](./procedure-overloading.md)
- [过程疑难解答](./troubleshooting-procedures.md)
- [如何：定义一个过程的多个版本](./how-to-define-multiple-versions-of-a-procedure.md)
- [如何：调用重载过程](./how-to-call-an-overloaded-procedure.md)
- [如何：重载带有可选参数的过程](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [重载决策](./overload-resolution.md)
