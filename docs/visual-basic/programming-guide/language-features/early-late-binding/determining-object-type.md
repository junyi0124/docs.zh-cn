---
title: 确定对象类型
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic], discovering which an object belongs to
- types [Visual Basic], determining Visual Basic object types
- object variables [Visual Basic], testing values
- TypeOf...Is expression, object type at run time
- TypeName function
- objects [Visual Basic], type determining
ms.assetid: d95e7ad1-cd63-41d6-9a28-d7a1380d49c1
ms.openlocfilehash: ae338bc9bad9646abc045a652d4ef33a8863354b
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086057"
---
# <a name="determining-object-type-visual-basic"></a>确定对象类型 (Visual Basic)

泛型对象变量 (即声明为) 的变量 `Object` 可以包含任何类中的对象。 使用类型的变量时 `Object` ，可能需要根据对象的类执行不同的操作; 例如，某些对象可能不支持特定的属性或方法。 Visual Basic 提供了两种方法来确定存储在对象变量中的对象类型： `TypeName` 函数和 `TypeOf...Is` 运算符。  
  
## <a name="typename-and-typeofis"></a>TypeName 和 TypeOf .。。未  

 `TypeName`如果需要存储或显示对象的类名，则函数返回一个字符串，是最佳选择，如以下代码片段所示：  
  
 [!code-vb[VbVbalrOOP#92](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#92)]  
  
 `TypeOf...Is`运算符是测试对象类型的最佳选择，因为它比使用等效的字符串比较快得多 `TypeName` 。 以下代码片段 `TypeOf...Is` 在语句中使用 `If...Then...Else` ：  
  
 [!code-vb[VbVbalrOOP#93](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#93)]  
  
 此处有一则注意事项。 `TypeOf...Is` `True` 如果对象是特定类型的，或派生自特定类型，则运算符返回。 几乎对 Visual Basic 所做的一切都涉及对象，这些对象包括通常不会视为对象的某些元素，如字符串和整数。 这些对象派生自，并从继承方法 <xref:System.Object> 。 当向传递 `Integer` 并使用计算时 `Object` ， `TypeOf...Is` 运算符将返回 `True` 。 下面的示例报告参数 `InParam` 既是 `Object` 又是 `Integer` ：  
  
 [!code-vb[VbVbalrOOP#94](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#94)]  
  
 下面的示例使用 `TypeOf...Is` 和 `TypeName` 来确定参数中传递给它的对象的类型 `Ctrl` 。 此 `TestObject` 过程调用 `ShowType` 了三种不同类型的控件。  
  
#### <a name="to-run-the-example"></a>运行示例  
  
1. 创建新的 Windows 应用程序项目，并向 <xref:System.Windows.Forms.Button> 窗体添加控件、 <xref:System.Windows.Forms.CheckBox> 控件和 <xref:System.Windows.Forms.RadioButton> 控件。  
  
2. 在窗体上的按钮中，调用 `TestObject` 过程。  
  
3. 将以下代码添加到你的窗体：  
  
     [!code-vb[VbVbalrOOP#95](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#95)]  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Information.TypeName%2A>
- [使用字符串名调用属性或方法](calling-a-property-or-method-using-a-string-name.md)
- [Object Data Type](../../../language-reference/data-types/object-data-type.md)
- [If...Then...Else 语句](../../../language-reference/statements/if-then-else-statement.md)
- [String 数据类型](../../../language-reference/data-types/string-data-type.md)
- [Integer 数据类型](../../../language-reference/data-types/integer-data-type.md)
