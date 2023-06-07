---
title: 返回 CStr 函数的值
ms.date: 07/20/2015
helpviewer_keywords:
- times [Visual Basic], CStr Function return values
- type conversion [Visual Basic]
- dates [Visual Basic], CStr Function return values
- CStr function
- strings [Visual Basic], return value
- Date data type [Visual Basic], converting
- dates [Visual Basic]
- String data type [Visual Basic], converting
ms.assetid: 3aa744e7-1419-45d5-85e3-e5abc2953673
ms.openlocfilehash: cc5e5cc437175e9aebfba559488ca74283faa9ad
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90870160"
---
# <a name="return-values-for-the-cstr-function-visual-basic"></a>返回 CStr 函数的值 (Visual Basic)

下表描述了的 `CStr` 不同数据类型的返回值 `expression` 。  
  
|如果 `expression` 类型为|`CStr` 返回|  
|-----------------------------|--------------------|  
|[布尔数据类型](../data-types/boolean-data-type.md)|包含 "True" 或 "False" 的字符串。|  
|[日期数据类型](../data-types/date-data-type.md)|一个字符串，其中包含 `Date` 以您的系统的短日期格式 (日期和时间) 值。|  
|[数值数据类型](../../programming-guide/language-features/data-types/numeric-data-types.md)|表示数字的字符串。|  
  
## <a name="cstr-and-date"></a>CStr 和日期  

 `Date`类型始终包含日期和时间信息。 出于类型转换的目的，Visual Basic 会考虑 (1/1/0001 年1月1日) 为日期的 *非特定值* ，00:00:00 (午夜) 为时间的非特定值。 `CStr` 不会在生成的字符串中包含非特定值。 例如，如果转换 `#January 1, 0001 9:30:00#` 为字符串，则结果为 "9:30:00 AM"; 日期信息将被取消。 但是，日期信息仍以原始值的形式存在 `Date` ，可以通过等函数进行恢复 <xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A> 。  
  
> [!NOTE]
> `CStr`函数根据应用程序的当前区域性设置来执行转换。 若要获取特定区域性中的数字的字符串表示形式，请使用数字的 `ToString(IFormatProvider)` 方法。 例如，在将 <xref:System.Double.ToString%2A?displayProperty=nameWithType> 类型的值转换为时使用 `Double` `String` 。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A>
- [Type Conversion Functions](type-conversion-functions.md)
- [布尔数据类型](../data-types/boolean-data-type.md)
- [日期数据类型](../data-types/date-data-type.md)
