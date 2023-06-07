---
title: “<membername>”在继承接口“<interfacename1>”和“<interfacename2>”之间不明确
ms.date: 07/20/2015
f1_keywords:
- vbc30685
- bc30685
helpviewer_keywords:
- BC30685
ms.assetid: 756add7a-23d5-4b4f-a48d-8297d6459c73
ms.openlocfilehash: f666bf380f8b94c50c2bc5fd865b37d96e92991f
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162435"
---
# <a name="bc30685-membername-is-ambiguous-across-the-inherited-interfaces-interfacename1-and-interfacename2"></a>BC30685： " \<membername> " 在继承接口 " \<interfacename1> " 和 "" 之间不 \<interfacename2> 明确

接口从多个接口继承具有相同名称的两个或多个成员。

 **错误 ID：** BC30685

## <a name="to-correct-this-error"></a>更正此错误

- 将值强制转换为要使用的基接口;例如：

    ```vb
    Interface Left
        Sub MySub()
    End Interface

    Interface Right
        Sub MySub()
    End Interface

    Interface LeftRight
        Inherits Left, Right
    End Interface

    Module test
        Sub Main()
            Dim x As LeftRight
            ' x.MySub()  'x is ambiguous.
            CType(x, Left).MySub() ' Cast to base type.
            CType(x, Right).MySub() ' Call the other base type.
        End Sub
    End Module
    ```

## <a name="see-also"></a>另请参阅

- [接口](../../programming-guide/language-features/interfaces/index.md)
