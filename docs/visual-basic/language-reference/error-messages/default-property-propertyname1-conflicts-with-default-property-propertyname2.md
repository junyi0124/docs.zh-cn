---
title: 默认属性“<propertyname1>”与“<propertyname2>”中的默认属性“<classname>”冲突，因此应声明为“Shadows”
ms.date: 07/20/2015
f1_keywords:
- vbc40007
- bc40007
helpviewer_keywords:
- BC40007
ms.assetid: 692ccf76-5715-4f11-a972-84cf9de30bc1
ms.openlocfilehash: 290971a3173c59f08fbd279b6fffe3bcb618cb72
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160602"
---
# <a name="bc40007-default-property-propertyname1-conflicts-with-default-property-propertyname2-in-classname-and-so-should-be-declared-shadows"></a>BC40007：默认属性 " \<propertyname1> " 与 "" 中的默认属性 "" 冲突 \<propertyname2> \<classname> ，因此应声明为 "Shadows"

使用与基类中定义的属性相同的名称声明属性。 在这种情况下，此类中的属性应隐藏基类属性。

 此消息是一个警告。 默认假定`Shadows` 。 有关隐藏警告或将警告视为错误的详细信息，请参阅 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC40007

## <a name="to-correct-this-error"></a>更正此错误

- 将 `Shadows` 关键字添加到声明中，或更改所声明的属性的名称。

## <a name="see-also"></a>另请参阅

- [阴影](../modifiers/shadows.md)
- [Visual Basic 中的隐藏](../../programming-guide/language-features/declared-elements/shadowing.md)
