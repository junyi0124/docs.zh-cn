---
title: Like 运算符
ms.date: 07/20/2015
f1_keywords:
- Like
- vb.Like
helpviewer_keywords:
- similar to
- pattern matching
- Like operator [Visual Basic]
- '? symbol, wildcard character'
- string comparison [Visual Basic], Like operator
- strings [Visual Basic], comparing
- comparison operators [Visual Basic]
- symbols, wildcard
- wildcards, Like operator
- strings [Visual Basic], matching
- string comparison [Visual Basic], sorting data
- data [Visual Basic], sorting
- text [Visual Basic], comparing
- operators [Visual Basic], pattern-matching
- data [Visual Basic], string comparisons
- string comparison [Visual Basic], Like operators
ms.assetid: 966283ec-80e2-4294-baa8-c75baff804f9
ms.openlocfilehash: 49dfe5cf5dbcf8dc6f79f569a92e36aa81806913
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866773"
---
# <a name="like-operator-visual-basic"></a>Like 运算符 (Visual Basic)

将字符串与模式进行比较。  

> [!IMPORTANT]
> `Like`.Net Core 和 .NET Standard 项目目前不支持运算符。

## <a name="syntax"></a>语法  
  
```vb  
result = string Like pattern  
```  
  
## <a name="parts"></a>组成部分  

 `result`  
 必需。 任何 `Boolean` 变量。 结果是一个 `Boolean` 值，该值指示是否 `string` 满足 `pattern` 。  
  
 `string`  
 必需。 任何 `String` 表达式。  
  
 `pattern`  
 必需。 `String`符合 "备注" 中所述的模式匹配约定的任何表达式。  
  
## <a name="remarks"></a>备注  

 如果中的值 `string` 满足中包含的模式 `pattern` ， `result` 则为 `True` 。 如果字符串不满足模式， `result` 则为 `False` 。 如果 `string` 和 `pattern` 都为空字符串，则结果为 `True` 。  
  
## <a name="comparison-method"></a>比较方法  

 运算符的行为 `Like` 取决于 [Option Compare 语句](../statements/option-compare-statement.md)。 每个源文件的默认字符串比较方法为 `Option Compare Binary` 。  
  
## <a name="pattern-options"></a>模式选项  

 内置模式匹配为字符串比较提供了多种工具。 使用模式匹配功能，可以将中的每个字符与 `string` 特定字符、通配符、字符列表或字符范围匹配。 下表显示了中允许的字符 `pattern` 以及它们匹配的内容。  
  
|字符 `pattern`|匹配项 `string`|  
|-----------------------------|-------------------------|  
|`?`|任何单个字符|  
|`*`|零个或多个字符|  
|`#`|任何单个数字 (0 – 9) |  
|`[charlist]`|中的任何单个字符 `charlist`|  
|`[!charlist]`|不在中的任何单个字符 `charlist`|  
  
## <a name="character-lists"></a>字符列表  

 括在括号中的一个或多个字符组 (`charlist`)  (`[ ]`) 可用于匹配中的任何单个字符 `string` ，并可包括几乎所有字符代码（包括数字）。  
  
 开始时 () 的感叹号 `!` 表示在中 `charlist` 找到除中的字符以外的任何字符时进行匹配 `charlist` `string` 。 当在方括号外使用时，感叹号与自身匹配。  
  
## <a name="special-characters"></a>特殊字符  

 若要匹配特殊字符左方括号 (`[`) 、问号 (`?`) 、数字符号 () `#` 和星号 () `*` ，请将它们括在括号中。 `]`不能在组内使用右方括号 () ，而是可以将其作为单个字符在组外使用。  
  
 字符序列 `[]` 被视为 () 长度为零的字符串 `""` 。 但是，它不能是括在括号中的字符列表的一部分。 如果要检查中的某个位置是否 `string` 包含一组字符或不包含任何字符，可以使用 `Like` 两次。 有关示例，请参阅 [如何：将字符串与模式匹配](../../programming-guide/language-features/operators-and-expressions/how-to-match-a-string-against-a-pattern.md)。  
  
## <a name="character-ranges"></a>字符范围  

 通过使用连字符 (`–`) 来分隔范围的下限和上限， `charlist` 可以指定字符范围。 例如， `[A–Z]` 如果中的相应字符位置 `string` 包含范围内的任何字符，则会导致匹配 `A` `Z` ; `[!H–L]` 如果相应的字符位置包含范围之外的任何字符， `H` `L` 则结果将生成匹配项。  
  
 指定字符范围时，它们必须按升序排序，即从最低到最高。 因此， `[A–Z]` 是有效的模式，但 `[Z–A]` 不是。  
  
### <a name="multiple-character-ranges"></a>多字符范围  

 若要为同一字符位置指定多个范围，请将它们放在不带分隔符的相同括号中。 例如， `[A–CX–Z]` 如果中的相应字符位置 `string` 包含范围或范围内的任何字符，则会导致 `A` 匹配 `C` `X` `Z` 。  
  
### <a name="usage-of-the-hyphen"></a>连字符的用法  

 连字符 (`–`) 可以出现在感叹号后的开头 (，如果有任何) 或末尾，则 `charlist` 为。 在其他任何位置，连字符标识由连字符两侧的字符分隔的一系列字符。  
  
## <a name="collating-sequence"></a>排序顺序  

 指定范围的含义取决于运行时的字符排序，由确定， `Option Compare` 以及运行代码的系统的区域设置。 对于 `Option Compare Binary` ，范围 `[A–E]` 匹配、、、 `A` `B` `C` `D` 和 `E` 。 With、匹配、、、、、、、、、、 `Option Compare Text` `[A–E]` `A` `a` `À` `à` `B` `b` `C` `c` `D` `d` `E` 和 `e` 。 该范围不匹配， `Ê` 也不是 `ê` 因为重音字符在排序顺序中逐重音字符排序。  
  
## <a name="digraph-characters"></a>连字符  

 在某些语言中，有表示两个不同字符的字母字符。 例如，几种语言使用字符 `æ` 来表示字符 `a` 以及 `e` 它们一起显示的字符。 `Like`运算符识别单个连字符和两个单个字符是否等效。  
  
 如果在系统区域设置中指定了使用连字符的语言，则或中的单个连字符的出现位置 `pattern` `string` 与另一个字符串中的等效双字符序列匹配。 同样， `pattern` 括在括号中的一条带区字符 (自身、列表或范围中) 与中等效的双字符序列匹配 `string` 。  
  
## <a name="overloading"></a>重载  

 `Like`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 此示例使用 `Like` 运算符将字符串与各种模式进行比较。 结果将进入一个 `Boolean` 变量，该变量指示每个字符串是否满足此模式。  
  
 [!code-vb[VbVbalrOperators#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#30)]  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Strings.InStr%2A>
- <xref:Microsoft.VisualBasic.Strings.StrComp%2A>
- [比较运算符](comparison-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [Option Compare 语句](../statements/option-compare-statement.md)
- [运算符和表达式](../../programming-guide/language-features/operators-and-expressions/index.md)
- [如何：将字符串与模式相匹配](../../programming-guide/language-features/operators-and-expressions/how-to-match-a-string-against-a-pattern.md)
