---
title: 如何：声明对象变量并为其分配对象
ms.date: 07/20/2015
helpviewer_keywords:
- object variables [Visual Basic], declaring
- declaring object variables [Visual Basic]
ms.assetid: 2fa77dde-1fb2-439a-80d4-3e9787649fad
ms.openlocfilehash: d9a8c1fb30bfa5988d48202e41202e7ede0f5f27
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410498"
---
# <a name="how-to-declare-an-object-variable-and-assign-an-object-to-it-in-visual-basic"></a>如何：在 Visual Basic 中声明对象变量并为它分配对象

通过[Object Data Type](../../../language-reference/data-types/object-data-type.md) `As Object` 在[Dim 语句](../../../language-reference/statements/dim-statement.md)中指定来声明对象数据类型的变量。 通过 `=` 在赋值语句或初始化子句中将对象放置在等号（）之后，可以将对象分配给此类变量。

## <a name="example"></a>示例

下面的示例声明一个 `Object` 变量，并将当前的实例分配给它。

```vb
Dim thisObject As Object
thisObject = "This is an Object"
```

可以通过将变量初始化为其声明的一部分，来合并声明和赋值。 下面的示例与前面的示例等效。

```vb
Dim thisObject As Object= "This is an Object"
```

## <a name="compile-the-code"></a>编译代码

此示例需要：

- 对 <xref:System> 命名空间的引用。

- 要在其中放置语句的类、结构或模块 `Dim` 。

- 要在其中放置赋值语句的过程。

## <a name="see-also"></a>另请参阅

- [变量声明](variable-declaration.md)
- [对象变量](object-variables.md)
- [对象变量声明](object-variable-declaration.md)
- [Object Data Type](../../../language-reference/data-types/object-data-type.md)
- [Dim 语句](../../../language-reference/statements/dim-statement.md)
- [局部类型推理](local-type-inference.md)
- [Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)
