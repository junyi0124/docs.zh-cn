---
title: IsFalse 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.isfalse
helpviewer_keywords:
- AndAlso operator [Visual Basic]
- IsFalse operator [Visual Basic]
ms.assetid: 37fc9dbf-e5cc-4570-b93f-7213447974df
ms.openlocfilehash: bbcdb9bcf645a4e9cb54c657ccd46e04437d207e
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873377"
---
# <a name="isfalse-operator-visual-basic"></a>IsFalse 运算符 (Visual Basic)

确定表达式是否为 `False` 。  
  
 你不能 `IsFalse` 在代码中显式调用，但是 Visual Basic 编译器可以使用它来生成代码 from `AndAlso` 子句。 如果定义类或结构，然后在子句中使用该类型的变量 `AndAlso` ，则必须 `IsFalse` 在该类或结构上进行定义。  
  
 编译器将 `IsFalse` 和运算符视为 `IsTrue` *匹配的对*。 这意味着，如果定义其中一个类型，则还必须定义另一个。  
  
> [!NOTE]
> `IsFalse`运算符可以*重载*，这意味着当类或结构的操作数具有该类或结构的类型时，该类或结构可以重新定义它的行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例定义了包含和运算符定义的结构的轮廓 `IsFalse` `IsTrue` 。  
  
 [!code-vb[VbVbalrOperators#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#28)]  
  
## <a name="see-also"></a>另请参阅

- [IsTrue 运算符](istrue-operator.md)
- [如何：定义运算符](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [AndAlso 运算符](andalso-operator.md)
