---
title: “<membername>”不能通过 <typename>“<containertype>”在项目外部公开类型“<containertypename>”
ms.date: 07/20/2015
f1_keywords:
- bc30909
- vbc30909
helpviewer_keywords:
- BC30909
ms.assetid: ffa7395d-e182-4087-8ce8-079810fdae54
ms.openlocfilehash: a3972eabfe297b89c0e4d0f36943ac58e5bdf688
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162461"
---
# <a name="bc30909-membername-cannot-expose-type-typename-outside-the-project-through-containertype-containertypename"></a>BC30909： " \<membername> " 不能 \<typename> 通过 \<containertype> "" 在项目外部公开类型 "" \<containertypename>

变量、过程参数或函数返回在其容器外部公开，但它被声明为不得在容器外部公开的类型。

 以下框架代码显示了生成此错误的情况。

```vb
Private Class privateClass
End Class
Public Class mainClass
    Public exposedVar As New privateClass
End Class
```

 声明为、、或的类型 `Protected` `Friend` 旨在在 `Protected Friend` `Private` 其声明上下文之外进行有限的访问。 将其用作受限访问的变量的数据类型将会降低此目的。 在前面的代码中， `exposedVar` 是，将 `Public` 公开 `privateClass` 给不应访问它的代码。

 **错误 ID：** BC30909

## <a name="to-correct-this-error"></a>更正此错误

- 更改变量、过程参数或函数返回的访问级别，使其至少与数据类型的访问级别具有相同的限制。

## <a name="see-also"></a>另请参阅

- [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)
