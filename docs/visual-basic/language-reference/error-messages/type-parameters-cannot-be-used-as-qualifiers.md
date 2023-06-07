---
title: 类型参数不能用作限定符
ms.date: 07/20/2015
f1_keywords:
- vbc32098
- bc32098
helpviewer_keywords:
- BC32098
ms.assetid: bab05325-dde8-4621-a5f6-368b5b7b2d76
ms.openlocfilehash: 14e6094b0cc129eba86db1808c0f0575955f5e75
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161187"
---
# <a name="bc32098-type-parameters-cannot-be-used-as-qualifiers"></a>BC32098：不能将类型参数用作限定符

使用包含类型参数的限定字符串限定编程元素。

类型参数表示在构造泛型类型时要提供的类型的要求。 它不表示特定的已定义类型。 限定字符串必须只包含在编译时定义的元素。

以下代码可能会生成此错误：

```vb
Public Function CheckText(Of c As System.Windows.Forms.Control)(
    badText As String) As Boolean

    Dim saveText As c.Text
    ' Insert code to look for badText within saveText.
End Function
```

 **错误 ID：** BC32098

## <a name="to-correct-this-error"></a>更正此错误

1. 从限定字符串中删除类型参数，或将其替换为定义的类型。

2. 如果需要使用构造类型来查找要限定的编程元素，则必须使用其他程序逻辑。

## <a name="see-also"></a>另请参阅

- [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Type List](../statements/type-list.md)
