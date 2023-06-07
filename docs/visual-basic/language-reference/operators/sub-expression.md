---
title: Sub 表达式
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [Visual Basic], sub expression
- Sub Expression [Visual Basic]
- subroutines [Visual Basic], sub expressions
ms.assetid: 36b6bfd1-6539-4d8f-a5eb-6541a745ffde
ms.openlocfilehash: e564fa3f717fc1a9f4e9961d9b3e961912a4d56b
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873322"
---
# <a name="sub-expression-visual-basic"></a>Sub 表达式 (Visual Basic)

声明定义子程序 lambda 表达式的参数和代码。  
  
## <a name="syntax"></a>语法  
  
```vb  
Sub ( [ parameterlist ] ) statement  
- or -  
Sub ( [ parameterlist ] )  
  [ statements ]  
End Sub  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`parameterlist`|可选。 表示过程参数的局部变量名称的列表。 即使此列表为空，也必须存在括号。 有关更多信息，请参见 [Parameter List](../statements/parameter-list.md)。|  
|`statement`|必需。 单个语句。|  
|`statements`|必需。 语句的列表。|  
  
## <a name="remarks"></a>备注  

 *Lambda 表达式*是不具有名称并执行一个或多个语句的子例程。 可以在任何可使用委托类型的位置使用 lambda 表达式，但参数除外 `RemoveHandler` 。 有关委托的详细信息以及对委托使用 lambda 表达式的详细信息，请参阅 [委托语句](../statements/delegate-statement.md) 和 [宽松委托转换](../../programming-guide/language-features/delegates/relaxed-delegate-conversion.md)。  
  
## <a name="lambda-expression-syntax"></a>Lambda 表达式语法  

 Lambda 表达式的语法与标准子例程的语法相似。 不同之处如下：  
  
- Lambda 表达式没有名称。  
  
- Lambda 表达式不能具有修饰符，如 `Overloads` 或 `Overrides` 。  
  
- 单行 lambda 表达式的主体必须是语句，而不能是表达式。 正文可以包含对 sub 过程的调用，但不能由对函数过程的调用组成。  
  
- 在 lambda 表达式中，所有参数都必须具有指定的数据类型，或者必须推断所有参数。  
  
- `ParamArray`Lambda 表达式中不允许使用可选的和参数。  
  
- Lambda 表达式中不允许使用泛型参数。  
  
## <a name="example"></a>示例  

 下面是将值写入控制台的 lambda 表达式的示例。 该示例显示了子程序的单行和多行 lambda 表达式语法。 有关更多示例，请参阅 [Lambda 表达式](../../programming-guide/language-features/procedures/lambda-expressions.md)。  
  
 [!code-vb[VbVbalrLambdas#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#15)]  
  
## <a name="see-also"></a>另请参阅

- [Sub 语句](../statements/sub-statement.md)
- [Lambda 表达式](../../programming-guide/language-features/procedures/lambda-expressions.md)
- [运算符和表达式](../../programming-guide/language-features/operators-and-expressions/index.md)
- [语句](../../programming-guide/language-features/statements.md)
- [宽松委托转换](../../programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
