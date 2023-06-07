---
title: 泛型过程
ms.date: 07/20/2015
helpviewer_keywords:
- generic methods [Visual Basic], type inference
- generics [Visual Basic], type inference
- procedures [Visual Basic], generic
- generic procedures
- type inference, generics
- generic methods [Visual Basic]
- type inference
- generics [Visual Basic], procedures
- generic procedures [Visual Basic], type inference
ms.assetid: 95577b28-137f-4d5c-a149-919c828600e5
ms.openlocfilehash: 558601f038fccdcb9b94acb7c796e2b49fb6e6f4
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91059192"
---
# <a name="generic-procedures-in-visual-basic"></a>Generic Procedures in Visual Basic

*泛型过程*（也称为*泛型方法*）是定义了至少一个类型参数的过程。 这样，每次调用该过程时，调用代码都可以根据其需求来定制数据类型。  
  
 过程不是泛型的，只是在泛型类或泛型结构中进行定义。 若要为泛型，该过程除了需要使用任何常规参数外，还必须使用至少一个类型参数。 泛型类或结构可以包含非泛型过程，非泛型类、结构或模块可以包含泛型过程。  
  
 泛型过程可以在其常规参数列表中使用其类型参数（如果有），并在其过程代码中使用它的返回类型。  
  
## <a name="type-inference"></a>类型推断  

 无需提供任何类型参数即可调用泛型过程。 如果以这种方式对其进行调用，则编译器将尝试确定要传递给该过程的类型参数的相应数据类型。 这称为 " *类型推理*"。 下面的代码演示了一个调用，其中编译器会推断它应将类型传递 `String` 到类型参数 `t` 。  
  
 [!code-vb[VbVbalrDataTypes#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#15)]  
  
 如果编译器无法从调用的上下文推断类型参数，则会报告错误。 导致这种错误的一个可能原因是数组秩不匹配。 例如，假设将普通参数定义为类型参数的数组。 如果调用泛型过程来提供不同级别的数组 (维度) 数量，则不匹配会导致类型推理失败。 下面的代码演示一个调用，其中，二维数组传递到需要一维数组的过程中。  
  
```vb  
Public Sub demoSub(Of t)(ByVal arg() As t)
End Sub

Public Sub callDemoSub()
    Dim twoDimensions(,) As Integer
    demoSub(twoDimensions)
End Sub
```
  
 您只能通过省略所有类型参数来调用类型推理。 如果提供了一个类型参数，则必须提供所有参数。  
  
 仅泛型过程支持类型推理。 不能对泛型类、结构、接口或委托调用类型推理。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 下面的示例定义了一个泛型 `Function` 过程，用于查找数组中的特定元素。 它定义了一个类型参数，并使用它在参数列表中构造两个参数。  
  
### <a name="code"></a>代码  

 [!code-vb[VbVbalrDataTypes#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#14)]  
  
### <a name="comments"></a>注释  

 前面的示例需要能够与 `searchValue` 的每个元素进行比较 `searchArray` 。 为了保证这种能力，它约束类型参数 `T` 来实现 <xref:System.IComparable%601> 接口。 此代码使用 <xref:System.IComparable%601.CompareTo%2A> 方法而不是 `=` 运算符，因为无法保证为提供的类型自变量 `T` 支持 `=` 运算符。  
  
 可以 `findElement` 通过以下代码测试该过程。  
  
 [!code-vb[VbVbalrDataTypes#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#13)]  
  
 上述调用 `MsgBox` 分别显示 "0"、"1" 和 "-1"。  
  
## <a name="see-also"></a>请参阅

- [Generic Types in Visual Basic](generic-types.md)
- [如何：定义可对不同数据类型提供相同功能的类](how-to-define-a-class-that-can-provide-identical-functionality.md)
- [如何：使用泛型类](how-to-use-a-generic-class.md)
- [过程](../procedures/index.md)
- [过程形参和实参](../procedures/procedure-parameters-and-arguments.md)
- [Type List](../../../language-reference/statements/type-list.md)
- [参数列表](../../../language-reference/statements/parameter-list.md)
