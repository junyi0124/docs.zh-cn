---
title: 运算符声明必须是以下其中之一： +,-, *,-,-, ^、 &amp; 、Like、Mod、And、Or、Xor、Not、 <<、 >>、=、 <>、<、<=、>、>=、CType、IsTrue、IsFalse
ms.date: 07/20/2015
f1_keywords:
- bc33000
- vbc33000
helpviewer_keywords:
- BC33000
ms.assetid: 15c5d8eb-3a8c-4141-8f41-33151afabf97
ms.openlocfilehash: a94e62e33427987a302a6244b2b8ce8d295e4f11
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92159893"
---
# <a name="bc33000-operator-declaration-must-be-one-of----amp-like-mod-and-or-xor-not--"></a>BC33000：运算符声明必须是以下之一： +,-, *、 \, /、^、 &amp; 、Like、Mod、And、Or、Xor、Not、 \<\<, >> .。。

您只能声明一个适合重载的运算符。 下表列出了可以声明的运算符。

|类型|运算符|
|----------|---------------|
|一元|`+`, `-`, `IsFalse`, `IsTrue`, `Not`|
|二进制|`+`, `-`, `*`, `/`, `\`, `&`, `^`, `>>`, `<<`, `=`, `<>`, `>`, `>=`, `<`, `<=`, `And`, `Like`, `Mod`, `Or`, `Xor`|
|转换（一元）|`CType`|

 请注意， `=` 二元列表中的运算符是比较运算符，而不是赋值运算符。

 **错误 ID：** BC33000

## <a name="to-correct-this-error"></a>更正此错误

- 从一组可重载运算符中选择一个运算符。

- 如果你需要重载无法直接重载的运算符这一功能，请创建用于获取适当参数并返回适当值的 `Function` 过程。

## <a name="see-also"></a>另请参阅

- [Operator Statement](../statements/operator-statement.md)
- [运算符过程](../../programming-guide/language-features/procedures/operator-procedures.md)
- [如何：定义运算符](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [如何：定义转换运算符](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [Function 语句](../statements/function-statement.md)
