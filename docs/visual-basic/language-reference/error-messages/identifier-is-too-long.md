---
title: 标识符太长
ms.date: 07/20/2015
f1_keywords:
- bc30033
- vbc30033
helpviewer_keywords:
- BC30033
ms.assetid: 3d07f6d0-9a2f-49ca-94e8-1e354932e855
ms.openlocfilehash: eafcb2fdf706e12fa606a442ad577c88ed5a7427
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162851"
---
# <a name="bc30033-identifier-is-too-long"></a>BC30033：标识符太长

每个编程元素的名称或标识符限制为1023个字符。 此外，完全限定的名称不能超过1023个字符。 这意味着 () 的整个标识符字符串的 `<namespace>.<...>.<namespace>.<class>.<element>` 长度不能超过1023个字符，包括成员访问运算符 (`.`) 字符。

 **错误 ID：** BC30033

## <a name="to-correct-this-error"></a>更正此错误

- 减小标识符的长度。

## <a name="see-also"></a>另请参阅

- [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)
