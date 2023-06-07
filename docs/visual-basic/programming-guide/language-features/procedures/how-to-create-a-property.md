---
title: 如何：创建属性
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- Visual Basic code, properties
- properties [Visual Basic]
ms.assetid: 4d229712-6be8-4c5c-bac5-06995ce9185a
ms.openlocfilehash: bd138177d5f4b7ee1eb63833360d227baa54f66d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072738"
---
# <a name="how-to-create-a-property-visual-basic"></a>如何：创建属性 (Visual Basic)

将属性定义包含在 `Property` 语句和 `End Property` 语句之间。 在此定义中，你将定义 `Get` 过程、 `Set` 过程或同时定义这两者。 所有属性的代码都位于这些过程中。  
  
 `Get`过程检索属性的值，并且 `Set` 过程存储值。 如果希望属性具有读/写访问权限，则必须定义这两个过程。 对于只读属性，只定义 `Get` ，对于只写属性，只定义 `Set` 。  
  
### <a name="to-create-a-property"></a>创建属性  
  
1. 在任何属性或过程外部，使用 [Property 语句](../../../language-reference/statements/property-statement.md)，后跟 `End Property` 语句。  
  
2. 如果属性采用参数，请在关键字后面跟 `Property` 上过程的名称，然后将参数列表括在括号中。  
  
3. 在括号后跟一个 `As` 子句以指定属性值的数据类型。 即使对于只写属性，也必须指定数据类型。  
  
4. `Get` `Set` 根据需要添加和过程。 请参阅以下说明。  
  
### <a name="to-create-a-get-procedure-that-retrieves-a-property-value"></a>创建检索属性值的 Get 过程  
  
1. 在 `Property` 和 `End Property` 语句之间，编写 [Get 语句](../../../language-reference/statements/get-statement.md)，后跟一个 `End Get` 语句。 无需为过程定义任何参数 `Get` 。  
  
2. 放置代码语句以检索和语句之间的属性值 `Get` `End Get` 。 除了生成并返回属性的值外，此代码还可以包含其他计算和数据操作。  
  
3. 使用 `Return` 语句将属性的值返回到调用代码。  
  
 必须 `Get` 为读写属性以及只读属性编写过程。 不得 `Get` 为只写属性定义过程。  
  
### <a name="to-create-a-set-procedure-that-writes-a-propertys-value"></a>创建写入属性值的 Set 过程  
  
1. 在 `Property` 和 `End Property` 语句之间，编写一个 [Set 语句](../../../language-reference/statements/set-statement.md)，后跟一个 `End Set` 语句。  
  
2. 在 `Set` 语句中，在 `Set` 关键字后面加上括号中的参数列表。 此参数列表必须至少包含调用代码传递的值的值参数。 此值参数的默认名称为 `Value` ，但你可以根据需要使用不同的名称。 Value 参数的数据类型必须与属性本身的数据类型相同。  
  
3. 放置代码语句，以在和语句之间存储值 `Set` `End Set` 。 除了验证和存储属性值外，此代码还可以包含其他计算和数据操作。  
  
4. 使用 value 参数接受调用代码提供的值。 您可以直接在赋值语句中存储此值，也可以在表达式中使用该值来计算要存储的内部值。  
  
 您必须为 `Set` 读写属性和只写属性编写一个过程。 不得 `Set` 为只读属性定义过程。  
  
## <a name="example"></a>示例  

 下面的示例创建一个读/写属性，该属性将全名存储为两个构成名称，即名字和姓氏。 在调用代码读取时 `fullName` ，此 `Get` 过程将组合这两个构成名称并返回全名。 当调用代码分配新的全名时，该 `Set` 过程会尝试将其分成两个构成名称。 如果找不到空间，则会将其存储为名字。  
  
 [!code-vb[VbVbcnProcedures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#8)]  
  
 下面的示例演示对的属性过程的典型调用 `fullName` 。 第一次调用设置属性值，第二次调用将检索它。  
  
 [!code-vb[VbVbcnProcedures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [Property 过程](./property-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Visual Basic 中属性和变量的差异](./differences-between-properties-and-variables.md)
- [如何：声明具有混合访问级别的属性](./how-to-declare-a-property-with-mixed-access-levels.md)
- [如何：调用 Property 过程](./how-to-call-a-property-procedure.md)
- [如何：在 Visual Basic 中声明和调用默认属性](./how-to-declare-and-call-a-default-property.md)
- [如何：在属性中放置值](./how-to-put-a-value-in-a-property.md)
- [如何：从属性获取值](./how-to-get-a-value-from-a-property.md)
