---
title: 如何：声明枚举
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], enumerations
- enumerations [Visual Basic], declaring
- declaring enumerations [Visual Basic]
ms.assetid: db4ca1c3-f429-4c81-ae81-29e0157b29fd
ms.openlocfilehash: 752b425ba32efe41a1ab1aa75de20039d36f5e50
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058893"
---
# <a name="how-to-declare-enumerations-visual-basic"></a>如何：声明枚举 (Visual Basic)

使用 `Enum` 类或模块的声明部分中的语句创建枚举。 不能在方法中声明枚举。 若要指定适当的访问级别，请使用 `Private` 、、 `Protected` `Friend` 或 `Public` 。  
  
 `Enum`类型具有名称、基础类型和字段集，每个字段表示一个常量。 该名称必须是有效的 Visual Basic .NET 限定符。 基础类型必须是整数类型之一： `Byte` 、 `Short` `Long` 或 `Integer` 。 `Integer` 为默认值。 枚举总是强类型化的，并且不能与整数类型互换。  
  
 枚举的值不能为浮点值。 如果为枚举分配一个浮点值，则会 `Option Strict On` 产生编译器错误。 如果 `Option Strict` 为 `Off` ，则将值自动转换为 `Enum` 类型。  
  
 有关名称的信息，以及如何使用 `Imports` 语句使名称限定不必要，请参阅 [枚举和名称限定](enumerations-and-name-qualification.md)。  
  
### <a name="to-declare-an-enumeration"></a>声明枚举  
  
1. 编写包含代码访问级别、 `Enum` 关键字和有效名称的声明，如下面的示例所示，其中每个都声明了不同的 `Enum` 。  
  
     [!code-vb[VbEnumsTask#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#3)]  
  
2. 定义枚举中的常量。 默认情况下，枚举中的第一个常量被初始化为 `0` ，后面的常量被初始化为比上一个常数大1的值。 例如，下面的枚举包含一个名为、值为、值为、值为、 `Days` `Sunday` `0` `Monday` `1` `Tuesday` 、值为的 `2` 常量等的常量。  
  
     [!code-vb[VbEnumsTask#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#4)]  
  
3. 可以使用赋值语句将值显式分配给枚举中的常量。 可以分配任何整数值，包括负数。 例如，您可能希望值小于零的常量表示错误条件。 在下面的枚举中，将 `Invalid` 显式赋给常数 `–1` ，并为该常数 `Sunday` 分配值 `0` 。 由于它是枚举中的第一个常数，因此 `Saturday` 也会初始化为值 `0` 。 的值 `Monday` `1` (一个大于) 的值 `Sunday` ; 的值 `Tuesday` 为 `2` ，依此类推。  
  
     [!code-vb[VbEnumsTask#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#5)]  
  
### <a name="to-declare-an-enumeration-as-an-explicit-type"></a>将枚举声明为显式类型  
  
- 使用子句指定枚举的类型 `As` ，如下面的示例中所示。  
  
     [!code-vb[VbEnumsTask#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#6)]  
  
## <a name="see-also"></a>请参阅

- [枚举和名称限定](enumerations-and-name-qualification.md)
- [如何：引用枚举成员](how-to-refer-to-an-enumeration-member.md)
- [如何：在 Visual Basic 中循环访问枚举](how-to-iterate-through-an-enumeration.md)
- [如何：确定与枚举值关联的字符串](how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [何时使用枚举](when-to-use-an-enumeration.md)
- [常量概述](constants-overview.md)
- [常数和文本数据类型](constant-and-literal-data-types.md)
- [常量和枚举](../../../language-reference/constants-and-enumerations.md)
