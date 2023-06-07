---
title: 语句在方法/多行 lambda 内无效
ms.date: 07/20/2015
f1_keywords:
- vbc30024
- bc30024
helpviewer_keywords:
- BC30024
ms.assetid: 758e7a8f-429b-42c1-9a78-778e5b480e04
ms.openlocfilehash: cef992c3eaa2b82bbf5e8993f9fccd64ae388c95
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92159672"
---
# <a name="bc30024-statement-is-not-valid-inside-a-methodmultiline-lambda"></a>BC30024：语句在方法/多行 lambda 内无效

语句在 `Sub` 、 `Function` 、属性 `Get` 或属性过程中无效 `Set` 。 某些语句可以放置在模块或类级别。 其他类（如 `Option Strict` ）必须位于命名空间级别，并位于所有其他声明之前。

 **错误 ID：** BC30024

## <a name="to-correct-this-error"></a>更正此错误

- 删除过程中的语句。

## <a name="see-also"></a>另请参阅

- [Sub 语句](../statements/sub-statement.md)
- [Function 语句](../statements/function-statement.md)
- [Get 语句](../statements/get-statement.md)
- [Set 语句](../statements/set-statement.md)
