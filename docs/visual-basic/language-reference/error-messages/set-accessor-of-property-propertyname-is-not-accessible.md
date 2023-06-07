---
title: 属性“<propertyname>”的“Set”访问器不可访问
ms.date: 07/20/2015
f1_keywords:
- vbc31102
- bc31102
helpviewer_keywords:
- BC31102
ms.assetid: 6f7b31b7-3656-4ae1-8851-90f5f4c6950a
ms.openlocfilehash: 3cf828eb5f11090a74a65388e2b89a191046a456
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162240"
---
# <a name="bc31102-set-accessor-of-property-propertyname-is-not-accessible"></a>BC31102：属性 "" 的 "Set" 访问器 \<propertyname> 不可访问

当某个语句无权访问该属性的过程时，它将尝试存储该属性的值 `Set` 。

 如果 [Set 语句](../statements/set-statement.md) 是使用比 [属性语句](../statements/property-statement.md)更严格的访问级别进行标记，则在以下情况下，尝试设置该属性值可能会失败：

- `Set`语句标记为[Private](../modifiers/private.md) ，并且调用代码位于定义该属性的类或结构之外。

- `Set`语句被标记为[受保护](../modifiers/protected.md)，调用代码不在定义该属性的类或结构中，也不在派生类中。

- `Set`语句被标记为[Friend](../modifiers/friend.md) ，调用代码不在定义该属性的程序集中。

 **错误 ID：** BC31102

## <a name="to-correct-this-error"></a>更正此错误

- 如果你可以控制定义属性的源代码，请考虑 `Set` 使用与属性本身相同的访问级别声明过程。

- 如果你不能控制定义属性的源代码，或者必须限制 `Set` 过程访问级别，而不是属性本身，请尝试将设置属性值的语句移到对属性具有更好访问权限的代码区域。

## <a name="see-also"></a>另请参阅

- [Property 过程](../../programming-guide/language-features/procedures/property-procedures.md)
- [如何：声明具有混合访问级别的属性](../../programming-guide/language-features/procedures/how-to-declare-a-property-with-mixed-access-levels.md)
