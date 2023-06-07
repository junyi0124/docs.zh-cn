---
title: 如何：加速访问具有长限定路径的对象
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], accessing
- variables [Visual Basic], object
- With statement [Visual Basic]
- With block
- object variables [Visual Basic], accessing
ms.assetid: 3eb7657f-c9fe-4e05-8bc3-4bb14d5ae585
ms.openlocfilehash: fe93e7bac2a21f1060d1f93765eb35e1ad0c7eb0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410407"
---
# <a name="how-to-speed-up-access-to-an-object-with-a-long-qualification-path-visual-basic"></a>如何：加速访问具有长限定路径的对象 (Visual Basic)

如果经常访问的对象需要多个方法和属性的限定路径，则可以通过不重复限定路径来加快代码的速度。

可以通过两种方法来避免重复限定路径。 可以将对象分配给一个变量，也可以在 `With` ... 块中使用它。 `End With`

### <a name="to-speed-up-access-to-a-heavily-qualified-object-by-assigning-it-to-a-variable"></a>通过将严重限定对象赋给变量来加快对该对象的访问速度

1. 声明经常访问的对象类型的变量。 指定声明的初始化部分中的限定路径。

    ```vb
    Dim ctrlActv As Control = someForm.ActiveForm.ActiveControl
    ```

2. 使用变量访问对象的成员。

    ```vb
    ctrlActv.Text = "Test"
    ctrlActv.Location = New Point(100, 100)
    ctrlActv.Show()
    ```

### <a name="to-speed-up-access-to-a-heavily-qualified-object-by-using-a-withend-with-block"></a>若要通过使用 With，加速对严重限定对象的访问结尾为块

1. 将限定路径放在 `With` 语句中。

    ```vb
    With someForm.ActiveForm.ActiveControl
    ```

2. 在语句前面的块内访问对象的成员 `With` `End With` 。

    ```vb
        .Text = "Test"
        .Location = New Point(100, 100)
        .Show()
    End With
    ```

## <a name="see-also"></a>另请参阅

- [对象变量](object-variables.md)
- [With...End With 语句](../../../language-reference/statements/with-end-with-statement.md)
