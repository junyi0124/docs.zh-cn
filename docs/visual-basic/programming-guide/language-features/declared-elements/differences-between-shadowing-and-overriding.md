---
title: 隐藏和重写之间的差异
ms.date: 07/20/2015
helpviewer_keywords:
- shadowing, vs. overriding
- overriding, vs. shadowing
ms.assetid: 2d014a0b-7630-407d-8f4e-24bd87987923
ms.openlocfilehash: 98c073f8fa403416b2425431ff4334b990726f44
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095442"
---
# <a name="differences-between-shadowing-and-overriding-visual-basic"></a>隐藏和重写之间的差异 (Visual Basic)

在定义继承自基类的类时，有时需要重新定义派生类中的一个或多个基类元素。 隐藏和重写都可用于此目的。  
  
## <a name="comparison"></a>比较  

 当派生类从基类继承时使用隐藏和重写，并且这两种方法都将一个已声明的元素重定义为另一个。 但两者之间存在重大差异。  
  
 下表将隐藏与重写进行比较。  
  
||||  
|---|---|---|  
|比较点|阴影操作|替代|  
|用途|针对引入已在派生类中定义的成员的后续基类修改进行保护|通过定义具有相同调用序列的过程或属性的不同实现来实现多态性<sup>1</sup>|  
|重新定义的元素|任何声明的元素类型|只有过程 (`Function` 、 `Sub` 或 `Operator`) 或属性|  
|重定义元素|任何声明的元素类型|仅具有相同调用序列的过程或属性<sup>1</sup>|  
|重定义元素的访问级别|任何访问级别|无法更改重写元素的访问级别|  
|重定义元素的可读性和可写性|任何组合|无法更改重写属性的可读性或可写性|  
|控制重定义|基类元素无法强制或禁止隐藏|基类元素可以指定 `MustOverride` 、 `NotOverridable` 或 `Overridable`|  
|关键字用法|`Shadows`建议在派生类中使用;`Shadows`如果和均 `Shadows` 未 `Overrides` 指定<sup>2</sup> ，则假定|`Overridable` 或 `MustOverride` 在基类中是必需的; `Overrides` 派生类中需要|  
|从派生类派生的类的重定义元素的继承|由进一步的派生类继承的隐藏元素;隐藏的元素仍隐藏<sup>3</sup>|重写由进一步派生的类继承的元素;重写的元素仍将被重写|  
  
 <sup>1</sup> *调用序列* 由元素类型（ (`Function` 、 `Sub` 、 `Operator` 或 `Property`) 、名称、参数列表和返回类型）组成。 不能使用属性或其他方法来重写过程。 不能用另一种类型的过程 (`Function` 、 `Sub` 或 `Operator`) 进行重写。  
  
 <sup>2</sup> 如果未指定 `Shadows` 或 `Overrides` ，则编译器会发出警告消息，帮助您确定要使用哪种重新定义。 如果忽略该警告，则使用隐藏机制。  
  
 <sup>3</sup> 如果无法在进一步的派生类中访问隐藏元素，则不会继承隐藏。 例如，如果将隐藏元素声明为 `Private` ，派生自派生类的类将继承原始元素，而不是隐藏元素。  
  
## <a name="guidelines"></a>指南  

 在以下情况下，通常使用重写：  
  
- 你要定义多态派生类。  
  
- 您希望安全地让编译器强制执行相同的元素类型和调用序列。  
  
 通常会在以下情况下使用隐藏：  
  
- 你预计可能会修改基类，并使用与你的名称相同的名称来定义元素。  
  
- 您希望自由更改元素类型或调用序列。  
  
## <a name="see-also"></a>请参阅

- [References to Declared Elements](references-to-declared-elements.md)
- [Visual Basic 中的隐藏](shadowing.md)
- [如何：隐藏与变量同名的变量](how-to-hide-a-variable-with-the-same-name-as-your-variable.md)
- [如何：隐藏继承的变量](how-to-hide-an-inherited-variable.md)
- [如何：访问被派生类隐藏的变量](how-to-access-a-variable-hidden-by-a-derived-class.md)
- [Shadows](../../../language-reference/modifiers/shadows.md)
- [替代](../../../language-reference/modifiers/overrides.md)
