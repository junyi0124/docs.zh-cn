---
title: 如何：重载带有可选参数的过程
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], parameters
- procedure overloading [Visual Basic], optional parameters
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
ms.assetid: 825f9d56-4cde-43fd-993a-b9171717e2eb
ms.openlocfilehash: 78ca6b2b95dfd5a7f208e5251f08dfccc5514946
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071516"
---
# <a name="how-to-overload-a-procedure-that-takes-optional-parameters-visual-basic"></a>如何：重载带有可选参数的过程 (Visual Basic)

如果过程有一个或多个 [可选](../../../language-reference/modifiers/optional.md) 参数，则不能定义与它的任何隐式重载匹配的重载版本。 有关详细信息，请参阅 [重载过程的注意事项](./considerations-in-overloading-procedures.md)中的 "可选参数的隐式重载"。  
  
## <a name="one-optional-parameter"></a>一个可选参数  
  
#### <a name="to-overload-a-procedure-that-takes-one-optional-parameter"></a>重载采用一个可选参数的过程  
  
1. 编写一个 `Sub` 或 `Function` 声明语句，其中包含参数列表中的可选参数。 不要 `Optional` 在此重载版本中使用关键字。  
  
2. 在或关键字之前加上 `Sub` `Function` [Overloads](../../../language-reference/modifiers/overloads.md) 关键字。  
  
3. 编写调用代码提供可选参数时应执行的过程代码。  
  
4. `End Sub`适当地终止带有或语句的过程 `End Function` 。  
  
5. 编写与第一个声明相同的第二个声明语句，只不过它不在参数列表中包含可选参数。  
  
6. 编写调用代码未提供可选参数时应执行的过程代码。 `End Sub`适当地终止带有或语句的过程 `End Function` 。  
  
     下面的示例演示了一个使用可选参数定义的过程，这是一组等效的两个重载过程，最后是无效和有效重载版本的示例。  
  
     [!code-vb[VbVbcnProcedures#59](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#59)]  
  
     [!code-vb[VbVbcnProcedures#60](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#60)]  
  
     [!code-vb[VbVbcnProcedures#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#61)]  
  
## <a name="multiple-optional-parameters"></a>多个可选参数  

 对于具有多个可选参数的过程，通常需要两个以上的重载版本。 例如，如果有两个可选参数，并且调用代码可以单独提供或省略每个参数，则需要四个重载版本，每个可用于提供的参数的每个可能组合。  
  
 当可选参数的数目增加时，重载的复杂性会增加。 除非提供的参数的某些组合是不可接受的，否则对于 N 个可选参数，你需要 2 ^ N 个重载版本。 根据过程的性质，你可能会发现逻辑清晰地会调整定义所有重载版本的额外工作量。  
  
#### <a name="to-overload-a-procedure-that-takes-more-than-one-optional-parameter"></a>重载一个具有多个可选参数的过程  
  
1. 确定所提供的可选参数的哪些组合对于过程逻辑是可接受的。 如果一个可选参数依赖于另一个参数，则可能会产生不可接受的组合。 例如，如果一个参数接受某人的名字，另一个参数接受该用户的年龄，则提供年龄但省略名称的参数的组合是不可接受的。  
  
2. 对于提供的可选参数的每个可接受组合，编写一个 `Sub` `Function` 定义相应参数列表的或声明语句。 不要使用 `Optional` 关键字。  
  
3. 在每个声明中，在或关键字之前加上 `Sub` `Function` [Overloads](../../../language-reference/modifiers/overloads.md) 关键字。  
  
4. 在每个声明之后，编写调用代码提供对应于该声明的参数列表的参数列表时应执行的过程代码。  
  
5. 根据需要，用或语句终止每个过程 `End Sub` `End Function` 。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [可选参数](./optional-parameters.md)
- [参数数组](./parameter-arrays.md)
- [过程重载](./procedure-overloading.md)
- [过程疑难解答](./troubleshooting-procedures.md)
- [如何：定义一个过程的多个版本](./how-to-define-multiple-versions-of-a-procedure.md)
- [如何：调用重载过程](./how-to-call-an-overloaded-procedure.md)
- [如何：重载参数数量不确定的过程](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [重载决策](./overload-resolution.md)
