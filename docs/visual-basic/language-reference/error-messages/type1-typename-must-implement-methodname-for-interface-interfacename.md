---
title: <type1>“<typename>”必须为接口“<interfacename>”实现“<methodname>”
ms.date: 07/20/2015
f1_keywords:
- vbc30149
- bc30149
helpviewer_keywords:
- BC30149
ms.assetid: 29d1b7f4-dca7-478c-bbe7-c657f342c183
ms.openlocfilehash: 68c6f65e6be229cc74458fa56fe3d3aa889c18f7
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161863"
---
# <a name="bc30149-type1typename-must-implement-methodname-for-interface-interfacename"></a>BC30149： \<type1> " \<typename> " 必须 \<methodname> 为接口 "" 实现 " \<interfacename> "

类或结构声明实现接口，但不实现由接口定义的过程。 必须实现接口的每个成员。

 **错误 ID：** BC30149

## <a name="to-correct-this-error"></a>更正此错误

1. 使用在接口中定义的相同名称和签名声明过程。 请确保至少包含 `End Function` 或 `End Sub` 语句。

2. 将 `Implements` 子句添加到或语句的末尾 `Function` `Sub` 。 例如：

    ```vb
    Public Sub DoSomething() Implements IBaseInterface.DoSomething
    ```

## <a name="see-also"></a>另请参阅

- [Implements 语句](../statements/implements-statement.md)
- [接口](../../programming-guide/language-features/interfaces/index.md)
