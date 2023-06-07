---
title: Implements 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Implements
- Implements
helpviewer_keywords:
- Implements statement [Visual Basic], syntax
- Implements statement [Visual Basic]
- interface implementation [Visual Basic], Implements statement
ms.assetid: 1fafb83f-f55a-4215-8ea9-681e8622613d
ms.openlocfilehash: b982057d2094f807b68d5190dfad388fb9a2c65a
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873234"
---
# <a name="implements-statement"></a>Implements 语句

指定一个或多个接口或接口成员，它们必须在它所出现的类或结构定义中实现。  
  
## <a name="syntax"></a>语法  
  
```vb  
Implements interfacename [, ...]  
' -or-  
Implements interfacename.interfacemember [, ...]  
```  
  
## <a name="parts"></a>组成部分  

 `interfacename`  
 必需。 一个接口，其属性、过程和事件由类或结构中的相应成员实现。  
  
 `interfacemember`  
 必需。 正在实现的接口的成员。  
  
## <a name="remarks"></a>备注  

 接口是表示接口封装 (属性、过程和事件) 成员的原型的集合。 接口只包含成员的声明;类和结构实现这些成员。 有关详细信息，请参阅[接口](../../programming-guide/language-features/interfaces/index.md)。  
  
 `Implements`语句必须紧跟 `Class` 或 `Structure` 语句。  
  
 实现接口时，必须实现在接口中声明的所有成员。 省略任何成员均视为语法错误。 若要实现单个成员，请在[Implements](implements-clause.md) `Implements` 声明类或结构中的成员时，指定与语句) 不同的 Implements 关键字 (。 有关详细信息，请参阅[接口](../../programming-guide/language-features/interfaces/index.md)。  
  
 类可以使用属性和过程的 [私有](../modifiers/private.md) 实现，但只能通过将实现类的实例强制转换为声明为接口类型的变量来访问这些成员。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 `Implements` 语句来实现接口的成员。 它 `ICustomerInfo` 使用事件、属性和过程来定义名为的接口。 类 `customerInfo` 实现在接口中定义的所有成员。  
  
 [!code-vb[VbVbalrStatements#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#33)]  
  
 请注意，类对 `customerInfo` `Implements` 单独的源代码行使用语句，以指示类实现了接口的所有成员 `ICustomerInfo` 。 然后，类中的每个成员使用 `Implements` 关键字作为其成员声明的一部分，以指示它实现了该接口成员。  
  
## <a name="example"></a>示例  

 下面的两个过程演示如何使用在前面的示例中实现的接口。 若要测试实现，请将这些过程添加到项目中，然后调用 `testImplements` 过程。  
  
 [!code-vb[VbVbalrStatements#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#34)]  
  
## <a name="see-also"></a>另请参阅

- [实现](implements-clause.md)
- [Interface 语句](interface-statement.md)
- [接口](../../programming-guide/language-features/interfaces/index.md)
