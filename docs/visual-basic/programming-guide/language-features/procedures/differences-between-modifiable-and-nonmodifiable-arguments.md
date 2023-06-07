---
title: 可修改和不可修改参数之间的差异
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedure arguments
- arguments [Visual Basic], nonmodifiable
- Visual Basic code, procedures
- arguments [Visual Basic], modifiable
ms.assetid: 87b2df69-e1f7-4657-9caf-b3f48d693428
ms.openlocfilehash: 662ad3039bb3fd5c44847d5b2a97a033a18ad063
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071952"
---
# <a name="differences-between-modifiable-and-nonmodifiable-arguments-visual-basic"></a>可修改和不可修改自变量之间的差异 (Visual Basic)

调用过程时，通常会向其传递一个或多个参数。 每个参数都对应于一个基础编程元素。 基础元素和参数本身可以是可修改的，也可以是不可更改的。  
  
## <a name="modifiable-and-nonmodifiable-elements"></a>可修改和不可更改元素  

 编程元素可以是可修改的 *元素*（其值已更改）或 *不可更改元素*（在创建后具有固定值）。  
  
 下表列出了可修改和不可更改的编程元素。  
  
|可修改元素|不可更改元素|  
|-------------------------|----------------------------|  
| (在过程) 中声明的局部变量，包括对象变量（只读除外）|只读变量、字段和属性|  
|字段 (模块、类和结构的成员变量) ，只读除外|常量和文本|  
|属性（只读除外）|枚举成员|  
|数组元素|即使表达式的元素可修改，表达式 () |  
  
## <a name="modifiable-and-nonmodifiable-arguments"></a>可修改和不可修改参数  

 可 *修改的自变量* 是具有可修改基础元素的自变量。 调用代码随时可以存储新值，并且如果传递参数 [ByRef](../../../language-reference/modifiers/byref.md)，则过程中的代码还可以修改调用代码中的基础元素。  
  
 *不可更改的参数*具有不可更改的基础元素，或被传递了[ByVal](../../../language-reference/modifiers/byval.md)。 过程不能修改调用代码中的基础元素，即使它是可修改的元素。 如果它是不可更改的元素，则调用代码本身无法修改它。  
  
 被调用的过程可能会修改其不可更改参数的本地副本，但该修改不会影响调用代码中的基础元素。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [如何：将参数传递给过程](./how-to-pass-arguments-to-a-procedure.md)
- [按值和按引用传递参数](./passing-arguments-by-value-and-by-reference.md)
- [通过值传递参数和通过引用传递参数之间的差异](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [如何：更改过程参数的值](./how-to-change-the-value-of-a-procedure-argument.md)
- [如何：防止过程参数的值被更改](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [如何：强制通过值传递参数](./how-to-force-an-argument-to-be-passed-by-value.md)
- [按位置和按名称传递自变量](./passing-arguments-by-position-and-by-name.md)
- [Value Types and Reference Types](../data-types/value-types-and-reference-types.md)
