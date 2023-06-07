---
title: 数据类型摘要
ms.date: 07/20/2015
helpviewer_keywords:
- Boolean data type [Visual Basic], supported types in Visual Basic
- storage [Visual Basic], order of storage
- data types [Visual Basic], Visual Basic
- Single data type [Visual Basic], supported types in Visual Basic
- notation [Visual Basic], scientific
- memory requirements, data types
- user-defined data types [Visual Basic], Visual Basic
- Date data type [Visual Basic], Visual Basic
- Visual Basic, data types
- storage [Visual Basic], allocation
- Integer data type [Visual Basic], Visual Basic data types
- storage [Visual Basic], space
- Variant data types [Visual Basic], supported types in Visual Basic
- Char data type [Visual Basic], Visual Basic data types
- intrinsic data types [Visual Basic]
- memory consumption [Visual Basic], data types
- single-precision numbers
- data types [Visual Basic], order of storage
- Long data type [Visual Basic], supported types in Visual Basic
- String data type [Visual Basic], Visual Basic data types
- storage order, data types
- StructLayoutAttribute class, Visual Basic data type storage
- scientific notation
- Double data type [Visual Basic], Visual Basic data types
- Byte data type [Visual Basic], Visual Basic data types
- Object data type [Visual Basic], supported types in Visual Basic
- data types [Visual Basic], storage allocation
- double-precision numbers
- data types [Visual Basic], summary
- dates [Visual Basic], data types
- strings [Visual Basic], data types
- memory consumption
- storage order, controlling in Visual Basic
- data types [Visual Basic], memory requirements
ms.assetid: e975cdb6-64d8-4a4a-ae27-f3b3ed198ae0
ms.openlocfilehash: 5eb52ef937a677c0b7498d058b5a39a375351ddc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415617"
---
# <a name="data-type-summary-visual-basic"></a>数据类型摘要 (Visual Basic)

下表显示了 Visual Basic 的数据类型、其支持的公共语言运行时类型、其名义存储分配及其值范围。  
  
|Visual Basic 类型|公共语言运行时类型结构|标称存储分配|取值范围|  
|-----------------------|--------------------------------------------|--------------------------------|-----------------|  
|[布尔值](boolean-data-type.md)|<xref:System.Boolean>|依赖于实现平台|`True` 或 `False`|  
|[Byte](byte-data-type.md)|<xref:System.Byte>|1 个字节|0到255（无符号）|  
|[Char](char-data-type.md) （单个字符）|<xref:System.Char>|2 个字节|0到65535（无符号）|  
|[Date](date-data-type.md)|<xref:System.DateTime>|8 个字节|0:00:00 年1月1日至12月31日下午11:59:59，（午夜）9999|  
|[小数](decimal-data-type.md)|<xref:System.Decimal>|16 个字节|0到 +/-79228162514264337593543950335 （+/-7.9...E + 28） <sup>†</sup> ，无小数点;0到 +/-7.9228162514264337593543950335，小数点右侧有28个位置;<br /><br /> 最小非零数字为 +/-0.0000000000000000000000000001 （+/-1E-28） <sup>†</sup>|  
|[双](double-data-type.md)精度（双精度浮点）|<xref:System.Double>|8 个字节|-1.79769313486231570 e + 308 到-4.94065645841246544 E-324 <sup>†</sup>表示负值;<br /><br /> 4.94065645841246544 e-324 到 1.79769313486231570 E + 308 <sup>†</sup>用于正值|  
|[整数](integer-data-type.md)|<xref:System.Int32>|4 个字节|-2147483648 到2147483647（有符号）|  
|[Long](long-data-type.md) （长整型）|<xref:System.Int64>|8 个字节|-9223372036854775808 到9223372036854775807（9.2. E + 18 <sup>†</sup>）（有符号）|  
|[对象](object-data-type.md)|<xref:System.Object>班级|32位平台上的4个字节<br /><br /> 64位平台上的8个字节|任何类型都可以存储在类型的变量中`Object`|  
|[SByte](sbyte-data-type.md)|<xref:System.SByte>|1 个字节|-128 到127（有符号）|  
|[Short](short-data-type.md) （短整型）|<xref:System.Int16>|2 个字节|-32768 到32767（有符号）|  
|[单](single-data-type.md)精度（单精度浮点）|<xref:System.Single>|4 个字节|-3.4028235 e + 38 到-1.401298 E-45 <sup>†</sup>表示负值;<br /><br /> 1.401298 e-45 到 3.4028235 E + 38 <sup>†</sup>用于正值|  
|[字符串](string-data-type.md)（可变长度）|<xref:System.String>班级|依赖于实现平台|0到约 2000000000 Unicode 字符|  
|[UInteger](uinteger-data-type.md)|<xref:System.UInt32>|4 个字节|0到4294967295（无符号）|  
|[ULong](ulong-data-type.md)|<xref:System.UInt64>|8 个字节|0到18446744073709551615（1.8 ... E + 19 <sup>†</sup>）（无符号）|  
|[用户定义的](user-defined-data-type.md)（结构）|（继承自 <xref:System.ValueType> ）|依赖于实现平台|结构的每个成员都有一个由其数据类型确定的范围，并与其他成员的范围无关|  
|[UShort](ushort-data-type.md)|<xref:System.UInt16>|2 个字节|0到65535（无符号）|  
  
 <sup>†</sup>使用*科学记数法*时，"E" 指的是10的幂。 因此，3.56 E + 2 表示 3.56 x 10<sup>2</sup>或356，3.56 e-2 表示 3.56/10<sup>2</sup>或0.0356。  
  
> [!NOTE]
> 对于包含文本的字符串，请使用 <xref:Microsoft.VisualBasic.Strings.StrConv%2A> 函数从一种文本格式转换为另一种文本格式。  
  
 除了在声明语句中指定数据类型之外，还可以使用类型字符强制某些编程元素的数据类型。 请参阅[类型字符](../../programming-guide/language-features/data-types/type-characters.md)。  
  
## <a name="memory-consumption"></a>内存消耗  

 在声明基本数据类型时，不安全地假定其内存消耗与其名义存储分配相同。 这是由于下列注意事项：  
  
- **存储分配。** 公共语言运行时可以基于执行应用程序的平台的当前特性来分配存储空间。 如果内存几乎已满，则可能会将声明的元素尽可能紧密地打包在一起。 在其他情况下，它可能会将内存地址与自然硬件边界对齐，以优化性能。  
  
- **平台宽度。** 64位平台上的存储分配不同于32位平台上的分配。  
  
### <a name="composite-data-types"></a>复合数据类型  

 相同的注意事项也适用于复合数据类型的每个成员，如结构或数组。 你不能只是将类型成员的标称存储分配组合在一起。 此外，还有其他注意事项，如以下内容：  
  
- **费用.** 某些复合类型具有额外的内存需求。 例如，数组对数组本身和每个维度使用额外的内存。 在32位平台上，此开销目前为每个维度12个字节加上8个字节。 在64位平台上，此要求翻倍。  
  
- **存储布局。** 不能安全地假设内存中存储的顺序与声明顺序相同。 甚至不能对字节对齐进行假设，如2字节或4字节边界。 如果要定义类或结构，并且需要控制其成员的存储布局，则可以将属性应用于 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 类或结构。  
  
### <a name="object-overhead"></a>对象开销  

 `Object`除数据类型中包含的数据外，引用任何基本数据类型或复合数据类型的也使用4个字节。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Strings.StrConv%2A>
- <xref:System.Runtime.InteropServices.StructLayoutAttribute>
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [转换摘要](../keywords/conversion-summary.md)
- [类型字符](../../programming-guide/language-features/data-types/type-characters.md)
- [有效使用数据类型](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
