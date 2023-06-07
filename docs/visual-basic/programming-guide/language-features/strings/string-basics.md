---
title: 字符串基础
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], Like operator
- strings [Visual Basic], Visual Basic
- strings [Visual Basic], regular expressions
ms.assetid: 5674418d-f00d-4f72-9f98-d15897793350
ms.openlocfilehash: 44736f4db9977d9f69a0571cc80fa327dcf96581
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072504"
---
# <a name="string-basics-in-visual-basic"></a>字符串基础 (Visual Basic)

`String` 数据类型表示一系列字符（每个字符都进而表示 `Char` 数据类型的一个实例）。 本主题介绍 Visual Basic 中的字符串的基本概念。  
  
## <a name="string-variables"></a>字符串变量  

 可以向字符串的实例分配表示一系列字符的文本值。 例如：  
  
 [!code-vb[VbVbalrStrings#63](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#63)]  
  
 `String` 变量还可以接受计算结果为字符串的任何表达式。 示例如下所示：  
  
 [!code-vb[VbVbalrStrings#64](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#64)]  
  
 分配给 `String` 变量的任何文本都必须括在引号 ("") 内。 这意味着字符串中的引号不能使用引号进行表示。 例如，下面的代码会导致编译器错误：  
  
 [!code-vb[VbVbalrStrings#65](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#65)]  
  
 此代码会导致错误，因为编译器会在第二个引号之后终止字符串，字符串的其余部分会解释为代码。 若要解决此问题，Visual Basic 将字符串文本中的两个引号解释为字符串中的一个引号。 下面的示例演示在字符串中包含引号的正确方法：  
  
 [!code-vb[VbVbalrStrings#66](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#66)]  
  
 在上面的示例中，字词 `Look` 前面的两个引号会成为字符串中的一个引号。 行尾的三个引号表示字符串中的一个引号和字符串终止符。  
  
 字符串可以包含多行：  
  
```vb  
Dim x = "hello  
world"  
```  
  
 生成的字符串包含在字符串中使用的换行符序列（vbcr、vbcrlf 等）。  不再需要使用旧的解决方法：  
  
```vb  
Dim x = <xml><![CDATA[Hello  
World]]></xml>.Value  
```  
  
## <a name="characters-in-strings"></a>字符串中的字符  

 可以将字符串视为一系列 `Char` 值，`String` 类型具有内置函数，可用于对字符串执行很多操作（类似于数组允许的操作）。 与 .NET Framework 中的所有数组一样，这些是从零开始的数组。 还可以通过 `Chars` 属性引用字符串中的特定字符，该属性提供了一种方法，用于通过字符在字符串中出现的位置来访问它。 例如：  
  
 [!code-vb[VbVbalrStrings#67](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#67)]  
  
 在上面的示例中，字符串的 `Chars` 属性返回字符串中的第四个字符（即 `D`），并将它分配给 `myChar`。 还可以通过 `Length` 属性获取特定字符串的长度。 如果需要对字符串执行多个数组类型的操作，则可以使用字符串的 `ToCharArray` 函数将它转换为 `Char` 实例的数组。 例如：  
  
 [!code-vb[VbVbalrStrings#68](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#68)]  
  
 变量 `myArray` 现在包含 `Char` 值的数组，其中每个值都表示 `myString` 中的一个字符。  
  
## <a name="the-immutability-of-strings"></a>字符串的不可变性  

 字符串是 *不可变*的，这意味着在创建后，不能更改其值。 但是，这不会阻止你将多个值分配给字符串变量。 请考虑以下示例：  
  
 [!code-vb[VbVbalrStrings#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#69)]  
  
 这里会创建一个字符串变量（向它提供值），然后更改其值。  
  
 更具体地说，在第一行中，会创建类型 `String` 的实例，并提供值 `This string is immutable`。 在示例的第二个行中，会创建一个新实例并向它提供值 `Or is it?`，字符串变量会放弃它对第一个实例的引用，并存储对新实例的引用。  
  
 与其他内部数据类型不同，`String` 是引用类型。 当引用类型的变量作为参数传递给函数或子例程时，会传递对数据存储位置的内存地址的引用（而不是字符串的实际值）。 因此在上面的示例中，变量的名称保持不变，但它会指向 `String` 类的另一个新实例（该实例会保存新值）。  
  
## <a name="see-also"></a>请参阅

- [字符串介绍 (Visual Basic)](introduction-to-strings.md)
- [String 数据类型](../../../language-reference/data-types/string-data-type.md)
- [Char 数据类型](../../../language-reference/data-types/char-data-type.md)
- [基本字符串操作](../../../../standard/base-types/basic-string-operations.md)
