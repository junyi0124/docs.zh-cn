---
title: 声明上下文和默认访问级别
ms.date: 07/20/2015
helpviewer_keywords:
- module level, defined
- declaration contexts, Visual Basic
- procedure level, defined
- namespace level, defined
- access levels, Visual Basic
- access levels, default levels
ms.assetid: bf63b96e-e825-4745-88c8-5dae222728db
ms.openlocfilehash: a659481b34b8b44f1f387b464d5d9656ed98ab3f
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874954"
---
# <a name="declaration-contexts-and-default-access-levels-visual-basic"></a>声明上下文和默认访问级别 (Visual Basic)

本主题介绍了哪些 Visual Basic 类型可以在哪些其他类型中进行声明，如果未指定，它们的访问级别将默认为。  
  
## <a name="declaration-context-levels"></a>声明上下文级别  

 编程元素的 *声明上下文* 是声明它的代码区域。 这通常是另一个编程元素，该元素称为 " *包含元素*"。  
  
 声明上下文的级别如下：  
  
- *命名空间级别* -在源文件或命名空间中，但不在类、结构、模块或接口中  
  
- *模块级别* -在类、结构、模块或接口中，但不在过程或块中  
  
- *过程级别* -在过程或块中 (如 `If` 或 `For`)   
  
 下表显示了各种已声明的编程元素的默认访问级别，具体取决于它们的声明上下文。  
  
|已声明的元素|命名空间级别|模块级别|过程级别|  
|----------------------|---------------------|------------------|---------------------|  
|变量 ([Dim 语句](dim-statement.md)) |不允许|`Private` (`Public` 在中 `Structure` ，不允许在) 中使用 `Interface`|`Public`|  
|常量 ([Const 语句](const-statement.md)) |不允许|`Private` (`Public` 在中 `Structure` ，不允许在) 中使用 `Interface`|`Public`|  
|枚举 ([枚举语句](enum-statement.md)) |`Friend`|`Public`|不允许|  
|类 [语句](class-statement.md) (类) |`Friend`|`Public`|不允许|  
|结构 ([结构语句](structure-statement.md)) |`Friend`|`Public`|不允许|  
|Module ([Module 语句](module-statement.md)) |`Friend`|不允许|不允许|  
|Interface ([Interface 语句](interface-statement.md)) |`Friend`|`Public`|不允许|  
|Procedure ([函数语句](function-statement.md)， [Sub 语句](sub-statement.md)) |不允许|`Public`|不允许|  
|外部引用 ([声明语句](declare-statement.md)) |不允许|`Public`不允许在) 中 (`Interface`|不允许|  
|运算符 ([运算符语句](operator-statement.md)) |不允许|`Public``Interface`或) 不允许 (`Module`|不允许|  
|Property ([属性语句](property-statement.md)) |不允许|`Public`|不允许|  
|[默认属性 (默认](../modifiers/default.md)属性) |不允许|`Public`不允许在) 中 (`Module`|不允许|  
|Event ([事件语句](event-statement.md)) |不允许|`Public`|不允许|  
|委托 ([委托语句](delegate-statement.md)) |`Friend`|`Public`|不允许|  
  
 有关详细信息，请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。  
  
## <a name="see-also"></a>另请参阅

- [Friend](../modifiers/friend.md)
- [专用](../modifiers/private.md)
- [公共](../modifiers/public.md)
