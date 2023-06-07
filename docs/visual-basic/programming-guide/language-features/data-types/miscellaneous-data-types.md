---
title: 杂项数据类型
ms.date: 07/20/2015
helpviewer_keywords:
- Object data type [Visual Basic], data types
- data types [Visual Basic], choosing
ms.assetid: 64c71a12-9057-4dbf-baca-7379c4aada69
ms.openlocfilehash: 45b44abc24b968f19b456cbe0be0f25efc8f0ce8
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91095455"
---
# <a name="miscellaneous-data-types-visual-basic"></a>其他数据类型 (Visual Basic)

Visual Basic 提供了多种数据类型，这些数据类型不是面向数字或字符。 而是处理专用数据，如 "是/否" 值、日期/时间值和对象地址。  
  
 有关显示 Visual Basic 数据类型的并行比较的表，请参阅 [数据类型](../../../language-reference/data-types/index.md)。  
  
## <a name="boolean-type"></a>布尔类型  

 [布尔数据类型](../../../language-reference/data-types/boolean-data-type.md)是解释为或的无符号值 `True` `False` 。 其数据宽度取决于实现平台。 如果变量只能包含两个状态值，如 true/false、yes/no 或 on/off，则将其声明为 `Boolean` 。  
  
## <a name="date-type"></a>日期类型  

 [日期数据类型](../../../language-reference/data-types/date-data-type.md)是包含日期和时间信息的64位值。 每个增量都表示100毫微秒，自开始 (12:00 AM) ，截止时间为公历第1年1月1日。 如果变量可以包含日期值和/或时间值，则将其声明为 `Date` 。  
  
## <a name="object-type"></a>对象类型  

 [Object 数据类型](../../../language-reference/data-types/object-data-type.md)是一个32位地址，它指向应用程序或其他应用程序中的对象实例。 `Object`变量可以引用应用程序识别的任何对象，或引用任何数据类型的数据。 这包括 *值类型*（如 `Integer` 、 `Boolean` 和结构实例）和 *引用类型*（即从和等类创建的对象的实例 `String` <xref:System.Windows.Forms.Form> ）和数组实例。  
  
 如果变量存储指向在编译时不知道的类的实例的指针，或者如果该变量可以指向各种数据类型的数据，则将其声明为 `Object` 。  
  
 数据类型的优点在于， `Object` 您可以使用数据类型来存储任何数据类型的数据。 缺点是您会产生额外的操作，这些操作需要更长的执行时间，并使应用程序的运行速度更慢。 如果 `Object` 对值类型使用变量，则会产生 *装箱* 和 *取消装箱*。 如果将其用于引用类型，则会产生 *后期绑定*。  
  
## <a name="see-also"></a>请参阅

- [类型字符](type-characters.md)
- [基本数据类型](elementary-data-types.md)
- [数值数据类型](numeric-data-types.md)
- [字符数据类型](character-data-types.md)
- [数据类型疑难解答](troubleshooting-data-types.md)
- [早期绑定和后期绑定](../early-late-binding/index.md)
