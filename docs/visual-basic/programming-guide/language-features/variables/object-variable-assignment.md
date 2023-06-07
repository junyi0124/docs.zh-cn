---
title: 对象变量赋值
ms.date: 07/20/2015
helpviewer_keywords:
- Nothing keyword [Visual Basic], object variable assignment
- object variables [Visual Basic], initializing
- variables [Visual Basic], initializing
- objects [Visual Basic], current instance
- object variables [Visual Basic], assigning
- variables [Visual Basic], object variables
- current instance [Visual Basic], defined
- variables [Visual Basic], assigning
- assignment statements [Visual Basic], object variable assignment
- Me keyword [Visual Basic], as object variable
ms.assetid: 3706811d-fd40-44fe-8727-d692e8e55d6d
ms.openlocfilehash: 9ae1a307e8c886166d516140b7f100a411cedcfa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410369"
---
# <a name="object-variable-assignment-visual-basic"></a>对象变量赋值 (Visual Basic)

使用常规赋值语句将对象分配给对象变量。 可以分配对象表达式或[Nothing](../../../language-reference/nothing.md)关键字，如下面的示例所示。

```vb
Dim thisObject As Object
' The following statement assigns an object reference.
thisObject = Form1
' The following statement discontinues association with any object.
thisObject = Nothing
```

`Nothing`表示当前没有对象分配给该变量。

## <a name="initialization"></a>初始化

当你的代码开始运行时，你的对象变量将初始化为 `Nothing` 。 其声明包含初始化的将重新初始化为在执行声明语句时指定的值。

可以通过使用[New](../../../language-reference/operators/new-operator.md)关键字在声明中包含初始化。 以下声明语句声明对象变量 `testUri` 并 `ver` 为其分配特定的对象。 每个都使用适当类的重载构造函数之一来初始化对象。

```vb
Dim testUri As New System.Uri("https://www.microsoft.com")
Dim ver As New System.Version(6, 1, 0)
```

## <a name="disassociation"></a>Disassociation

设置对象变量以使 `Nothing` 变量与任何特定对象的关联停止。 这样可以防止意外更改变量来更改对象。 它还允许您测试对象变量是否指向有效对象，如下面的示例所示。

```vb
If otherObject IsNot Nothing Then
    ' otherObject refers to a valid object, so your code can use it.
End If
```

如果变量所引用的对象在另一应用程序中，则此测试无法确定该应用程序是否已终止或只是使该对象无效。

值为的对象变量 `Nothing` 也称为*空引用*。

## <a name="current-instance"></a>当前实例

对象的*当前实例*是当前正在执行代码的实例。 由于所有代码都在过程中执行，因此当前实例是在其中调用过程的实例。

`Me`关键字充当引用当前实例的对象变量。 如果过程不是[共享](../../../language-reference/modifiers/shared.md)的，它可以使用 `Me` 关键字获取指向当前实例的指针。 共享过程不能与类的特定实例相关联。

使用 `Me` 对于将当前实例传递到另一个模块中的过程特别有用。 例如，假设您有多个 XML 文档，并且想要将一些标准文本添加到其中。 下面的示例定义了用于执行此操作的过程。

```vb
Sub addStandardText(XmlDoc As System.Xml.XmlDocument)
    XmlDoc.CreateTextNode("This text goes into every XML document.")
End Sub
```

然后，每个 XML 文档对象都可以调用该过程，并将其当前实例作为参数传递。 下面的示例对这种情况进行了演示。

```vb
addStandardText(Me)
```

## <a name="see-also"></a>另请参阅

- [对象变量](object-variables.md)
- [对象变量声明](object-variable-declaration.md)
- [对象变量值](object-variable-values.md)
- [如何：在 Visual Basic 中声明对象变量并为它分配对象](how-to-declare-an-object-variable-and-assign-an-object-to-it.md)
- [如何：使对象变量不引用任何实例](how-to-make-an-object-variable-not-refer-to-any-instance.md)
- [Me、My、MyBase 和 MyClass](../../program-structure/me-my-mybase-and-myclass.md)
