---
title: <type1>“<typename>”必须为接口“<interfacename>”实现“<membername>”
ms.date: 07/20/2015
f1_keywords:
- vbc30154
- bc30154
helpviewer_keywords:
- BC30154
ms.assetid: 259afdfa-3608-4760-adcb-88ec0da5020d
ms.openlocfilehash: a036097d778daae59ef3bbd74ab8507cd16e6e24
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161850"
---
# <a name="bc30154-type1typename-must-implement-membername-for-interface-interfacename"></a>BC30154： \<type1> " \<typename> " 必须 \<membername> 为接口 "" 实现 " \<interfacename> "

" \<typename> " 必须为接口 "" 实现 "" \<membername> \<interfacename> 。 实现属性必须具有匹配的 "ReadOnly"/"WriteOnly" 说明符。

 类或结构声明实现接口，但不实现由接口定义的过程、属性或事件。 必须实现接口的每个成员。

 **错误 ID：** BC30154

## <a name="to-correct-this-error"></a>更正此错误

1. 使用在接口中定义的相同名称和签名声明成员。 请确保至少包含 `End Function` 、 `End Sub` 或 `End Property` 语句。

2. 将 `Implements` 子句添加到 `Function` 、、 `Sub` `Property` 或 `Event` 语句的末尾。 例如：

    ```vb
    Public Event ItHappened() Implements IBaseInterface.ItHappened
    ```

3. 实现某个属性时，请确保 `ReadOnly` 或的 `WriteOnly` 使用方式与接口定义中的相同。

4. 在实现属性时，请 `Get` `Set` 根据需要声明和过程。

## <a name="see-also"></a>另请参阅

- [Implements 语句](../statements/implements-statement.md)
- [接口](../../programming-guide/language-features/interfaces/index.md)
