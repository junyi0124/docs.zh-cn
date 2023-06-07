---
title: 用户定义常数
ms.date: 07/20/2015
helpviewer_keywords:
- constants [Visual Basic], circular references
- Const statement [Visual Basic], user-defined constants
- user-defined constants [Visual Basic]
- scope [Visual Basic], constants
- constants [Visual Basic], user-defined
- circular references between constants [Visual Basic]
ms.assetid: a1206d5c-c45e-4ac2-970a-4a0be6a05fdd
ms.openlocfilehash: 351bdb6963e278341c13e53ef19aea0876010aa9
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095637"
---
# <a name="user-defined-constants-visual-basic"></a>用户定义的常量 (Visual Basic)

常量是有意义的名称，用来代替不会更改的数字或字符串。 顾名思义，常量存储在应用程序的执行过程中保持不变的值。 您可以使用您使用的控件或组件定义的常量，也可以创建自己的常量。 您自己创建的常量被描述为 *用户定义*的常量。  
  
 使用语句声明一个常数 `Const` ，使用与创建变量名称相同的准则。 如果 `Option Strict` 为 `On` ，则必须显式声明常量类型。  
  
## <a name="const-statement-usage"></a>Const 语句用法  

 `Const`语句可以表示数学或日期/时间数量：  
  
 [!code-vb[VbEnumsTask#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#10)]  
  
 它还可以定义 `String` 常量：  
  
 [!code-vb[VbEnumsTask#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#13)]  
  
 相等符号 ( ) 右侧的表达式 `=` 通常是数字或文本字符串，但它也可以是导致数字或字符串的表达式 (但该表达式不能包含对函数) 的调用。 甚至可以根据以前定义的常量来定义常量：  
  
 [!code-vb[VbEnumsTask#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#15)]  
  
## <a name="scope-of-user-defined-constants"></a>用户定义的常量的作用域  

 `Const`语句的范围与同一位置中声明的变量的范围相同。 可以通过以下任一方式指定作用域：  
  
- 若要创建仅存在于一个过程中的常量，请在该过程中对其进行声明。  
  
- 若要创建一个可用于类中的所有过程的常量，而不是该模块外的任何代码，请在类的声明部分中将其声明为。  
  
- 若要创建可用于程序集的所有成员而不是程序集外部的常量，请 `Friend` 在类的声明部分使用关键字进行声明。  
  
- 若要创建可在整个应用程序中使用的常量，请 `Public` 在类的声明部分使用关键字进行声明。  
  
 有关详细信息，请参阅 [如何：声明常量](how-to-declare-a-constant.md)。  
  
### <a name="avoiding-circular-references"></a>避免循环引用  

 由于可以使用其他常量来定义常量，因此可能会在两个或多个常量之间意外创建 *循环*或循环引用。 如果有两个或多个公共常量，则会发生循环，其中每个常量都是以其他方式定义的，如以下示例中所示：  
  
 [!code-vb[VbEnumsTask#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#16)]  
[!code-vb[VbEnumsTask#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#17)]  
  
 如果发生循环，Visual Basic 会生成编译器错误。  
  
## <a name="see-also"></a>请参阅

- [Const 语句](../../../language-reference/statements/const-statement.md)
- [常数和文本数据类型](constant-and-literal-data-types.md)
- [常量和枚举](index.md)
- [常量和枚举](../../../language-reference/constants-and-enumerations.md)
- [枚举概述](enumerations-overview.md)
- [常量概述](constants-overview.md)
- [如何：声明枚举](how-to-declare-enumerations.md)
- [枚举和名称限定](enumerations-and-name-qualification.md)
- [Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)
