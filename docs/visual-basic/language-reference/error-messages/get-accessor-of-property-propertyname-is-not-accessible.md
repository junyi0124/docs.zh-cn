---
title: 属性“<propertyname>”的“Get”访问器不可访问
ms.date: 07/20/2015
f1_keywords:
- vbc31103
- bc31103
helpviewer_keywords:
- BC31103
ms.assetid: 3c346c32-7669-4b04-841d-7a9df9cb703e
ms.openlocfilehash: bd6830e16ba3d1f76c61519b4456832a2efec716
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162903"
---
# <a name="bc31103-get-accessor-of-property-propertyname-is-not-accessible"></a>BC31103：属性 "" 的 "Get" 访问器 \<propertyname> 不可访问

当某个语句无权访问该属性的过程时，它将尝试检索该属性的值 `Get` 。

 如果 [Get 语句](../statements/get-statement.md) 使用比 [属性语句](../statements/property-statement.md)更严格的访问级别进行标记，则在以下情况下读取属性值的尝试可能会失败：

- `Get`语句标记为[Private](../modifiers/private.md) ，并且调用代码位于定义该属性的类或结构之外。

- `Get`语句被标记为[受保护](../modifiers/protected.md)，调用代码不在定义该属性的类或结构中，也不在派生类中。

- `Get`语句被标记为[Friend](../modifiers/friend.md) ，调用代码不在定义该属性的程序集中。

 **错误 ID：** BC31103

## <a name="to-correct-this-error"></a>更正此错误

- 如果你可以控制定义属性的源代码，请考虑 `Get` 使用与属性本身相同的访问级别声明过程。

- 如果你不能控制定义属性的源代码，或者必须限制 `Get` 过程访问级别，而不是属性本身，请尝试将读取属性值的语句移到对属性具有更好访问权限的代码区域。

## <a name="see-also"></a>另请参阅

- [Property 过程](../../programming-guide/language-features/procedures/property-procedures.md)
- [如何：声明具有混合访问级别的属性](../../programming-guide/language-features/procedures/how-to-declare-a-property-with-mixed-access-levels.md)
