---
title: '>> 运算符'
ms.date: 07/20/2015
f1_keywords:
- vb.>>
helpviewer_keywords:
- operator>>
- '>> operator [Visual Basic]'
- bit shift operators [Visual Basic]
- operator >>
- right shift operators [Visual Basic]
ms.assetid: 054dc6a6-47d9-47ef-82da-cfa2b59fbf8f
ms.openlocfilehash: 00f43bc9bae6d550ed175906777ac273fc8e9a23
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873347"
---
# <a name="-operator-visual-basic"></a>>> 运算符 (Visual Basic) 

对位模式执行算术右移位运算。  
  
## <a name="syntax"></a>语法  
  
```vb  
result = pattern >> amount  
```  
  
## <a name="parts"></a>组成部分  

 `result`  
 必需。 整数数值。 对该位模式进行移位的结果。 数据类型与 `pattern` 的数据类型相同。  
  
 `pattern`  
 必需。 整型数值表达式。 要进行移位的位模式。 数据类型必须为整型（`SByte`、`Byte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long` 或 `ULong`）。  
  
 `amount`  
 必需。 数值表达式。 要将该位模式移位的位数。 数据类型必须为 `Integer` 或扩展到 `Integer`。  
  
## <a name="remarks"></a>备注  

 算术移位不是循环的，这意味着，不会在另一端重新引入结果的末尾以外的位。 在算术右移位中，将丢弃超出最右位位置的位，并将最左侧的 (符号) 位传播到左端空出的位位置。 这意味着，如果 `pattern` 具有负值，空出位置将设置为 1; 否则，将设置为零。  
  
 请注意，数据类型 `Byte` 、 `UShort` 、 `UInteger` 和 `ULong` 都是无符号的，因此没有要传播的符号位。 如果 `pattern` 为任何无符号类型，空出的位置始终设置为零。  
  
 若要防止移位比结果可容纳的位数多，Visual Basic `amount` 用与的数据类型相对应的大小掩码来屏蔽的值 `pattern` 。 这些值的二进制和均用于移位量。 大小掩码如下：  
  
|数据类型 `pattern`|大小掩码 (decimal) |大小掩码 (十六进制) |  
|----------------------------|---------------------------|-------------------------------|  
|`SByte`, `Byte`|7|&H00000007|  
|`Short`, `UShort`|15|&H0000000F|  
|`Integer`, `UInteger`|31|&H0000001F|  
|`Long`, `ULong`|63|&H0000003F|  
  
 如果 `amount` 为零，则的值与的 `result` 值相同 `pattern` 。 如果 `amount` 为负，则将其视为无符号值并使用适当的大小掩码屏蔽。  
  
 算术移位从不产生溢出异常。  
  
## <a name="overloading"></a>重载  

 `>>`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `>>` 运算符对整数值执行算术右移位运算。 结果始终与要移动的表达式的数据类型相同。  
  
 [!code-vb[VbVbalrOperators#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#14)]  
  
 上述示例的结果如下所示：  
  
- `result1` 为 2560 (0000 1010 0000 0000) 。  
  
- `result2` 为 160 (0000 0000 1010 0000) 。  
  
- `result3` 为 2 (0000 0000 0000 0010) 。  
  
- `result4` 为 640 (0000 0010 1000 0000) 。  
  
- `result5` 为 0 (向右) 移动15个位置。  
  
 的移位量 `result4` 计算为18和15，这等于2。  
  
 下面的示例演示如何对负值进行算术移位运算。  
  
 [!code-vb[VbVbalrOperators#55](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#55)]  
  
 上述示例的结果如下所示：  
  
- `negresult1` 为-512 (1111 1110 0000 0000) 。  
  
- `negresult2` 为-1 (将) 传播到符号位。  
  
## <a name="see-also"></a>另请参阅

- [移位运算符](bit-shift-operators.md)
- [赋值运算符](assignment-operators.md)
- [>>= 运算符](right-shift-assignment-operator.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [算术运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
