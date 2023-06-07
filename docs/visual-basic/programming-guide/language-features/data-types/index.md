---
title: 数据类型
ms.date: 07/20/2015
helpviewer_keywords:
- data types [Visual Basic], declaring
- typing
- data types [Visual Basic]
- Visual Basic code, data types
- data types [Visual Basic], improving speed with
ms.assetid: 5e1b9aaf-c7ca-4b29-9b22-0e82ed8e85e2
ms.openlocfilehash: 8cb9642c1d7e6876030efe17f5c09e4888700a24
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095585"
---
# <a name="data-types-in-visual-basic"></a>Visual Basic 中的数据类型

编程元素的*数据类型*是指可以保留的数据种类以及相应类型数据的存储方式。 数据类型适用于所有可以存储到计算机内存中或参与表达式求值的值。 每个变量、文本、常量、枚举、属性、过程参数、过程自变量和过程返回值都有对应的数据类型。  
  
## <a name="declared-data-types"></a>已声明的数据类型  

 可以使用声明语句定义编程元素，并使用 `As` 子句指定其数据类型。 下表列出了用于声明各种元素的语句。  
  
|编程元素|数据类型声明|  
|-------------------------|---------------------------|  
|变量|使用 [Dim 语句](../../../language-reference/statements/dim-statement.md)<br /><br /> `Dim`   `amount As Double`<br /><br /> `Static`   `yourName As String`<br /><br /> `Public`   `billsPaid As Decimal = 0`|  
|文本|含文本类型字符；请参阅[类型字符](type-characters.md)中的“文本类型字符”<br /><br /> `Dim searchChar As Char = "."`  `C`|  
|常数|使用 [Const 语句](../../../language-reference/statements/const-statement.md)<br /><br /> `Const`   `modulus As Single = 4.17825F`|  
|枚举|使用 [Enum 语句](../../../language-reference/statements/enum-statement.md)<br /><br /> `Public`   `Enum`   `colors`|  
|属性|使用 [Property 语句](../../../language-reference/statements/property-statement.md)<br /><br /> `Property`   `region() As String`|  
|过程参数|使用 [Sub 语句](../../../language-reference/statements/sub-statement.md)、[Function 语句](../../../language-reference/statements/function-statement.md)或 [Operator 语句](../../../language-reference/statements/operator-statement.md)<br /><br /> `Sub addSale(ByVal`   `amount`   `As Double)`|  
|过程自变量|在调用的代码中；每个自变量都是一个已声明的编程元素或包含已声明元素的表达式<br /><br /> `subString = Left(`  `inputString`  `,`   `5`  `)`|  
|过程返回值|使用 [Function 语句](../../../language-reference/statements/function-statement.md)或 [Operator 语句](../../../language-reference/statements/operator-statement.md)<br /><br /> `Function convert(ByVal b As Byte)`   `As String`|  
  
 有关 Visual Basic 数据类型的列表，请参阅[数据类型](../../../language-reference/data-types/index.md)。  
  
## <a name="see-also"></a>请参阅

- [类型字符](type-characters.md)
- [基本数据类型](elementary-data-types.md)
- [复合数据类型](composite-data-types.md)
- [Generic Types in Visual Basic](generic-types.md)
- [Value Types and Reference Types](value-types-and-reference-types.md)
- [Visual Basic 中的类型转换](type-conversions.md)
- [结构](structures.md)
- [元组](tuples.md)
- [数据类型疑难解答](troubleshooting-data-types.md)
- [数据类型](../../../language-reference/data-types/index.md)
- [有效使用数据类型](efficient-use-of-data-types.md)
