---
title: “Is”要求具有引用类型的操作数，但此操作数的值类型为“<typename>”。
ms.date: 07/20/2015
f1_keywords:
- bc30020
- vbc30020
helpviewer_keywords:
- BC30020
ms.assetid: 228afebd-1203-4bd3-8d7a-c5c56f3cedc4
ms.openlocfilehash: 5c9f156272cd0c3cbaeadbc0e162129f41619cc6
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163345"
---
# <a name="bc30020-is-requires-operands-that-have-reference-types-but-this-operand-has-the-value-type-typename"></a>BC30020： "Is" 需要具有引用类型的操作数，但此操作数的值类型为 " \<typename> "

`Is`比较运算符确定两个对象变量是否引用相同的实例。 没有为值类型定义此比较。

 **错误 ID：** BC30020

## <a name="to-correct-this-error"></a>更正此错误

- 使用适当的算术比较运算符或 `Like` 运算符比较两个值类型。

## <a name="see-also"></a>另请参阅

- [Is 运算符](../operators/is-operator.md)
- [Like 运算符](../operators/like-operator.md)
- [比较运算符](../operators/comparison-operators.md)
