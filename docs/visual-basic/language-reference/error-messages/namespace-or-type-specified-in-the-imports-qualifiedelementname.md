---
title: Imports“<qualifiedelementname>”中指定的命名空间或类型不包含任何公共成员，或者找不到该命名空间或类型
ms.date: 07/20/2015
f1_keywords:
- bc40056
- vbc40056
helpviewer_keywords:
- BC40056
ms.assetid: b59f5754-444f-4378-9272-9678b437e84a
ms.openlocfilehash: 284a8c71fee8835f78ca5435932819fded1b1f30
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160127"
---
# <a name="bc40056-namespace-or-type-specified-in-the-imports-qualifiedelementname-doesnt-contain-any-public-member-or-cannot-be-found"></a>BC40056：在导入 "" 中指定的命名空间或类型 \<qualifiedelementname> 不包含任何公共成员，或者找不到该命名空间或类型

导入 "" 中指定的命名空间或类型 \<qualifiedelementname> 不包含任何公共成员或找不到该命名空间或类型。 请确保定义命名空间或类型，并且至少包含一个公共成员。 请确保别名不包含其他别名。

`Imports`语句指定了一个包含元素，该元素无法找到或未定义任何 `Public` 成员。

*包含元素*可以是命名空间、类、结构、模块、接口或枚举。 包含元素包含变量、过程或其他包含元素等成员。

导入的目的是允许你的代码访问命名空间或类型成员，而无需对其进行限定。 你的项目可能还需要添加对命名空间或类型的引用。 有关详细信息，请参阅对已 [声明元素的引用](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)中的 "导入包含元素"。

如果编译器找不到指定的包含元素，则它无法解析使用它的引用。 如果找到元素但元素未公开任何 `Public` 成员，则不会成功进行引用。 在这两种情况下，导入元素是毫无意义的。

请记住，如果导入包含元素并为其分配了导入别名，则不能使用该导入别名导入另一个元素。 下面的代码生成编译器错误。

```vb
Imports winfrm = System.Windows.Forms

' The following statement is INVALID  because it reuses an import alias.

Imports behave = winfrm.Design.Behavior`
```

**错误 ID：** BC40056

## <a name="to-correct-this-error"></a>更正此错误

1. 验证是否可从你的项目访问包含元素。

2. 验证包含元素的规范是否不包括来自其他导入的任何导入别名。

3. 验证包含元素是否至少公开一个 `Public` 成员。

## <a name="see-also"></a>另请参阅

- [Imports 语句（.NET 命名空间和类型）](../statements/imports-statement-net-namespace-and-type.md)
- [Namespace 语句](../statements/namespace-statement.md)
- [公共](../modifiers/public.md)
- [Visual Basic 中的命名空间](../../programming-guide/program-structure/namespaces.md)
- [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
