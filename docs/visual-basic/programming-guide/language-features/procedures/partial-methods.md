---
title: 分部方法
ms.date: 07/20/2015
f1_keywords:
- vb.PartialMethod
- PartialMethod
helpviewer_keywords:
- custom logic into code [Visual Basic]
- partial methods [Visual Basic]
- partial [Visual Basic], methods [Visual Basic]
- methods [Visual Basic], partial methods
- inserting custom logic into code
ms.assetid: 74b3368b-b348-44a0-a326-7d7dc646f4e9
ms.openlocfilehash: 0e7c1315df50e83c919270f76405e80862bdd03b
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071256"
---
# <a name="partial-methods-visual-basic"></a>分部方法 (Visual Basic)

利用分部方法，开发人员可以在代码中插入自定义逻辑。 通常，代码是设计器生成的类的一部分。 分部方法在由代码生成器创建的分部类中定义，并且通常用于提供已更改内容的通知。 开发人员可使用它们指定自定义行为来响应更改。  
  
 代码生成器的设计器仅定义方法签名和对方法的一次或多次调用。 如果开发人员想要自定义生成的代码的行为，则可以为方法提供实现。 当未提供实现时，编译器将删除对方法的调用，从而无额外的性能开销。  
  
## <a name="declaration"></a>声明  

 生成的代码通过将关键字置于签名行的开头来标记分部方法的定义 `Partial` 。  
  
```vb  
Partial Private Sub QuantityChanged()  
End Sub  
```  
  
 定义必须满足以下条件：  
  
- 方法必须是 `Sub` 而不是 `Function` 。  
  
- 方法的主体必须留空。  
  
- 访问修饰符必须是 `Private` 。  
  
## <a name="implementation"></a>实现  

 实现主要包含在分部方法的主体中进行填充。 实现通常在独立于定义的分部类中，并由想要扩展生成的代码的开发人员编写。  
  
```vb  
Private Sub QuantityChanged()  
'    Code for executing the desired action.  
End Sub  
```  
  
 前面的示例将在声明中完全复制签名，但可能会发生变化。 特别是，可以添加其他修饰符，如 `Overloads` 或 `Overrides` 。 只 `Overrides` 允许使用一个修饰符。 有关方法修饰符的详细信息，请参阅 [Sub 语句](../../../language-reference/statements/sub-statement.md)。  
  
## <a name="use"></a>使用  

 调用分部方法的方法与调用任何其他过程一样 `Sub` 。 如果已实现方法，则将计算参数并执行方法的主体。 但请记住，实现分部方法是可选的。 如果未实现该方法，则对它的调用不起作用，并且不计算作为参数传递给方法的表达式。  
  
## <a name="example"></a>示例  

 在名为 "node.js" 的文件中，定义一个 `Product` 具有属性的类 `Quantity` 。  
  
 [!code-vb[VbVbalrPartialMeths#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrPartialMeths/VB/Class1.vb#4)]  
  
 在名为 ".vb" 的文件中，提供的实现 `QuantityChanged` 。  
  
 [!code-vb[VbVbalrPartialMeths#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrPartialMeths/VB/Class1.vb#5)]  
  
 最后，在项目的 Main 方法中，声明一个 `Product` 实例，并为其属性提供初始值 `Quantity` 。  
  
 [!code-vb[VbVbalrPartialMeths#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrPartialMeths/VB/Class1.vb#6)]  
  
 此时将显示一个消息框，显示此消息：  
  
 `Quantity was changed to 100`  
  
## <a name="see-also"></a>请参阅

- [Sub 语句](../../../language-reference/statements/sub-statement.md)
- [Sub 过程](./sub-procedures.md)
- [可选参数](./optional-parameters.md)
- [Partial](../../../language-reference/modifiers/partial.md)
- [LINQ to SQL 中的代码生成](../../../../framework/data/adonet/sql/linq/code-generation-in-linq-to-sql.md)
- [通过使用分部方法添加业务逻辑](../../../../framework/data/adonet/sql/linq/adding-business-logic-by-using-partial-methods.md)
