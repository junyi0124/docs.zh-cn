---
title: 应为初始值设定项
ms.date: 07/20/2015
f1_keywords:
- vbc30996
- bc30996
helpviewer_keywords:
- BC30996
ms.assetid: 6e183fe0-8888-43ed-a062-01571079455f
ms.openlocfilehash: cbe77bab3e4f8bf2094c70c1c16d95ee897c729e
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163007"
---
# <a name="bc30996-initializer-expected"></a>BC30996：应为初始值设定项

您尝试使用对象初始值设定项声明类的实例，其中初始化列表为空，如下面的示例中所示。

 `' Not valid.`

 `' Dim aStudent As New Student With {}`

 至少必须在初始值设定项列表中初始化一个字段或属性，如下面的示例中所示。

 `Dim aStudent As New Student With {.year = "Senior"}`

 **错误 ID：** BC30996

## <a name="to-correct-this-error"></a>更正此错误

- 在初始值设定项中初始化至少一个字段或属性，或不使用对象初始值设定项。

## <a name="see-also"></a>另请参阅

- [对象初始值设定项：命名和匿名类型](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [如何：使用对象初始值设定项声明对象](../../programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md)
