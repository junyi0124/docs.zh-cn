---
title: 属性“<propertyname>”并非在所有代码路径上都返回值
ms.date: 07/20/2015
f1_keywords:
- bc42107
- vbc42107
helpviewer_keywords:
- BC42107
ms.assetid: 06800966-9c3b-4844-9f13-83ac95607d32
ms.openlocfilehash: 0736e2cbcdea1422d40161c88f898d487f72e3bc
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162383"
---
# <a name="bc42107-property-propertyname-doesnt-return-a-value-on-all-code-paths"></a>BC42107：属性 " \<propertyname> " 不会在所有代码路径上都返回值

属性 " \<propertyname> " 不会在所有代码路径上都返回值。 使用该结果时，可能会在运行时发生 null 引用异常。

属性 `Get` 过程至少有一个可能的路径，其代码中不返回值。

 可以通过 `Get` 以下任一方式从属性过程返回值：

- 将值分配给属性名称，然后执行 `Exit Property` 语句。

- 将值分配给属性名称，然后执行 `End Get` 语句。

- 在 [Return 语句](../statements/return-statement.md)中包含值。

如果控件传递给 `Exit Property` 或 `End Get` ，并且没有为属性名称分配任何值，则该 `Get` 过程将返回属性数据类型的默认值。 有关详细信息，请参阅 [Function 语句](../statements/function-statement.md)中的 "行为"。

默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的详细信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

**错误 ID：** BC42107

## <a name="to-correct-this-error"></a>更正此错误

- 检查控制流逻辑，并确保在导致返回的每个语句之前赋值。

  如果始终使用语句，则更容易保证每次从过程返回一个值 `Return` 。 如果执行此操作，则之前的最后一个语句 `End Get` 应为 `Return` 语句。

## <a name="see-also"></a>另请参阅

- [Property 过程](../../programming-guide/language-features/procedures/property-procedures.md)
- [Property Statement](../statements/property-statement.md)
- [Get 语句](../statements/get-statement.md)
