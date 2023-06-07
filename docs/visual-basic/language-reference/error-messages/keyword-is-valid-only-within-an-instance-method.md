---
title: “<keyword>”只在实例方法中有效
ms.date: 07/20/2015
f1_keywords:
- bc30043
- vbc30043
helpviewer_keywords:
- BC30043
ms.assetid: 7973aa82-a681-440c-9bca-242627d7ba86
ms.openlocfilehash: ad39ade294b362b20f2dfb93455445bf41d056cd
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163319"
---
# <a name="bc30043-keyword-is-valid-only-within-an-instance-method"></a>BC30043： " \<keyword> " 仅在实例方法中有效

`Me`、 `MyClass` 和 `MyBase` 关键字引用特定的类实例。 不能在共享或过程中使用它们 `Function` `Sub` 。

*错误 ID：** BC30043

## <a name="to-correct-this-error"></a>更正此错误

- 从过程中删除关键字，或 `Shared` 从过程声明中删除关键字。

## <a name="see-also"></a>另请参阅

- [对象变量赋值](../../programming-guide/language-features/variables/object-variable-assignment.md)
- [Me、My、MyBase 和 MyClass](../../programming-guide/program-structure/me-my-mybase-and-myclass.md)
- [继承基础知识](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
