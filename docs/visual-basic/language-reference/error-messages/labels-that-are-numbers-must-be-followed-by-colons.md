---
title: 数字标签后面必须跟冒号
ms.date: 07/20/2015
f1_keywords:
- vbc30801
- bc30801
helpviewer_keywords:
- BC30801
ms.assetid: 67743319-2d1c-496e-bfd9-22b046b43b5a
ms.openlocfilehash: 6b89f23731e75b5248390d02e3c05410104bde6b
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163306"
---
# <a name="bc30801-labels-that-are-numbers-must-be-followed-by-colons"></a>BC30801：数字标签后面必须跟冒号

行号与其他类型的标签遵循相同的规则，并且必须包含一个冒号。

 **错误 ID：** BC30801

## <a name="to-correct-this-error"></a>更正此错误

- 将该数字后跟一个冒号，放在代码行的开头;例如：

    ```vb
    400:    X += 1
    ```

## <a name="see-also"></a>另请参阅

- [GoTo 语句](../statements/goto-statement.md)
