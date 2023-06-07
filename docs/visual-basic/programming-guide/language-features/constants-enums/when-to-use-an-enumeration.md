---
title: 何时使用枚举
ms.date: 07/20/2015
helpviewer_keywords:
- enumerations [Visual Basic]
ms.assetid: e6e47b5b-3ed9-452d-a481-9c3fed88519a
ms.openlocfilehash: 7b1b269a5d28d89cd491bac88fbefd4547fdc3c3
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095624"
---
# <a name="when-to-use-an-enumeration-visual-basic"></a>何时使用枚举 (Visual Basic)

枚举提供了一种简单的方法来处理相关的常量集。 枚举（或 `Enum` ）是一组值的符号名称。 枚举被视为数据类型，可以使用它们来创建用于变量和属性的常量集。  
  
## <a name="when-to-use-an-enumeration"></a>何时使用枚举  

 只要过程接受有限的一组变量，就应考虑使用枚举。 枚举可用于更清晰且更易读的代码，特别是在使用有意义的名称时。  
  
 使用枚举的优点包括：  
  
- 减少由转置或错误错误导致的错误。  
  
- 以后可以轻松地更改值。  
  
- 使代码更易于阅读，这意味着错误会在其中蔓延。  
  
- 确保向前兼容性。 对于枚举，如果将来有人更改了与成员名称相对应的值，你的代码将不太可能失败。  
  
## <a name="naming-enumerations"></a>命名枚举  

 为枚举成员使用命名约定。 当 Visual Basic 遇到枚举成员名称时，如果其他引用的类型库包含相同的名称，可能会引发异常。 使用唯一的前缀来标识应用程序或组件中的值。  
  
 当引用枚举的成员时，必须使用枚举名称限定成员名称，否则请使用 `Imports` 语句。 有关详细信息，请参阅 [枚举和名称限定](enumerations-and-name-qualification.md)。  
  
## <a name="predefined-enumerations"></a>预定义枚举  

 Visual Basic 提供了许多预定义的枚举（如 `FirstDayOfWeek` 和 `MsgBoxResult` ），以方便您的代码。 有关这些变量的列表，请参阅 [常量和枚举](../../../language-reference/constants-and-enumerations.md)。  
  
## <a name="see-also"></a>请参阅

- [如何：声明枚举](how-to-declare-enumerations.md)
- [如何：引用枚举成员](how-to-refer-to-an-enumeration-member.md)
- [枚举和名称限定](enumerations-and-name-qualification.md)
- [如何：在 Visual Basic 中循环访问枚举](how-to-iterate-through-an-enumeration.md)
- [如何：确定与枚举值关联的字符串](how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [Enum 语句](../../../language-reference/statements/enum-statement.md)
- [常量和枚举](../../../language-reference/constants-and-enumerations.md)
