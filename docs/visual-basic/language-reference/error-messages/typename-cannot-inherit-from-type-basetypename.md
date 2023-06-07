---
title: “<typename>”将对基 <type> 的访问扩展到程序集的外部，因此无法从 <basetypename>“<type>”继承
ms.date: 07/20/2015
f1_keywords:
- vbc30910
- bc30910
helpviewer_keywords:
- BC30910
ms.assetid: 68fc05c5-5d55-4742-9a3b-ea04312594f4
ms.openlocfilehash: 5c019f9d74b11e48aa05a1480b9449fa28488b43
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161837"
---
# <a name="bc30910-typename-cannot-inherit-from-type-basetypename-because-it-expands-the-access-of-the-base-type-outside-the-assembly"></a>BC30910： " \<typename> " 不能从 \<type> " \<basetypename> " 继承，因为它将对基的访问扩展到 \<type> 程序集的外部

类或接口继承自基类或接口，但具有限制性较低的访问级别。

 例如， `Public` 接口从 `Friend` 接口继承，或 `Protected` 从 `Private` 类继承。 这会公开要访问的基类或接口超出目标级别。

 **错误 ID：** BC30910

## <a name="to-correct-this-error"></a>更正此错误

- 更改派生类或接口的访问级别，使其至少与基类或接口的访问级别具有相同的限制。

     或

- 如果需要限制性较低的访问级别，请删除 `Inherits` 语句。 不能从更受限制的基类或接口继承。

## <a name="see-also"></a>另请参阅

- [Class 语句](../statements/class-statement.md)
- [Interface 语句](../statements/interface-statement.md)
- [Inherits Statement](../statements/inherits-statement.md)
- [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)
