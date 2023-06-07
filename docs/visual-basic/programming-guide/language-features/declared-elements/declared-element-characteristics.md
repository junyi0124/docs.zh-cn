---
title: 已声明元素的特性
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic], lifetime
- access levels, declared elements
- declared elements [Visual Basic], scope
- visibility [Visual Basic], declared elements
- elements [Visual Basic], programming
- scope [Visual Basic], declared elements
- lifetime [Visual Basic], declared elements
- declared elements [Visual Basic], access level
- data types [Visual Basic], declared elements
- declared elements [Visual Basic], visibility
ms.assetid: 1bc40fb8-b67c-4428-90a4-76b630ae2583
ms.openlocfilehash: 36c55475b4930dc6c3202d52ef742072d5cee3e1
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075273"
---
# <a name="declared-element-characteristics-visual-basic"></a>已声明元素的特性 (Visual Basic)

已声明元素的 *特性* 是该元素的一个方面，它会影响代码与之进行交互的方式。 每个已声明的元素都具有与之关联的一个或多个以下特性：  
  
- *数据类型* -元素可以保存的值，以及存储这些值的方式。 有关详细信息，请参阅[数据类型](../../../language-reference/data-types/index.md)。  
  
- *生存期* —元素可供使用的执行时间段。 有关详细信息，请参阅 [Visual Basic 中的生存期](lifetime.md)。  
  
- *作用域* —可以引用元素但不限定其名称的所有代码集。 有关详细信息，请参阅 [如何：控制变量的范围](how-to-control-the-scope-of-a-variable.md)。  
  
- *访问级别* -代码使用元素的权限。 有关详细信息，请参阅 [如何：控制变量的可用性](how-to-control-the-availability-of-a-variable.md)。  
  
## <a name="characteristics-of-the-elements"></a>元素的特征  

 下表显示了已声明的元素和应用于每个元素的特性。  
  
|元素|数据类型|生存期|作用域 <sup>1</sup>|访问级别|  
|-------------|---------------|--------------|------------------------|------------------|  
|变量|是|是|是|是|  
|常数|是|否|是|是|  
|枚举|是|否|是|是|  
|结构|否|否|是|是|  
|属性|是|是|是|是|  
|方法|否|是|是|是|  
|过程 (`Sub` 或 `Function`) |否|是|是|是|  
|过程参数|是|是|是|否|  
|函数返回|是|是|是|否|  
|运算符|是|否|是|是|  
|接口|否|否|是|是|  
|类|否|否|是|是|  
|事件|否|否|是|是|  
|委托|否|否|是|是|  
  
 <sup>1</sup> 范围有时称为 *可见性*。  
  
## <a name="see-also"></a>请参阅

- [已声明元素](index.md)
- [Declared Element Names](declared-element-names.md)
- [References to Declared Elements](references-to-declared-elements.md)
- [Visual Basic 中的生存期](lifetime.md)
- [Visual Basic 中的范围](scope.md)
- [Visual Basic 中的访问级别](access-levels.md)
- [数据类型](../data-types/index.md)
- [变量声明](../variables/variable-declaration.md)
