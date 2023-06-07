---
title: << 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.<<
helpviewer_keywords:
- bit shift operators [Visual Basic]
- << operator [Visual Basic]
- operator <<, Visual Basic left shift operator
ms.assetid: fdb93d25-81ba-417f-b808-41207bfb8440
ms.openlocfilehash: 77bf26d4e6bb068f9130deed5eb1ecbaee62afce
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866793"
---
# <a name="-operator-visual-basic"></a>\<\< 操作员 (Visual Basic) 

对位模式执行算术左移位运算。  
  
## <a name="syntax"></a>语法  
  
```vb  
result = pattern << amount  
```  
  
## <a name="parts"></a>组成部分  

 `result`  
 必需。 整数数值。 对该位模式进行移位的结果。 数据类型与 `pattern` 的数据类型相同。  
  
 `pattern`  
 必需。 整型数值表达式。 要进行移位的位模式。 数据类型必须为整型（`SByte`、`Byte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long` 或 `ULong`）。  
  
 `amount`  
 必需。 数值表达式。 要将该位模式移位的位数。 数据类型必须为 `Integer` 或扩展到 `Integer`。  
  
## <a name="remarks"></a>备注  

 算术移位不是循环的，这意味着，不会在另一端重新引入结果的末尾以外的位。 在算术左移位中，将丢弃超出结果数据类型范围的位，并将在右侧空出的位位置设置为零。  
  
 若要防止移位比结果可以容纳的位数多，Visual Basic 用与的 `amount` 数据类型相对应的大小掩码来屏蔽的值 `pattern` 。 这些值的二进制和均用于移位量。 大小掩码如下：  
  
|数据类型 `pattern`|大小掩码 (decimal) |大小掩码 (十六进制) |  
|----------------------------|---------------------------|-------------------------------|  
|`SByte`, `Byte`|7|&H00000007|  
|`Short`, `UShort`|15|&H0000000F|  
|`Integer`, `UInteger`|31|&H0000001F|  
|`Long`, `ULong`|63|&H0000003F|  
  
 如果 `amount` 为零，则的值与的 `result` 值相同 `pattern` 。 如果 `amount` 为负，则将其视为无符号值并使用适当的大小掩码屏蔽。  
  
 算术移位从不产生溢出异常。  
  
> [!NOTE]
> `<<`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果代码对这样的类或结构使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `<<` 运算符对整数值执行算术左移位。 结果始终与要移动的表达式的数据类型相同。  
  
 [!code-vb[VbVbalrOperators#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#12)]  
  
 上述示例的结果如下所示：  
  
- `result1` 为 192 (0000 0000 1100 0000) 。  
  
- `result2` 为 3072 (0000 1100 0000 0000) 。  
  
- `result3` 为-32768 (1000 0000 0000 0000) 。  
  
- `result4` 为 384 (0000 0001 1000 0000) 。  
  
- `result5` 为 0 (将15个位置向左移动) 。  
  
 的移位量 `result4` 计算为17和15，这等于1。  
  
## <a name="see-also"></a>另请参阅

- [移位运算符](bit-shift-operators.md)
- [赋值运算符](assignment-operators.md)
- [<<= 运算符](left-shift-assignment-operator.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [算术运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
