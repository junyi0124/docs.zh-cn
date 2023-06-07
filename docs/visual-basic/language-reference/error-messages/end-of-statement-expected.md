---
title: 需要语句结束
ms.date: 07/20/2015
f1_keywords:
- bc30205
- vbc30205
helpviewer_keywords:
- BC30205
ms.assetid: 53c7f825-a737-4b76-a1fa-f67745b8bd40
ms.openlocfilehash: 36883989fe5aa0b5c70aa9ab1f7c991fab99e778
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163124"
---
# <a name="bc30205-end-of-statement-expected"></a>BC30205：应为语句结尾

语句在语法上完成，但在完成该语句的元素后附加了一个编程元素。 每个语句的末尾都需要行结束符。

 行结束符将 Visual Basic 源文件中的字符分为多行。 行结束符的示例包括 Unicode 回车符 ( # B0 HD) ，Unicode 换行符 ( # B1 HA) ，Unicode 回车符后跟 Unicode 换行符。 有关行终止符的详细信息，请参阅 [Visual Basic 语言规范](~/_vblang/spec/lexical-grammar.md#line-terminators)。

 **错误 ID：** BC30205

## <a name="to-correct-this-error"></a>更正此错误

1. 检查两个不同的语句是否被无意中放在同一行上。

2. 在完成该语句的元素之后插入一个行结束符。

## <a name="see-also"></a>另请参阅

- [如何：在代码中拆分和合并语句](../../programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
- [语句](../../programming-guide/language-features/statements.md)
